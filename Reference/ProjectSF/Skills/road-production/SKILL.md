---
name: road-production
description: Produce and repair editable, regenerable road networks from approved road sources.
---

# Road Production

승인된 도로 원천과 현재 작업 조건을 해석해 Road Design Input을 만들고, 현재 Unreal 환경에서 사용할 수 있는 capability를 탐색해 적절한 표현을 선택·생성·검증한다.

## Plan

- 특정 원천 형식이나 표현 방식을 canonical contract로 고정하지 않는다.
- source identity와 연결 관계를 추적할 수 있어야 하며, 원천에 없는 의미나 연결을 임의로 발명하지 않는다.
- 생성 전에 목표 범위, 보호 대상, 편집 지점, 재생성 경계를 명확히 한다.
- 현재 단계의 형상 기준을 정한다. 변환 완료된 World Geometry Actor가 승인 입력이면 그 Actor의 polygon/boundary/centerline과 component 표현을 확인해 사용한다. 형상·좌표 결함이 관측되지 않는 한 원본 GIS 재파싱이나 변환 검증으로 돌아가지 않는다.
- 공용 ResourceProduction의 선택적 보충 조사를 적용한다. 기존 형상에서 도로망·교차/분기·수계 횡단을 해석하고, 도로명/등급·폭/차선 등 현재 설계에 부족한 의미만 조회한다. 지도/사진에서 추론한 값을 실측으로 기록하지 않으며, 보충 자료는 기존 Geometry 교체가 아니라 게임용 설계 판단의 근거로 연결한다. 실측이 필수가 아니면 authored 프리셋으로 제작하고 미확인 제원을 새 blocker로 만들지 않는다.

## Select representation

- 현재 Unreal 환경에서 사용할 수 있는 capability와 재사용 가능한 표현을 먼저 탐색한다.
- 입력 호환성, 편집 가능성, 생성 소유권, 재생성 범위와 검증 가능성을 기준으로 표현을 선택한다.
- 특정 Tool, graph, asset 또는 구현 방식을 Skill 계약에 고정하지 않는다.
- 도로 영역 polygon/boundary가 충분하면 그 영역을 보존하는 표면 생성을 먼저 검토한다. centerline + width 재구성은 필요한 제작 효과와 형상 손실을 비교해 선택하며 필수 중간 단계로 만들지 않는다. 표면과 PCG/spline 장식에 서로 다른 수단을 사용할 수 있다.

## Produce

- Road Design Input과 생성 출력을 분리한다.
- 공용 ResourceProduction의 데이터 중심 제작·현재 pass 기준을 적용한다. 도로 ID/이름·등급 프리셋·기존 영역 또는 경로(시작/끝과 필요한 중간점)·폭/차선과 예외 override를 입력으로 묶는다. 등급 기본값과 실측값을 구분하고 polygon에서 영역 표면을 만들 때 불필요한 축 추론을 추가하지 않는다. 기존 도구가 폭/차선 의도를 소비하지 못하면 구현된 효과만 보고한다.
- 승인 도로망은 목록/배치로 채우고 접합·횡단·부유 등 실제 예외를 묶어 수정한다. rough에서 명시되지 않은 토목 정밀도나 cm 단위 오차를 새 완료 조건으로 만들지 않는다. 폴리싱 항목을 남기되 이동을 막는 충돌·단절은 현재 pass에서 해결한다.
- 연결 관계와 설계 연속성을 유지하고, 부분 재생성이 다른 source-derived 결과나 수작업 결과를 덮어쓰지 않게 한다.
- 작업 범위에 필요한 도로 형태와 구조적 관계는 현재 입력과 설계 조건에서 결정한다.
- 첫 대표 구간을 실제 대상의 도로 결과로 제작하고, 형상·접지·연결 검증을 통과한 결과와 재사용 입력/규칙을 유지해 인접 구간으로 확장한다. Road Design Input은 기존 Geometry와 편집 파라미터로 충족할 수 있으며 새 설계 Actor를 항상 만들 필요는 없다.
- 격리 trial은 실제 제작 전에 해결해야 할 구체적 불확실성이 있을 때 선택한다. 확인할 질문, 합격/실패 기준과 실제 제작 복귀 지점을 정하고, trial 자체를 반복 제작의 기본 산출물로 삼지 않는다. 합격한 실제 도로 후보는 시험했다는 이유만으로 제거하지 않는다.

## Output and scale

- 편집·재생성 입력(Geometry, spline, 제작 파라미터/규칙)과 저장·runtime 출력을 구분하고 대응을 유지한다. Procedural Mesh, spline mesh, PCG라는 이름만으로 성능 적합성을 판정하지 않는다. 생성·편집 수단과 최종 저장 표현은 달라도 된다.
- 넓은 범위로 확장하기 전에 실제 대표 구역에서 로드된 Actor·컴포넌트·메시 섹션·재질/고유 메시 수와 geometry·collision 비용을 확인하고, 가능한 CPU/GPU·메모리·streaming 관측을 목표 플랫폼/작업 예산과 대조한다. 전체 저장 수와 동시에 로드된 수를 구분한다. 예산·측정이 없으면 UNKNOWN/NOT_TESTED로 기록하며 성능 PASS를 주장하지 않는다. 유효한 관측은 재사용하고 매 구간마다 전체 성능 검증을 반복하지 않는다.
- 관측과 부분 재생성·사람 편집 요구에 맞춰 Procedural Mesh 유지, Static Mesh 저장, 청크 병합 또는 반복 자산의 PCG/ISM 배치를 선택한다. 청크 범위는 streaming/culling·collision·편집 비용을 함께 고려하며 도시 전체를 한 메시로 합치지 않는다. 타일마다 고유 메시·graph를 만들고 인스턴스 하나만 배치하는 구조를 인스턴싱 최적화로 간주하지 않는다.
- 확정 출력에서 불필요한 trial/debug·중복 렌더링/충돌이 남는지 확인한다. 편집 원본을 보존하면서 필요에 따라 runtime 제외 또는 출력 교체를 수행한다. 출력 교체 시 형상·접지·연결·재질·필요한 충돌과 저장/부분 재생성을 확인한다. 출력 방식 조사는 실제 구간 제작을 대체하지 않으며, 미확인 성능은 승인된 국소 제작을 자동 차단하지 않는다. 관측된 예산 초과나 확장 위험은 해당 범위를 조정해 해결한다.

## Validate

- technical correctness, persistence, visual readability, regeneration safety, Owner acceptance를 구분한다.
- 생성 성공이나 일부 수치 검증만으로 전체 품질을 PASS하지 않는다.
- 대표 구간 검증과 전체 승인 범위 Acceptance를 구분한다.
- 표현 적합성은 기준 Geometry와 생성 도로의 면적·경계·hole·교차부를 같은 구간에서 비교해 판단한다. 필요한 허용 오차와 의도된 설계 차이를 기록하고, 형상 일치·접지·연결을 확인한 뒤 유지된 실제 결과에 저장/재생성 검증을 적용한다.

## Invariants

- source representation != road production contract
- terrain grounding != road design elevation
- technical validity != visual readability
- representation choice != source identity
- generated output != human-editable design input
- partial regeneration must preserve unaffected source-derived and manual results
