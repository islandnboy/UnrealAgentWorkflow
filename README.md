# AI Agent 기반 Unreal Engine 개발·제작 워크플로

**UnrealAgentWorkflow**는 AI Agent와 사람이 같은 제작 계약을 공유하며 Unreal Engine 개발·제작 결과를 반복 개선하는 워크플로 R&D입니다.

개인 프로젝트 **ProjectSF (Unreal Engine 5.8 계열)**의 운영 구조와 제작 사례를 정리합니다. 비공개 **ProjectSF가 Source of Truth**이며 이 공개 저장소는 설명과 선별 원본의 Snapshot이지 별도 범용 프레임워크가 아닙니다. 문서 기준: ProjectSF `main` `08715c68b04f` (2026-10-22).

**제작 모델:** 기획·프로그래밍·운영 등 전문성을 결합하는 소규모 팀 R&D이며, 인원 수를 고정 계약으로 주장하지 않습니다.

## 둘러보기

| 카테고리 | 내용 |
| --- | --- |
| [제작 흐름](#제작-흐름) | Agent의 작업 판단·실행·검증과 사람의 검토 흐름 |
| [제작 기반](#제작-기반) | 공통 제작 기반, Unreal 도구 연결, 월드·UI 제작 사례 |
| [네트워크](#네트워크) | RPCNet과 프로토콜 생성·Unreal 클라이언트 연동 |
| [서버](#서버) | 온라인 서버 구성과 접속·인증·영속화 |
| [데이터](#데이터) | DataForge와 게임 데이터 변환·검증 |

## 제작 철학

**Creator는 설계와 결과를 책임지고, AI Agent는 구현과 기본 회귀 검증을 수행합니다. Programmer는 완료된 이슈들을 종합해 구조적·기능적 품질을 검토하고 제작 공정을 개선합니다.**

Creator는 특정 직군이 아니라 설계와 실제 결과를 책임지는 제작자입니다. 개별 이슈의 구현·기본 검증은 Agent가 수행하고, Programmer는 완료된 이슈를 묶어 구조적·기능적 품질과 반복 문제를 점검합니다. 검토 결과는 리팩터링과 Workflow·Skill·Guideline 개선으로 환원합니다.

- [Creator-Centric Production Engineering](Docs/CreatorCentricProduction.md) — 역할과 책임, 완료 이슈 기반 품질 관리, 장기 확장 방향

## 제작 흐름

요청을 실행 가능한 작업으로 나누고, 최신 상태와 검증 근거를 바탕으로 다음 행동을 결정합니다.

```text
Creator 요청 → Issue (경량 제작) / Spec (플랜·설계) / 직접 작업 선택
→ 선택한 경로의 최신 원본 및 저장소 확인 → 공식 기능·기존 구조 우선
→ 필요한 Rule·Skill·Recipe → AI Agent 구현·기본 회귀 검증
→ 제품 결과·Evidence → Creator 검수
→ 완료 이슈를 모아 Programmer가 구조·기능 품질 검토
→ 리팩터링 / Workflow·Skill·Guideline 개선
```

**Standard First, Simple is Best.** 공식 엔진·Codex 기능과 기존 구현을 우선 확인하고 불필요한 Manager/Router/Framework를 추가하지 않습니다. **AI Agent는 의미와 다음 행동을 판단하고, Runtime은 결정론적 실행·검증을 지원합니다.** 도구 실행 성공만으로 완료를 판정하지 않고, 실제 제품 결과와 목표의 Acceptance를 확인합니다.

| 문서 | 다루는 내용 |
| --- | --- |
| [실행 모델](Docs/ExecutionModel.md) | Issue·Spec·직접 작업, 최신 상태와 완료 판정 |
| [TaskExecution 원본 Snapshot](Reference/ProjectSF/TaskExecution.md) | 실제 원본의 일반 작업 계약 |
| [Creator 제작 흐름](Docs/CreatorWorkflow.md) | 제작자의 요청, 직접 제작·검토·수정, 다음 작업 연결 |
| [Designer Production](CaseStudies/DesignerProduction.md) | 기획 의도를 제작 계약과 결과 검증으로 연결한 사례 |
| [Agent-Driven Game Production 사례](CaseStudies/AgentDrivenGameProduction.md) | 월드·전투·MetaHuman 제작 과정, 실제 화면과 데이터 연결 구조 |
| [실행 진행·관측](Reference/ProjectSF/ExecutionObservability.md) | 실행 상태와 검증 근거를 기록하는 방식 |
| [문맥 관리 정책](Reference/ProjectSF/ContextBudgetPolicy.md) | 필요한 문맥을 선택하고 확장하는 기준 |
| [제작 산출물 예시](Samples/DesignerArtifactSet.md) | 제작자와 Agent가 공유하는 제작 산출물 예시 |

## 제작 기반

공통 제작 계약과 실행 도구를 연결하는 기반입니다. 현재 공개 문서는 AgentPipeline·Unreal MCP 연결과 이를 적용한 콘텐츠 제작 사례를 중심으로 구성합니다.

| 문서 / 사례 | 다루는 내용 |
| --- | --- |
| [구조와 책임 경계](Docs/Architecture.md) | Workflow·Rule·Skill·Recipe·Runtime·Evidence의 책임과 경계 |
| [제작 모델](Docs/ProductionModel.md) | 공통 제작 계약, 반복 개선과 재사용 도구 승격 |
| [제작 Recipe 양식](CaseStudies/RecipeProduction.md) | 영웅·몬스터·월드·도로·수계·데이터·서버의 제작 계약 양식과 사용 사례 |
| [Unreal MCP 실행](Docs/MCPExecution.md) | Unreal Editor 실행과 도구 연결 방식 |
| [원본 저장소 구조](Reference/ProjectSF/Structure.md) | 작업 저장소의 디렉터리 구조와 역할 |
| [월드 제작](CaseStudies/WorldProduction.md) | 지형·도로·수계 등 Unreal 월드 제작 과정 |
| [UI 제작](CaseStudies/UIProduction.md) | Unreal UI 제작과 검증 과정 |

참고: [ProjectSF 공개 자료 범위](Reference/ProjectSF/README.md) · [실사 범위와 미확인 항목](Docs/SourceAudit-2026-10-22.md) · [Samples](Samples/README.md)

## 네트워크

**RPCNet** — Runtime·Session·Transport·Codec을 분리하고, 프로토콜 생성기와 Unreal 런타임 플러그인을 연결하는 네트워크 시스템입니다.

- [RPCNet Case Study](CaseStudies/RPCNet.md) — 구조, Consumer Integration과 실제 Unreal 연동 검증
- [Protocol → Unreal Client Guide (PDF)](Docs/Guides/RPCNet_Protocol_to_Unreal_Client_Guide.pdf) — 프로토콜에서 Unreal 클라이언트까지의 연결 가이드

## 서버

**Online Server** — Gateway·Account Server·Game Server를 연결해 접속·인증·게임 진입·영속화 흐름을 구성합니다.

- [Server Production Case Study](CaseStudies/ServerProduction.md) — 서버 구성, Player·World 상태 저장과 클라이언트 연동 검증
- [Server Architecture / Topology](Docs/Server/ServerArchitecture.md) — 서비스 구성, Authority와 데이터·프로토콜 경계
- [Gateway Deployment Design](Docs/Server/GatewayDeploymentDesign.md) — AS 풀·LB·Coordination·로컬 런처·배포 설계

## 데이터

**DataForge** — Excel 입력에서 Unreal DataTable·서버 JSON까지 게임 데이터를 변환하고 결과의 일치 여부를 검증하는 파이프라인입니다.

- [DataForge Case Study](CaseStudies/DataForge.md) — GUI·CLI 공통 처리, 증분 변환과 Bake·Readback·Parity 검증
- [DataForge 사용 가이드 (PDF)](Docs/Guides/DataForgeUserGuide.pdf) — 화면 안내, DT 빌드, DA·Config 조회와 결과·오류 확인

## 현재 상태

**공개 포트폴리오 / 연구·개발 진행 중.** 적용·검증된 범위와 연구 중인 항목은 [구현·검증 현황](Docs/ImplementationStatus.md)에서 구분합니다.

기존 GIS·NeoSeoul 월드 제작은 **이전 프로토타입의 검증 사례**이며, 2026-10-14 이후의 압축된 선형 탐험형 신규 월드가 검증됐다는 뜻이 아닙니다.

사례에 기록된 테스트·연동 결과는 해당 검증 시점의 근거입니다. 기술 검증, 화면 결과 검토, 최종 제작 승인도 구분해 기록합니다. 비공개 게임 코드·원본 데이터·Issue·Production Evidence 전체는 공개하지 않습니다.
