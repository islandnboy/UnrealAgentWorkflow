# ProjectSF 저장소 구조

## 진입점

~~~text
AGENTS.md
└─ resolves Workflow / Rule / Skill / Recipe before mutation
~~~

## Agent 실행 지침

~~~text
AI/
├─ Workflows/
│  ├─ CommonAIWorkflow.md
│  ├─ ContextBudgetPolicy.md
│  ├─ ExecutionObservability.md
│  ├─ ProductionConvergence.md
│  ├─ ProductionEvolution.md
│  └─ TaskExecution.md
├─ Rules/
│  ├─ Programming.md
│  ├─ ResourceProduction.md
│  └─ VerificationDepth.md
└─ Skills/
   ├─ issue-context-sync/
   ├─ terrain-production/
   ├─ landscape-production/
   ├─ road-production/
   ├─ water-terrain-preparation/
   ├─ water-production/
   ├─ world-structure/
   ├─ world-validation/
   ├─ ui-production/
   ├─ ui-architecture/
   ├─ server-feature-production/
   └─ server-bot-verification/
~~~

## 실행 기반

~~~text
Tools/AgentPipeline/
├─ Config/
├─ Registry/
├─ Schemas/
├─ Scripts/
├─ Templates/
├─ Examples/
├─ Tests/
└─ Runtime.md
~~~

Runtime은 자연어 의미를 소유하지 않습니다.  
Agent가 선택한 Workflow / Step / capability를 구조적으로 검증하고 실제 실행을 지원합니다.

## 제작 상태와 근거

~~~text
Production/
├─ Data/
└─ Evidence/
~~~

- Data: 승인된 입력
- Evidence: 실행·검증 근거
- GitHub Issue: Issue 제작의 Goal / Acceptance / current state
- SF Team Hub: Spec / Revision / Stage / Gate / Plan·design 원본
- Direct request: 영속 일감이 필요 없는 요청의 범위
- Docs: 장기 제품·기술·제작 계약

## Codex의 Skill 발견

~~~text
.agents/skills/<skill-name>/SKILL.md
            ↓ 간결한 진입점
AI/Skills/<skill-name>/SKILL.md
            ↓ 원본(Source of Truth)
~~~

Skill의 실제 절차를 두 위치에 복제하지 않는 것이 원칙입니다.
