# Gateway and stored-data proof for OpenClaw PR #198706

Retained evidence from one private VM using PR head `62c6373c5b467446938e95ee6a29bcb493420985`, the repository mock provider, SQLite, and synthetic credentials. This publication does not repeat the VM run. The VM ran macOS 26.6.2. The harness and published stable Gateway used Node 24.21.0; the PR source launcher selected its Bun 1.4.4 runtime (its compatibility report lists Node 26.12.1). These runtime identities are retained in the Gateway logs.

## Inspect the cases

| Case | Retained files |
|---|---|
| Completion and lifecycle settlement | [terminal output](terminal-output.txt), [lifecycle card](snapshots/fresh-lifecycle.json), [completed card](snapshots/fresh-completed.json), [Gateway log](gateway-logs/fresh-gateway.log) |
| Verification survives a later comment and restart | [after comment / before restart](snapshots/fresh-verified-after-write.json), [after restart](snapshots/fresh-after-restart.json) |
| Fresh install and build | [dependency install](install-build-logs/install.log), [runtime build](install-build-logs/build.log), initial fresh flow in [terminal output](terminal-output.txt) |
| Stable upgrade: stored data before and after | [stable before upgrade](snapshots/stable-before-upgrade.json), [PR after verification and comment](snapshots/upgrade-verified-after-write.json), [PR after restart](snapshots/upgrade-after-restart.json), [comparison](stored-data-comparison.json) |
| New post-upgrade run | [lifecycle](snapshots/post-upgrade-lifecycle.json), [completion](snapshots/post-upgrade-completed.json), [Gateway log](gateway-logs/upgrade-restart-gateway.log) |
| Main-format conversion: stored data before and after | [legacy read before write](snapshots/legacy-read-before-write.json), [first write](snapshots/legacy-first-write.json), [after restart](snapshots/legacy-after-restart.json), [expected converted verdict](snapshots/legacy-expected-verdict.json), [comparison](stored-data-comparison.json) |

The stable package was `openclaw@2026.11.3`. The same state directory was opened by the PR build. The retained after-upgrade snapshot was captured after adding a checker verdict and a comment; its existing attempts, proof and artifacts match the stable snapshot exactly. The harness also asserted equality on the initial PR read, as recorded in the terminal output; no separate full snapshot of that initial read was retained.

The main-format case is a synthetic compatibility fixture, not a main Gateway execution: main's production evidence builders constructed claim, observation and verification records, seeded into isolated SQLite while the Gateway was stopped. The original metadata size was 24,080 bytes. Reading preserved `automation.evidence`. The first manual move to done converted the verdict, removed `kind` and `claimId`, settled the existing attempt and kept all twelve artifacts. The harness checked an empty metadata write and a later Gateway restart too. Snapshots are Gateway-returned card JSON, exposing the persisted card data; they are not raw database pages.

[Stored-data comparison](stored-data-comparison.json) is derived from the retained JSON, rather than a new VM run. Full artifact URLs, record IDs and timestamps remain available for direct comparisons. [Harness sources](harness/gateway-proof.mjs.txt) and [provenance](provenance.json) describe the assertions and input identity.

## Redactions and scope

Public copies redact IP addresses, key/token values (including synthetic credentials), non-public endpoints, user/temp paths, email/phone-like strings and mock model labels. ANSI color escapes and trailing horizontal whitespace are removed. Original private files are unchanged. No real provider keys or user app data were in the VM. Provider request bodies and prompts are not published.

Output retains setup failures and intentional stop/start messages. The harness corrected legacy fixture config through Doctor and waited for Workboard registration after the early WebSocket bind. Some intentionally short Gateway processes were stopped while later startup work was still settling; their logs retain interrupted/superseded startup diagnostics. The saved card mutations and restart comparisons are shown separately. No product code was changed.

The build output includes UI bundle-size warnings. This proof covers Gateway/SQLite with mock inference, WebSocket RPC and HTTP tool invocation. It does not exercise real providers, external channels, UI, PostgreSQL, the updater command or broad CI, and does not decide the Workboard owner contract question.
