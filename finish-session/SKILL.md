---
name: finish-session
description: Use when the user says "finish up this session", "wrap up", "close out", "we're done for today" or types /finish-session, i.e. when a working session is ending and its state must be left clean and recorded for the next one.
---

# Finish a session

A session is finished when **nothing of mine is still running, nothing of mine is uncommitted or
unmerged, the next session can pick up from durable records, and the user knows what is left.**
Touch only what this session created; everything else is reported, never "tidied".

## Steps (in order, batch the read-only checks into one command)

1. **Inventory.** `ListAgents`; `git status --porcelain`, `git log --oneline origin/main..main`,
   `git worktree list`, `git branch`; this session's servers/processes (by the PIDs or ports you
   started). Compare with the session-start `gitStatus` snapshot to tell mine from pre-existing.
2. **Stop what is mine.** Subagents still running (`TaskStop`), test servers and child processes —
   kill by verified PID, never by image name. Tell a peer session in the same checkout which paths
   and ports you have released.
3. **Land the work.** Commit my finished changes by path (repo commit rules apply). Merge finished
   worktree branches per the repo's merge gate; push `origin main` only where a standing ruling
   allows it after a green gate. **Never deploy** unless the user ordered it this session.
   Unfinished work stays on its branch and is named in the report.
4. **Reclaim worktrees** I created and merged: unlink `node_modules` first, then remove the
   worktree and its branch; sweep orphan directories.
5. **Update the records the work changed** — in place, never appended (`grep` first):
   - the project's tracked docs/briefs that now describe stale state (open items, plan status,
     a brief like `VIDEO-BRIEF.md`);
   - memory: one note per durable finding or ruling from this session, plus its `MEMORY.md`
     index line; rewrite the existing line rather than adding a second one.
   Skip anything git history or the code already records.
6. **Verify.** Re-run the inventory from step 1; the only differences from the session-start
   snapshot should be the commits you made.

## The report (this shape, short)

1. One line: wrapped up, and whether anything of mine is still open.
2. **Code / Git:** what was committed, merged, pushed (SHA); "not deployed" unless it was.
3. **Cleanup:** worktrees, servers, agents stopped.
4. **Records:** which docs and memory notes were updated.
5. **Not mine, left untouched:** pre-existing edits, peers' commits/worktrees, unknown processes.
6. **Needs you:** each decision or loose end the user owns (deploy, an unruled question, a
   broken ruling). Omit the section when empty.

## Common mistakes

| Mistake | Instead |
|---|---|
| Committing, stashing or reverting the user's pre-existing edits | Report them under "Not mine" |
| Pushing a peer's unpushed commits | Report them; their session pushes |
| Killing `php.exe`/`node.exe`/an agy process you did not start | Kill by PID you recorded; report the rest |
| Adding a memory line next to a stale one | Rewrite the stale line |
| "Memory updated" without an index line | Write both |
| Waiting/polling for a straggler agent | Stop it, or report it as still running |
