# Recipe 작성 기준

Recipe는 특정 실행 로그가 아니라 **반복 가능한 제작 입력·결과·검증 계약**입니다. 필요한 경우에만 기존 Recipe를 사용하거나 신규 Recipe로 분리합니다. 한 번의 성공만으로 공통 Recipe를 확정하지 않습니다.

## 공통 입력

| 항목 | 목적 |
| --- | --- |
| Goal / CreatorIntent | 무엇을 만들며 어떤 경험을 원하는가 |
| Scope / Inputs | 승인된 대상·원본과 변경 가능 범위 |
| Preserve / CreatorSurface | 기존 결과와 사람이 계속 편집할 데이터·Editor 위치 |
| ResultContract | 필요한 결과 표현, 허용 Preview, 금지 대체물 |
| Acceptance / Evidence | 관측·검사·사람의 수용 기준 |
| Dependencies / Capabilities | 선행 조건, 실제 도구 기능, 권한 경계 |

```text
Creator 목표 → 필요한 경우 Recipe·Issue/Spec 확정
→ AI Agent가 현재 상태와 공식 기능 조사
→ 승인된 입력으로 제작 → 제품 결과·검증 근거
→ Creator 검수 → 필요 범위만 후속 변경
```

영웅·몬스터·월드·도로·수계·데이터·서버 등 작업 종류별 템플릿은 **작성 양식**이며 기능 구현·전체 검증 완료를 의미하지 않습니다. 자세한 필드 기준은 [공개 Recipe 원본 Snapshot](../Reference/ProjectSF/Recipes/Recipe.template.md)과 [제작 모델](ProductionModel.md)을 참고합니다. 원본 프로젝트의 개별 값·이슈·Evidence는 공개하지 않습니다.
