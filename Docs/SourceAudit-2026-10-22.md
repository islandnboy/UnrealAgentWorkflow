# Source inspection and publication boundary — 2026-10-22

> Portfolio maintenance audit. This log distinguishes the **repository tree inventory**, **retrieved and inspected text**, **historical evidence referenced**, and **unverified areas**. It is not a claim of end-to-end code review or runtime validation.

## Source of truth / pinned revision

| Role | Repository | Branch | Revision at inspection | Visibility |
| --- | --- | --- | --- | --- |
| Working source of truth | `islandnboy/ProjectSF` | `main` | `08715c68b04fb6472df941a3dd0fd9d19cac4e49` | Private |
| Published portfolio before refresh | `islandnboy/UnrealAgentWorkflow` | `main` | `2f5ccf467031cb30a3d4a73313c0a610e692c6f2` | Public |

This is a **selected publication snapshot**, not a branch mirror, fork, or independent generic framework. Future ProjectSF changes are not automatically reflected here.

## Inventory checked (names, sizes, paths; not a contents review)

- ProjectSF recursive `main` tree was returned without truncation: **17,970 entries**. The code, binary assets, runtime data, documents, and evidence names are mixed in that count; it is **not** a number of reviewed documents.
- Portfolio recursive `main` tree was returned without truncation: **61 entries**.
- Inventory inspected by category: `AGENTS.md`, `.agents/skills`, `AI/Rules`, `AI/Skills`, `AI/Workflows`, `Docs/Development/AgentWorkflow`, `Docs/Production/Recipes`, `Docs/Project`, `Docs/Technical`, `Production/Evidence`, `Client/.AI`, `Server/.AI`, `Tools/AgentPipeline`, and public `CaseStudies`, `Docs`, `Reference`, `Samples`.

## Document content retrieved and inspected

**Portfolio existing files:** `README.md`; `Docs/{Architecture,CreatorCentricProduction,CreatorWorkflow,ExecutionModel,ImplementationStatus,MCPExecution,ProductionModel}.md`; `CaseStudies/{AgentDrivenGameProduction,WorldProduction,DesignerProduction,RecipeProduction,UIProduction,ServerProduction,RPCNet,DataForge}.md`; `Reference/ProjectSF/{README,Structure,ContextBudgetPolicy,ExecutionObservability}.md`; `Reference/ProjectSF/Skills/{road-production,ui-production}/SKILL.md`; `Reference/ProjectSF/Recipes/Recipe.template.md`; `Samples/{README,DesignerArtifactSet}.md`. Metadata and selected content/headings were reviewed; this is not a full editorial audit of every paragraph or attached media.

**ProjectSF source documents:** `AGENTS.md`; `AI/Workflows/{CommonAIWorkflow,TaskExecution,ProductionConvergence,ProductionEvolution,ContextBudgetPolicy,ExecutionObservability,SubAgentDelegation}.md`; `AI/Rules/{Programming,VerificationDepth,ResourceProduction,FailureRecovery}.md`; `AI/Skills/{road-production,ui-production,issue-context-sync,discussion-convergence,world-structure,world-validation}/SKILL.md`; `Docs/Project/{Vision,ProjectIntroduction,ProjectOverview}.md`; `Docs/Production/Recipes/{Overview,README,RecipeFormat}.md`; `Docs/Development/AgentWorkflow/{Architecture,MaintenancePolicy,AgentLean,CreatorProductionGuide,SharedProductionGuide,SharedProductionReview,SFTeamHubAgentAPI,SFTeamHubGitHubRelay,WorldMutationGate}.md`; `Docs/Technical/Architecture/GeoWorldProductionArchitecture.md`; `Docs/Production/World/NeoSeoulFoundationComparison.md`; `Production/Evidence/README.md`. Full file responses were retrieved, and relevant sections, rules, headings, and status markers were inspected; long files received **selective inspection**, not a line-by-line certification.

## Not inspected / not verified

- **Not read:** C++, headers, C#/Python/JS/TS source, scripts, executable runtime, binaries, Unreal `.uasset`/`.umap`, and unpublished local changes. A file tree only confirms presence, not behavior.
- **Not read in full:** all other source `AI/Skills`, `Docs/Design`, `Docs/Technical`, `Docs/Production/Recipes` templates, historical review documents, per-Issue `Production/Evidence`, GitHub Issue bodies/comments, Team Hub Spec/Revision and Library content. They are neither reproduced nor assumed complete.
- **Not run:** Unreal Editor/PIE, MCP live calls, tests, builds, runtime deployment, data-bake parity, art/quality review, or Creator acceptance. No new throughput, test pass count, or issue closure is asserted.
- Existing public PDF and image attachments are **not revalidated as evidence** by this audit. Historical figures remain labelled with their original checkpoint and limits.
- Nonpublic credentials, tokens, endpoints, operational configuration, unpublished story/design/production inputs and private issue/evidence bodies are **outside publication scope**. Private GitHub links would not be useful to public readers.

## Interpretation for the refresh

1. Current source explicitly distinguishes **Issue**, **SF Team Hub Spec**, and **direct task** entry paths. Do not represent the historical Issue-first diagram as compulsory for every task.
2. **Standard First, Simple is Best** and official engine/tool capabilities come before new custom layers.
3. Creator owns goal/design/acceptance; AI Agent implements and performs baseline regression; Programmer reviews closed issues **across** systems and returns findings to refactoring, Workflow, Skill, and Guideline.
4. Tool/build success, technical evidence, Creator acceptance and `main` publication are separate checkpoints.
5. GIS/NeoSeoul large-world evidence is **historical prototype evidence**; ProjectSF's post-2026-10-14 product direction is a linear exploration action-adventure with a smaller curated world. Historical snapshots should not be relabelled as validation of the new world.
6. A document calling a capability `UNIMPLEMENTED` or `NOT_TESTED` overrides optimistic portfolio language for that capability.

## Publication / validation checklist

- [x] Inspect both main trees and prior portfolio before edits.
- [x] Separate inspected files from inventory-only and unverified scope.
- [ ] Apply only selected documentation/snapshot updates.
- [ ] Review changed Markdown for inadvertent private material.
- [ ] Verify local Markdown links and source-attribution paths in the public repository.
- [ ] Read back final portfolio `main`, record commit and unresolved checks.

This checklist is updated during publication.