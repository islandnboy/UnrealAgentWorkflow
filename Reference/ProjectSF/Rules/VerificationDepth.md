# ProjectSF — 검증 깊이 및 요구사항 추적 규칙

이 규칙은 구현의 자율성과 탐색 범위를 제한하기 위한 것이 아니다. 구현 단계에서는 영향 범위를 넓게 조사하고 적절한 설계를 선택할 수 있다. 완료 판정 단계에서는 요구사항을 좁고 깊게 추적한다.

> **Execution은 넓게, Validation은 좁고 깊게.**

## 1. 요구사항 원장을 먼저 만든다

Active Issue / Work Task, 사용자 요청, 계약 또는 원본 입력 문서에서 검증 가능한 문장을 Atomic Requirement로 분해한다. 대표 기능 몇 개를 전체 요구사항으로 축약하지 않는다. 각 Requirement에는 안정적인 ID를 부여하고 최소한 다음을 구분한다.

- 명시 동작과 결과
- 상태 및 상태 전이
- 경계값·빈 값·최소/최대·중복
- Invalid State와 Failure Path
- 권한·수명·동시성·취소·재시도처럼 해당 기능에 실제 관련된 제약
- 다른 Requirement와 상호작용하는 규칙

Issue / Work Spec에 없는 요구사항을 새 필수 조건으로 만들지 않는다. 더 나은 구현을 선택해도 원 Requirement를 암묵적으로 삭제·완화·재해석하지 않는다. 변경이 필요하면 차이와 이유를 명시한다.

## 2. Requirement → Implementation → Validation → Evidence

완료 전 다음 Trace를 유지한다.

| 항목 | 의미 |
| --- | --- |
| Requirement | 무엇을 만족해야 하는가 |
| Implementation | 어떤 코드·데이터·설정이 책임지는가 |
| Validation | 어떤 Test/검사로 판정하는가 |
| Evidence | 실제 실행 결과·로그·관찰·산출물은 무엇인가 |

Trace는 별도 대형 문서를 항상 만들라는 뜻이 아니다. 작업 규모가 작으면 Issue / Handoff / ValidationReport의 짧은 표로 충분하다. 중요한 것은 모든 필수 Requirement가 마지막까지 추적되는 것이다.

## 3. Coverage는 Requirement에서 파생한다

일반적인 `정상 동작 확인`, `예외 확인`, `UI 확인` 문구만으로 Test Plan을 끝내지 않는다. 각 Atomic Requirement에서 필요한 경우 아래 축을 파생한다.

1. 정상 경로
2. 상태 전이 전/중/후
3. 경계값과 등가 클래스
4. Invalid/Unavailable State
5. 실패·타임아웃·취소·복구 경로
6. 관련 Requirement 간 조합
7. 변경으로 영향받는 기존 동작의 회귀

모든 축을 기계적으로 곱집합하지 않는다. 실제 규칙·위험·변경 영향이 있는 조합을 우선하고, 제외한 고위험 조합은 이유를 남긴다. 이 방식으로 Medium 모델에서도 깊이를 확보하되 불필요한 Test Matrix와 Context 폭증을 막는다.

## 4. Implementer와 Reviewer 관점을 분리한다

구현 직후의 자기검증은 구현 의도를 증명하는 방향으로 편향될 수 있다. Validation 단계에서는 구현 방법을 정당화하는 질문 대신 다음 순서로 다시 본다.

1. 원 Requirement만 기준으로 기대 결과를 재구성한다.
2. 구현이 틀렸다고 가정했을 때 가장 먼저 깨질 상태·경계·조합을 찾는다.
3. 구현 코드에서 파생한 Test만 사용하지 말고 Specification에서 독립적으로 파생한 Test와 대조한다.
4. 구현자가 추가한 가정이 Requirement처럼 취급되고 있지 않은지 확인한다.
5. 실패를 발견하면 Validation에서 제품 코드를 즉시 고쳐 통과시키지 말고 Revision 대상으로 돌린다.

별도 Agent를 항상 생성할 필요는 없다. 논리적 역할 전환을 기본으로 하고, 고위험·대규모 변경에서만 독립 Reviewer/Agent를 사용한다.

## 5. 완료 상태와 Evidence

Requirement와 Validation 항목은 필요할 때 다음 상태를 사용한다.

