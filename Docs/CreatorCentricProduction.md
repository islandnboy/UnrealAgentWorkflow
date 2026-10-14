# Creator-Centric Production Engineering

> Portfolio architecture and operating model. Applied in ProjectSF; multi-project expansion is a long-term goal.

## Core principle

**Creator owns the design and outcome. AI Agent implements and performs baseline regression validation. Programmer reviews engineering quality across completed work and improves the production workflow.**

게임 디자인을 실행 가능한 시스템 설계로 다루고, Creator가 구현 담당자의 작업을 기다리지 않고 플레이 가능한 결과물을 직접 제작·검수하도록 한다. Creator는 기획자만이 아니라 목표를 정의하고 결과를 책임지는 모든 제작자를 의미한다.

## Responsibility model

| Role | Responsibility | Completion boundary |
| --- | --- | --- |
| **Creator** | Goal, game/system design, Scope, Constraints, Acceptance, playtest and product judgment | 실제 게임 경험이 의도를 충족하는지 판단 |
| **AI Agent** | Repository inspection, Skill/Rule/Recipe resolution, implementation, automated checks, regression verification, correction and evidence | 기술 검증 결과와 근거를 기록; 제품 승인을 대신하지 않음 |
| **Programmer** | Cross-issue architecture review, structural and functional quality assessment, refactoring, recurring failure analysis, Workflow/Skill/Guideline improvement | 반복 가능한 품질 개선과 회귀 검증 |

Programmer는 모든 개별 변경의 동기식 승인자가 아니다. 다만 보안, 데이터 손상, Authority, 공용 API 파괴 등 높은 위험의 변경에는 기존 사전 Gate를 유지한다.

## Production lifecycle

```text
Creator Intent / Engineering Design
  → Issue (Goal / Scope / Requirements / Acceptance)
  → Agent Context Inspection + Rule / Skill / Recipe
  → Agent Implementation / Tool Execution
  → Automated Tests / Regression / Evidence
  → Creator Playtest / Acceptance
  → Issue Closure
  → Programmer Cross-Issue Review
  → Refactoring / Workflow / Skill / Guideline Improvement
  → Regression of the improved production process
  → Next Creator Production
```

이 흐름은 단일 Issue 완료와 횡단 기술 품질 관리를 분리한다. 여러 Issue가 각각 PASS여도 중복 추상화, 책임 경계 침범, 공통 API 불일치가 생길 수 있으므로 완료된 작업들을 묶어 검토한다.

## Current ProjectSF practice

- Creator가 시스템 설계와 게임 내 완료 기준을 정의한다.
- Agent가 구현하고 기본적인 회귀 검증을 수행한다.
- Creator가 실제 플레이 결과를 검수하고 수정 방향을 정한다.
- Programmer가 닫힌 Issue의 변경 및 검증 결과를 종합해 구조적·기능적 문제를 검토하고 리팩터링한다.
- 발견한 반복 문제는 적합한 Workflow, Rule, Skill, Recipe, Tool 또는 Test 계층에 반영한다.

이 문서는 운영 모델을 설명한다. 정량적인 인력 절감률, 프로젝트 간 확장성 또는 완전한 무인 개발을 입증하는 자료로 해석하지 않는다. 개별 구현 증거와 회귀 결과는 각 Case Study의 Evidence로 별도 제시한다.

## Long-term direction

프로젝트별 전담 프로그래머 없이 Creator와 Agent가 직접 게임을 제작할 수 있도록 공통 제작 플랫폼을 발전시킨다. Programmer는 Agent Workflow 고도화, 구조적 품질 관리, 플랫폼 빌드·최적화·출시 기술지원에 집중한다.

이는 프로그래밍 자체의 제거가 아니라 **구현 책임과 엔지니어링 품질 책임의 재배치**다.

## Portfolio evaluation

- Creator가 다른 구현 직군의 대기 없이 결과물을 제작·검증할 수 있는가?
- 기본 회귀 검증과 Evidence가 이슈마다 재현 가능한가?
- 완료된 Issue의 횡단 리뷰에서 발견한 문제가 공정 개선으로 환원되는가?
- 공정 개선 후 동일 유형의 문제가 감소하는가?
- 프로젝트가 늘어날 때 Programmer의 개별 구현 개입이 비례해 증가하지 않는가?

마지막 두 항목은 장기 측정 및 검증 목표다.

## Related documents

- [Creator Workflow](CreatorWorkflow.md)
- [Production Model](ProductionModel.md)
- [Architecture](Architecture.md)
- [Execution Model](ExecutionModel.md)
