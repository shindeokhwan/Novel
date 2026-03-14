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

Sub-agents 7개를 활용한 8단계 파이프라인. (구 Phase 0.5는 Phase 1에 통합됨)

### Phase 1: Context Loading + Pre-Flight [Lead]

`scene-writer` Phase 1-2 실행 + 능력 업그레이드 사전 점검을 통합한다.

#### Pre-Flight (구 Phase 0.5 — 5줄 요약)
1. `reference/ability-upgrade-current.md` 마지막 20줄만 읽기 → 마지막 소(小) 업그레이드 챕터 확인
2. `(현재 챕터) - (마지막 소 업그레이드 챕터)` 계산: 4ch 초과=필수, 3-4ch=권장, 1-2ch=선택
3. 같은 트랙 3회 연속 여부 확인 → 해당 시 다른 트랙으로 강제 전환
4. 결과를 `UPGRADE_REQUIRED` / `RECOMMENDED_TRACK` / `RECOMMENDED_REPERTOIRE` 플래그로 Phase 2에 전달

#### 3-Tier 컨텍스트 로딩

**Tier 1: 항상 (~30KB)**
1. `00-핵심관리/현재작업상태.md` 읽기
2. 직전 에피소드 마지막 1,000자 읽기 (이음새용)
3. `reference/ability-upgrade-current.md` 마지막 20줄 (Pre-Flight에서 이미 로드됨 — 재읽기 금지)

**Tier 2: 캐릭터 (~20-40KB — 해당 챕터 등장 캐릭터만)**
4. 등장 캐릭터 파일 + 감정추적 파일 로드 (비등장 캐릭터 로드 금지)

**Tier 3: 플롯 (~15KB)**
5. `03-플롯/메인플롯-상세/PartN.md` 해당 챕터 섹션만
6. `03-플롯/서사줄기추적.md` 최근 5챕터만
7. `03-플롯/복선추적.md` active 복선만 + `03-플롯/쾌감카운터.md` (E패턴 연속/저사용)
8. 직전 2ep 패턴 유형 확인 — 챕터 성격 3연속 방지
9. `01-세계관/타임라인.md`, `01-세계관/역사배경.md` 확인
10. 바둑-전략 대응 현황 확인

#### 조건부 로드 (해당 시에만)
11. **전투씬 포함 시에만**: `reference/style-guide-battle.md`
12. **쾌감 규칙 전체가 필요한 경우만**: `reference/entertainment-density.md` — Part별 전략·저사용 현황. 핵심 규칙은 `core-rules.md` §12로 자동주입됨
13. **행위소/복선/바둑매핑 필요 시만**: `reference/plot-structure.md` — 챕터 성격·클라이맥스는 `core-rules.md` §13으로 자동주입됨

#### 선언 (매 챕터)
14. **챕터 성격 유형 선언** — 포석형/돌파형/여운형/인간형/전환형 중 하나
15. **씬 클라이맥스 위치 선언** — 전면배치/중간배치/후면배치 중 하나
16. **엔딩 방식 확인** — 직전 챕터와 동일 방식 금지

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

**각 에이전트에게 전달** (`reference/sub-agent-briefing-writer.md` 템플릿 사용):
- 템플릿의 `[PLACEHOLDER]`를 Phase 1-2에서 수집한 실제 값으로 대체하여 전달
- 아웃라인 해당 에피소드 부분, 캐릭터 상태 요약, 이음새, 챕터 메타 포함
- **`core-rules.md` 자동주입됨** — 별도 포함 불필요. 전투씬은 `style-guide-battle.md` Read 지시

**에이전트 지시**:
- `scene-writer` Phase 4 (Writing) 비트 구조 준수
- **reference/ 파일 추가 Read 금지** — `core-rules.md`가 프롬프트에 포함됨 + 아웃라인에 필요 정보 포함됨. 전투씬은 예외(`style-guide-battle.md` Read 허용)
- 마크다운 형식 금지, 순수 산문
- 에피소드 종결 방식 = 아웃라인에 지정된 유형
- 글자수 목표: ~3,300자 (챕터 ~9,900자의 1/3)
- 전투씬 포함 시: 기풍 서술 원칙 준수, 개인 전투 최소 5교환
- 시대착오어 11종 절대 사용 금지
- SVO 단문 연쇄 금지, 분리호칭 패턴 금지

### Phase 4: Assembly + Mechanical Scan [Lead]

