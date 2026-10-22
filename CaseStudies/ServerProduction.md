# 온라인 서버 제작 사례

## 온라인 게임 서비스 기반

ProjectSF 서버는 단일 게임 서버 프로세스에 모든 책임을 넣지 않고 **외부 진입, 인증, 영속 게임 상태, 향후 실시간 Gameplay simulation**을 분리합니다.

Agent Workflow는 이 서버 작업에도 동일한 Issue / Step / Skill / Evidence 모델을 적용합니다.

## 참고 아키텍처

- [Server Architecture / Topology](../Docs/Server/ServerArchitecture.md) — 기존 구조 문서에서 선별한 토폴로지·Authority·Persistence·Protocol 경계
- [Gateway Deployment Design](../Docs/Server/GatewayDeploymentDesign.md) — 기존 Gateway·AS 풀·Coordination·설치·런처 설계안의 공개 참고본

목표 아키텍처와 설계안의 당시 검증 범위를 구분합니다. 후속 Client 연동과 검증 checkpoint는 이 사례의 Verification / Actual Unreal Client flow에서 확인합니다.

## 목표 구조

~~~text
Client
  ↓
Gateway
  ├─ 상태 확인 / 장애 전환
  ├─ 연결 분산
  ├─ TLS 외부 진입
  └─ 향후 지역별 경로 선택
  ↓
Account Server Cluster
  ↓
GameServer Directory / Registry
  ↓
Game Server Cluster
  ↓
Repository / Persistence
  ├─ 선택적 Cache
  └─ Primary DB

향후 범위:
Game Server ↔ Unreal Dedicated Server
~~~

## 권한과 책임 경계

### Gateway

- 공개 서비스 진입점
- 비즈니스 메시지 의미를 해석하지 않는 전달
- 상태 검사를 통과한 AS 풀
- 연결 부하 분산
- TLS / 연결 제한
- 향후 지역 / 점검 경로 선택

### 계정 서버(AS)

- 계정 / 인증
- 로그인 세션과 GS 인계 티켓
- GS 선택
- persistent gameplay traffic relay는 하지 않음

### GameServer Directory

- GS 등록
- Heartbeat / 상태 확인
- 수용량 / 버전 메타데이터
- 연결 대상 선택

초기에는 AS 내부 logical component로 유지하고 scale 필요 전 별도 service로 강제 분리하지 않습니다.

### 게임 서버(GS)

- Player
- Character
- Inventory
- Quest / Party
- World 영속 상태
- PlayerScope

### Unreal Dedicated Server — 향후 범위

- 이동
- 전투
- GAS
- 물리
- 실시간 게임플레이 시뮬레이션

Persistent Player / World 데이터의 최종 SoT와 realtime simulation authority를 구분합니다.

## 영속화 책임 경계

~~~text
AS / GS
  ↓
Domain Service
  ↓
Repository
  ↓
선택적 Cache
  ↓
Primary DB
~~~

현재 로컬 구현은 SQLite 기반 데이터 권한을 사용하지만, 비즈니스 로직이 특정 데이터베이스에 직접 의존하지 않도록 Repository 경계를 둡니다.

Peer AS가 authority SQLite 파일을 직접 열지 않고 RPCNet을 통해 접근하도록 하여 향후 RDB 교체 경계를 보존합니다.

## RPCNet 연동

새 Server 통신은 RPCNet protobuf / generated contract를 사용합니다.

~~~text
Transport
→ Connection
→ Session
→ Protocol / Dispatch
→ Generated Stub
→ Final Endpoint
→ Domain Service
→ Repository
~~~

RPCNet은 통신 기반을 제공하고, Server는 Account / Player / World에 대한 게임 도메인 의미를 소유합니다.

## 로컬 배포 구성

개인 개발 환경에서도 실제 서비스 구성을 검증할 수 있도록 다음 기반을 구성했습니다.

- Gateway
- 다중 AS / GS 실행 프로필
- 상태 확인 / 실행 준비 상태
- 가중치 기반 연결 분산
- TLS ingress
- Launcher / Supervisor
- 계정 등록
- Bot 검증
- 로그 / 백업 / 단계적 업데이트 절차
- 별도 Runtime 설치를 최소화한 win-x64 배포 패키지

규모 확장이 필요하다는 근거가 생기기 전에는 Redis, 마이크로서비스, Orchestration을 무조건 도입하지 않는 것이 원칙입니다.

## 검증 기록

### 서버 초기 기반 검증

기존 아키텍처 문서에 기록된 당시 서버 기반 검증 결과:

- 서버와 외부 Bot 연동 PASS
- 빌드 결과 **경고 0건 / 오류 0건**
- **테스트 52개 PASS**

### Gateway 배포 검증 시점

Gateway / 공유 상태 조정 / TLS를 확장한 별도 배포 검증에서는:

- Release 빌드 PASS
- **회귀 테스트 58개 PASS**
- 가중치 기반 실제 연결 분산의 개별 검증 PASS
- 패키지 파일 무결성 검사 PASS

회귀 검사 58개와 목적별 개별 검사를 합쳐 하나의 59개 전체 테스트 실행으로 과장하지 않고, 별도 검증으로 기록합니다.

## 실제 Unreal Client 연동 흐름

후속 Client 연동에서는 실제 UE Client가 서버 흐름을 이용합니다.

~~~text
Entry
→ Login UI
→ Gateway
→ 계정 인증
→ 자동 GS 입장
→ 초기 PlayerScope 적용
→ NeoSeoul OpenLevel
~~~

GameInstance가 LoginFlowCoordinator를 소유하고, AS / GS ConnectionHandler를 분리하며 PlayerScopeSubsystem이 인증/PlayerScope 상태를 맵 이동 동안 유지합니다.

실제 검증에는 다음이 포함됩니다.

- SFEditor UHT / C++ / DLL 빌드 PASS
- Gateway / AS / GS 실제 프로세스 실행
- 로그인 Widget 제출
- 인증 실패 처리
- GS 연결 끊김 정리
- 재로그인
- 서버 재시작
- 초기 PlayerScope 적용
- Entry → NeoSeoul 맵 이동

## 아직 완료로 주장하지 않는 범위

현재 포트폴리오에서 완료된 것으로 주장하지 않는 항목:

- 공용 WAN 환경의 Client 실행
- 운영 수준 고가용성(HA)
- 운영용 공유 RDB
- 운영용 Redis / Cache 배포
- 패키징 게임의 전체 연동
- Unreal Dedicated Server 게임플레이 복제
- 운영 수준의 다중 지역 배포

## Agent Workflow와의 연결

~~~text
Issue / Goal
→ 현재 원본 확인
→ server-feature-production Skill
→ 구조 / 권한 경계 조사
→ RPCNet 계약 영향 검토
→ 구현
→ 빌드 / 결정론적 테스트
→ Bot / 실제 프로세스 검증
→ 필요한 경우 Client 연동
→ Evidence
~~~

서버 기능을 추가할 때 전체 토폴로지를 다시 설계하지 않고 기존 권한·확장 지점 안에서 구현을 확장합니다.

## 이 사례의 핵심

이 Case Study의 핵심은 단순한 로그인 서버 구현이 아닙니다.

**Gateway / Account / Game persistent authority / future Dedicated Server를 분리하고, 실제 Bot과 Unreal Client가 동일 protocol contract를 통해 끝까지 연결되는 온라인 게임 서버 기반을 단계적으로 구축하고 검증한 것**입니다.
