# Unreal UI 제작 Workflow

UI 제작의 일반 실행·검증 계약입니다. **완료된 특정 화면의 Case Study가 아닙니다.** 실제 제품 사례와 수용 여부는 별도 Evidence를 요구합니다.

## 설계 기준

- 기존 Widget·UI 계층·공용 컴포넌트를 먼저 조사합니다.
- View의 표시·입력과 Model/ViewModel의 상태·명령 책임을 구분합니다.
- 기존 CommonUI/MVVM과 ProjectSF UI 규칙을 우선 적용합니다.
- Tool 이름을 Skill에 고정하지 않고 현재 지원되는 Unreal MCP 기능을 조회합니다.
- 임시 Placeholder를 승인된 최종 화면으로 처리하지 않습니다.

## 제작 흐름

```text
Creator UI 목표 / Issue · Spec · 직접 요청
→ 기존 UI Asset·바인딩·상태 원본 조회
→ ui-production / ui-architecture Skill 선택
→ 필요한 MCP Capability 확인
→ Widget·Binding·Interaction 제작 및 수정
→ Compile / Save / Readback
→ 입력·Focus·Activation·데이터 갱신 검사
→ 실제 화면·동작 검수 및 Evidence
```

검증 항목은 Asset identity, 실제 저장 경로, 컴파일, 재조회, ViewModel 바인딩, 입력·Focus·Activation, 필요 시 PIE/Runtime 및 Creator 시각 검수입니다. 검사하지 않은 부분은 `NOT_TESTED`로 남깁니다.

공개된 [원본 UI 제작 Skill](../Reference/ProjectSF/Skills/ui-production/SKILL.md)은 현재 ProjectSF에서 선별한 작성 규칙의 Snapshot입니다. 특정 Tool 실행이나 화면 완성을 입증하지 않습니다.
