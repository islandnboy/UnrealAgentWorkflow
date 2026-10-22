# DataForge 데이터 제작 도구 사례

## Creator를 위한 게임 데이터 제작 도구

DataForge는 게임 데이터를 코드나 Unreal Editor에 직접 반복 입력하지 않고, **기획자가 익숙한 authoring source를 유지하면서 Client와 Server 산출물을 같은 데이터 의미에서 생성·검증하는 제작 툴**입니다.

독립 실행형 GUI와 CLI가 같은 Core를 사용합니다.

📄 [DataForge 사용 가이드 (PDF)](../Docs/Guides/DataForgeUserGuide.pdf) — 실제 화면과 DT·DA·Config 작업, 결과 확인·오류 대응·실행 방법을 담은 7페이지 가이드입니다. (2026-10-07 구현 기준)

## 해결하려는 문제

게임 데이터 제작에는 자주 다음 문제가 생깁니다.

- 기획 데이터와 Unreal DataTable이 서로 달라짐
- Server JSON을 별도로 관리하면서 값이 어긋남
- 전체 데이터를 매번 rebuild함
- generated UAsset을 사람이 직접 고쳐 Source of Truth가 모호해짐
- Unreal 고유 DataAsset / Config 의미를 외부 툴이 잘못 재구현함
- invalid source가 기존 정상 artifact를 덮어씀

DataForge는 Source of Truth와 generated artifact의 방향을 명확히 합니다.

## 데이터 종류별 원본과 책임

| 데이터 종류 | 작성 원본 | DataForge의 역할 |
| --- | --- | --- |
| DataTable | XLSX / XLSM | 검사·정규화·빌드·Bake·검증 |
| DataAsset / PrimaryDataAsset | Unreal UAsset | 엔진 기반 읽기 전용 추출·조회 |
| INI Config | Unreal Config | 엔진 기반 읽기 전용 추출·조회 |

V1에서 Excel authoring은 DataTable에만 적용합니다.

DA / INI를 억지로 Excel로 변환하거나 자동 write-back하지 않습니다.

## DataTable 제작 흐름

~~~text
XLSX / XLSM
    ↓
DataForge
  파싱 / 정규화 / 검사
    ↓
정규화된 빌드 산출물
   ↙                 ↘
Server JSON       UE 빌드 JSON
                      ↓
             SFDataBuildCommandlet
                UDataTable Bake
                      ↓
              별도 프로세스 재조회
                      ↓
                 데이터 일치 검증
                      ↓
                  결과 반영
~~~

Client와 Server가 서로 다른 변환 로직을 가지지 않고 동일 normalized data 의미에서 파생됩니다.

## DA / Config의 엔진 기반 조회

~~~text
UDataAsset / PrimaryDataAsset
          ↓
SFDataBuildCommandlet Extract
          ↓
       DataForge

Unreal Config
     ↓
GConfig 엔진 기능으로 조회
     ↓
DataForge 조회 / 추출
~~~

DataForge가 `.uasset` binary format이나 Unreal Config merge semantics를 Python으로 다시 구현하지 않습니다.

Unreal이 자신의 데이터를 직접 해석하고 DataForge는 그 결과를 소비합니다.

## Creator가 사용하는 GUI

Windows x64에서는 `DataForge.exe`를 직접 실행할 수 있으며 Python 설치가 필요하지 않습니다.

GUI는 다음 작업을 제공합니다.

- DT / DA / Config 탭
- Source / Dataset 목록
- Dirty State — 변경 상태
- Schema Status — Schema 상태
- Warning / Error — 경고·오류
- Last successful build — 마지막 정상 빌드
- Refresh — 상태 새로고침
- Build Selected — 선택한 데이터셋 빌드
- Build Changed — 변경한 데이터셋 빌드
- Rebuild All — 전체 재빌드
- Extract Selected — 선택한 데이터 추출
- Diff / Results / Diagnostics — 변경 비교·결과·진단
- Template / Migration 지원

기획자는 Excel에서 데이터를 작성하고 DataForge에서 변경 대상을 확인한 뒤 필요한 dataset만 build할 수 있습니다.

