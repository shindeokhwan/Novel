---
name: auto-agents
description: 챕터 집필/수정 파이프라인 오케스트레이션. Mode A(초고 Sub-agents 7개), Mode B(수정 Agent Teams). 파이프라인, 집필 시작, 수정 파이프라인 시 사용.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task
---

# Auto-Agents - 챕터 집필/수정 오케스트레이션 스킬

검증된 에이전트 패턴을 통합한 재사용 워크플로우.
두 가지 모드를 제공: **Mode A (Draft)** = 초고 집필, **Mode B (Revision)** = 수정/피드백 반영.

## Trigger
- "파이프라인", "집필 시작", "수정 파이프라인", "auto-agents", "챕터 집필", "수정 시작"

## Mode Selection

사용자에게 모드 확인:
- **Mode A (Draft)**: 새 챕터 초고 집필 — Sub-agents 8개
- **Mode B (Revision)**: 기존 챕터 수정/피드백 반영 — Agent Teams + Sub-agents

---

## Mode A: Draft (초고 집필)

Sub-agents 7개를 활용한 7단계 파이프라인.

### Phase 1: Context Loading [Lead]

`scene-writer` Phase 1 (Context Loading) + Phase 2 (Pre-Write Validation) 전체를 실행한다.

1. `00-핵심관리/현재작업상태.md` 읽기
2. 이전 2-3 에피소드 원고 읽기 (문맥 연속성)
3. 등장 캐릭터 파일 + 감정추적 파일 로드
4. `03-플롯/메인플롯-상세/PartN.md` 해당 챕터 상세 확인
5. `03-플롯/서사줄기추적.md` 최근 5챕터 균형 확인
6. `03-플롯/복선추적.md` — 복선 확인 + `03-플롯/쾌감카운터.md` (E패턴 연속/저사용) 확인
7. 직전 2ep 패턴 유형 확인 — 챕터 성격(포석/돌파/여운/인간/전환) 3연속 방지
8. `01-세계관/타임라인.md`, `01-세계관/역사배경.md` 확인
9. `reference/entertainment-density.md` — 쾌감 전략, 축적/해소 위치, 엔딩 방식(P4) 확인
10. **능력 업그레이드 빈도 점검** (`reference/ability-upgrade-current.md`) — 직전 소 업그레이드 이후 챕터 수 확인. 4챕터 초과 시 이번 챕터에 소 업그레이드 필수 배치. 트랙(AT1-AT4) 균형 확인
11. 바둑-전략 대응 현황 확인 (현재 포석/중반전/끝내기 단계)
12. **전투씬 포함 여부 확인** — 포함 시 기풍 선언 준비 (`reference/style-guide.md` 바둑 기풍 전투 시스템 참조). 규모별(소/중/대) 적용 방식 결정
13. **챕터 성격 유형 선언** (`reference/plot-structure.md` P1 참조) — 포석형/돌파형/여운형/인간형/전환형 중 하나 명시적 선택
14. **씬 클라이맥스 위치 선언** (`reference/plot-structure.md` P2 참조) — 전면배치/중간배치/후면배치 중 하나 선택
15. **엔딩 방식 확인** (`reference/entertainment-density.md` P4 참조) — 직전 챕터 엔딩 방식 확인, 동일 방식 금지

### Phase 2: Outline Creation [Lead]

`scene-writer` Phase 3 (Outline Creation)을 실행한다.

1. `11-템플릿/챕터아웃라인템플릿.md` 로드
2. Phase 1 컨텍스트 기반 아웃라인 작성
3. 저장: `04-원고/[권]/챕터/ch-[NNN]_outline.md`
4. 포함 항목:
   - 챕터 개요 (핵심 사건, 변화, 독자 경험)
   - 씬별 상세 (POV, 장소, 시간, 갈등, 목표, 톤, 비트)
   - 감각 묘사 계획 (POV 심리 상태 기반)
   - 핵심 대사 + 서브텍스트
   - 복선 설치/힌트/회수 계획
   - 행위소 모델, 감정 변화 요약, 쾌감 기록
   - 챕터 성격 유형, 씬 클라이맥스 위치, 엔딩 방식 명시
   - 능력 업그레이드 배치 여부 + 트랙/레퍼토리 코드
   - 전투씬 포함 시: 기풍 선언, 규모, 적장 기풍 분석
