# RPCNet 네트워크 시스템 사례

## 게임 프로젝트 안에서 재사용 가능한 네트워크 기반

RPCNet은 ProjectSF의 Client / Server 통신을 위해 개발했지만, 게임의 Login·Inventory·World 같은 Domain 의미와 분리한 **재사용 가능한 Network Framework**입니다.

핵심 목표는 프로젝트 코드에 socket / packet boilerplate를 반복해서 만들지 않고, **Runtime + Protocol Code Generation + Consumer Integration**의 경계를 명확히 하는 것입니다.

> 실무 흐름을 그림 중심으로 정리한 포트폴리오 문서: [Protocol → Unreal Client Guide (PDF)](../Docs/Guides/RPCNet_Protocol_to_Unreal_Client_Guide.pdf)

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
│  └─ PluginInstaller
│
└─ Integration/
   └─ ProjectSF
      ├─ Protocol Schema
      ├─ Generation Configuration
      └─ Consumer Targets
~~~

### Runtime

~~~text
NetworkRuntimeSubsystem
        ↓
NetworkRuntime
        ↓
NetworkSession
        ↓
INetworkTransport
   ├─ TCP
   ├─ WebSocket
   ├─ Mock
   └─ Memory
        ↓
Packet Codec / Envelope
~~~

Mock / Memory Transport를 Framework 내부에 두어 실제 서버 없이도 상위 Network 흐름을 검증할 수 있습니다.

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
- Unreal Runtime 플러그인
- 플러그인 설치 도구
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

- canonical Unreal plugin source는 하나
- Client plugin copy는 installer output
- framework source에서 Client / Server gameplay code 역참조 금지
- Project-specific integration은 별도 디렉터리
- `Validate-Boundary.ps1`로 boundary 검사

따라서 필요하면 Runtime / Toolchain을 별도 repository나 SDK로 추출할 수 있는 구조를 유지합니다.

## 검증 근거

ProjectSF server foundation의 RPCNet 관련 검증 checkpoint에서 다음을 확인했습니다.

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

**게임 프로젝트에서 반복되는 네트워크 코드를 Framework와 Consumer 계약으로 분리하고, 코드 생성·Runtime·Mock·실제 Client/Server interop까지 하나의 검증 가능한 개발 체계로 만든 것**이 핵심입니다.
