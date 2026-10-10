# Arslan's fork of agent-desktop

This branch (`arslan/main-9d7ba42`) is upstream `main` at
`9d7ba42a94bc4ec018e624f3033602188f64af63` (v0.9.4 plus four commits, no release yet) plus
the carried patches below and vendoring commits: every dependency
vendored under `vendor/` and `.cargo/config.toml` pointing crates.io at it, so
Arslan's release pipeline builds it with
`cargo build --release --locked --offline -p agent-desktop` and downloads nothing.

Arslan pins this branch's commit in `packaging/hands/agent-desktop.pin` and runs
`cargo deny check` on it. Arslan uses twelve commands (snapshot, find, get,
click, type, set-value, select, press, scroll, list-apps, list-windows, wait);
any build that replaces this one must pass Arslan's contract check
(`scripts/hands_contract_check.py` in the Arslan repository).

Monthly: read upstream releases and the diff of `crates/core` and `crates/macos`,
take security fixes, re-run `cargo vendor --locked vendor`, move the pin, rerun
the contract check and the smoke on a Mac.

Before committing a re-vendor, `git status --porcelain --ignored -- vendor` must
show nothing ignored (`!!`). Cargo checksums every vendored file, and a file that
an ignore rule kept out of git is present in your checkout but missing from a
clean one: the first `.vim/` rule here dropped memchr's and aho-corasick's
`.vim/coc-settings.json`, so the offline build failed only from a clean fetch.
The last line of `.gitignore` re-includes all of `vendor/`; Arslan's CI also
checks every vendored file against its `.cargo-checksum.json`.

License: Apache-2.0, unchanged (see LICENSE). Source files are changed only by the carried
patches below, and each changed file says so in its first lines (Apache-2.0 §4(b)).

## Carried patches

Each one is offered upstream; when upstream has it, the next sync drops ours.

1. **A complete app inventory leaves out an application whose process is gone**
   (`crates/macos/src/system/workspace_apps.rs`, two tests in `workspace_apps_tests.rs`).
   LaunchServices kept a record of a Preview that had never finished launching (its pid gone,
   launch time null); `list-apps` treated it as an inventory still changing and retried until
   its deadline, so every complete inventory failed and Arslan saw no app at all (2026-10-10,
   on a real Mac). A scoped lookup still reports such an app. Same shape as upstream's skip of
   process-less (pid <= 0) applications. Upstream: not yet offered.
