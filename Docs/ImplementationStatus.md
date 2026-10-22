# 구현 및 검증 현황

> **문서 실사 기준:** ProjectSF `main` `08715c68b04f` (2026-10-22). 아래는 원본 계약과 과거 검증 기록의 정리입니다. 이번 갱신에서 코드·Unreal Editor·새 테스트를 실행하지 않았습니다. [읽은 범위와 미확인 사항](SourceAudit-2026-10-22.md)을 함께 확인하세요.

이 문서는 실제 ProjectSF 운영 구조와 공개 포트폴리오 설명을 맞추기 위한 상태표입니다.

## 문서로 확인한 운영 구조 (이번 갱신에서 실행 검증하지 않음)

### 작업 흐름

- 저장소 `AGENTS.md` 기반 초기 진입 확인
- 현재 원본 상태 확인
- Issue start / resume 및 Issue / Recipe partition gate
- Team Hub의 Spec / Revision / Stage / Gate 계약 (개별 계정의 플러그인 가용성은 미검증)
- 영속 일감 없이 직접 작업하는 경로
- TaskExecution
- ProductionConvergence
- ProductionEvolution
- ExecutionObservability
- ContextBudgetPolicy

### 규칙과 Skill

- 공통 리소스 제작 규칙
- 프로그래밍 규칙
- 검증 깊이 규칙
- 저장소에서 발견할 수 있는 Skill 패키지
- `.agents/skills`의 간결한 진입점
- UI 제작 / 아키텍처
- 지형 / Landscape
- 도로 제작
- 수계 지형 준비 / 수계 제작
- 월드 구조 / 검증
- 서버 기능 제작 / Bot 검증
- Issue 문맥 동기화
- 논점 수렴 / Spec 페이지 도구

### 실행 기반

`Tools/AgentPipeline`은 문서상 다음 구성으로 책임을 구분합니다:

- Config
- Registry
- Schemas
- Scripts
- Templates
- Examples
- Tests
- Runtime contract

### 제작 상태와 근거

- `Production/Data`
- `Production/Evidence`
- Issue 경로: GitHub Issue의 영속 업무 상태
- Spec 경로: Team Hub의 Revision / Stage / Gate / 확정 Plan·설계·UML
- 사람과 AI가 공유하는 제작 상태
- 입력 Revision과 검증 근거

## 제작 공정으로 개발한 기술 시스템

### RPCNet

- 재사용 가능한 Unreal 네트워크 Runtime
- TCP / WebSocket / Mock / Memory 전송 수단
- Session / Codec / Proxy / Stub 책임 경계
- C# ProtocolGenerator
- 생성 코드와 수작업 코드의 소유권 분리
- ProjectSF의 Consumer Integration 경계
- 프레임워크 경계 검사기
- 당시 검증 기록: 테스트 37개 및 실제 Unreal 상호운용성 PASS

### 온라인 서버

- Gateway / AS / GS 서비스 구성
- GS Directory / Registry
- Player / World / PlayerScope 영속 상태 권한
- Repository와 영속화 책임 경계
- 가중치 기반 Gateway 연결 분산
- TLS 외부 진입
- Launcher / Bot 검증
- 당시 검증 기록: 빌드 경고·오류 0건 및 테스트 52개 PASS
- UE의 Entry → Login → GS → PlayerScope → NeoSeoul 실제 연동

### DataForge

- 독립 실행형 Windows GUI와 CLI의 공통 Core
- XLSX / XLSM 기반 DataTable 제작 원본
- 데이터셋 단위 증분 변경 탐지
- SFDataBuildCommandlet Bake / Readback
- 생성된 Server JSON의 값 일치 검증
- DA / PrimaryDataAsset / INI의 엔진 기반 읽기 전용 추출
- 실패 시 롤백 / 마지막 정상 결과 게시 경계
- 첫 수직 절편의 데이터 일치 검증 PASS
- Core 회귀 테스트 25개 PASS

### Unreal 콘텐츠 제작

과거 프로토타입 기록과 제작 문서에서 다룬 범위:

- Terrain과 Landscape
- 도로 일괄 제작
- 수계 / 강 / 호수 제작
- 월드 구조와 검증
- UI 제작
- 서버·클라이언트 온라인 연동
- 데이터 제작

## 연구·개발 진행 중

- 계정 간 Spec 연결·쓰기를 포함한 실제 배포 및 권한 검증
- 문맥 인계와 비용·토큰 효율의 동일 조건 측정 (실측 없는 절감률은 주장하지 않음)
- 새 선형 탐험형 어드벤처 월드의 압축/절차 제작 검증; 과거 GIS 증거와 별개
- 도로·수계·월드 결과물의 제작 및 실행 성능 측정
- 월드 변경 보호 Gate: 2026-10-13 설계 문서 기준 **Runtime 연동 미구현(UNIMPLEMENTED)**
- 검증된 Recipe 기반 반복 일괄 실행의 재사용성 개선
- 사람 ↔ AI 간 수정·재개 왕복의 검증 근거 보강
- DataForge 고급 데이터 타입 지원 및 실제 제작 소비자 적용
- 서버의 운영 수준 HA / RDB / WAN 배포
- 중복 규칙과 오래된 Workflow 경로의 지속 정리

## 과거 검증 기록의 적용 범위

RPCNet의 37, Server의 52, DataForge의 25 테스트 등 수치는 이전 시점의 checkpoint 기록입니다. **2026-10-22에 재실행하지 않았으며** 최신 전체 제품 PASS나 Creator 승인의 근거로 승격하지 않습니다.

## 완료 성과로 주장하지 않는 범위

이 저장소는 다음을 완료된 범용 제품으로 주장하지 않습니다.

- 사람의 판단 없이 진행되는 완전 자율 게임 개발
- Unreal 모든 기능에 대한 범용 지원
- 사람의 시각 검수 없는 제작 완성
- 프로젝트별 Rule / Skill 없이 어느 게임에나 즉시 적용되는 이식성
- 운영 수준 다중 지역 서버 배포
- DataForge의 Unreal 모든 데이터 형식 지원
- 도구 호출 성공만으로 제품 품질이 입증됐다는 주장
