---
name: ui-production
description: Produce and validate Unreal UI assets and connected UI behavior from an approved UI goal.
---

# UI Production

UI 화면·Widget·binding·interaction을 실제 사용 가능한 결과로 제작하는 Skill이다.

## 판단

- 기존 UI asset과 공용 component를 먼저 조사한다.
- 화면의 표시/입력 책임과 상태/명령 책임을 구분한다.
- 새 Widget, ViewModel, binding 또는 asset을 만들기 전에 재사용 가능한 기존 구조를 확인한다.
- placeholder는 승인된 preview가 아니면 완료 결과로 승격하지 않는다.
- UI mutation은 필요한 logical capability를 먼저 정의하고 현재 binding을 live discovery로 확인한다.

## Widget 생성 위치

- ProjectSF 게임 Widget Blueprint는 반드시 `Client/Content/UI/[Domain]/WBP_[Content]`에 생성한다. Unreal asset 경로는 `/Game/UI/[Domain]/WBP_[Content]`다.
- 생성 전에 기존 `UI` 하위 도메인 폴더를 확인해 재사용하고, 필요한 도메인 폴더가 없으면 `UI` 하위에 만든다. 캐릭터·입력·맵 등 다른 Content 폴더에 Widget Blueprint를 생성하지 않는다.
- 화면뿐 아니라 HUD, list entry, 공용 component, diegetic Widget Blueprint에도 같은 위치·이름 규칙을 적용한다. C++ 소스와 엔진/플러그인 소유 에셋은 이 이동·생성 규칙의 대상이 아니다.
- MCP 생성 호출 전에 목적지 경로를 확인하고, 생성 후 실제 asset identity가 `/Game/UI/` 하위와 `WBP_` 이름 규칙을 충족하는지 readback으로 검증한다.

## Logical Capability

작업에 따라 다음과 같은 능력을 요구할 수 있다.

- Widget/UI asset inspect
- Widget create/edit
- ViewModel source/binding configure
- compile/save/readback
- runtime/PIE validation

실제 Tool 이름과 ProjectSF class는 이 Skill에 고정하지 않는다. Widget 생성 경로는 위 규칙을 따른다.

## Validation

- 생성·수정 Asset/Object identity
- 생성한 Widget Blueprint의 `/Game/UI/[Domain]/WBP_[Content]` 경로
- compile/save/readback
- 입력·focus·activation 등 요청한 interaction
- ViewModel/model과 표시 결과의 연결
- 필요한 visual/human review
- 미검증 항목은 `NOT_TESTED`

ProjectSF Unreal 구현 규칙은 `Client/.AI/UI/` 문서를 따른다.
