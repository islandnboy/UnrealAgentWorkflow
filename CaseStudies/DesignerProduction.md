# Creator 주도 제작 사례

> **과거 프로토타입:** 수계 제작 사례는 NeoSeoul 시기의 GIS 입력과 2026-10-07 검증 기록을 사용합니다. Creator와 AI Agent의 제작 사례로 보존하며, 2026-10-14 방향 전환 이후 신규 월드의 검증 성과로 간주하지 않습니다.

## 사례: 강과 수계 제작

이 Case Study는 ProjectSF의 실제 Water Production 작업을 포트폴리오용으로 요약한 것입니다.

내부 저장소 경로, 개인 로컬 경로, 원본 데이터 hash 등은 공개 범위에서 제외했습니다.

## 1. Creator의 제작 의도

초기 요구는 단순히 “강을 만들어라”가 아니었습니다.

핵심 의도는 다음과 같았습니다.

- 서울 기반 World에서 한강과 연결 수계를 데이터 기반으로 재생성할 것
- 기존 정상 결과는 재사용하고 사람 편집을 보호할 것
- River를 단순한 정적 수면으로 만들지 않을 것
- 향후 Swimming / Physics가 사용할 downstream flow 정보를 남길 것
- 러프 전체를 먼저 확보하고 디테일을 후속 반복에서 다듬을 것
- 기술적인 생성 성공과 시각 품질 완료를 구분할 것

## 2. 제작 계약

이 요구를 지속 가능한 Issue 계약으로 정리했습니다.

~~~text
Goal (목표)
  데이터 입력으로 재생성 가능한 실제 수계

Scope (범위)
  현재 World의 승인된 한강 / 연결 수계 / 호수

Protect (보존 대상)
  기존 기본 지형
  기존 도로
  사람의 직접 수정 결과
  기존 정상 수계 결과

Acceptance (완료 기준)
  지형 준비
  Native Water 생성
  수심과 호안의 관계
  흐름 데이터
  저장 / 재로드
  재생성 시 기존 결과 보호
  시각 검수
~~~

기획자의 자연어 요구가 Tool 호출 목록으로 바로 변환되는 것이 아니라 **제품 Goal과 검수 기준**으로 먼저 정리되는 것이 핵심입니다.

## 3. AI Agent의 제작 과정

실행에서는 하나의 거대한 World Skill 대신 필요한 전문 Skill을 조합했습니다.

~~~text
현재 원본 확인
→ water-terrain-preparation
→ water-production
→ world-validation
→ Evidence
~~~

실제 Unreal mutation은 Official Unreal MCP의 현재 live capability를 확인한 뒤 수행합니다.

## 4. 결과 Snapshot

2026-10-07 validation checkpoint 기준으로 다음 실제 결과를 확인했습니다.

- 강 구간 11개
- 강 제어점 / 표본점 2,470개
- 호수 17개
- 호수 지점 545개
- Landscape Layer 8개
- Landscape Component 128개
- 검증 중 추적한 NeoSeoul Scene 파일 173개는 바이트 단위 동일 상태로 보존

River 결과에서는 위치, 폭, 깊이, velocity와 tangent 계열 데이터를 readback하여 기존 Production input과 비교했습니다.

Lake 결과에서는 datum / depth / boundary point가 저장된 Production snapshot과 일치하는지 검증했습니다.

## 5. 완료로 판단하지 않은 항목

기술 검증이 통과했다고 전체 Goal을 완료 처리하지 않았습니다.

당시 아직 남아 있던 항목:

- 수면의 시각적 품질
- 하천·호수 가장자리와 지형의 자연스러운 접합
- 최종 시각 품질
- 전체 Runtime / 성능 검증
- 사람의 직접 수정 후 AI 재개 검증
- Creator의 최종 수용

즉 다음 둘을 분리했습니다.

~~~text
기술 검증 PASS
≠
Creator의 최종 수용
~~~

이 구분은 포트폴리오에서 중요한 부분입니다. Agent가 만든 결과를 스스로 “완성”이라고 선언하는 것이 아니라, 사람이 판단해야 할 경험·미적 품질은 명시적으로 남깁니다.

## 6. Creator 검수 및 수정

기획자는 결과를 보고 다음 중 하나를 선택할 수 있습니다.

~~~text
현재 결과 수용
또는
자연어로 수정 요청
또는
Unreal Editor에서 직접 수정
~~~

예를 들어:

~~~text
강바닥 깊이는 괜찮지만
호안이 너무 딱딱하게 붙어 보인다.

전체 강을 다시 만들지 말고
현재 강의 형상은 유지하면서
호안 연결 부분만 자연스럽게 다듬어줘.
~~~

이 요청은 기존 validated river result를 baseline으로 유지하고 영향 범위만 다시 작업하게 합니다.

## 7. 포트폴리오 산출물

이 Case Study를 공개 포트폴리오에서는 다음 순서로 보여주는 것이 목표입니다.

~~~text
[1] Designer requirement
[2] Issue / Acceptance excerpt
[3] Agent execution trace
[4] Unreal before / after capture
[5] Technical validation summary
[6] Designer review / correction
[7] Revised result
~~~

텍스트만으로 Agent 구조를 설명하는 대신, **기획자의 입력과 실제 Unreal 결과를 한 세트로 연결**합니다.
