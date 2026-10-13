# Handover 2026-10-13 — the desktop logouts (from the /home terminal agent, session 52eb8ad2)

**Root cause, proven:** `kill_tree` in `crates/comparegraph/src/providers/external.rs` (lane C1, worktree `comparegraph-wt-6c`)
ran `/usr/bin/kill -KILL -<pid>`. procps-ng `kill` keeps only the first digit of a negative pid after a `-SIG` option; the build host pids
are ~1.6M, so it became `kill(-1, SIGKILL)` — every process of the user, including the GNOME session, all terminals and all
Claude sessions. It fired each time `cargo test -p comparegraph` ran in wt-6c (tests `a_hanging_provider_times_out`,
`timeout_kills_grandchildren_too`). Full chain + red/green proof: `docs/reports/2026-10-13-user-wide-kill-from-kill-tree.md`.

**Already done:** the one-line fix is in the wt-6c working tree, UNCOMMITTED: `args(["-s", "KILL", "--", &format!("-{}", child.id())])`
plus a doc comment. Rebuilt test binary `target/debug/deps/external-32a1abee81bb0ca8` is green in a container with 7-digit pids.
wt-6c also holds C1's own uncommitted edits (build.rs, store/mod.rs, tests/external.rs) — not touched.

**For the C1 lane owner:** review the hunk, commit it with the C1 work, and grep every repo for `kill -` spellings that put a
`-<pgid>` right after a signal — each needs `--` (or `libc::kill(-pid, SIGKILL)` instead of a shell-out).

**Before resuming anything:** do NOT run the external tests from an unpatched checkout (main, other lanes) on the build host — it logs
everyone out again. The 10:32 "session restarted while you were working" resumes were all this; the chains that "die with the
Claude session" (the build host-build-machine memory, 2026-10-13 line) died because the whole user was killed, not because of setsid/nohup.

