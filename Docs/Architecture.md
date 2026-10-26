# 구조와 책임 경계

**2026-10-22 기준 Snapshot:** ProjectSF `main` `08715c68b04f`. 이는 공개용 설명이며 독립 실행형 프레임워크가 아닙니다. **Standard First, Simple is Best**를 따릅니다.

현재 Agent Workflow의 구조는 ProjectSF에서 실제 운영 중인 Agent Workflow를 기준으로 합니다. 공통 서버 MCP와 현재 Spec 상태는 [2026-10-26 보완 실사](SourceAudit-2026-10-26.md), 실제 화면은 [사이트 공정](SFTeamHubWorkflow.md)을 따릅니다. 아래 구조 설명·원본 복사 Snapshot의 기준일을 일괄 최신으로 바꾸지 않습니다.

## 1. 저장소 진입점

실제 ProjectSF 작업은 원본 저장소 루트의 `AGENTS.md`에서 시작합니다.

`AGENTS.md`는 거대한 정책 문서가 아니라 다음 역할만 가집니다.

- 저장소 식별
- 필수 진입 확인
- Workflow / Rule / Skill 선택 경로
- 하위 영역별 지침 연결
- 변경 전에 확인해야 할 최소 실행 문맥

~~~text
AGENTS.md
  ↓
AI/Workflows
AI/Rules
AI/Skills
Docs/Production/Recipes
  ↓
Tools/AgentPipeline
  ↓
Production/Data + Production/Evidence
~~~

Codex의 repository Skill 자동 발견을 위해 `.agents/skills/<name>/SKILL.md`를 얇은 entrypoint로 두고, 실제 Skill 원본은 `AI/Skills/<name>/SKILL.md`가 소유합니다.

## 2. 책임 경계

| 구성 요소 | 담당 범위 | 담당하지 않는 범위 |
| --- | --- | --- |
| `AGENTS.md` | bootstrap, routing | 제품 설계, 도구 구현 |
| `AI/Workflows` | 실행 상태, 재개, Step 전환, 완료 판정 | 제품 고유 규칙 |
| `AI/Rules` | 공통 안전·품질 불변 조건 | 특정 작업 계획 |
| `AI/Skills` | 전문 판단, 입력 확인, 검증 방법, 필요한 프로젝트 제약 | 작업 상태·승인 원본 |
| `Docs/Production/Recipes` | 반복 가능한 제작 계약 | 매 실행의 상태 |
| `Tools/AgentPipeline` | schema, registry, binding, preflight, evidence tooling | 자연어 의미 판단 |
| `Production/Data` | 승인된 제작 입력 | workflow policy |
| `Production/Evidence` | 실행 및 검증 증거 | Goal / task state |
| GitHub Issue | Issue 제작의 Goal, Scope, Acceptance, current state, decisions | Spec Stage / Gate |
| SF Team Hub Spec | Spec Revision, Stage, Gate, 확정 Plan, design/UML | Issue state |
| Direct request | 영속 일감 불필요한 현재 요청의 승인 범위 | Issue/Spec 자동 생성 |
| `Docs` | 제품·기술·제작 설계 | Agent runtime state |

## 3. AI Agent와 Runtime의 역할

이 구조에서 가장 중요한 경계입니다.

### AI Agent

Agent가 판단합니다.

- 요청의 실제 의도
- 현재 Workflow와 Step
- 필요한 Rule / Skill / Recipe
- 미해결 질문
- 요구 capability
- 다음 행동
- Goal Acceptance 충족 여부

### AgentPipeline Runtime

Runtime은 Agent 판단을 실행 가능한 형태로 검증합니다.

- schema validation
- capability / tool binding
- registry lookup
- preflight
- deterministic scripts
- execution support
- evidence / manifest handling
- regression tests

과거의 keyword 기반 Prompt Compiler가 자연어 의미를 대신 판단하는 구조는 현재 모델의 중심이 아닙니다.

## 4. 현재 Workflow 구성

### CommonAIWorkflow

공통 실행 계약의 단일 원본입니다.

- bootstrap
- current source of truth
- Issue / Spec / 직접 작업의 원본 조회와 재개
- authority boundary
- Step transition
- Main / Sub delegation
- task outcome

### TaskExecution

조사, 설계, 진단, 수정처럼 일반적인 작업을 수행합니다.

### ProductionConvergence

연결된 제품 결과를 대상으로 반복 제작합니다.

~~~text
Make
→ Observe
→ Evaluate
→ Refine
→ re-evaluate Goal
~~~

### ProductionEvolution

Production 중 드러난 제작 기반 결함을 해결합니다.

예:

- 필요한 reusable Tool 부재
- Recipe 문제
- Skill 경계 문제
- validation path 부족

개선 후 원래 Production Step으로 복귀하는 것이 핵심입니다.

### ExecutionObservability

작업자가 현재 Agent의 선택과 실행 상태를 확인할 수 있도록 화면 Trace를 정의합니다.

## 5. 현재 Skill 모델

Skill은 직군 분업 단위가 아니라 **반복 가능한 전문 판단 단위**입니다.

ProjectSF의 현재 예:

- issue-context-sync
- discussion-convergence
- sf-page-tools
- ability-production
- trigger-production
- building-production
- terrain-production
- landscape-production
- road-production
- water-terrain-preparation
- water-production
- world-structure
- world-validation
- ui-production
- ui-architecture
- server-feature-production
- server-bot-verification

하나의 Goal은 여러 Skill을 Step별로 조합할 수 있습니다.

## 6. 지속 작업 문맥

대화 기억을 장기 상태 저장소로 사용하지 않습니다.

~~~text
GitHub Issue (경량 제작)
  └─ Goal / Scope / Acceptance / decisions / next action

SF Team Hub (Spec 제작)
  └─ Spec / Revision / Stage / Gate / confirmed Plan, design, UML

Direct request (영속 일감 불필요)
  └─ 허용 범위와 산출물

Production/Evidence
  ├─ manifest
  ├─ measurements
  ├─ validation
  └─ artifact references
~~~

다른 세션에서도 Issue는 최신 GitHub Issue를, Spec은 Team Hub의 실제 ID·Revision을 다시 읽습니다. 기억이나 연결되지 않은 서비스 응답으로 현재 상태를 추측하지 않습니다.

## 7. 공통 서버 MCP와 클라이언트 역할

```text
개인 GPT / 로컬·원격 Codex
        ↓ 허용된 계정·도구
SF Team Hub 서버 MCP
        ↓ 버전·작성자·권한 검사
Spec Store / Revision / 문서·결정·Evidence
        ↓ 실제 결과 재조회
현재 Stage·Gate / 다음 행동
```

팀원별 플러그인 배포는 채택하지 않았습니다. 기존 서버 MCP를 공통으로 사용하고 계정별 허용 범위와 작성자 귀속을 확인합니다. GPT는 플랜·설계 협업, Codex는 실제 구현 적합성 검토·제작·검증을 담당합니다. 코드·에셋 구현의 FAIL/PENDING은 설계 생성으로 PASS가 되지 않습니다.

Spec ID로 새 세션에서 현재 상태를 복원하며 최신 버전·결정·필요한 기록을 대조합니다. GPT의 기획 협업에 Codex 전용 부분 조회·토큰 최적화 순서를 강제하지 않습니다. Docs는 사람이 읽는 프로젝트 설명이며 실행 계약·코드의 별도 사본이 되지 않습니다.
