# UI 제작 사례

## 제작 목표

자연어 UI 요청을 기존 Unreal UI 구조와 연결해 **실제 사용 가능한 Widget / binding / interaction**으로 제작합니다.

## 작업 흐름

~~~text
요청 / Issue / Spec
→ 초기 지침 및 현재 원본 확인
→ 기존 UI Asset과 구조 조사
→ ui-production / ui-architecture Skill 선택
→ 필요한 기능 정의
→ 실제 MCP 기능 연결 확인
→ 생성 / 수정
→ 컴파일 / 저장 / 재조회
→ 상호작용·제작 의도 검증
→ Evidence
~~~

## 제작 원칙

- 새 Widget을 만들기 전에 기존 구조를 조사합니다.
- 표시 책임과 상태 / 명령 책임을 구분합니다.
- reusable component와 ViewModel source를 먼저 검토합니다.
- placeholder를 승인된 결과로 승격하지 않습니다.
- 실제 Tool 이름은 Skill에 고정하지 않고 live discovery로 해결합니다.

## 검증 기준

- Asset / Object 식별
- 컴파일
- 저장 / 재조회
- 입력 / Focus / Activation
- 데이터 Binding 결과
- 시각적 검수
- 검증하지 못한 항목은 `NOT_TESTED`로 표시

이 사례의 핵심은 “UI를 Agent가 만들었다”가 아니라 **프로젝트의 기존 UI 규칙과 실제 Editor 결과를 같은 계약 안에서 연결했다는 것**입니다.
