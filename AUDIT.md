# Security Audit

Date: 2026-07-28
Scope: Source code (`src/`, `media/main.js`), `package.json`, `package-lock.json`, and packaging config (`.vscodeignore`), reviewed for malicious or unsafe behavior prior to installing/publishing this extension.

## Verdict: No malicious code found — safe to install

## Findings

- **Dependencies**: only `papaparse` (CSV parsing) and `font-list` (enumerate system fonts) — both widely-used, legitimate packages. `package-lock.json` has 442 `resolved` entries, all pointing to the official `registry.npmjs.org`; no typosquats or non-standard registries.
- **No install-time hooks**: no `postinstall`/`preinstall` scripts in `package.json` or the lockfile that could run arbitrary code during `npm install`.
- **No dangerous APIs**: no `child_process`, `exec`/`spawn`, `eval`, `Function()`, dynamic `require`, raw `http(s)` requests, `WebSocket`, or obfuscated/base64 payloads anywhere in `src/` or `media/main.js`.
- **Webview sandboxing**: strict CSP is set on the custom editor webview (`default-src 'none'`, nonce'd `<script>` tags, restricted `img-src`/`style-src`) — correct, standard VS Code hardening.
- **Only "network-like" code**: the clickable-links feature (`src/CsvEditorProvider.ts:2049`) prefixes bare domains typed into a cell with `https://` so Ctrl/Cmd+click can open them in the user's browser. This is local UI behavior triggered only by explicit user click — no telemetry, no background requests, no data exfiltration.
- **Packaging** (`.vscodeignore`): excludes source maps, `.ts` sources, and tests from the published `.vsix`. Normal practice; doesn't hide anything since the source is public in this repo.
- **Provenance**: repository history is a coherent, incremental feature/fix log with no suspicious or unexplained commits.

## Conclusion

Nothing resembling malicious behavior, data exfiltration, or supply-chain risk was found. The extension is safe to install as of this review.
