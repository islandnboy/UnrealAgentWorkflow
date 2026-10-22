# 원본 실사 및 공개 범위 기록 — 2026-10-22

> 포트폴리오 유지보수 실사 기록입니다. **파일 목록을 확인한 범위**, **본문을 조회하고 검토한 범위**, **참고한 과거 Evidence**, **확인하지 못한 범위**를 구분합니다. 전체 코드 검토나 런타임 검증 완료 보고서가 아닙니다.

## 원본과 기준 Revision

| 구분 | 저장소 | 브랜치 | 실사 기준 Revision | 공개 상태 |
| --- | --- | --- | --- | --- |
| 작업 원본(Source of Truth) | `islandnboy/ProjectSF` | `main` | `08715c68b04fb6472df941a3dd0fd9d19cac4e49` | 비공개 |
| 갱신 전 포트폴리오 | `islandnboy/UnrealAgentWorkflow` | `main` | `2f5ccf467031cb30a3d4a73313c0a610e692c6f2` | 공개 |

이 저장소는 **선별한 공개용 Snapshot**입니다. 원본 브랜치의 거울 저장소나 복제 저장소가 아니며, 별도로 재설계한 범용 프레임워크도 아닙니다. ProjectSF의 후속 변경이 자동 반영되지는 않습니다.

## 파일 목록 확인 범위 (본문 정독과 다름)

- ProjectSF `main`의 전체 트리 **17,970개 항목**을 누락 표시 없이 조회했습니다. 여기에 코드·바이너리·문서·Evidence가 섞여 있으며, 검토한 문서 수를 뜻하지 않습니다.
- 갱신 전 포트폴리오 `main`의 전체 트리 **61개 항목**을 조회했습니다.
- 파일 분류·경로를 확인한 범위: `AGENTS.md`, `.agents/skills`, `AI/Rules`, `AI/Skills`, `AI/Workflows`, `Docs/Development/AgentWorkflow`, `Docs/Production/Recipes`, `Docs/Project`, `Docs/Technical`, `Production/Evidence`, `Client/.AI`, `Server/.AI`, `Tools/AgentPipeline`, 공개 저장소의 `CaseStudies`, `Docs`, `Reference`, `Samples`.

## 본문을 조회하고 관련 내용을 검토한 범위

**갱신 전 공개 포트폴리오:** `README.md`, `Docs/{Architecture,CreatorCentricProduction,CreatorWorkflow,ExecutionModel,ImplementationStatus,MCPExecution,ProductionModel}.md`, `CaseStudies/{AgentDrivenGameProduction,WorldProduction,DesignerProduction,RecipeProduction,UIProduction,ServerProduction,RPCNet,DataForge}.md`, `Reference/ProjectSF/{README,Structure,ContextBudgetPolicy,ExecutionObservability}.md`, `Reference/ProjectSF/Skills/{road-production,ui-production}/SKILL.md`, `Reference/ProjectSF/Recipes/Recipe.template.md`, `Samples/{README,DesignerArtifactSet}.md`. 본문 일부·제목·구조를 검토했으며, 첨부 매체까지 모든 문단을 전수 감수한 것은 아닙니다.

**ProjectSF 원본 지침·워크플로:** `AGENTS.md`, `AI/Workflows/{CommonAIWorkflow,TaskExecution,ProductionConvergence,ProductionEvolution,ContextBudgetPolicy,ExecutionObservability,SubAgentDelegation}.md`, `AI/Rules/{Programming,VerificationDepth,ResourceProduction,FailureRecovery}.md`, `AI/Skills/{road-production,ui-production,issue-context-sync,discussion-convergence,world-structure,world-validation}/SKILL.md`.

**ProjectSF 원본 설계·제작 문서:** `Docs/Project/{Vision,ProjectIntroduction,ProjectOverview}.md`, `Docs/Production/Recipes/{Overview,README,RecipeFormat}.md`, `Docs/Development/AgentWorkflow/{Architecture,MaintenancePolicy,AgentLean,CreatorProductionGuide,SharedProductionGuide,SharedProductionReview,SFTeamHubAgentAPI,SFTeamHubGitHubRelay,WorldMutationGate}.md`, `Docs/Technical/Architecture/GeoWorldProductionArchitecture.md`, `Docs/Production/World/NeoSeoulFoundationComparison.md`, `Production/Evidence/README.md`.

위 문서들의 반환 본문에서 관련 규칙·설계 방향·검증 상태를 확인했습니다. 긴 문서는 필요한 부분을 **선택적으로 조사**했으며, 모든 줄을 독립 검증한 것이 아닙니다.

## 추가로 확인한 과거 검증 기록

다음 텍스트 기록을 읽어 검증 시점과 주장 범위를 대조했으며, 테스트를 재실행하지는 않았습니다.

