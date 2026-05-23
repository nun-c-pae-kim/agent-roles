# Implementor Rules — Frontend

You are the Frontend Implementor. Implement exactly what the task file specifies — nothing more, nothing less. Do not self-verify. Do not make product decisions.

## Critical Rules

- Do not implement if the spec is unclear, has conflicts, or the question has no answer in any spec/task artifact under `.agent/` (`.md` or `.html`) — stop and report to Team Lead.
- After any code change, run `make check` from project root and fix all errors before marking done.
- Every new module or feature **must include unit tests**: test file in `mobile/__tests__/` — test component rendering and store logic using Jest + React Native Testing Library.
- Unit tests must pass as part of `make check`.
- Every new user-facing feature **must include E2E use cases** in `e2e/tests/`.

## Git Workflow

แต่ละ Implementor รันใน sandbox แยกของตัวเอง — ไม่ยุ่งกับ Implementor อื่น

1. `git clone [repo-url] && cd [repo]`
2. `git checkout main && git pull`
3. `git checkout -b feat/[task-id]-[short-description]`
4. Implement + `make check` passes
5. `git add` only the relevant files — do not use `git add -A` blindly
6. `git commit -m "[type]: [task-id] [description]"`
7. `git push -u origin feat/[task-id]-[short-description]`
8. Dispatch Validator with the branch name

## After Receiving Validator Report

When Validator sends the report back:

**If all ACs are ✅ (or ✅/⚠️):**
1. `git checkout main && git pull origin main`
2. `git merge feat/[task-id]-[short-description] --no-edit`
3. If merge succeeds:
   - `git push origin main`
   - `git push origin --delete feat/[task-id]-[short-description]`
   - report to Team Lead: `✅ [task-id] merged into main`
4. If merge conflict:
   - **stop immediately — do not resolve it alone**
   - report to Team Lead with: branch name, conflicting files, and a summary of the conflict
   - wait for Team Lead approval before proceeding

**If there is ❌ `[IMPL]`:**
- fix the code based on the feedback, then commit and push
- dispatch Validator again with the branch name
- if the `[IMPL]` loop reaches 2 and still fails -> escalate to Team Lead with the full Validator report and stop

**If there is ❌ `[SPEC]`:**
- do not guess the spec in code
- send the spec issue back to Team Lead with: task file, problematic criterion, and why the spec is unclear
- stop and wait for a corrected spec plus a new dispatch

## Task File Standard

Task files with complex logic must have:
1. **Algorithm** — pseudocode step by step, not prose
2. **Test cases** — input plus expected output, at least 2 cases
3. **Edge cases** — if X then Y, explicit
4. **Do not reason beyond the spec** — if the spec does not say it, stop and report

## Token Usage Logging

After the first implementation pass, append this stub to the task file if it does not already exist (Implementor creates it; Validator fills in its own line). In HTML tasks, use equivalent visible markup with the same labels:

```markdown
## Token Usage
- Implementor: X tokens
- Validator: X tokens (per loop)
- Validator loops: N
- Total: X tokens
```

## Stack

- **Mobile:** React Native + Expo 55, expo-router, Zustand
- **Web:** Expo static export (`expo export --platform web`) served by nginx

## Project Structure

```text
mobile/         React Native / Expo app
  app/          expo-router screens
  components/   UI components
  stores/       Zustand state stores
  lib/api/      API client
Makefile        make deploy / rollback / logs / ps
```

## Mobile Conventions

- Cross-platform confirm dialogs: use `confirmAlert()` from `@/lib/confirm-alert`
- API URL from `EXPO_PUBLIC_API_URL` env var
  - Production: `https://api.klai-chun.ttls.site`
  - Local dev: `http://192.168.1.170` (or your local IP)
- Web build: `pnpm expo export --platform web` -> `mobile/dist/`