3개 에피소드 결과물을 조립하고, Grep 기반 기계적 스캔을 실행한다.
기계적 스캔을 Phase 5(심층 리뷰) 이전에 처리하여 R1의 부담을 줄인다.

#### 4A: 조립
1. 에피소드 간 이음새 편집 (톤/시제/위치 연속성)
2. 중복 표현, 반복 비유 크로스체크
3. 씬 전환 마커 (`* * *`) 삽입
4. 전체 읽기 흐름 확인
5. 저장: `04-원고/[권]/챕터/ch-[NNN]_v1_초고.md`

#### 4B: 기계적 스캔 (Grep — 조립 직후 실행)
초고 파일에 대해 아래 패턴을 Grep으로 검출한다. 발견 시 **즉시 수정** (Phase 6 대기 없이).

| 스캔 | Grep 패턴 | 대응 |
|------|----------|------|
| 시대착오어 11종 | `타이밍\|리듬\|패턴\|시스템\|스타일\|카리스마\|퍼센트\|이미지\|레벨\|에너지\|포인트` | 대체 표현 즉시 적용 |
| 수치 시간 | `0\.\d+초\|\d+초간` | 체감 표현으로 교체 |
| em-dash 과다 | `—` (5회 초과 여부 카운트) | 초과분 대체 수단으로 교체 |
| 분리호칭 | `"\S+\." .+이\|가 말했다\. "` | 합치기/행동 비트로 교체 |
| "말했다" 단독 | `" .+이\|가 말했다\.` (수식어 없는 형태) | 태그 삭제/다른 동사/행동 비트 |

**원칙**: 4B에서 잡히는 것은 기계적으로 확정된 오류이므로 판단 없이 즉시 수정한다. Phase 5 R1은 4B에서 잡히지 않는 **심층 패턴** (대화 자연스러움, 구조 다양성, 감각 밀도, 갈등 깊이)에 집중한다.

### Phase 5: Parallel Review [Sub-agents x2~3]

2~3개 리뷰 에이전트 병렬 실행. Phase 4B에서 기계적 패턴은 이미 수정됨 — Phase 5는 **심층 분석**에 집중한다.

**에이전트 구성**:

#### R1: Humanization Depth Review (인간화 심층 리뷰)
`humanization-review` 스킬의 **Phase 3 (Depth Analysis)** 중심 실행.
Phase 4B 기계적 스캔 완료분은 **생략**, 심층 항목에 집중.
**`reference/sub-agent-briefing-reviewer.md`의 R1 섹션을 브리핑으로 사용.**
- 대상: Phase 4B 수정 완료된 초고
- **humanization-rules.md 별도 Read 불필요** — 핵심은 `core-rules.md` §9-11로 자동주입됨. 금지패턴 전체 목록 필요 시만 Read
- **심층 검사 항목**: 대화 자연스러움 / 갈등 해결 속도 / 구조 다양성 / 감각 다양성
- 점수 산출 (0-100) + P1/P2 수정 사항 목록
- **4B 잔존 확인**: 누락분 있으면 P1 추가 보고
- **7ch 초과 시 분할 필수** (L1)

#### R2: Consistency + Narrative Structure (일관성 + 서사구조 통합 검사)
`consistency-check` 스킬의 Phase 2-6 + `story-architect` 에이전트 역할 통합.
**`reference/sub-agent-briefing-reviewer.md`의 R2 섹션을 브리핑으로 사용.**
- **plot-structure.md, entertainment-density.md 별도 Read 불필요** — 핵심은 `core-rules.md` §12-13으로 자동주입됨
- **world-building.md는 역사/무예 검증 필요 시만 Read**
- Phase 1 컨텍스트 요약을 우선 사용. 직접 Read는 타임라인·역사배경에 한정
- 대상: 초고 + 관련 설정 파일
- 점수 산출 (0-100) + Critical/Major/Minor 분류
- **7ch 초과 시 분할 필수** (L12): R2a (일관성) + R2b (서사구조)로 분리. 각각 별도 Sub-agent로 실행
- **일관성 검사 항목** (R2a — 분할 시):
  - 복선 상태 감사
  - 역사 정합성 + 무예 등급 정합성 검증
  - 바둑-전략 매핑 정확도 검증
