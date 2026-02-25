---
name: consistency-check
description: 전체 작품의 교차 검증. 역사, 지리, 무예, 바둑 전략, 복선의 종합 일관성 검사.
---

# Consistency Check - 일관성 검사 스킬

## Trigger
- "일관성 검사", "검증", "플롯 홀 체크"

## Workflow

### Phase 1: File Loading
1. `01-세계관/세계관규칙.md` 로드
2. `01-세계관/타임라인.md` 로드
3. `01-세계관/역사배경.md` 로드
4. `03-플롯/복선추적.md` 로드
5. `03-플롯/플롯홀.md` 기존 이슈 확인

### Phase 2: Timeline Validation
1. 원고 내 시간 참조 vs 타임라인 교차
2. 사건 순서 논리성
3. 이동 시간/거리 정합성
4. 계절/날씨 일관성

### Phase 3: Character Validation
1. 물리적 위치 연속성
2. 정보 범위 확인
3. 감정 연속성
4. 무예/기 수준 일관성

### Phase 4: Historical Validation
1. 실존 인물 위치/행동의 역사적 가능성
2. 역사 변경 사항의 파급효과 추적
3. 관직/작위 시기 적합성
4. 시대고증 (의식주, 기술)

### Phase 5: Military Validation
1. 병력 숫자 추적
2. 보급/군량 현실성
3. 전투 결과와 전력 대비 정합성
4. 진형/전술의 합리성

### Phase 6: Go Strategy Validation
1. 바둑 용어 사용의 정확성
2. 바둑→전략 매핑 일관성
3. 주인공 수읽기/대국관의 합리적 범위

### Phase 7: Foreshadowing Audit
1. 설치된 복선 현황
2. 회수 상태 확인
3. 미회수 복선 목록

### Output
```
# 일관성 검사 리포트 - [날짜]
## Critical / Major / Minor
## 복선 상태
## 일관성 점수: X/100
```
