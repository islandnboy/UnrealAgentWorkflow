# Creator 중심 제작과 엔지니어링 품질 관리

> ProjectSF에 적용된 제작 구조와 운영 모델을 설명합니다. 다른 프로젝트로 확장하는 것은 장기 목표이며, 이미 입증된 성과로 주장하지 않습니다.

## 핵심 원칙

**Creator는 설계와 결과를 책임진다. AI Agent는 구현과 기본 회귀 검증을 수행한다. Programmer는 완료된 작업을 종합해 엔지니어링 품질을 점검하고 제작 공정을 개선한다.**

게임 디자인을 실행 가능한 시스템 설계로 다루고, Creator가 구현 담당자의 작업을 기다리지 않고 플레이 가능한 결과물을 직접 제작·검수하도록 한다. Creator는 기획자만이 아니라 목표를 정의하고 결과를 책임지는 모든 제작자를 의미한다.

## 역할과 책임

| 역할 | 담당 책임 | 완료 판단 기준 |
| --- | --- | --- |
| **Creator** | 목표·게임/시스템 설계·작업 범위·제약·수용 기준 정의, 실제 플레이와 제품 품질 판단 | 실제 게임 경험이 의도를 충족하는지 판단 |
| **AI Agent** | 저장소 조사, 필요한 Skill/Rule/Recipe 선택, 구현, 자동 검사·회귀 검증·수정 및 근거 기록 | 기술 검증 결과와 근거를 기록; 제품 승인을 대신하지 않음 |
| **Programmer** | 완료된 이슈들을 묶어 아키텍처·구조·기능 품질 검토, 반복 결함 분석, 리팩터링과 Workflow/Skill/Guideline 개선 | 반복 가능한 품질 개선과 회귀 검증 |

Programmer는 모든 개별 변경의 동기식 승인자가 아니다. 다만 보안, 데이터 손상, Authority, 공용 API 파괴 등 높은 위험의 변경에는 기존 사전 Gate를 유지한다.

## 제작과 품질 개선의 순환

```text
Creator 의도 / 시스템 설계
  → Issue 또는 Spec / 직접 작업 (목표·범위·요구사항·수용 기준)
  → AI Agent의 현재 상태 확인 및 필요한 Rule / Skill / Recipe 선택
  → AI Agent 구현 / 도구 실행
  → 자동 테스트 / 회귀 검증 / Evidence
  → Creator 플레이 검수 / 수용
  → Issue 종료 (Issue 경로인 경우)
  → Programmer의 완료 이슈 종합 검토
  → 리팩터링 / Workflow·Skill·Guideline 개선
  → 개선된 제작 공정의 회귀 검증
  → 다음 Creator 제작
```

이 흐름은 단일 Issue 완료와 횡단 기술 품질 관리를 분리한다. 여러 Issue가 각각 PASS여도 중복 추상화, 책임 경계 침범, 공통 API 불일치가 생길 수 있으므로 완료된 작업들을 묶어 검토한다.

## ProjectSF의 현재 운영 방식

- Creator가 시스템 설계와 게임 내 완료 기준을 정의한다.
- Agent가 구현하고 기본적인 회귀 검증을 수행한다.
- Creator가 실제 플레이 결과를 검수하고 수정 방향을 정한다.
- Programmer가 닫힌 Issue의 변경 및 검증 결과를 종합해 구조적·기능적 문제를 검토하고 리팩터링한다.
- 발견한 반복 문제는 적합한 Workflow, Rule, Skill, Recipe, Tool 또는 Test 계층에 반영한다.

이 문서는 운영 모델을 설명한다. 정량적인 인력 절감률, 프로젝트 간 확장성 또는 완전한 무인 개발을 입증하는 자료로 해석하지 않는다. 개별 구현 증거와 회귀 결과는 각 Case Study의 Evidence로 별도 제시한다.

## 장기 방향

프로젝트별 전담 프로그래머 없이 Creator와 Agent가 직접 게임을 제작할 수 있도록 공통 제작 플랫폼을 발전시킨다. Programmer는 Agent Workflow 고도화, 구조적 품질 관리, 플랫폼 빌드·최적화·출시 기술지원에 집중한다.

이는 프로그래밍 자체의 제거가 아니라 **구현 책임과 엔지니어링 품질 책임의 재배치**다.

## 포트폴리오 평가 기준

- Creator가 다른 구현 직군의 대기 없이 결과물을 제작·검증할 수 있는가?
- 기본 회귀 검증과 Evidence가 이슈마다 재현 가능한가?
- 완료된 Issue의 횡단 리뷰에서 발견한 문제가 공정 개선으로 환원되는가?
- 공정 개선 후 동일 유형의 문제가 감소하는가?
- 프로젝트가 늘어날 때 Programmer의 개별 구현 개입이 비례해 증가하지 않는가?

마지막 두 항목은 장기 측정 및 검증 목표다.

## 관련 문서

- [Creator Workflow](CreatorWorkflow.md)
- [Production Model](ProductionModel.md)
- [Architecture](Architecture.md)
- [Execution Model](ExecutionModel.md)