5. **사용자 확인 후** Phase 3 진행

### Phase 3: Parallel Writing [Sub-agents x3]

3개 에피소드를 Sub-agent로 병렬 집필.

**에이전트 구성**:
- `writer-ep-1`: 에피소드 1 (씬 1) 집필
- `writer-ep-2`: 에피소드 2 (씬 2) 집필
- `writer-ep-3`: 에피소드 3 (씬 3) 집필

**각 에이전트에게 전달**:
- 아웃라인에서 해당 에피소드 부분
- Phase 1에서 로드한 캐릭터 파일, 감정 상태
- 직전 에피소드 마지막 문단 (이음새용)
- 해당 에피소드의 챕터 성격 유형, 쾌감 패턴
- `core-rules.md` 자동 주입됨. 전투씬 포함 시 `reference/style-guide.md` 참조 지시

**에이전트 지시**:
- `scene-writer` Phase 4 (Writing) 비트 구조 준수
- 마크다운 형식 금지, 순수 산문
- 에피소드 종결 방식 = 아웃라인에 지정된 유형
- 글자수 목표: ~3,300자 (챕터 ~9,900자의 1/3)
- 전투씬 포함 시: 기풍 서술 원칙 준수, 개인 전투 최소 5교환
- 시대착오어 11종 절대 사용 금지
- SVO 단문 연쇄 금지, 분리호칭 패턴 금지

### Phase 4: Assembly [Lead]

3개 에피소드 결과물을 하나의 챕터 파일로 조립.

1. 에피소드 간 이음새 편집 (톤/시제/위치 연속성)
2. 중복 표현, 반복 비유 크로스체크
3. 씬 전환 마커 (`* * *`) 삽입
4. 전체 읽기 흐름 확인
5. 저장: `04-원고/[권]/챕터/ch-[NNN]_v1_초고.md`

### Phase 5: Parallel Review [Sub-agents x2]

2개 리뷰 에이전트 병렬 실행.

**에이전트 구성**:

#### R1: Humanization Review (인간화 리뷰)
`humanization-review` 스킬의 Phase 2-4 실행.
- 대상: 조립 완료된 챕터 전문
- 점수 산출 (0-100) + P1/P2 수정 사항 목록
- 시대착오어 11종 전수검사
- SVO 단문 연쇄, 분리호칭 패턴, em-dash 남용 검사
- **7ch 초과 시 분할 필수** (교훈: "Prompt too long")

#### R2: Consistency + Narrative Structure (일관성 + 서사구조 통합 검사)
`consistency-check` 스킬의 Phase 2-6 + `story-architect` 에이전트 역할 통합.
참조: `reference/plot-structure.md`, `reference/entertainment-density.md`, `reference/world-building.md`
- 대상: 조립 완료된 챕터 + 관련 설정 파일
- 점수 산출 (0-100) + Critical/Major/Minor 분류
- **일관성 검사 항목**:
  - 복선 상태 감사
  - 역사 정합성 + 무예 등급 정합성 검증
  - 바둑-전략 매핑 정확도 검증
- **서사구조 검사 항목**:
  - 쾌감 패턴 배치 적절성 (E패턴 연속 2회 금지, 저사용 패턴 점검)
  - 서사줄기 비율 확인 (천하/인간/정체성)
  - 챕터 성격 유형 정합성 검증
  - 페이싱 평가
- **전투씬 포함 시**: 기풍 선언 존재 여부, 무예 등급 정합성, 개인 전투 교환 수, 살상 묘사 원칙 준수
- **능력 업그레이드 포함 시**: 4요소 체크 (Before/After, 자기 인식, 타인 반응, 구체적 결과)

### Phase 6: 수정 + 퇴고 (1차수정) [Lead]

2개 리뷰 결과를 종합하여 수정 + 퇴고 체크리스트를 **한 패스로** 실행한다.

