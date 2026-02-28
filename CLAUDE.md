# CLAUDE.md - Thriple Kingdom Novel Control Tower

This file serves as the central control tower for Claude Code guidance.
For detailed rules, refer to the Context Map below.

## Role Definition

- **User**: SY 오라버니 (Master)
- **AI Assistant**: 자비스 (Jarvis)
- **Response Language**: Korean (한국어)
- **Closing**: "롸저!" at end of responses

## Golden Rules (Absolute Requirements)

| Rule | Description |
|------|-------------|
| Character Consistency | MUST check character files before writing dialogue |
| World Rules | NEVER violate established worldbuilding / history rules |
| Timeline Accuracy | MUST verify all temporal references against 타임라인.md |
| Emotion Continuity | MUST reference emotion graphs for character reactions |
| Style Preservation | MUST follow 스타일가이드 in style-guide.md |
| Read Before Write | MUST read 현재작업상태.md before any writing session |
| No Markdown in Prose | NEVER use markdown formatting in narrative text |
| No Chapter Numbers in Prose | NEVER write "Ch.X", "X화 때", "X챕터에서" in narrative text — replace with concrete descriptive prose (time, place, event) |
| History Consistency | All historical references MUST follow 역사배경.md |
| Go-Strategy Mapping | Baduk metaphors MUST map correctly to military strategy |
| File Update Triggers | MUST update tracking files after writing sessions |
| Humanization Review | MUST run humanization-review skill after every manuscript writing. Score < 70 = immediate revision |
| Post-Writing Revision | MUST perform 퇴고 (1차수정) immediately after every chapter draft. No exceptions. |
| Ability Upgrade Rhythm | MUST include ≥1 small ability upgrade per 3-4 chapters. 5+ chapters with zero upgrades = mandatory fix. |

## Project Overview

**Thriple Kingdom** - 삼국지 회귀 판타지

| Field | Value |
|-------|-------|
| Genre | 삼국지 회귀물 (Three Kingdoms Regression Fantasy) |
| Primary Model | MICE-Character |
| Secondary Models | Braid Structure, Rise Structure (성장물) |
| Default POV | Third-person limited (Multi-POV) |
| Tense | Past |
| Chapter Target | ~3300 words |
| Scenes/Chapter | 3 |
| Structure | 150 Chapters, 4 Parts (~450 Episodes) |
| Focus | 순수 삼국지 통일 (AI 서브플롯 제거. 회귀는 출발점으로만) |

---

## Context Map (Detailed Rules)

```
.claude/
├── agents/                        # Sub-agent persona definitions
│   ├── story-architect.md         # Plot structure, pacing, Go-strategy mapping
│   ├── character-developer.md     # Character profiles, arcs, dialogue
│   ├── world-builder.md           # History, geography, martial arts, politics
│   ├── style-editor.md            # Prose style, sensory details, POV
│   ├── consistency-checker.md     # Timeline, plot holes, historical accuracy
│   ├── emotion-tracker.md         # Emotion graphs, regression psychology
│   ├── humanization-reviewer.md   # AI fingerprint detection, humanization scoring
│   ├── editor-critic.md           # Pacing, pattern repetition, market evaluation
│   ├── general-reader.md          # General reader persona (25-35), immersion check
│   └── genre-reader.md            # Genre reader persona (30-40), 삼국지/회귀물 expertise
├── rules/                         # AUTO-INJECTED to all agents (keep minimal!)
│   └── core-rules.md             # 시대착오어, POV, 산문, SVO, 대화 패턴 — ~80줄
├── reference/                     # NOT auto-injected — Read on demand
│   ├── writing-process.md         # Before/during/after checklists, 퇴고, 분량
│   ├── character-management.md    # Character file structure, emotion tracking
│   ├── world-building.md          # Absolute/flexible rules, history verification
│   ├── plot-structure.md          # Narrative models, actantial model, Go mapping
│   ├── style-guide.md             # Writing style, POV, sensory details, 전투씬
│   ├── entertainment-density.md   # Entertainment patterns, density rules
│   ├── humanization-rules.md      # AI 흔적 방지 상세, 갈등 깊이, 비합리성
│   ├── ability-upgrade-current.md # 4트랙 현재 상태 + 레퍼토리 + 빈도 규칙
│   └── ability-upgrade-history.md # Ch.1-현재 전체 업그레이드 이력
└── skills/
    ├── scene-writer/SKILL.md      # Scene writing workflow
    ├── character-creator/SKILL.md # Character creation workflow
    ├── consistency-check/SKILL.md # Full consistency validation
    ├── emotion-analysis/SKILL.md  # Emotion tracking & analysis
    ├── humanization-review/SKILL.md # AI fingerprint detection & humanization scoring
    ├── entertainment-review/SKILL.md # 3-persona entertainment review (100-point scoring)
    └── auto-agents/SKILL.md       # Chapter draft/revision pipeline orchestration
```

