# RPCNet 네트워크 시스템 사례

## 게임 프로젝트 안에서 재사용 가능한 네트워크 기반

RPCNet은 **상용 Unreal Engine 프로젝트에서 클라이언트 네트워크 전환과 프레임워크를 직접 설계·구현한 경험**을 바탕으로, 개인 ProjectSF에서 별도로 설계·개발한 통신 프레임워크입니다. 원본 상용 코드나 자료를 공개하지 않으며, 이 문서는 ProjectSF의 RPCNet 구현·검증 범위만 다룹니다.

목표는 게임의 Login·Inventory·World 같은 Domain 의미와 분리한 **재사용 가능한 Network Framework**를 구축하고 실제 Client/Server 통신으로 검증하는 것입니다.

핵심 목표는 프로젝트 코드에 socket / packet boilerplate를 반복해서 만들지 않고, **Runtime + Protocol Code Generation + Consumer Integration**의 경계를 명확히 하는 것입니다.

> [현재 적용 가이드](#현재-적용-가이드) · 이전 구조를 그림으로 설명한 자료: [Protocol → Unreal Client Guide (PDF)](../Docs/Guides/RPCNet_Protocol_to_Unreal_Client_Guide.pdf)

## 해결하려는 문제

게임 네트워크 구현이 프로젝트 전용 코드와 강하게 결합되면 다음 문제가 생깁니다.

- Client와 Server에서 반복되는 프로토콜 코드
- 전송 수단 변경이 게임플레이 코드까지 전파됨
- 생성 코드와 직접 작성 코드의 소유권 혼재
- 테스트할 때마다 실제 Server가 필요함
- 다른 프로젝트에 프레임워크를 재사용하기 어려움

RPCNet은 이 경계를 분리합니다.

## 구조

~~~text
RPCNet/
├─ Runtime/
│  └─ Unreal/RPCNet
│     ├─ Runtime
│     ├─ Session
│     ├─ Transport
│     ├─ Packet Codec / Envelope
│     └─ Proxy / Stub base
│
├─ Toolchain/
│  ├─ ProtocolGenerator
│  └─ SDKInstaller
│
└─ Integration/
   └─ ProjectSF
      ├─ Protocol Schema
      ├─ Generation Configuration
      └─ Consumer Targets
~~~

### Runtime

~~~text
UGameInstance
└─ UNetworkManagerSubsystem : UGameInstanceSubsystem
   ├─ FNetworkRuntime
   │  └─ 세션 핸들 → FNetworkSession
   │     ├─ INetworkRequestSender
   │     ├─ Packet Codec / Envelope
   │     └─ INetworkTransport
   └─ TCP 세션 생성 경로: FTcpNetworkTransport 생성·종료 관리
~~~

`UNetworkManagerSubsystem`이 초기화·종료와 게임 스레드의 펌프를 맡습니다. `FNetworkRuntime`은 세션을 핸들로 관리하고 각 세션을 펌프하며 기본 세션 핸들을 보관합니다. `FNetworkSession`은 요청·응답, 패킷 인코딩·디코딩과 전송을 담당합니다. Codec은 Session에서 사용하며 Transport 뒤의 별도 네트워크 계층이 아닙니다.

ProjectSF의 실제 연결 Handler는 GameInstance에서 `UNetworkManagerSubsystem`을 얻습니다. 월드 입장 요청은 찾은 GS 세션을 생성된 Proxy에 직접 넘깁니다. GS Handler는 세션 준비 시 해당 핸들을 기본 세션으로 지정하고, 해제할 때 같은 핸들이 기본값이면 지웁니다.

Mock·Memory Transport를 이용한 프레임워크 테스트와 ProjectSF Handler의 실제 TCP 연결은 별개입니다.

### 프로토콜 생성기

Protocol Generator는 ProjectSF의 protocol 의미를 직접 알지 않습니다.

~~~text
Consumer Schema + Generation Config
              ↓
      RPCNet ProtocolGenerator
              ↓
      Generated Consumer Code
          ↙             ↘
       Client          Server
~~~

Generator Core가 Login / Inventory / C2AS 같은 프로젝트 Domain을 하드코딩하지 않고, Consumer Integration이 실제 schema와 target을 선택합니다.

## 소유권과 책임 경계

### RPCNet의 책임

- 전송 수단 추상화
- 연결 / 세션 수명 관리
- 패킷 Envelope / Codec
- Proxy / Stub 확장 기반
- 프로토콜 파싱 / 검증 / 코드 생성 기반
- Unreal Runtime 소스 SDK
- SDK 설치·일치 검증 도구
- 프레임워크 책임 경계 검증

### ProjectSF의 책임

- protobuf Schema
- 메시지 ID
- Login / Game / Inventory 도메인 의미
- Endpoint 정책
- 코드 생성 대상 선택
- 직접 작성하는 Endpoint / Service
- 게임플레이 연동

핵심 규칙:

~~~text
RPCNet = 통신 기반
ProjectSF = 게임별 프로토콜 의미
~~~

## 생성 코드와 직접 작성 코드

Generated code를 최종 business implementation으로 사용하지 않습니다.

~~~text
Protocol Schema
→ Generated DTO / Proxy / Stub
→ Handwritten Final Endpoint
→ Domain Service
~~~

이 구조를 통해 protocol 재생성 시 handwritten logic을 보존합니다.

## 프레임워크 분리 경계

RPCNet은 현재 ProjectSF monorepo 안에 있지만 `RPCNet/`을 독립적인 Framework Boundary로 유지합니다.

- Runtime 원본은 `RPCNet/Runtime/Unreal/RPCNet` 하나입니다.
- `Client/Source/SF/Network/RPCNet`은 SF 모듈에서 컴파일하는 버전 관리 설치본입니다. `.uproject`에 RPCNet 플러그인을 추가하지 않습니다.
- framework source에서 Client / Server gameplay code 역참조 금지
- Project-specific integration은 별도 디렉터리
- `Validate-Boundary.ps1`로 boundary 검사

따라서 필요하면 Runtime / Toolchain을 별도 repository나 SDK로 추출할 수 있는 구조를 유지합니다.

## 현재 적용 가이드

ProjectSF 원격 main `ff8e11e89f9d`의 SDK·생성기·Consumer를 기준으로 설명합니다.

| 바꾸려는 것 | 수정할 원본 | 변경 후 확인 |
| --- | --- | --- |
| Runtime·전송 | RPCNet Runtime SDK | SDKInstaller로 SF 모듈 설치본 갱신 → 원본 일치 → 영향 범위 빌드 |
| 메시지·공용 타입 | Consumer protobuf 스키마·생성 설정 | DTO·Codec·Proxy·Stub 재생성 → 변경 범위와 재현성 확인 |
| 게임 상태 | 직접 작성한 Endpoint·Connection·도메인 모델 | 타입이 정해진 요청·수신·상태 적용 검증 |

송신은 Unreal 요청 DTO와 생성된 final Proxy를 사용합니다. 수신 StubBase가 protobuf decode와 DTO 변환을 맡고, 직접 작성한 Endpoint가 Handler에 전달합니다. 연결 전환·재시도·게임 상태는 Consumer 책임입니다. SDK 설치본과 생성물에는 직접 로직을 추가하지 않습니다.

**Shared DTO와 enum**은 서버 C# 타입과 Unreal 값 타입을 같은 스키마에서 생성하는 공용 계약입니다. PlayerScope는 생성된 Shared snapshot 하나를 저장하며, 기존 Blueprint 타입은 읽기용으로 계산합니다.

**인벤토리 적용:** SPEC-95에서 Item 통신과 장비 상태의 책임을 분리했습니다. 아이템 분류는 Shared enum, 착용·해제 요청과 상태는 InventoryModel이 소유합니다. [실제 사례와 발행 상태](GameplayWorkflow-20261113.md#인벤토리와-무기-착용)를 참고하세요.

**SF 설치본의 기본 세션 경로:** 인벤토리 호출은 Consumer 객체를 생성된 Item Proxy에 넘깁니다. SF 설치본의 `FNetworkProxyBase` 편의 생성자는 객체의 Outer 체인에서 GameInstance 또는 GameInstanceSubsystem을 찾아 `UNetworkManagerSubsystem`의 기본 세션을 요청 발신자로 얻습니다. 유효한 기본 세션이 없는 상태에서 쓰는 경로는 아닙니다. 대조한 SDK 원본의 Proxy Base에는 발신자를 명시적으로 받는 생성자만 있습니다. 따라서 편의 생성자를 원본 SDK 공통 API로 설명하지 않습니다.

## 검증 방법 — 실제 서버 구성

RPCNet을 테스트하기 위해 ProjectSF에 **Gateway / Account Server / Game Server**를 구성했습니다. Mock/Memory Transport 검사와 별개로 실제 TCP 연결, 프로토콜 처리, 로그인·GS 입장, Client 상태 반영을 통합 시험하기 위한 환경입니다.

이후 서버 쪽의 인증·PlayerScope·영속화·연결 분산 등도 구성해 테스트 범위를 확장했습니다. 서버 개발은 별도 상용 서비스 실적이 아니라 **RPCNet의 실제 사용·상호운용성 검증에서 출발한 작업**입니다. 세부 구현·검증 기록은 [통신 검증용 서버 사례](ServerProduction.md)를 참고합니다.

## 검증 근거

ProjectSF 서버 기반과 실제 Unreal Client 통신의 RPCNet 관련 검증 checkpoint에서 다음을 확인했습니다.

- 안정적인 코드 재생성
- 생성된 Stub에서 직접 작성한 최종 Endpoint로 연결
- 잘못된 Route / Kind / Direction 거부
- 잘못된 입력 / Replay / Epoch / 길이 처리
- 실제 TCP 연결과 Session 정리
- Client 형식의 패킷이 Gateway / AS / 독립 GS를 통과하는 흐름
- **테스트 37개 PASS**
- **실제 Unreal Client 상호운용성 PASS**

이 수치는 해당 validation checkpoint의 결과이며 이후 모든 기능을 포괄하는 숫자로 사용하지 않습니다.

## Agent Workflow와의 연결

RPCNet 작업도 일반 코드 생성과 다르게 다음 계약을 따릅니다.

~~~text
Issue 목표
→ 현재 RPCNet / Consumer 경계 조사
→ 필요한 프로토콜 동작
→ Schema / Generator / Runtime 영향 검토
→ 구현
→ 재생성
→ 책임 경계 검증
→ Client / Server 상호운용성
→ Evidence
~~~

Agent가 새 기능을 구현할 때 Runtime에 Project-specific 의미를 밀어 넣지 않는지까지 Acceptance로 검증합니다.

## 이 사례의 핵심

RPCNet에서 보여주고 싶은 것은 “TCP를 구현했다”가 아닙니다.

**상용 프로젝트에서 얻은 네트워크 프레임워크 설계 경험을 개인 R&D에서 RPCNet으로 확장하고, 자체 구성한 실제 서버와 Unreal Client의 통신까지 확인했다**는 점입니다. 프레임워크/Consumer 책임 분리와 생성 코드 관리, Mock 및 실제 연결 검증은 그 과정의 구체적인 설계·검증 내용입니다.
