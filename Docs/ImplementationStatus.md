# 구현 및 검증 현황

> **2026-11-13:** ProjectSF 원격 main `ff8e11e89f9d`와 Hub 상세 상태 기준. [갱신 범위](SourceAudit-2026-11-13.md) · [10월 26일 공정 실사](SourceAudit-2026-10-26.md).

## 제작 공정

| 구성 | 공개된 운영 계약 |
| --- | --- |
| 진입 | Issue / Team Hub Spec / 직접 요청. 모든 작업에 Issue를 요구하지 않음 |
| 실행 | AGENTS.md → 필요한 Rule / Skill / Recipe → Agent 구현 / 기본 회귀검증 |
| 상태 | Issue는 GitHub, Spec은 Team Hub Revision/Stage/Gate, 직접 요청은 승인 범위 |
| 검증 | Requirement → Implementation → Validation → Evidence. Tool 실행 성공과 제품 완료 구분 |
| 품질 관리 | Creator 검수 후 Programmer가 완료 이슈들을 모아 구조·기능 품질 재검토 |

실제 운영 계약은 [설계·책임](CreatorCentricProduction.md), [아키텍처](Architecture.md), [실행 모델](ExecutionModel.md)을 참고합니다. Creator가 Team Hub 제작 공정을 실제 운영·검증했습니다. 팀원별 플러그인 배포는 채택하지 않았고 공통 서버 MCP로 조회·저장·readback과 두 승인 계정 Android 공정을 검증했습니다. 제작 효율의 정량 개선은 별도 평가 대상입니다.

## 기술 사례와 기록

| 분야 | 확인된 설계·구현 및 과거 검증 | 남은 범위 |
| --- | --- | --- |
| [SF Team Hub](../CaseStudies/TeamHubProduction.md) | **공통 서버 MCP 검증**. 조회·저장·readback·새 대화 복원, 두 계정 Android 확인. SPEC-94 승인 범위 6개 완료·정식 VERIFY. SPEC-74 R3 CURRENT 및 당시 20개 구현·18개 PASS는 별도 이력 | iOS 후속 보류, Spec94 원격 main·CURRENT 발행, 후속 설계 개선(#87) |
| [RPCNet](../CaseStudies/RPCNet.md) | 상용 클라이언트 네트워크 설계·구현 경험을 바탕으로 개인 R&D로 개발. SF 모듈 소스 SDK, Shared Unreal DTO와 생성 코드·Consumer 경계. SPEC-95 Item 연동 `CURRENT`. **당시 테스트 37개 및 실제 Unreal 상호운용성 PASS**는 기존 검증 이력 | 상용 프로젝트의 비공개 코드와 분리. 대규모 실서비스 latency/throughput 개선 수치 없음 |
| [RPCNet 검증 서버](../CaseStudies/ServerProduction.md) | **RPCNet 통신 시험을 목적으로 직접 구축한** Gateway/AS/GS 및 Unreal Client 로컬 연동. Foundation **52개**, Gateway 배포 **58개** 회귀 PASS는 **각각 다른 시점의 검사** | 독립 상용 서버 출시 성과 아님. Unreal DS Gameplay replication, Public TLS/WAN UE Client, 운영 HA/RDB 미완료 |
| [DataForge](../CaseStudies/DataForge.md) | XLSX DataTable·Unreal Bake/Readback·Server JSON Parity. **당시 Core 25개 PASS** | 고급 타입·실제 모든 제작 소비자 전환 미완료 |
| [판교 시작 지역](../CaseStudies/PangyoWorldProduction.md) | 월드·제작 체크포인트 원격 반영. 지상·지하 정적 제작, 실제 Data Layer 저장 관측과 제한된 HLOD 대조 | Spec 구현 중. 전체 플레이·Creator 수용·패키지·성능 별개 |
| [그래플링·인벤토리](../CaseStudies/GameplayWorkflow-20261113.md) | 기본 그래플링, 인벤토리, 입력·락온 리팩터링은 원격 main 반영 및 `CURRENT` | 그래플 자원·HUD 후속 Spec은 구현 중 |
| [초기 Unreal 제작](../CaseStudies/AgentDrivenGameProduction.md) | NeoSeoul GIS·전투·MetaHuman의 이전 검증 기록 | 현재 판교 월드의 완성 근거로 사용하지 않음 |

검증 숫자는 실행 당시 해당 테스트 모음의 결과입니다. 합산하거나 현재 모든 기능이 PASS라고 해석하지 않습니다.

## 미완료 / 미검증

- 완전 무인 게임 개발과 프로젝트 간 즉시 이식성
- Creator 검수 없는 실제 UI·전투·월드 품질 완료
- 판교 지상·지하 전체 플레이 경로, Creator 최종 수용과 목표 환경 성능 검증
- 월드 변경 보호 Gate의 완전한 Runtime 연동 (2026-10-13 문서에 UNIMPLEMENTED)
- DataForge 고급 Adapter·실제 소비자 전체 전환
- iOS 실환경 후속 검증과 Spec94 원격 main 확인·CURRENT 발행
- 새 대화에서 최신 결정 누락을 막는 기록 대조. 단발 무인 요약 정확성 보장 없음
- 동일 조건의 토큰·시간·인력 절감 효과 측정
- Public/WAN Unreal Client 및 운영 수준 다중 지역 서버 배포
