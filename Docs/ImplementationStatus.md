# Implementation Status

> **문서 실사 기준:** ProjectSF `main` `08715c68b04f` (2026-10-22). 아래는 원본 계약과 과거 검증 기록의 정리입니다. 이번 갱신에서 코드·Unreal Editor·새 테스트를 실행하지 않았습니다. [읽은 범위와 미확인 사항](SourceAudit-2026-10-22.md)을 함께 확인하세요.

이 문서는 실제 ProjectSF 운영 구조와 공개 포트폴리오 설명을 맞추기 위한 상태표입니다.

## Documented operating model (not newly runtime-verified)

### Workflow

- repository `AGENTS.md` bootstrap gate
- Current Source of Truth Gate
- Issue start / resume 및 Issue / Recipe partition gate
- Team Hub Spec / Revision / Stage / Gate contract (개별 계정 플러그인 가용성 미검증)
- 영속 일감 없이 직접 작업하는 경로
- TaskExecution
- ProductionConvergence
- ProductionEvolution
- ExecutionObservability
- ContextBudgetPolicy

### Rules and Skills

- common resource production rule
- programming rule
- verification depth rule
- repository-discoverable Skill packages
- thin `.agents/skills` entrypoints
- UI production / architecture
- terrain / landscape
- road production
- water terrain preparation / water production
- world structure / validation
- server feature production / bot verification
- issue context sync
- discussion convergence / Spec page tools

### Runtime

`Tools/AgentPipeline` currently separates:

- Config
- Registry
- Schemas
- Scripts
- Templates
- Examples
- Tests
- Runtime contract

### Production state

- `Production/Data`
- `Production/Evidence`
- Issue 경로: GitHub Issue의 영속 업무 상태
- Spec 경로: Team Hub의 Revision / Stage / Gate / 확정 Plan·설계·UML
- human / AI shared production state
- input revision and validation evidence

## Engineering systems built with the workflow

### RPCNet

- reusable Unreal network Runtime
- TCP / WebSocket / Mock / Memory transport
- Session / Codec / Proxy / Stub boundary
- C# ProtocolGenerator
- generated / handwritten ownership separation
- ProjectSF Consumer Integration boundary
- framework boundary validator
- checkpoint: 37 tests + actual Unreal interoperability PASS

### Online Server

- Gateway / AS / GS topology
- GS Directory / Registry
- Player / World / PlayerScope persistent authority
- Repository / persistence boundary
- weighted Gateway connection balancing
- TLS ingress
- Launcher / Bot verification
- current-phase clean build 0 warnings/errors + 52 tests PASS
- actual UE Entry → Login → GS → PlayerScope → NeoSeoul integration

### DataForge

- standalone Windows GUI + CLI shared Core
- XLSX / XLSM DataTable Authoring SoT
- incremental dataset-level dirty detection
- SFDataBuildCommandlet Bake / Readback
- generated Server JSON parity
- DA / PrimaryDataAsset / INI native read-only extraction
- rollback / last-successful publication gate
- first vertical slice parity PASS
- 25 Core regression tests PASS

### Unreal production

Historical prototype records and production documents cover:

- terrain and landscape
- road batch production
- water / river / lake production
- world structure and validation
- UI production
- server and client-online integration
- data production

## Active R&D

- cross-account Spec 연결·쓰기를 포함한 실제 배포/권한 검증
- Context handoff와 비용·토큰 효율의 동일 조건 측정 (실측 없는 절감률 미주장)
- 새 선형 탐험형 어드벤처 월드의 압축/절차 제작 검증; 과거 GIS 증거와 별개
- production performance measurement for road / water / world output
- world mutation gate: 2026-10-13 설계 문서에는 **runtime integration UNIMPLEMENTED**로 명시
- more reusable batch execution around verified Recipes
- stronger human ↔ AI round-trip editing evidence
- DataForge advanced data types and production consumer rollout
- production-grade Server HA / RDB / WAN deployment
- continued reduction of duplicated rules and stale workflow paths

## Historical verification boundary

RPCNet의 37, Server의 52, DataForge의 25 테스트 등 수치는 이전 시점의 checkpoint 기록입니다. **2026-10-22에 재실행하지 않았으며** 최신 전체 제품 PASS나 Creator 승인의 근거로 승격하지 않습니다.

## Not claimed

이 저장소는 다음을 완료된 범용 제품으로 주장하지 않습니다.

- fully autonomous game development
- universal Unreal capability coverage
- zero-review visual production
- arbitrary project portability without project-specific Rules / Skills
- production-grade multi-region Server deployment
- DataForge support for every Unreal data shape
- tool success as proof of product quality
