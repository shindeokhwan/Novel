---
name: sync
description: 리모트 브랜치와 현재 로컬 브랜치를 동기화한다. fetch + pull/rebase 자동 처리.
---

# Sync - 브랜치 동기화 스킬

리모트 브랜치의 최신 상태를 로컬 브랜치로 가져온다.

## Trigger
- "동기화", "sync", "리모트 동기화", "브랜치 동기화", "pull"

## Workflow

### Phase 1: 현재 상태 확인
1. `git status`로 작업 디렉토리 상태 확인
2. `git branch -vv`로 현재 브랜치 및 트래킹 리모트 확인
3. 커밋되지 않은 변경사항(unstaged/staged) 존재 여부 파악

### Phase 2: 안전 조치
- **변경사항이 있는 경우**: 사용자에게 알리고 `git stash` 여부를 확인
  - 승인 시 `git stash push -m "auto-stash before sync [날짜]"`
  - 거부 시 중단
- **변경사항이 없는 경우**: 바로 Phase 3 진행

### Phase 3: 리모트 fetch
1. `git fetch origin` 실행
2. 로컬과 리모트 간 커밋 차이 확인:
   - `git log HEAD..origin/[현재브랜치] --oneline` (리모트에만 있는 커밋)
   - `git log origin/[현재브랜치]..HEAD --oneline` (로컬에만 있는 커밋)

### Phase 4: 동기화 전략 결정 및 실행

| 상황 | 전략 | 명령 |
|------|------|------|
| 리모트에만 새 커밋 (fast-forward 가능) | Fast-forward merge | `git merge --ff-only origin/[브랜치]` |
| 양쪽 모두 새 커밋 (diverged) | 사용자에게 merge/rebase 선택 요청 | `git pull --rebase origin/[브랜치]` 또는 `git merge origin/[브랜치]` |
| 로컬에만 새 커밋 | 이미 최신 — 동기화 불필요 안내 | 없음 |
| 리모트 브랜치 없음 | 트래킹 설정 안내 | `git push -u origin [브랜치]` 제안 |

### Phase 5: 후처리
1. 충돌 발생 시: 충돌 파일 목록 표시 + 해결 안내
2. Phase 2에서 stash 했으면: `git stash pop` 실행
3. stash pop 충돌 시: 충돌 파일 표시 + 수동 해결 안내
4. 최종 `git log --oneline -5`로 결과 확인

## Output
- 동기화 전/후 상태 요약
- 가져온 커밋 목록 (있으면)
- 충돌 여부 및 해결 상태