## 증분 빌드

Refresh가 전체 rebuild를 의미하지 않습니다.

~~~text
Git이 찾은 변경 후보 파일
        ↓
변경된 원본만 조사
        ↓
데이터셋 정규화
        ↓
데이터 의미 기반 Fingerprint
        ↓
마지막 정상 Manifest와 비교
        ↓
Clean / New / Modified /
SchemaChanged / DependencyDirty /
Removed / Invalid
~~~

Git은 후보 범위를 줄이는 데 사용하고 최종 dirty 판정은 dataset semantic diff로 수행합니다.

같은 workbook 안에서도 dataset 단위로 fingerprint를 계산합니다.

`Build Changed`는 실제 영향받은 dataset과 필수 dependency만 처리합니다.

## 안전한 결과 반영

생성된 결과는 바로 publish하지 않습니다.

~~~text
Build
→ PENDING_UE_PARITY
→ Unreal Bake
→ separate Readback
→ semantic parity
→ promote Server JSON / baseline
~~~

실패하면 기존 정상 UAsset / Server JSON / last-successful baseline을 보존하거나 복구합니다.

Refresh는 read-only이며 삭제된 source를 발견했다고 기존 artifact를 자동 삭제하지 않습니다.

## 첫 실제 검증 사례

ProjectSF의 기존 StatGrowth DataTable을 첫 실제 slice로 사용했습니다.

~~~text
Existing UDataTable
→ migration candidate
→ Creator approval
→ XLSX Authoring SoT
→ DataForge build
→ SFDataBuildCommandlet bake
→ UDataTable readback
→ Server JSON
→ parity
~~~

검증 결과:

- Migration 적용 승인(APPROVED)
- 기존 원본 → 작성 데이터 → Bake된 UDataTable 값 일치 PASS
- Bake된 UDataTable → Server JSON 값 일치 PASS
- 별도 Unreal 프로세스에서 재조회 PASS
- 역직렬화 실패 시 기존 Asset Hash 보존
- 사용하지 않는 작성 필드는 경고로 처리
- 누락된 Unreal 필드는 엔진 기본값 적용 가능 여부에 따라 정보 / 경고로 처리
- **핵심 회귀 테스트 25개 PASS**

## 원본 데이터 조회 검증

같은 DataForge entrypoint에서 추가로 확인했습니다.

- 기존 DataTable의 엔진 기반 추출 PASS
- PrimaryDataAsset 읽기 전용 추출
- GConfig를 통한 Unreal Config의 유효 설정값 조회
- 반복 추출 시 정규화된 산출물 일치
- 원본 UAsset / INI 바이트 보존

## 현재 한계

아직 완료된 범위로 주장하지 않는 항목:

- 고급 중첩 / 배열 / GameplayTag / 참조형 Adapter
- 실제 제작 소비자의 데이터 원본 전환
- Server Runtime 로드 / 패키징
- Template / Migration의 전체 기능 범위
- GUI 사용성과 시각적 검수

현재 GUI 실제 실행과 기능 경로는 검증했지만 최종 UX 품질 평가는 별도입니다.

## Agent Workflow와의 연결

DataForge 자체도 Agent Workflow의 중요한 예입니다.

처음부터 모든 데이터를 지원하는 거대한 시스템을 만든 것이 아니라:

~~~text
데이터 제작 목표
→ 기존 데이터 계약 조사
→ 첫 대표 DataTable 선택
→ 실제 기능 흐름의 수직 절편 제작
→ 값 일치 / Rollback 검증
→ Creator의 적용 판단
→ DA / INI 엔진 기반 조회 확장
→ GUI / 사용 가이드
→ reusable Recipe
~~~

실제 제작 결과를 바탕으로 도구와 Recipe를 확장했습니다.

## 이 사례의 핵심

DataForge에서 보여주고 싶은 것은 “Excel importer를 만들었다”가 아닙니다.

**기획자 authoring, Unreal runtime artifact, Server data를 하나의 검증 가능한 production pipeline으로 연결하고, incremental build·rollback·native readback까지 포함한 데이터 제작 체계를 만든 것**이 핵심입니다.
