# Arslan's fork of agent-desktop

This branch (`arslan/0.9.4`) is upstream `v0.9.4`
(`a4a695fdd1f673426579696c7e17074910e799fc`) plus one commit: every dependency
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

License: Apache-2.0, unchanged (see LICENSE). No source files are modified.
