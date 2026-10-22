# Unreal Agent Workflow

Unreal Engine 5.8 기반 개인 프로젝트 **ProjectSF**의 AI Agent 제작 공정과 기술 사례를 정리한 공개 포트폴리오입니다.

비공개 `ProjectSF`가 **Source of Truth**입니다. 이 저장소는 선별한 문서와 검증 기록의 **공개 Snapshot**이며, 별도 범용 프레임워크나 완성된 상용 서비스가 아닙니다.

- 원본 문서 기준: ProjectSF `main` `08715c68b04f` (2026-10-22)
- 공개 저장소 정리: 2026-10-23 (새 빌드·실행 검증 없음)

## 제작 방식

**Creator**는 목표·설계·결과 수용을 책임집니다. **AI Agent**는 현재 원본을 조사하고 구현과 기본 회귀 검증을 수행합니다. **Programmer**는 완료된 작업을 묶어 구조·기능 품질을 점검하고 공통 제작 기반을 개선합니다.

```text
Creator 목표 / 설계 / Acceptance
 → Issue · Team Hub Spec · 직접 작업 중 선택
 → 현재 원본·공식 기능 확인
 → AI Agent 구현 / 기본 검증 / Evidence
 → Creator 실제 결과 검수
 → Programmer의 완료 작업 종합 검토
 → 리팩터링 / Workflow·Skill·Guideline 개선
```

**Standard First, Simple is Best.** 공식 Unreal/Codex 기능과 기존 구조를 먼저 사용하며, 필요성이 확인되지 않은 새 Manager·Router·Framework는 추가하지 않습니다. 도구 실행 성공과 제품 검증, Creator 최종 수용은 별도로 판정합니다.

## 핵심 자료

| 주제 | 문서 | 내용 |
| --- | --- | --- |
| Agent Workflow | [제작 모델과 역할](Docs/CreatorCentricProduction.md) | Creator / AI Agent / Programmer의 책임과 품질 관리 |
| Agent Workflow | [구조 및 책임 경계](Docs/Architecture.md) | Workflow·Rule·Skill·Recipe·Runtime·Evidence |
| Agent Workflow | [실행·재개·검증](Docs/ExecutionModel.md) | Issue / Spec / 직접 요청, 최신 원본, 완료 판정 |
| 제작 사례 | [실제 Unreal 제작](CaseStudies/AgentDrivenGameProduction.md) | 월드·전투·MetaHuman 작업과 단계별 검증 기록 |
| RPCNet | [네트워크 설계 및 검증](CaseStudies/RPCNet.md) | Runtime·ProtocolGenerator·Consumer 책임 분리와 Unreal 연동 |
| Online Server | [서버 구현 사례와 현재 범위](CaseStudies/ServerProduction.md) | Gateway/AS/GS, 영속화, 로그인·접속 흐름, 로컬 검증 |

## 추가 자료

- 제작 기준: [Creator의 작업 흐름](Docs/CreatorWorkflow.md) · [제작 입력과 Evidence](Docs/ProductionModel.md) · [Unreal MCP 실행 경계](Docs/MCPExecution.md)
- 제작 지침: [UI 제작](Docs/UIProduction.md) · [Recipe 작성](Docs/RecipeProduction.md) · [원본 지침 Snapshot](Reference/ProjectSF/README.md)
- 추가 사례: [월드 제작(이전 GIS 프로토타입)](CaseStudies/WorldProduction.md) · [DataForge](CaseStudies/DataForge.md)
- 상세 참고: [서버 아키텍처](Docs/Server/ServerArchitecture.md) · [Gateway 배포 설계](Docs/Server/GatewayDeploymentDesign.md) · [RPCNet 가이드 PDF](Docs/Guides/RPCNet_Protocol_to_Unreal_Client_Guide.pdf)
- 기록: [구현·검증 현황](Docs/ImplementationStatus.md) · [2026-10-22 원본 실사 범위](Docs/SourceAudit-2026-10-22.md)

## 검증과 공개 범위

각 문서의 **PASS는 당시 명시된 검사 범위**에 한정합니다. 코드·Unreal Editor·서버를 이번 저장소 정리에서 재실행하지 않았습니다. 이전 NeoSeoul/GIS 제작 근거를 2026-10-14 이후 새 월드 완성 근거로 사용하지 않으며, 운영 수준 HA·WAN 배포와 Unreal Dedicated Server 연동 역시 완료로 주장하지 않습니다.

상용 코드, 원본 게임 리소스, 비공개 작업 기록·인증정보는 공개하지 않습니다.
