# SF Team Hub — Issue 제작에서 Spec 기반 제작으로 확장한 사례

ProjectSF에서 기존 **GitHub Issue 기반 경량 제작을 유지**하면서, 다자간 기획·설계 합의와 구현·검증·발행이 필요한 작업에 **SF Team Hub Spec 경로**를 도입한 사례입니다.

이 문서는 공개된 [ProjectSF #74](https://github.com/islandnboy/ProjectSF/issues/74), [Team Hub v133 변경 기록 #92](https://github.com/islandnboy/ProjectSF/issues/92), ProjectSF `main` `08715c68b04f`의 Workflow·Skill·API 문서를 기준으로 작성했습니다. 이번 포트폴리오 갱신에서 Team Hub 사이트를 새로 조작하거나 회귀 테스트를 재실행하지 않았습니다.

## 변경 배경

기존 Issue는 개별 Goal·Scope·Acceptance와 코드 변경·검증을 추적하기에 적합했습니다. 반면 복합 Spec에서는 기획 초안, 기술 논점, 설계 합의, 구현 대상, AI·사람 검증과 발행 이력을 Issue 본문·댓글 하나에 계속 누적하는 방식만으로는 **현재 확정된 버전과 다음 단계**를 구분하기 어려웠습니다.

모든 Issue를 옮기거나 Team Hub를 유일한 작업 경로로 만드는 대신 다음처럼 책임을 나눴습니다.

| 실행 경로 | 사용 범위 | 상태 원본 |
| --- | --- | --- |
| GitHub Issue | 소규모 시스템 정비, 기능 추가, 버그 수정 | Goal·Scope·Acceptance·현재 결정·진척 |
| Team Hub Spec | 다자간 플랜, 설계·논점 합의, 구현 목록, 검증·발행 | Spec·Revision·Stage·Gate·문서/판정 이력 |
| 직접 요청 | 영속 일감이 필요 없는 작은 조회·수정 | 현재 승인 요청과 실제 대상 |

**Issue 자체를 Team Hub로 이관하지 않습니다.** 이미 존재하는 Issue는 원본 요구와 연결되고, 상세 실행·논점·버전·수용 상태는 Spec이 관리합니다. CommonAIWorkflow는 경로별 상태를 중복 정의하지 않고 공통 실행·안전·검증만 관리합니다.

## 설계한 실행 흐름

```text
Creator 목표 / 기존 Issue 또는 새 Recipe
              ↓
       Team Hub Spec 등록
              ↓
    플랜 작성·보완 / 제작자 확정
              ↓
   설계 정합성 검토 / 논점 합의
              ↓
    설계 승인 / 구현 항목 생성
              ↓
   AI Agent 구현 / 단계별 Evidence
              ↓
     AI 검증 / 사람 검증
              ↓
     가이드 / 원격 main 확인
              ↓
           CURRENT 발행
```

- **GitHub Issue:** 최초 요구와 완료 조건을 보존합니다.
- **Spec Store:** 각 Revision의 문서·질문·논점·구현 항목·검증·결정·발행 기록을 소유합니다.
- **업무보드:** 저장된 Stage·Gate와 차단 원인, 다음 행동을 보여주며 별도 완료 상태를 만들지 않습니다.
- **Agent:** 현재 Spec/문서 버전을 조회한 뒤 필요한 항목만 읽고, 승인된 작업을 실행·기록한 후 결과를 다시 조회합니다.
- **Creator:** 플랜·설계의 명시적 확정과 실제 제품 결과 수용, 발행 판단을 책임집니다.

출력 성공과 서버에 저장된 결과, 구현 항목 완료, AI 검증, 사람 검증, 원격 `main` 반영, CURRENT 발행은 각각 다른 상태입니다. 수정 중 새 Revision이 생겨도 기존 결정과 발행본을 임의로 삭제하거나 덮어쓰지 않습니다.

## 실제 적용 사례 — SPEC-74 Revision 3

**대상:** [ProjectSF Issue #74 — 업무보드·제작 워크플로](https://github.com/islandnboy/ProjectSF/issues/74)

Issue #74에서 Team Hub 자체를 기존 기능과 대조해 정식 요구·검증 경계를 정리했습니다. 기존 프로젝트 자료·다른 Spec의 Revision/발행본을 보존하면서 플랜·설계·구현·검증·매뉴얼·발행을 연결했습니다.

2026-10-19~21 공개된 [v133 변경 기록 #92](https://github.com/islandnboy/ProjectSF/issues/92)의 당시 결과:

| 관측 항목 | 기록된 결과 |
| --- | --- |
| 제품 배포 | SF Team Hub **v133** 배포 성공 |
| 해당 Spec | **SPEC-74 Revision 3 / CURRENT** 발행 |
| 구현 항목 | **20개 완료** |
| 기술 검증 | **18개 PASS** |
| 제작자 판정 | 완료 수락 기록 |
| 연결된 GitHub Issue | **#74 CLOSED** 재조회 |
| 사용량 기록 | FINAL, 단 **coverage PARTIAL** |

위 수치는 **해당 시점 SPEC-74의 기록**입니다. 모든 Team Hub 기능과 모든 팀원 환경이 검증됐다는 의미는 아닙니다.

### 제작 공정에서 확인한 변경

- 레시피 작성·기존 Issue 연결·Spec 등록과 초기 플랜 작성 경로를 분리했습니다.
- 작업자 계정과 일반 Agent 권한, 관리자 승인·사람 판정 권한을 구분했습니다.
- 플랜과 설계의 문서·요청 버전을 확인하고, 필요한 부분만 읽고 갱신하도록 통신 경계를 정리했습니다.
- 서버 receipt와 readback으로 실제 저장된 결과와 현재 Gate를 확인하고 중복 요청·버전 충돌·중단의 처리를 분리했습니다.
- CURRENT 발행 전에 검증, 필수 가이드, 원격 `main` 포함 확인을 각각 요구하도록 했습니다.

## 현재 한계와 후속 작업

- 팀원별 다른 ChatGPT 개인 계정·모바일 환경에서 전체 쓰기 Workflow가 동일하게 동작하는지는 미검증입니다. 기존 플러그인 설치 방식과 연결 권한 문제가 남아 있으며 [#93](https://github.com/islandnboy/ProjectSF/issues/93)은 별도의 진행 중인 R&D입니다.
- [#87](https://github.com/islandnboy/ProjectSF/issues/87)의 **GPT 주도 항목별 설계·합의 및 UML 선택화**는 제안·검토 중인 별도 과제입니다. 기존 배포 버전에서 구현 완료된 것으로 설명하지 않습니다.
- 변경 기록의 `FINAL / PARTIAL` 토큰 통계를 측정되지 않은 실행까지 포함한 총비용이나 생산성 향상 수치로 사용하지 않습니다.
- 이번 공개 Snapshot에서는 비공개 Spec 본문·계정 권한·토큰·원본 작업 기록을 제공하지 않습니다.

관련 공개 자료: [제작 모델](../Docs/CreatorCentricProduction.md) · [실행 모델](../Docs/ExecutionModel.md) · [구조와 책임](../Docs/Architecture.md).