**리뷰 기반 수정**:
1. P1 (Critical) 이슈 즉시 수정
2. P2 (Major) 이슈 검토 후 수정
3. 수정 유형별 처리:
   - 시대착오어 → 대체 표현 적용
   - 반복 표현 / AI 지문 → 대안 적용
   - SVO 단문 연쇄 → SF01-SF05 규칙 적용
   - 분리호칭 패턴 → 합치기/행동 비트/동작 전환
   - em-dash 초과 → 대체 수단 분산
   - 일관성 오류 → 설정 파일 대조 후 수정
   - 서사 구조 → 패턴 유형 조정, 쾌감 위치 이동
   - 전투씬 → 기풍 서술/교환 수/살상 묘사 보완

**퇴고 체크리스트** (동시 적용):
4. em-dash(—) 챕터당 5회 이하 확인
5. 메타 서술 라벨 + 독자 설명 라벨 + 중복 설명 제거
6. 에피소드 종결 패턴 — 직전 챕터와 동일 방식 금지
7. POV 일관성 + 대화 노이즈 최소 3회 확인
8. 능력 업그레이드 점검 — 배치 후 `reference/ability-upgrade-current.md` 기록 형식으로 기재
9. **전투씬 퇴고** — 대인(이름/교환/결말/반응) + 집단(승리 논리/교차 편집/포위)
10. **문장 단편화 점검** — SF01-SF05 위반 없는가
11. **분량 점검** — 챕터 총 글자 수 7,000자 이상, 씬별 2,000자 이상
12. 수정 건수 기록 (P1 N건, P2 N건)
13. 퇴고 완료 후 저장: `ch-[NNN]_v2_1차수정.md`

### Phase 7: Parallel File Updates [Sub-agents x2]

추적 파일 업데이트를 2개 에이전트로 분리 (파일 소유권 충돌 방지).

#### Updater-A: 서사/플롯 추적
**소유 파일**: 복선추적, 서사줄기추적, 플롯홀, 메인플롯-상세/PartN
- `03-플롯/복선추적.md` — 신규 복선 상태 업데이트
- `03-플롯/서사줄기추적.md` — 줄기 비율 + 챕터 성격 유형 기록
- `03-플롯/플롯홀.md` — 리뷰에서 발견된 이슈
- `03-플롯/쾌감카운터.md` — E패턴 카운터 업데이트

#### Updater-B: 상태/감정 추적
**소유 파일**: 현재작업상태, 진행상황, 타임라인, 감정추적, ability-upgrade-current.md
- `00-핵심관리/현재작업상태.md` 업데이트
- `00-핵심관리/진행상황.md` 업데이트
- `05-심리추적/감정추적/` 감정 변화 기록
- `01-세계관/타임라인.md` 업데이트
- `reference/ability-upgrade-current.md` — 능력 업그레이드 기록 추가 (트랙/규모/레퍼토리/4요소 체크)

### Phase 8: Verification [Lead]

최종 검증 및 보고.

1. 챕터 파일 존재 + 글자수 확인 (9,000~12,000자, 최소 7,000자)
2. 추적 파일 업데이트 확인 (양쪽 Updater 결과)
3. 리뷰 점수 종합 보고:
   - R1 인간화: X/100 (시대착오어 N건)
   - R2 일관성+서사구조: X/100, P1 N건
4. 능력 업그레이드 기록 확인 (트랙/규모/레퍼토리 코드)
5. 다음 챕터 주요 컨텍스트 요약

---

## Mode B: Revision (수정/피드백 반영)

Agent Teams + Sub-agents 하이브리드.

### 규모 판단

| 규모 | 챕터 수 | Teammates | Sub-agents | 총 에이전트 | 기준 |
|------|---------|-----------|------------|------------|------|
| Small | 1-3ch | 2 | 2 | 4 | 조기 진단, 단일 챕터 수정 |
| Medium | 4-7ch | 2 | 3-5 | 5-7 | 반 Part 수정 |
| Large | 8-15ch | 4 | 5-9 | 9-13 | Full Part 수정 |

### Agent Teams Fallback

Agent Teams 기능이 비활성(`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` 미설정)인 경우:
- Teammates를 Sub-agents로 대체
- 파일 소유권 분리는 프롬프트 내 명시적 지시로 유지
- 순차 실행으로 전환 (병렬성 손실 감수)