---

## Sub-Agent Configuration

| Agent | Purpose | Trigger Keywords |
|-------|---------|------------------|
| story-architect | Plot design, Go-strategy mapping | "플롯", "구조", "페이싱", "바둑전략" |
| character-developer | Character profiles, arcs | "캐릭터", "인물", "대화", "관계" |
| world-builder | History, geography, martial arts | "세계관", "역사", "무예", "지리" |
| style-editor | Prose style, sensory details | "문체", "묘사", "스타일", "다듬기" |
| consistency-checker | Validation, cross-referencing | "일관성", "검증", "플롯홀", "타임라인" |
| emotion-tracker | Emotion graphs, psychology | "감정", "심리", "동기", "내면" |
| humanization-reviewer | AI fingerprint detection, humanization | "인간화", "AI흔적", "지문검사" |
| editor-critic | Pacing, patterns, market evaluation | "편집자", "페이싱", "패턴반복", "시장성" |
| general-reader | General reader immersion check | "독자반응", "몰입도", "스킵", "재미" |
| genre-reader | Genre expert (삼국지/회귀물/무협) evaluation | "장르독자", "삼국지평가", "회귀물", "고증" |

---

## AI Collaboration Commands

### Consistency Commands
- `일관성 검사` - Validate all established rules
- `플롯 홀 체크` - Check logical errors
- `타임라인 검증` - Verify chronological order
- `캐릭터 일관성` - Check character voice & behavior
- `역사 검증` - Check historical accuracy
- `무예 검증` - Validate martial arts power levels
- `바둑 전략 검증` - Verify Go-strategy mapping consistency

### Creative Support Commands
- `감정 분석 [캐릭터]` - Generate emotion graph
- `대화 다듬기` - Improve character voice
- `묘사 확장` - Enhance sensory details
- `페이싱 조절` - Optimize scene pacing
- `긴장감 증폭` - Strengthen conflict elements
- `전투 묘사` - Enhance battle scene description

### Narrative Structure Commands
- `구조 분석` - Analyze current narrative structure
- `다음 비트` - Suggest next required story beat
- `행위소 분석` - Actantial model analysis
- `쾌감 밀도 체크` - Check entertainment pattern density
- `서사 줄기 균형` - Check Strand balance (천하/인간/정체성)
- `성장 단계 체크` - Verify protagonist growth progression
- `바둑 대응 분석` - Analyze Go-to-strategy mapping
- `능력 성장 체크` - Check ability upgrade frequency & track balance (reference/ability-upgrade-current.md)

### Visualization Commands
- `이미지 프롬프트 [대상]` - Generate AI image prompt
- `관계도 업데이트` - Update character relationship map
- `세력도 업데이트` - Update faction power map
- `감정선 그래프` - Generate emotion change chart

---

## Memory Bank Structure

```
00-핵심관리/     → Dashboard, active context, progress
01-세계관/       → World rules, timeline, history, glossary
02-캐릭터/       → Main/supporting/extras character files
03-플롯/         → Main plot, foreshadowing, plot holes, relationships
04-원고/         → Manuscripts by volume/chapter/version
05-심리추적/     → Emotion tracking, motivation maps
06-리서치/       → Research materials (Three Kingdoms history, Go strategy)
07-비즈니스/     → Publishing, marketing
08-피드백/       → Reviews, beta reader feedback
09-생산성/       → Work tracking, daily logs
10-시각자료/     → Image prompts, visual references
11-템플릿/       → Reusable templates
```

---

## Quick Reference

### Do's
- Read 현재작업상태.md before every writing session
- Check character files before writing dialogue
- Verify timeline for all temporal references
- Include sensory details (minimum 3 senses per scene)
- Map Go terms to military strategy consistently
- Maintain protagonist's "감각 재발견" emotional thread
- Update tracking files after writing
- Validate historical accuracy for Three Kingdoms events
- Include ≥1 small ability upgrade (AT1-AT4) per 3-4 chapters

### Don'ts
- Don't violate established world/history rules
- Don't use markdown formatting in narrative prose
- Don't skip consistency checks after major scenes
- Don't ignore foreshadowing tracking
- Don't write without loading character emotional state
- Don't break POV within a scene
- Don't give protagonist overpowered martial arts (brain > brawn)
- Don't modernize dialogue excessively (maintain period flavor)
- Don't write 5+ chapters without any ability upgrade episode (reader growth-check failure)

---

## File Naming Convention

```
Manuscripts: ch-[NNN]_v[N]_[status].md
  Example:   ch-001_v1_초고.md

Git commits: [type] 챕터X: description
  Types:     feat(새 챕터) | fix(오류 수정) | refactor(구조 개선) | style(문체) | docs(문서)
```

---

*This control tower file should remain under 250 lines.*
*For detailed domain rules, refer to Context Map files.*