- `PASS`: 기대 결과를 실제 Evidence로 확인했다.
- `FAIL`: 실행 또는 검사 결과가 기대와 다르다.
- `BLOCKED`: 환경·의존성·도구 문제로 현재 검증할 수 없다.
- `NOT_APPLICABLE`: 해당 변경에는 적용되지 않으며 이유가 있다.
- `NOT_TESTED`: 검증하지 않았다. 성공으로 간주하지 않는다.

`코드 작성 완료`, `Build PASS`, `Test PASS`, `Requirement Validation PASS`는 서로 다른 상태다. Build 성공만으로 기능 완료를 선언하지 않는다. 실행하지 않은 검증을 추정해서 PASS로 바꾸지 않는다.

## 6. Definition of Done

프로그래밍 작업은 최소한 다음 Gate를 구분한다.

1. **Implementation** — 요구사항에 대응하는 변경이 존재한다.
2. **Build/Static Gate** — 변경 범위에 필요한 빌드·정적 검사가 통과했다.
3. **Programmer Test** — 변경한 규칙의 정상·경계·실패·상태 경로를 필요한 깊이로 확인했다.
4. **Requirement Validation** — Atomic Requirement가 Trace상 모두 `PASS`, `NOT_APPLICABLE` 또는 명시적인 `BLOCKED/NOT_TESTED` 상태다.
5. **Evidence/Handoff** — 실행한 것과 실행하지 못한 것을 구분해 남겼다.

필수 Requirement에 `FAIL`이 있으면 DONE이 아니다. `BLOCKED` 또는 `NOT_TESTED`가 남아 있으면 제한 없는 DONE으로 보고하지 않고 미검증 범위를 명시한다.

## 7. Context/Token 예산

### 변경 영향에 따른 실행 순서

검증 전에 기존 Trace에 `변경 경계 / 영향받은 Requirement·검사 / 재사용 근거와 무효화 조건`을 짧게 연결한다. 문서·화면 문구, 데이터·참조, 실행 흐름, 공통 기반·권한·수명·네트워크처럼 실제 변경 책임을 기준으로 선택하며 파일 수나 작업명만으로 검증 깊이를 정하지 않는다.

1. 관련 diff·입력·계약 확인과 필요한 정적 검사부터 시작한다. 문서만 바뀌면 링크·용어·경계·예제 명령을 확인하고 제품 재빌드를 자동 실행하지 않는다.
2. 변경 규칙의 정상·경계·실패 검사와 연결된 소비자의 회귀를 실행한다. 공통 기반·데이터 형식·권한·취소·수명·예측 경계가 바뀌면 관련 계열·저장/재로드·실제 원격 실행 등 필요한 범위로 확장한다.
3. 실패, 설명되지 않는 결과, 추가 의존성 또는 입력/환경 변경이 발견되면 영향 추적과 검증을 넓힌다. 실패한 검사를 제외하거나 성공한 대표 사례로 전체를 대신하지 않는다.
4. 같은 코드·테스트·입력·설정·의존성·필요한 live 상태에서 이미 확인한 결과는 범위를 명시해 재사용한다. 이를 확인할 수 없거나 이후 변경이 닿으면 해당 검사를 다시 실행한다. 최종 계약이 요구하는 통합·네트워크·Owner 검증은 유지한다.

실행하지 않은 검사는 `재사용(근거·현재성 확인)`, `NOT_APPLICABLE(이유)`, `NOT_TESTED/BLOCKED`를 구분한다. 비용 절감만을 이유로 필수 검사를 비해당으로 바꾸지 않는다. 해당 조건과 근거를 남겼으면 다음 단계에서 전체 로그를 다시 읽는 대신 영향받은 검사와 실패 주변 문맥만 연다.

검증 깊이를 높인다는 이유로 저장소 전체를 다시 읽지 않는다.

- 구현 단계에서 확보한 Requirement와 영향 범위 Trace를 Validation에 재사용한다.
- Validation은 Requirement별 필요한 코드·테스트·Evidence만 다시 연다.
- Test Matrix는 위험 기반으로 확장한다. 무관한 상태의 전체 조합을 생성하지 않는다.
- 동일 로그 전체를 반복 로드하지 말고 실패 위치와 필요한 주변 문맥을 우선한다.
- 추가 탐색은 `AI/Workflows/ContextBudgetPolicy.md`의 Evidence 기반 Expansion을 따른다.