### Phase 0: Context Prep [Lead]

1. 수정 대상 범위 확인 (챕터, 피드백 출처)
2. 피드백 분류: P0 (즉시) / P1 (단기) / P2 (중기)
3. 규모 판단 → Teammate 수 결정
4. Team 생성 (`TeamCreate`)
5. Task 목록 생성 (`TaskCreate`) — 파일 소유권 기반 분배

**피드백 소스별 처리**:
- 재미평가 리포트 → 피드백 항목별 대상 챕터/에피소드 매핑
- 인간화 리뷰 → P1/P2 수정 사항 직접 활용
- 사용자 직접 피드백 → 챕터별 분류

### Phase 1: Parallel Editing [Teammates]

#### Small (2 Teammates)
- `tracker`: 추적 파일 전담 (복선추적, 서사줄기추적, 카운터)
- `editor`: 원고 수정 전담 (전체 챕터)

#### Medium (2 Teammates)
- `editor-early`: 전반 챕터 원고 수정
- `editor-late`: 후반 챕터 원고 수정

#### Large (4 Teammates)
- `editor-a`: 챕터 그룹 A
- `editor-b`: 챕터 그룹 B
- `editor-c`: 챕터 그룹 C
- `editor-d`: 챕터 그룹 D

**파일 소유권 규칙** (충돌 방지):
- 각 Teammate는 자신의 소유 파일만 수정
- 원고 파일은 챕터 범위로 분리
- 추적 파일은 Small에서 tracker 전담, Medium/Large에서 Lead 전담

**Teammate 사망 대응 프로토콜**:
- Teammate 응답 없음 시 → respawn으로 동일 이름 재생성
- 미완료 Task를 새 Teammate에게 재할당
- 완료된 Task는 보존

### Phase 2: Parallel Review [Sub-agents]

수정 완료 후 품질 검증.

#### R1: Humanization Review
- `humanization-review` 스킬 실행
- **7ch 초과 시 반드시 분할**: R1a (전반) + R1b (후반)
- 점수 산출 + P1/P2 목록
- 시대착오어 전수검사

#### R2: Consistency + Narrative Structure (일관성 + 서사구조 통합 검사)
- `consistency-check` 스킬 + `story-architect` 에이전트 역할 통합
- 수정으로 인한 신규 불일치 집중 검사
- 구조 변경 건수 추적 (복선 줄기 보존 확인)
- 역사 정합성 + 무예 등급 + 쾌감 패턴 + 서사줄기 재검증

#### R3: Entertainment Review (선택 — Large 규모만)
- `entertainment-review` 스킬 실행
- 수정 전/후 점수 비교 (R2 통합 검사와 별개의 상세 재미평가)

### Phase 3: Follow-up Fixes [Sub-agents]

리뷰에서 발견된 추가 수정 사항 처리.

- `fixer-f1`: P1 이슈 수정 (범위 = R1/R2의 Critical/Major)
- `fixer-f2`: P2 이슈 수정 (범위 = R1/R2의 Minor + 인간화 세부)

**범위 분리 규칙**:
- F1과 F2가 같은 파일의 같은 에피소드를 수정하지 않도록 분배
- 겹치는 경우 F1 우선, F2는 해당 에피소드 건너뜀

### Phase 3.5: 퇴고 (수정본) [Lead]

Mode A Phase 6의 퇴고 체크리스트 부분과 동일하게 적용.
수정 범위 내 모든 챕터에 대해 실행.

### Phase 4: File Updates + Verify [Lead]

1. 추적 파일 업데이트 (Small의 tracker가 처리하지 않은 경우):
   - `03-플롯/쾌감카운터.md` E패턴 카운터
   - `03-플롯/서사줄기추적.md` 챕터 성격 유형
   - `00-핵심관리/현재작업상태.md`
   - `00-핵심관리/진행상황.md`
   - `reference/ability-upgrade-current.md` — 능력 업그레이드 변경 시 기록 업데이트
2. 버전 관리:
   - 수정본 저장: `ch-[NNN]_v[N+1]_[수정단계].md`
   - 원본(v[N]) 보존