- **서사구조 검사 항목** (R2b — 분할 시):
  - 쾌감 패턴 배치 적절성 (E패턴 연속 2회 금지, 저사용 패턴 점검)
  - 서사줄기 비율 확인 (천하/인간/정체성)
  - 챕터 성격 유형 정합성 검증
  - 페이싱 평가
- **전투씬 포함 시**: 기풍 선언 존재 여부, 무예 등급 정합성, 개인 전투 교환 수, 살상 묘사 원칙 준수
- **능력 업그레이드 포함 시**: 4요소 체크 (Before/After, 자기 인식, 타인 반응, 구체적 결과)

#### R3: Entertainment Review (재미평가 — 선택적)
`entertainment-review` 스킬 실행. 3인 페르소나(일반독자/장르독자/편집자) 교차 리뷰.
- **실행 조건**: 아래 중 하나 이상 해당 시 실행
  - 소아크 마지막 챕터 (클라이맥스)
  - 대형 쾌감(E패턴) 배치 챕터
  - 사용자가 명시적으로 요청
  - 직전 2챕터의 R2 페이싱 평가가 "보통" 이하
- **미실행 시**: Phase 5는 R1 + R2만 병렬 (Sub-agents x2)
- 점수 산출 (0-100) + 페이싱/흡인력/카타르시스 평가
- R2의 서사구조 검사와 **겹치지 않는 영역**: 독자 체감, 페이지 터너 감각, 감정 이입도

### Phase 5.5: Review Conflict Resolution [Lead]

R1/R2(/R3) 결과를 종합할 때 **리뷰 간 충돌**이 있으면 이 Phase에서 해소한다.

#### 충돌 감지 기준
- R1이 "이 표현 유지"라고 하는데 R2가 "이 표현 수정" → 충돌
- R1이 종결 방식 A를 긍정하는데 R2가 서사줄기 비율 왜곡을 이유로 B를 요구 → 충돌
- R3(실행 시)가 페이싱 문제를 지적하는데 R2는 구조적으로 적합하다고 판단 → 충돌

#### 해소 프로토콜
1. **충돌 항목 목록화**: 각 리뷰의 근거를 나란히 정리 (최대 5건)
2. **우선순위 규칙 적용**:
   - 세계관/역사 정합성 (R2) > 인간화 (R1): 설정 오류는 문체보다 우선
   - 인간화 (R1) > 서사구조 미세 조정 (R2): AI 흔적 제거가 구조 최적화보다 우선
   - 독자 체감 (R3) > 서사구조 이론 (R2): 재미가 구조보다 우선 (R3 실행 시)
3. **해소 불가 항목**: 사용자에게 선택지 제시 (최대 2건). Phase 6에서 사용자 결정 반영
4. **충돌 0건**: Phase 5.5 자동 스킵 → Phase 6 직행

### Phase 6: 수정 + 퇴고 (1차수정) [Lead]

리뷰 결과(+ Phase 5.5 충돌 해소 결과)를 종합하여 수정 + 퇴고 체크리스트를 **한 패스로** 실행한다.
**레퍼런스 파일 재읽기 금지** — `core-rules.md` §9-13 + R1/R2 리뷰 결과가 충분. 폴백: R1 < 70 또는 R2 < 75이면 `humanization-rules.md` + `plot-structure.md` 풀 Read 후 리뷰 재실행.

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
13. **회귀 검출 (Regression Detection)**: 이번 챕터에서 수정한 패턴이 직전 10챕터에도 존재하는지 Grep 스캔
    - 대상: `ch-[N-10]` ~ `ch-[N-1]` 최신 버전 파일
    - 검출 항목: Phase 6에서 **3건 이상** 수정한 패턴 유형만 (예: 분리호칭 5건 수정 → 직전 10ch에서 분리호칭 Grep)
    - **출력**: `[회귀 경고] ch-XXX: 분리호칭 N건, SVO 단문 N건` — Phase 8 보고에 포함
    - **조치 없음**: 자동 수정하지 않음. 다음 Mode B 수정 범위에 포함할 후보로 기록만
14. 퇴고 완료 후 저장: `ch-[NNN]_v2_1차수정.md`

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
   - Phase 4B 기계적 스캔: 시대착오어 N건, em-dash N건, 분리호칭 N건 (즉시 수정 완료)
   - R1 인간화 심층: X/100
   - R2 일관성+서사구조: X/100, P1 N건
   - R3 재미평가: X/100 (실행 시)
   - Phase 5.5 충돌: N건 해소 / N건 사용자 판단
