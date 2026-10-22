# ProjectSF current workflow snapshot

> Pinned source: private ProjectSF `main` `08715c68b04fb6472df941a3dd0fd9d19cac4e49` (2026-10-22). **Verbatim**과 **curated summary**를 명시적으로 구분합니다. 원본 전체 복제나 독립 범용 프레임워크가 아닙니다.

이 디렉터리는 비공개 working repository인 ProjectSF의 **현재 Agent Production 구조를 포트폴리오용으로 선별 정리한 Snapshot**입니다.

전체 게임 코드, 콘텐츠, Issue, Production Data/Evidence는 공개하지 않습니다.

## Current source structure

~~~text
AGENTS.md
.agents/skills/
AI/
├─ Workflows/
├─ Rules/
└─ Skills/

Docs/
├─ Project/
├─ Design/
├─ Technical/
├─ Production/
└─ Development/

Tools/AgentPipeline/
├─ Config/
├─ Registry/
├─ Schemas/
├─ Scripts/
├─ Templates/
├─ Examples/
└─ Tests/

Production/
├─ Data/
└─ Evidence/
~~~

## Included here

- [Structure](Structure.md)
- [Context Budget Policy (curated public summary)](ContextBudgetPolicy.md)
- [Execution Observability (curated public summary)](ExecutionObservability.md)
- [Task Execution (verbatim source)](TaskExecution.md)
- [Programming rule (curated)](Rules/Programming.md)
- [Verification Depth rule (verbatim source)](Rules/VerificationDepth.md)
- [UI Production Skill (verbatim source)](Skills/ui-production/SKILL.md)
- [Road Production Skill (verbatim source)](Skills/road-production/SKILL.md)
- [Inspection scope and limitations](../../Docs/SourceAudit-2026-10-22.md)

포트폴리오 설명은 상위 `Docs/`가 담당하고, 이 폴더는 실제 ProjectSF 구조가 어떤 형태로 운영되는지 확인하기 위한 선택적 Snapshot입니다.
