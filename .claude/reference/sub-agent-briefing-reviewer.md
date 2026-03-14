# Sub-Agent Briefing: Reviewer (리뷰 에이전트용)

이 템플릿은 Lead가 Phase 5 리뷰 Sub-agent에게 전달하는 브리핑 형식이다.
`[PLACEHOLDER]`는 Lead가 실제 값으로 대체한다.

---

## R1: Humanization Depth Review 브리핑

Phase 4B 기계적 스캔(시대착오어, SVO, em-dash, 분리호칭) **완료됨 — 생략**.

### 입력
- 원고: [MANUSCRIPT_PATH]
- 4B 스캔 결과 요약: [SCAN_SUMMARY] (N건 수정됨)

### 심층 검사 항목 (이것에 집중)
1. **대화 자연스러움**: 정보전달형 대사 비율, 대화 노이즈 존재(3회+), 캐릭터 음성 구분도
2. **갈등 해결 속도**: 해결-잔여 긴장 존재, 갈등 깊이 규칙 (소2/중4/대8ep)
3. **구조 다양성**: 에피소드 구조 패턴, 성장 곡선 비균일성, 비합리적 행동(1회+)
4. **감각 다양성**: 씬당 감각 3종+, 동일 조합 연속 여부, 환경 묘사 변주

### 출력 형식
- 점수: 0-100
- P1 (Critical) 수정 사항: 목록
- P2 (Major) 수정 사항: 목록
- 4B 잔존 확인: 누락분 있으면 P1 추가

### 참조 규칙
- `core-rules.md` §9-11 자동주입됨 — **humanization-rules.md 별도 Read 불필요**
- 금지패턴 전체 목록이 필요한 경우만 humanization-rules.md Read

---

## R2: Consistency + Narrative Structure 브리핑

### 입력
- 원고: [MANUSCRIPT_PATH]
- 아웃라인: [OUTLINE_PATH]
- 컨텍스트 요약 (Lead 전달): [CONTEXT_SUMMARY]
  <!-- Phase 1에서 로드한 캐릭터/메인플롯/서사줄기/복선/쾌감카운터 요약 -->

### 일관성 검사
1. 복선 상태 감사 (아웃라인 vs 원고)
2. 역사 정합성 + 무예 등급 정합성
3. 바둑-전략 매핑 정확도
4. 전투씬: 기풍 선언, 무예 등급, 교환 수, 살상 묘사
5. 능력 업그레이드: Before/After, 자기인식, 타인반응, 구체적 결과

### 서사구조 검사
1. 쾌감 패턴 배치 (E패턴 연속2회 금지, 저사용 점검)
2. 서사줄기 비율 (천하/인간/정체성)
3. 챕터 성격 유형 정합성 (3연속 금지)
4. 페이싱 평가

### 출력 형식
- 점수: 0-100
- Critical / Major / Minor 분류 목록

### 참조 규칙
- `core-rules.md` §12-13 자동주입됨 — **plot-structure.md, entertainment-density.md 별도 Read 불필요**
- `reference/world-building.md`는 역사/무예 검증 필요 시 Read 허용
- Phase 1 컨텍스트 요약을 우선 사용. 직접 Read는 검증 필요한 설정 파일(타임라인, 역사배경)에 한정
