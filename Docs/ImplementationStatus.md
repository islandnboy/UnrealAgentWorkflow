# 구현 및 검증 현황

> 공개 문서 기준: ProjectSF `main` `08715c68b04f` (2026-10-22). 2026-10-23 문서 구조를 정리했으며 코드·에디터·테스트는 새로 실행하지 않았습니다. [원본 실사 기록](SourceAudit-2026-10-22.md)과 각 사례의 적용 범위를 함께 참고하세요.

## 제작 공정

| 구성 | 공개된 운영 계약 |
| --- | --- |
| 진입 | Issue / Team Hub Spec / 직접 요청. 모든 작업에 Issue를 요구하지 않음 |
| 실행 | AGENTS.md → 필요한 Rule / Skill / Recipe → Agent 구현 / 기본 회귀검증 |
| 상태 | Issue는 GitHub, Spec은 Team Hub Revision/Stage/Gate, 직접 요청은 승인 범위 |
| 검증 | Requirement → Implementation → Validation → Evidence. Tool 실행 성공과 제품 완료 구분 |
| 품질 관리 | Creator 검수 후 Programmer가 완료 이슈들을 모아 구조·기능 품질 재검토 |

실제 운영 계약은 [설계·책임](CreatorCentricProduction.md), [아키텍처](Architecture.md), [실행 모델](ExecutionModel.md)을 참고합니다. 일부 계정의 Team Hub 접근성과 Agent 제작 효율의 정량 개선은 별도 미검증입니다.

## 기술 사례와 기록

| 분야 | 확인된 설계·구현 및 과거 검증 | 남은 범위 |
| --- | --- | --- |
| [RPCNet](../CaseStudies/RPCNet.md) | Runtime·Session·Transport·Codec, C# ProtocolGenerator, Consumer Integration. **당시 테스트 37개 및 Unreal 상호운용성 PASS** | 대규모 실서비스 latency/throughput 개선 수치 없음 |
| [Online Server](../CaseStudies/ServerProduction.md) | Gateway/AS/GS, 영속 Player/World, Local UE Login→PlayerScope. Foundation **52개**, Gateway 배포 **58개** 회귀 PASS는 **서로 다른 검증 시점** | Unreal DS Gameplay replication, Public TLS/WAN UE Client, 운영 HA/RDB 미완료 |
| [DataForge](../CaseStudies/DataForge.md) | XLSX DataTable·Unreal Bake/Readback·Server JSON Parity. **당시 Core 25개 PASS** | 고급 타입·실제 모든 제작 소비자 전환 미완료 |
| [Unreal 제작](../CaseStudies/AgentDrivenGameProduction.md) | 이전 GIS 월드·전투·MetaHuman 제작·저장·PIE 등 영역별 검증 기록 | 신규 압축 월드, 최종 Creator·시각·패키징·성능 수용 별개 |

검증 숫자는 실행 당시 해당 테스트 모음의 결과입니다. 합산하거나 현재 모든 기능이 PASS라고 해석하지 않습니다.

## 미완료 / 미검증

- 완전 무인 게임 개발과 프로젝트 간 즉시 이식성
- Creator 검수 없는 실제 UI·전투·월드 품질 완료
- 새 선형 탐험형 월드의 제작·성능 검증 (이전 GIS 근거와 구분)
- 월드 변경 보호 Gate의 완전한 Runtime 연동 (2026-10-13 문서에 UNIMPLEMENTED)
- DataForge 고급 Adapter·실제 소비자 전체 전환
- 계정 간 Team Hub Spec 쓰기와 실제 배포 권한 검증
- 동일 조건의 토큰·시간·인력 절감 효과 측정
- Public/WAN Unreal Client 및 운영 수준 다중 지역 서버 배포
