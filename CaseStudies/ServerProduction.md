# RPCNet 통신 검증용 서버 구축 사례

**RPCNet의 실제 Client/Server 통신을 검증하기 위해 구축한 서버 기반**입니다. ProjectSF에서 Gateway / Account Server(AS) / Game Server(GS)를 구현하고 실제 프로세스·Bot·Unreal Client 연동 검증을 수행했습니다. 인증·영속화·배포 등 검증 범위도 단계적으로 확장했지만, 독립적으로 출시한 상용 온라인 서비스가 아니라 **RPCNet 통신 검증에서 시작한 개발 사례**입니다.

목표 아키텍처와 로컬 환경의 실제 구현·검증 결과, 아직 수행하지 않은 운영 검증을 구분합니다.

- 기준: ProjectSF `main` `08715c68b04f` (2026-10-22)
- 근거: 원본 `Docs/Technical/Server/ServerArchitecture.md`, `GatewayDeploymentDesign.md`, `Docs/Technical/Client/ClientLoginIntegration.md`, `PlayerScopeSubsystem.md` 및 관련 Evidence
- 이번 공개 문서 정리에서 소스 빌드·서버·Unreal Editor 테스트는 **재실행하지 않음**

## 검증 목표와 적용 범위

- Mock 통신만으로는 확인할 수 없는 실제 TCP 연결, Session 수명 및 패킷 라우팅을 확인합니다.
- 생성된 Protocol 계약을 서버 Endpoint와 연결하고 실제 Gateway·AS·GS 경로에서 주고받습니다.
- Unreal Client의 로그인, GS 입장, PlayerScope 적용·맵 이동까지 연동을 확인합니다.
- 로컬 배포와 영속 상태 검증은 이 통신 기반을 소비하는 확장 시험으로 구분합니다.

## 구현된 구조

```text
Client / Bot
    ↓
Gateway — 상태 확인 / AS 연결 분산 / TLS 진입
    ↓
Account Server(AS) — Account 인증 / 세션·GS 입장권
    ↓
Game Server Directory / Registry
    ↓
Game Server(GS) — Player·World·PlayerScope의 영속 상태 권한
    ↓
Domain Service → Repository → SQLite (로컬 구현)

미구현/별도 검증: UE Dedicated Server 실시간 Gameplay Authority
```

Gateway는 계정 정보를 소유하지 않고, AS는 로그인과 GS 인계, GS는 지속되는 Player·World 데이터 책임을 맡습니다. 네트워크 직렬화와 전송은 [RPCNet](RPCNet.md)의 계약을 사용하며 서비스 도메인 로직을 RPCNet에 넣지 않습니다.

## 사례 1 — 로그인과 영속 Player 상태

Account 인증 후 GS 입장권을 발급하고, GS에서 권한과 Player 소유권을 확인합니다. AS·GS는 Repository 경계로 저장소 구현을 분리했습니다. 당시 서버 Foundation의 **실제 프로세스 + 외부 Bot 검증**, 빌드 경고/오류 0건 및 **회귀 테스트 52개 PASS**가 기록돼 있습니다. 이 수치는 서버 초기 기반 검증이며 이후 Gateway 확장 전체의 검사 건수가 아닙니다.

인증된 `GetPlayerScope` 콘텐츠 조회 사례에서는 Session의 Player 권한을 사용하고, GS Repository가 반환하는 영속 데이터를 읽습니다. 실제 Bot이 데이터 변경과 서버 재시작 전후의 조회 결과를 확인했습니다. 모든 Inventory·Quest·Party 콘텐츠가 구현됐다는 뜻은 아닙니다.

## 사례 2 — Gateway 연결 분산과 로컬 배포

Gateway는 RPCNet Health 검사에 통과한 AS 후보를 대상으로 연결을 분산하고, 장애·재접속 시 상태 전이를 처리합니다. 다중 AS, AS 간 Account Repository 접근 책임, GS 소유권, TTL/lease·reservation 경계를 정리했습니다. 각 AS가 타 AS의 SQLite 파일을 직접 공유하거나 Migration을 실행하지 않도록 했습니다.

개인 PC 배포용 Launcher/Supervisor, 단계적 시작·종료, 로그·백업, TLS ingress를 로컬 구현했습니다. 별도의 배포 검증 시점에 **회귀 테스트 58개 PASS**, 실제 연결 분산 개별 검증, 패키지 무결성 검사 PASS가 기록돼 있습니다. 서버 Foundation의 52개 테스트와 합산하지 않습니다.

## 사례 3 — 실제 Unreal Client의 게임 진입

```text
Entry 로그인 UI
→ Gateway / AS 인증
→ GS 자동 연결·입장
→ 초기 PlayerScope 검증·적용
→ NeoSeoul OpenLevel
```

GameInstance 소유 LoginFlowCoordinator가 AS/GS ConnectionHandler를 관리합니다. PlayerScopeSubsystem은 검증된 Shared 데이터의 **Client 측 적용 원본**이며, 영속 데이터 최종 Authority는 GS입니다. 계정·Player·World·Session·Revision을 확인한 뒤 상태를 적용합니다. 실패·중단·재로그인·서버 재시작 경로에서도 연결과 상태를 정리합니다.

2026-10-07에는 실제 서버 프로세스와 `UnrealEditor-Cmd -game -NullRHI`를 이용한 UI delegate 호출·로그인·월드 이동 검증이 기록됐습니다. 2026-10-08에는 RPCNet SDK 구조 변경 후 Local `RealUserFlow`를 새 서버 실행 상태에서 **2회 재검증**했고 Player identity·PlayerScope readback이 유지됐습니다. 이것은 사람의 실제 PIE 입력·렌더링 품질 검증과 구분합니다.

## 현재 확인 범위

| 항목 | 공개된 상태 |
| --- | --- |
| Gateway / AS / GS 로컬 서버 프로세스, 계정·GS 진입, 영속 Player 상태 | 구현 및 당시 Bot/통합 검증 기록 |
| Health 기반 AS 연결 분산, 설치용 TLS ingress, Launcher | 로컬 구현 및 당시 개별 검증 기록 |
| UE Client 로컬 TCP 로그인·PlayerScope 적용·맵 이동 | 실행·재조회 검증 기록 |
| Unreal Dedicated Server의 이동·전투·GAS 복제 | 향후 범위 / 미구현 |
| Public TLS/WAN Unreal Client, 패키징 게임 전체 흐름 | `NOT_TESTED` |
| 운영 수준 Gateway HA, 공유 운영 RDB, 다중 지역 서비스 | 미구현 / 운영 검증 없음 |

해당 테스트 수치와 PASS는 **과거 개별 checkpoint의 증거**입니다. 최신 제품 전체의 PASS, 운영 배포 안정성이나 대규모 부하 성능을 주장하지 않습니다.

## 설계 참고

- [공개 서버 아키텍처](../Docs/Server/ServerArchitecture.md) — 목표 Topology 및 Authority 분리
- [Gateway 배포 설계](../Docs/Server/GatewayDeploymentDesign.md) — 로컬 설치·연결 분산과 미검증 범위
- [RPCNet 구현 사례](RPCNet.md) — Runtime / Protocol / Consumer Integration 경계