4. 능력 업그레이드 기록 확인 (트랙/규모/레퍼토리 코드)
5. **회귀 검출 보고**: 직전 10ch에서 발견된 잔존 패턴 요약 (해당 시). Mode B 수정 후보 목록으로 기록
6. 다음 챕터 주요 컨텍스트 요약

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

Agent Teams 기능이 비활성(`CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS` 미설정)인 경우, 아래 대체 프로토콜을 적용한다.

#### Teammate → Sub-agent 매핑

| 규모 | 원래 Teammates | 폴백 Sub-agents | 실행 방식 |
|------|---------------|-----------------|----------|
| Small | tracker + editor | sub-tracker + sub-editor | **병렬** (파일 소유권 분리됨) |
| Medium | editor-early + editor-late | sub-editor-early → sub-editor-late | **순차** (이음새 의존) |
| Large | editor-a~d | sub-editor-a → sub-editor-b → sub-editor-c → sub-editor-d | **2병렬 순차**: (a∥b) → (c∥d). 겹치지 않는 챕터 그룹끼리만 병렬 |

#### 파일 소유권 유지 방법
- 각 Sub-agent 프롬프트에 `소유 파일 목록`을 명시적으로 전달
- 금지 파일 패턴 포함: "아래 파일만 수정. 그 외 파일 수정 절대 금지: [목록]"
- Lead가 결과물 수신 후 소유권 위반 여부 확인 (Grep으로 변경 파일 검증)

#### 순차 실행 시 이음새 처리
- Medium/Large에서 앞 그룹 Sub-agent 완료 후, 마지막 챕터 끝 500자를 다음 그룹 Sub-agent에게 컨텍스트로 전달
- 이음새 편집은 Lead가 Phase 2 종료 후 일괄 처리

#### Teammate 사망 대응 → Sub-agent 재시도
- Sub-agent 실패(타임아웃/오류) 시 동일 프롬프트로 1회 재시도
- 2회 연속 실패 시 해당 챕터 범위를 Lead가 직접 처리
- 완료된 다른 Sub-agent 결과물은 보존

#### 기능 제한 사항
- `TaskCreate`/`TaskUpdate`/`SendMessage` 사용 불가 → Lead가 수동으로 진행 추적
- 실시간 Teammate 간 협업 불가 → 결과물 기반 순차 전달로 대체
- Team 셧다운 불필요 (Sub-agent는 완료 시 자동 종료)

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

## Lessons Learned → `skills/auto-agents/lessons-learned.md`

검증된 교훈 L1-L20. 필요 시 Read.

## Skill Dependencies

이 스킬이 참조하는 다른 스킬/에이전트:

| 참조 대상 | 용도 | 참조 위치 |
|----------|------|----------|
| `scene-writer` | Phase 1-4 컨텍스트/아웃라인/집필 로직 | Mode A Phase 1-4 |
| `humanization-review` | R1 인간화 심층 리뷰 (기계적 스캔은 Phase 4B) | Mode A Phase 5, Mode B Phase 2 |
| `consistency-check` | R2 일관성+서사구조 통합 검사 | Mode A Phase 5, Mode B Phase 2 |
| `entertainment-review` | R3 재미평가 (Mode A 선택적, Mode B Large) | Mode A Phase 5, Mode B Phase 2 |
| `reference/core-rules.md` | 자동주입 규칙 (§9-13 확장됨 — humanization/entertainment/plot 핵심 포함) | 전 Phase |
| `reference/sub-agent-briefing-writer.md` | Phase 3 집필 Sub-agent 브리핑 템플릿 | Mode A Phase 3 |
| `reference/sub-agent-briefing-reviewer.md` | Phase 5 리뷰 Sub-agent 브리핑 템플릿 | Mode A Phase 5 |

## Output

### Mode A
- 완성된 챕터 퇴고본 (`ch-[NNN]_v2_1차수정.md`)
- 챕터 아웃라인 (`ch-[NNN]_outline.md`)
- 리뷰 점수 2~3종 (R1 인간화 / R2 일관성+서사구조 / R3 재미평가(선택))
- Phase 5.5 충돌 해소 로그 (해당 시)
- 업데이트된 추적 파일 목록
- 능력 업그레이드 기록 (해당 시)
- 회귀 검출 보고 (직전 10ch 잔존 패턴 — 해당 시)
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