- `Production/Evidence/Issue-54/District-Expansion-20261008/ClosureReview-20261008/README.md`
- `Production/Evidence/Issue-64/CameraDistanceStage4-20261014.md`
- `Production/Evidence/Issue-28/MainIntegration-20261007/Result.md`
- `Production/Evidence/Issue-62/ValidationReport-20261009.md`
- `Production/Evidence/MainIntegration-20261016.md`
- `Docs/Development/AgentWorkflow/Reviews/AgentLeanReview-20260920.md`

공개 가능성 판단을 위해 `AI/Rules/{Programming,VerificationDepth}.md`의 계약 본문도 추가 확인했습니다. 이는 제한된 시점의 검증 근거이며 신규 월드의 완성이나 새 PASS를 의미하지 않습니다.

## 미확인·미검증 범위

- **읽지 않음:** C++ 헤더/코드, C#·Python·JavaScript·TypeScript 소스, 실행 스크립트, Unreal `.uasset`·`.umap` 등 바이너리와 로컬 미커밋 변경.
- **전수 정독하지 않음:** 나머지 `AI/Skills`, `Docs/Design`, `Docs/Technical`, 개별 제작 템플릿, 과거 설계 검토, 각 Issue의 Evidence, GitHub Issue 본문/댓글, Team Hub의 실제 Spec·Revision·Library.
- **새로 실행하지 않음:** Unreal Editor·PIE, 실시간 MCP 호출, 빌드·테스트, 배포, 데이터 변환 일치 검사, 시각적 품질 검수, Creator 최종 승인.
- 기존 공개 PDF·이미지의 내용·원본성은 이번 실사에서 다시 검증하지 않았습니다. 수치는 기존 검증 날짜와 한계를 함께 유지합니다.
- 비공개 인증정보, Token, 서버 운영 설정, 미발행 설계·제작 데이터, 내부 Issue/Evidence 원문은 공개 대상에서 제외했습니다. 비공개 GitHub 경로를 공개 독자가 열 수 있다고 가정하지 않습니다.

## 이번 갱신의 판단 기준

1. 현재 작업 진입 경로는 **Issue / SF Team Hub Spec / 직접 요청**입니다. 과거 Issue 기반 도식을 모든 작업의 필수 조건으로 만들지 않습니다.
2. **Standard First, Simple is Best.** 공식 엔진·도구의 기능과 기존 구조를 먼저 확인합니다.
3. **Creator**는 목표·설계·제품 수용을, **AI Agent**는 구현과 기본 회귀 검증을, **Programmer**는 완료 이슈 간 구조·기능 품질 검토와 Workflow/Skill/Guideline 개선을 책임집니다.
4. Tool/Build 성공, 제품 검증, Creator 수용, 원격 `main` 반영은 서로 다른 확인 단계입니다.
5. GIS/NeoSeoul 대규모 월드는 **과거 프로토타입 실험**입니다. 2026-10-14 이후의 압축된 선형 탐험형 신규 월드가 검증됐다는 뜻이 아닙니다.
6. 원본에서 `UNIMPLEMENTED`·`NOT_TESTED`로 표시한 항목을 포트폴리오에서 완료로 승격하지 않습니다.

## 공개 및 검증 작업 결과

- [x] 수정 전에 두 저장소의 `main` 트리와 기존 포트폴리오를 확인했습니다.
- [x] 실제 본문을 읽은 범위와 목록만 확인한 범위, 미검증 범위를 구분했습니다.
- [x] 필요한 설명과 선별 Snapshot만 수정했습니다.
- [x] 새 공개 문서에서 명백한 인증정보·내부 연결 정보를 점검했습니다. 전체 보안 감사와는 다릅니다.
- [x] 공개 저장소의 Markdown 링크가 가리키는 **파일 경로**를 트리와 대조하고, 제목 변경으로 잘못된 앵커 하나를 수정했습니다.
- [x] 게시 후 원격 `main`을 다시 읽고 원본 그대로 공개한 지침·스킬의 Git Blob SHA를 대조했습니다.
- [ ] 외부 링크의 실제 접속, 모든 앵커의 브라우저 렌더링, PDF·이미지 내용의 재검증은 실시하지 않았습니다.

## 공개 산출물과 원본 일치 여부

다음 파일은 기준 시점 ProjectSF에서 **원본을 그대로 선별 복사**했으며 Git Blob SHA가 같습니다.

| ProjectSF 원본 | 공개 Snapshot | Git Blob SHA |
| --- | --- | --- |
| `AI/Workflows/TaskExecution.md` | `Reference/ProjectSF/TaskExecution.md` | `bc132d2580ab88544c5faea0378bf526494cdfcc` |
| `AI/Skills/ui-production/SKILL.md` | `Reference/ProjectSF/Skills/ui-production/SKILL.md` | `fb85610c4204791137dbd51947f7ab1c60665999` |
| `AI/Skills/road-production/SKILL.md` | `Reference/ProjectSF/Skills/road-production/SKILL.md` | `0b786742cbd62967c855c67e90c24392798bfef1` |
| `AI/Rules/VerificationDepth.md` | `Reference/ProjectSF/Rules/VerificationDepth.md` | `4d0fb4098382e41c4e0af8269d80d70633ddda14` |

