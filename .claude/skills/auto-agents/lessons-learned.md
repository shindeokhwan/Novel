# Auto-Agents Lessons Learned (검증된 교훈)

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
| L11 | Phase 0.5에서 업그레이드 의무 사전 확인 → Phase 6 재작업 방지 | 1단계 개선(2026-03-03) | Phase 0.5 |
| L12 | R2도 7ch 초과 시 분할 필수 (R2a 일관성 + R2b 서사구조) | L1 동일 원인 | Phase 5 R2, Mode B Phase 2 R2 |
| L13 | Phase 1 메타파일 조건부 로딩 — 전투/쾌감/구조 파일은 해당 시에만 | 토큰 절감 분석(2026-03-03) | Phase 1 |
| L14 | `reference/core-rules.md`를 Sub-agent 프롬프트에 포함 — reference/ 추가 Read 금지 강제 | Phase 3 컨텍스트 문제(2026-03-03) | Phase 3 |
| L15 | 기계적 스캔(시대착오어·em-dash·분리호칭 등)은 Phase 4B로 조기 이동 → R1 부담 경감 + 즉시 수정 | 2단계 개선(2026-03-03) | Phase 4B, Phase 5 R1 |
| L16 | R1/R2/R3 리뷰 충돌 시 Phase 5.5 우선순위 규칙 적용 (세계관 > 인간화 > 서사구조) | 2단계 개선(2026-03-03) | Phase 5.5 |
| L17 | R3 쾌감·페이싱 리뷰를 Mode A에도 조건부 실행 — 소아크 마지막/대형 쾌감/페이싱 저조 시 | 2단계 개선(2026-03-03) | Phase 5 R3 |
| L18 | style-editor 폐기 → humanization-reviewer + core-rules.md로 완전 대체. 파이프라인/수동 에이전트 역할 분리 | 3단계 개선(2026-03-03) | agents/ 정리 |
| L19 | Phase 6 회귀 검출: 수정 패턴 3건+ → 직전 10ch Grep → Mode B 후보 기록 (자동 수정 금지) | 3단계 개선(2026-03-03) | Phase 6 |
| L20 | Mode B 폴백: Teammate→Sub-agent 매핑 규모별 상세화 (Small 병렬, Medium 순차, Large 2병렬순차) | 3단계 개선(2026-03-03) | Mode B Fallback |
