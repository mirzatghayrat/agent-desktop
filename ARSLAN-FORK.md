# Arslan's fork of agent-desktop

This branch (`arslan/0.9.4`) is upstream `v0.9.4`
(`a4a695fdd1f673426579696c7e17074910e799fc`) plus vendoring commits: every dependency
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

License: Apache-2.0, unchanged (see LICENSE). No source files are modified.
