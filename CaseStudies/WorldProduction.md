# 월드 제작 사례 — 이전 GIS 프로토타입

> 이 문서는 2026-10-14 월드 방향 전환 **이전** NeoSeoul/GIS 기반 제작 기록입니다. 새로운 압축 선형 월드나 Houdini 제작 결과의 구현·검증 증거가 아닙니다.

## 목표와 결과 계약

서울 기반 월드의 지형·도로·수계를 데이터 입력으로 제작·부분 수정하고, 기존 결과 및 사람의 직접 편집을 보존하는 것이 당시 목표였습니다. Creator는 승인 범위, 보호할 지형·도로·수계, 실제 흐름/형상, 저장·재로드, 시각 품질과 검증 기준을 정의했습니다.

```text
승인된 GIS / Production 입력
→ terrain / landscape
→ road
→ water-terrain-preparation / water
→ world-structure / world-validation
→ 저장·재조회 / Evidence → Creator 검수
```

AI Agent는 필요한 Skill을 단계별로 조합하고 Unreal 도구로 실제 생성·수정을 수행합니다. Water Actor가 생성됐다는 사실만으로 전체 수계가 완료됐다고 판정하지 않습니다.

## 실제 구조와 편집 경계

| 구분 | 역할 |
| --- | --- |
| World Partition | 공간 로드·언로드와 Actor·Landscape 접근 |
| 제작 Region | GIS polygon과 지정 지역의 편집·보존 책임 |
| 도메인 | 지형 보정, 도로면·표시·보도, 물 Actor의 분리된 제작 책임 |
| 제작 청크·구간 | 안정적인 Feature identity, 배치·재생성·저장·재개 단위 |

행정구역/제작 청크를 World Partition Cell이나 Unreal Data Layer에 일대일로 대응시키지 않았습니다.

![공간 분할과 제작 소유권](AgentDrivenGameProduction/world-spatial-structure.png)

- **도로:** 도로면과 차선·보도의 표현 및 충돌을 구분하고, 구간 변경 시 기존 도로면과 주변 제작 결과를 보존했습니다.
- **지형:** Base 및 수계 굴착·호안 보정용 Landscape Edit Layer를 분리했습니다. 2026-10-07 저장 검증에서 **Edit Layer 8개, Landscape Component 128개**가 관측됐습니다.
- **수계:** 하상·호안 준비와 Native River/Lake 제작을 분리하고 수위·깊이·흐름·인접 지형·저장 상태를 검증했습니다.

![지형·수계·도로의 책임 레이어](AgentDrivenGameProduction/world-domain-layers.png)

## 당시 결과와 한계

강 구간 11개, 표본점 2,470개, 호수 17개와 호수 지점 545개 등이 2026-10-07 검증 기록에 있으며, 당시 일부 원본 Scene 파일의 변경 방지와 Native readback을 확인했습니다. 이 수치는 **당시 Snapshot**이며 신규 월드 상태가 아닙니다.

당시에도 전체 시각 품질, 자연스러운 호안 연결, 패키지 실행, 목표 하드웨어 성능, 사람이 직접 수정한 뒤 Agent가 재개하는 전 과정과 Creator의 최종 수용은 별도 미검증/후속 작업으로 구분했습니다.

실제 전·후 화면, 전투·MetaHuman 작업과 관련 Evidence의 범위는 [AI Agent 기반 제작 사례](AgentDrivenGameProduction.md)를 참고하세요.
