---
name: sync
description: 리모트 브랜치와 현재 로컬 브랜치를 양방향 동기화한다. 상황 자동 판단 → 설명 → 동의 후 실행.
---

# Sync - 양방향 브랜치 동기화 스킬

로컬↔리모트 상태를 자동 판단하여 필요한 동작(commit/pull/push)을 제안하고, 사용자 동의 후 실행한다.

## Trigger
- "동기화", "sync", "리모트 동기화", "브랜치 동기화", "pull", "push", "커밋하고 푸시"

## Workflow

### Phase 1: 상황 진단

아래 4가지를 **병렬로** 수집한다:
1. `git status` — 로컬 변경사항 (untracked/modified/staged)
2. `git branch -vv` — 현재 브랜치 + 트래킹 리모트
3. `git fetch origin` — 리모트 최신화
4. `git log --oneline -5` — 최근 커밋 히스토리

fetch 완료 후 추가 확인:
5. `git log HEAD..origin/[브랜치] --oneline` — 리모트에만 있는 커밋
6. `git log origin/[브랜치]..HEAD --oneline` — 로컬에만 있는 커밋
7. `git diff --stat` — 변경된 파일 요약

### Phase 2: 상황 분류 + 사용자 보고

아래 6가지 시나리오 중 해당하는 것을 **한국어로 간결하게** 설명하고 실행 계획을 제시한다.

| # | 상황 | 제안 동작 |
|---|------|----------|
| S1 | 로컬 변경 있음 + 리모트 변경 없음 | commit → push |
| S2 | 로컬 변경 없음 + 리모트 변경 있음 | pull (fast-forward) |
| S3 | 양쪽 모두 변경 있음 (로컬 uncommitted) | stash → pull → stash pop → commit → push |
| S4 | 양쪽 모두 커밋 있음 (diverged) | pull --rebase → push |
| S5 | 로컬 커밋만 있음 (push 안 된 상태) | push |
| S6 | 양쪽 모두 최신 | "이미 동기화됨" 안내 후 종료 |

**보고 형식 예시**:
```
📋 Sync 상황 진단:
- 브랜치: main ← origin/main
- 로컬 변경: 4개 파일 수정, 2개 신규
- 리모트 변경: 없음
- 판단: S1 — 로컬 변경사항을 커밋하고 푸시하면 됩니다.

실행 계획:
1. 변경 파일 스테이징
2. 커밋 (메시지 자동 생성)
3. push to origin/main

진행할까요?
```

**사용자 동의 없이 절대 실행하지 않는다.**

### Phase 3: 실행

사용자 동의 후 해당 시나리오의 동작을 순차 실행한다.

#### S1: commit → push
1. `git diff --stat` + `git diff`로 변경 내용 확인
2. 변경 내용 분석 → 커밋 메시지 자동 생성 (CLAUDE.md 커밋 컨벤션 준수)
3. 관련 파일만 `git add` (민감 파일 제외)
4. `git commit` (Co-Authored-By 포함)
5. `git push origin [브랜치]`

#### S2: pull
1. `git merge --ff-only origin/[브랜치]`
2. 실패 시 사용자에게 merge/rebase 선택 요청

#### S3: stash → pull → stash pop → commit → push
1. `git stash push -m "auto-stash before sync [날짜]"`
2. `git merge --ff-only origin/[브랜치]`
3. `git stash pop`
4. stash pop 충돌 시 → 충돌 파일 표시 + 수동 해결 안내 후 중단
5. 충돌 없으면 → S1 절차 (commit → push)

#### S4: pull --rebase → push
1. 사용자에게 rebase/merge 선택 확인
2. `git pull --rebase origin/[브랜치]` 또는 `git merge origin/[브랜치]`
3. 충돌 시 → 충돌 해결 안내 후 중단
4. 성공 시 → `git push origin [브랜치]`

#### S5: push
1. `git push origin [브랜치]`
2. 리모트 브랜치 없으면 `git push -u origin [브랜치]`

#### S6: 종료
1. "이미 동기화됨" 안내

### Phase 4: 결과 보고

```
✅ Sync 완료:
- 커밋: [커밋 해시] [메시지]
- push: origin/main ← [N]개 커밋
- 현재 상태: clean
```

## 안전 규칙

- **force push 절대 금지** — 필요 시 사용자에게 경고하고 수동 처리 안내
- **main/master에 force push 경고** — 요청해도 재확인
- 민감 파일(.env, credentials 등) 커밋 시 경고
- 충돌 발생 시 자동 해결 시도하지 않고 사용자에게 안내
- stash 사용 시 반드시 복원 (pop) 확인

## Output
- 동기화 전/후 상태 요약
- 실행된 동작 목록
- 커밋/push 결과 (해당 시)
- 충돌 여부 및 해결 상태