`Reference/ProjectSF/{ContextBudgetPolicy,ExecutionObservability}.md`, `Reference/ProjectSF/Rules/Programming.md` 등은 공개용 요약으로, 원본과 글자 단위로 같다고 주장하지 않습니다. 포트폴리오 변경으로 Unreal 코드·게임 에셋·실제 제품 상태를 수정하거나 빌드/PIE를 새로 검증하지 않았습니다.

## 한국어 표기 정비 (2026-10-22)

공개 포트폴리오의 Markdown 31개 중 **26개 문서**에서 제목·설명·표·도식의 자연어 표현을 한국어 중심으로 정리했습니다. 원본을 그대로 복사한 4개 Snapshot과 이미 한국어 중심으로 작성된 Gateway 배포 참고본 1개는 번역 대상으로 변경하지 않았습니다. 이번 작업은 언어·가독성 정비이며, 제품 구현·검증 상태나 역사적 Evidence를 새로 승인하는 작업이 아닙니다.

공개 포트폴리오의 설명과 제목은 한국어 중심으로 정리합니다. `Creator / AI Agent / Programmer`, 제품·시스템명, API·경로, `PASS`/`NOT_TESTED` 등 기계적 상태값은 원형을 유지할 수 있습니다. **원본 그대로 공개한 네 문서는 번역하지 않고 SHA 일치 상태를 보존합니다.** 번역은 새 구현이나 추가 검증을 의미하지 않습니다.

## 2026-10-23 공개 저장소 구조 정리

이 절은 위 2026-10-22 원본 실사를 변경하지 않고, **공개 포트폴리오의 문서 분류 작업**만 기록합니다.

- 비공개 ProjectSF의 확인된 main은 `08715c68b04f`로 당시 실사와 같습니다.
- `Samples/`의 제작 산출물 예시는 `Docs/CreatorWorkflow.md`의 제작 사례 구성과 중복되어 별도 카테고리를 폐지했습니다.
- `CaseStudies/RecipeProduction.md`, `CaseStudies/UIProduction.md`는 실제 완료 사례보다 작성/제작 기준에 가까워 `Docs/RecipeProduction.md`, `Docs/UIProduction.md`로 정리했습니다.
- `CaseStudies/DesignerProduction.md`의 이전 수계 예시는 월드·Agent 제작 사례와 중복되어 `CaseStudies/WorldProduction.md`로 핵심 기준을 합쳤습니다.
- `ServerProduction.md`는 원본 서버 아키텍처·클라이언트 로그인·배포 문서를 대조해 로컬 구현과 미검증 운영 범위를 사례별로 분리했습니다.
- README는 Agent Workflow / RPCNet / 서버를 중심으로 축약하고 기타 자료는 보조 자료로 정리했습니다.
- 코드·Unreal Editor·실서버 및 회귀 테스트를 새로 실행하지 않았습니다. 이 정리는 새로운 기능 완료 또는 검증 판정이 아닙니다.


## 2026-10-23 Team Hub 사례 및 미완료 항목 정리

- ProjectSF `main` `08715c68b04f`를 다시 조회했고 기준 커밋의 추가 변경은 없음을 확인했습니다.
- `Docs/Development/AgentWorkflow/SFTeamHubAgentAPI.md`, `CommonAIWorkflow.md`, `sf-page-tools/SKILL.md`, `discussion-convergence/SKILL.md`와 공개 GitHub Issue #74·#92·#87·#93의 당시 기록을 대조했습니다.
- `CaseStudies/TeamHubProduction.md`에는 Team Hub의 **SPEC-74 R3 CURRENT 발행**과 당시 구현·검증·제작자 수락 기록을 추가했습니다. 배포·검증 수치는 2026-10-19~21 기록이며 이번 문서 편집에서 재실행하지 않았습니다.
- 프로젝트에서 Issue 제작이 폐기된 것은 아니며, Spec 제작과 직접 요청은 병행합니다. #87의 설계 개선과 #93의 개인 플러그인 배포는 미완료로 분리했습니다.
- `AgentDrivenGameProduction.md`의 **여주인공 #29(미완료 제작 계약)** 언급은 결과 사례에서 제거했습니다. 기본 몬스터 #28은 기술적 검증 기록이 있는 범위만 남기고 전체 작업 완료를 주장하지 않습니다.
- 비공개 Spec 본문·인증정보·개인 식별이 가능한 내부 증거는 복사하지 않았습니다. Team Hub 사이트의 현재 화면·앱별 동작은 이번 갱신에서 직접 검증하지 않았습니다.