3. 검증:
   - 수정 건수 총계 (인간화 N건, 구조 N건, 기타 N건)
   - 리뷰 점수: R1 X/100, R2 X/100 P1 N건
   - 글자수 확인 (9,000~12,000자, 최소 7,000자)
4. Team 셧다운 (`SendMessage` type: shutdown_request)
5. 결과 보고

---

## Lessons Learned (검증된 교훈)

프로젝트 진행 중 축적된 핵심 교훈. 각 Phase에서 자동 적용.

| # | 교훈 | 출처 | 적용 위치 |
|---|------|------|----------|
| L1 | 인간화 리뷰 7ch 초과 시 분할 필수 | R1 "Prompt too long" | Phase 5 R1, Phase 2 R1 |
| L2 | 파일 소유권 완전 분리 → 충돌 0건 | tracker/editor 분리 | Phase 7, Phase 1 |
| L3 | Teammate 사망 시 respawn 대체 | 수정 파이프라인 교훈 | Phase 1 |
| L4 | 시대착오어 전수검사 필수 — 11종 외래어 잔존율 높음 | Ch.1-54 전수검사 | Phase 5 R1, Phase 6 |
| L5 | 조기 진단(3ch)이 Full Part보다 효율적 | 조기 진단 경험 | Mode B 규모 판단 |
| L6 | Teammate 작업 검증 필수 (미적용 누락 방지) | 수정 검증 교훈 | Phase 4 Lead 검증 |
| L7 | 이전 에피소드 원고 필독 (톤/문맥 연속성) | 전체 집필 과정 | Phase 1 |
| L8 | 전투씬은 기풍 선언 + 교환 수 + 살상 묘사 3요소 필수 | 전투씬 리터칭 교훈 | Phase 3, Phase 5 R2 |
| L9 | SVO 단문 연쇄는 가장 빈번한 AI 지문 — SF01-SF05 즉시 적용 | Ch.040 리터칭 | Phase 6 |
| L10 | 능력 업그레이드 4요소 체크 누락 시 독자 체감 급감 | ability-upgrade-current.md | Phase 5 R2, Phase 7 |

## Skill Dependencies

이 스킬이 참조하는 다른 스킬/에이전트:

| 참조 대상 | 용도 | 참조 위치 |
|----------|------|----------|
| `scene-writer` | Phase 1-4 컨텍스트/아웃라인/집필 로직 | Mode A Phase 1-4 |
| `humanization-review` | R1 인간화 점수 산출 | Mode A Phase 5, Mode B Phase 2 |
| `consistency-check` | R2 일관성+서사구조 통합 검사 | Mode A Phase 5, Mode B Phase 2 |
| `entertainment-review` | 상세 재미평가 (Large 수정만) | Mode B Phase 2 R3 |

## Output

### Mode A
- 완성된 챕터 퇴고본 (`ch-[NNN]_v2_1차수정.md`)
- 챕터 아웃라인 (`ch-[NNN]_outline.md`)
- 리뷰 점수 2종 (R1 인간화 / R2 일관성+서사구조)
- 업데이트된 추적 파일 목록
- 능력 업그레이드 기록 (해당 시)
- 다음 챕터 컨텍스트 요약

### Mode B
- 수정된 챕터 파일 (`ch-[NNN]_v[N+1]_[수정단계].md`)
- 수정 건수 총계 + 분류
- 리뷰 점수 (인간화/일관성, 선택적 재미평가)
- 업데이트된 추적 파일 목록

## Notes
- Mode A의 Phase 3 병렬 집필은 컨텍스트가 충분할 때만 유효. 컨텍스트 부족 시 순차 집필로 전환.
- Mode B의 Team 생성은 Agent Teams 활성 시에만. 비활성이면 Sub-agent Fallback.
- 글자수 부족 P1 발생 시 Phase 6에서 해당 에피소드 확장 (밀도 있는 확장만 — 내면 묘사/바둑 전략/대화 비트/감각 묘사).
- 각 Phase 완료 시 사용자에게 진행 상황 보고.
- Mode A 완료 시 Phase 6에서 수정+퇴고까지 자동 진행. 초고만 저장하고 끝내지 않는다.
