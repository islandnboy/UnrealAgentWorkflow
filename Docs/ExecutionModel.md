# 실행 모델

**Entry:** Issue (경량 제작), SF Team Hub Spec (플랜·설계·합의), 직접 작업 (영속 일감 불필요). 모든 작업에 Issue/Recipe 생성을 강제하지 않습니다.

## 1. 실행 전 필수 확인

실제 조사·진단·변경·제작 작업은 mutation 전에 실행 문맥을 먼저 확정합니다.

최소 문맥:

- Workflow
- Step
- Skill
- Recipe, if applicable
- Goal
- Acceptance
- Planned Evidence

필수 지침을 읽기 전에 구현을 먼저 시작하고 나중에 정당화하는 방식은 허용하지 않습니다.

## 2. 현재 원본 상태 확인

모든 경로의 기준은 이전 세션이 아니라 **현재 시점의 실제 상태**입니다.

~~~text
저장소 + Workflow
+ (GitHub Issue | Team Hub Spec/Revision/Stage/Gate | 직접 요청)
= 현재 작업의 판단 원본
~~~

확인 범위는 작업에 필요한 만큼만 확장합니다.

### 저장소

- branch / HEAD
- dirty diff
- 대상 구현과 입력
- 관련 Evidence
- 필요한 경우 실제 Unreal Editor 저장 상태

### 이슈와 작업 원본

- Issue: Goal / Scope / Acceptance / 최신 Creator 결정 / state / 관련 Evidence
- Spec: ID / Revision / Stage / Gate / 확정 Plan·설계·UML 버전
- 직접 작업: 현재 요청의 대상 / 승인 범위 / 완료 조건

### Workflow

- 현재 checkout의 `AGENTS.md`
- 적용 Workflow / Rule / Skill / Recipe

이전 대화와 handoff는 위치를 찾는 참고 자료일 뿐, 현재 계약을 대체하지 않습니다.

## 3. 필요한 범위를 먼저 확인하고 확장

Repository 전체를 먼저 읽지 않습니다.

1. 현재 Step의 질문을 정의합니다.
2. 해당 질문에 필요한 문서와 코드를 읽습니다.
3. 근거가 확보되면 탐색을 멈춥니다.
4. 충돌이나 근거 부족이 있을 때만 확장합니다.
5. 유효한 결과는 revision / identity와 함께 재사용합니다.

이 방식은 단순 token 절약이 아니라 **판단에 불필요한 context가 오히려 품질을 떨어뜨리는 문제**를 줄이기 위한 정책입니다.

## 4. Issue / Recipe 분리 기준

Issue/Recipe를 분리하기 전에 기존 계약을 확인합니다. Spec이나 직접 요청을 자동으로 Issue로 승격하지 않습니다.

- 기존 Recipe
- Parent / Child Issue
- 같은 영역의 기존 Issue
- Architecture / Production 문서의 책임 경계

기본값은 하나의 Goal과 Acceptance를 공유하면 같은 Issue의 Step으로 유지하는 것입니다.

새 Issue나 Recipe는 “작업량이 많다”는 이유가 아니라 **독립적인 Goal, Acceptance, lifecycle 또는 재사용 계약이 존재할 때**만 분리합니다.

## 5. 단계 실행

먼저 공식 엔진·플랫폼 기능과 기존 구현을 조사합니다. 새로운 추상화는 문제와 이점이 검증될 때만 도입합니다(**Standard First, Simple is Best**).

~~~text
현재 작업 상태 복원
→ 다음 단계 선택
→ 필요한 Skill / Recipe 읽기
→ 필요한 기능 정의
→ 실제 도구 조회
→ 승인된 범위에서 실행
→ 실제 결과 검증
→ 근거 기록
→ 다음 행동 판단
~~~

Tool 호출 성공과 Step PASS는 별도입니다.

## 6. 실행 상태 관측

진행 화면에서는 다음 연결이 보여야 합니다.

~~~text
Issue / 요청
→ 작업 유형
→ Workflow / Step
→ Skill
→ 필요한 기능
→ 실제 Toolset / Tool
→ 산출물
→ 검증 결과
→ 다음 행동
~~~

발견한 Tool 후보와 실제 호출한 Tool을 구분하고, 미확인 상태를 성공처럼 표현하지 않습니다.

## 7. 작업 결과

대표 상태:

- continue
- wait for user decision
- blocked
- complete
- not tested
- capability gap

Build 성공, Compile 성공, MCP 호출 성공만으로 `complete`가 되지 않습니다.

완료는 **Acceptance에 필요한 실제 제품 결과와 검증 Evidence가 충족됐을 때** 판정합니다.
