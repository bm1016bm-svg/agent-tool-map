# agent-tool-map

A concise, source-backed field guide to small tools that people and AI agents can use. A dependency-free CLI lists, filters, and compares the catalogue.

**Node.js 22+ · No API key · No LLM required**

[繁體中文](README.zh-TW.md) · [Contributing](CONTRIBUTING.md) · [MIT license](LICENSE)

Catalogue reviewed: **2026-10-04**. Per-source review dates are recorded in `data/tools.json`.

## Get started

Run these commands from the project directory. No dependency installation is needed.

```sh
git clone https://github.com/bm1016bm-svg/agent-tool-map
cd agent-tool-map
node bin/agent-tool-map.mjs list
node bin/agent-tool-map.mjs list --category office-editing
node bin/agent-tool-map.mjs list --interface cli --json
node bin/agent-tool-map.mjs show ask-json
node bin/agent-tool-map.mjs compare ccphoto bifrost
```

## Field guide

Use the fit and limitations columns to narrow your choices, then check the linked primary sources. CLI `--json` output is suitable for scripts and agents.

| Tool, category & interface | Purpose | Best fit | Limitations | Sources |
| --- | --- | --- | --- | --- |
| [AskJSON](https://github.com/reuters-graphics/ask-json)<br>`human-input`, `structured-data`<br>`cli`, `library` | Turns a JSON Schema into interactive terminal prompts that fill missing or invalid values and validate the resulting data. | Collecting required configuration from a person before a script or publishing workflow consumes it. | Needs interactive human input. Supports a limited set of schema keywords and homogeneous arrays; missing optional fields are not prompted. | [1](https://github.com/reuters-graphics/ask-json) |
| [Bifrost](https://github.com/axiom0x0/bifrost)<br>`file-transfer`, `human-input`<br>`cli`, `web` | Starts a local HTTP file-transfer page and prints a QR code for a phone browser, with send, receive and optional encrypted-transfer modes. | Moving files between a terminal session and a nearby phone without installing a phone app. | Devices need a reachable local network. The documented receive flow is general file upload, not an AI task-correlated photo callback. | [1](https://github.com/axiom0x0/bifrost) |
| [CCPhoto](https://github.com/stepankaiser/ccphoto)<br>`photo-input`, `human-input`<br>`cli`, `mcp`, `web` | Connects a phone's browser camera to a local server, returning photos through MCP or saving them with a standalone QR-based CLI. | Showing an assistant a physical label, whiteboard or component from a nearby phone. | The phone must reach the LAN server. Standalone mode keeps running for multiple captures; live video has additional HTTPS and browser restrictions. | [1](https://github.com/stepankaiser/ccphoto) |
| [Ciacola](https://github.com/joshrotenberg/ciacola)<br>`agent-runtime`, `budget-control`<br>`mcp`, `web` | Hosts durable Claude and Codex conversations with a SQLite ledger, restart recovery, scoped MCP access and provider-specific limits. | Operating persistent local agents when recovery and access scopes matter more than a small command wrapper. | Requires a source build and authenticated provider CLIs. Admission thresholds are not reservations, and response-boundary ceilings can overshoot. | [1](https://github.com/joshrotenberg/ciacola) |
| [codex-wrapper](https://github.com/joshrotenberg/codex-wrapper)<br>`cli-integration`, `budget-control`<br>`library` | Provides Rust command builders and structured execution integration around an installed Codex CLI, including native rollout budgets. | Embedding Codex CLI execution in Rust applications that need configurable run limits. | Requires Codex CLI. Native budget units depend on CLI/provider versions and checks happen after responses; they are not exact total-token or dollar caps. | [1](https://github.com/joshrotenberg/codex-wrapper#native-rollout-budgets) |
| [docx-cli](https://github.com/kklimuk/docx-cli)<br>`office-editing`, `document-review`<br>`cli` | Reads and edits DOCX through locators, supporting comments and native Word tracked changes through direct XML mutation. | Filling a Word template or producing a redlined copy that a person can review in Word. | Default edits overwrite the file and there is no built-in undo. Rendering needs Word or LibreOffice; upstream fidelity benchmarks were not reproduced here. | [1](https://github.com/kklimuk/docx-cli) |
| [Evidrift](https://github.com/bm1016bm-svg/evidrift)<br>`dependency-drift`, `json-reproduction`<br>`cli`, `mcp` | Records and rechecks selected TypeScript or local JSON contracts, and reduces loopback HTTP JSON failures into replayable examples. | Checking whether a selected dependency assumption still holds after an upgrade, or minimizing a local JSON failure. | Contract checks cover installed TypeScript signatures and repository-local JSON values. Remote replay, YAML and remote references are unsupported; free-text claims are not proved. | [1](https://github.com/bm1016bm-svg/evidrift) |
| [excel-git / xlgit](https://github.com/MichaelFowler1/excel-git)<br>`office-editing`, `version-control`<br>`cli` | Adds cell-aware spreadsheet diff and three-way merge to Git by editing workbook XML parts rather than rebuilding the entire workbook. | Reviewing and merging independent XLSX or XLSM edits made on separate Git branches. | Conflicting cell edits need manual resolution, and Excel recalculates formulas on opening. Preservation and fuzzing results are upstream reports, not this catalog's tests. | [1](https://github.com/MichaelFowler1/excel-git) |
| [Grape](https://github.com/gael55x/Grape)<br>`repository-context`, `context-transport`<br>`cli`, `mcp` | Builds repository context packs with session-scoped diffs, source/excerpt hashes, restore handles and stale-context invalidation. | Repeated repository tasks that need context changes tracked between turns instead of resending everything. | The agent must invoke its CLI or MCP tools. It does not prove answer correctness, and TypeScript/JavaScript receive stronger indexing than fallback languages. | [1](https://github.com/gael55x/Grape) |
| [LoopGain](https://github.com/loopgain-ai/loopgain)<br>`loop-control`, `convergence`<br>`library` | Uses an application's numeric error trajectory to classify convergence and stop on targets, stalls, oscillation or iteration limits. | Iterative agents or solvers with a meaningful numeric error measure and a best-so-far output. | The caller defines the error signal. Trend classifications do not establish task correctness; benchmark savings are author-reported and were not reproduced here. | [1](https://github.com/loopgain-ai/loopgain) · [2](https://github.com/loopgain-ai/loopgain/blob/main/loopgain/core.py) |
| [LoopLens MCP](https://github.com/musaceylan/looplens-mcp)<br>`loop-control`, `debugging`<br>`mcp` | Collects agent tool events and reports repeated patterns, error signatures and convergence proxy scores. | Inspecting debugging sessions and recurring tool-call patterns in an MCP-connected workflow. | Needs event ingestion and MCP integration. Tool diversity and input/output-size metrics are heuristics, not evidence that acceptance criteria passed. | [1](https://github.com/musaceylan/looplens-mcp) · [2](https://github.com/musaceylan/looplens-mcp/blob/master/src/looplens/convergence/metrics.py) |
| [Pueue](https://github.com/Nukesor/pueue)<br>`task-queue`, `process-control`<br>`cli`, `library`, `daemon` | Queues shell commands in a background daemon with groups, concurrency limits, task status, logs and wait controls. | Letting an assistant enqueue long local commands and later inspect or wait for their results. | Requires pueued and correct shell quoting. Designed for one user's modest task queue, not distributed execution, multiple users or advanced load balancing. | [1](https://github.com/Nukesor/pueue) |
| [PyMuPDF4LLM](https://github.com/pymupdf/pymupdf4llm)<br>`pdf-extraction`, `document-data`<br>`library` | Extracts PDF content as Markdown, JSON or plain text with layout information, page chunks and optional OCR. | Preparing local PDFs for an assistant or retrieval pipeline without a vision-model extraction service. | OCR needs an installed engine. Office document support uses PyMuPDF Pro; the project documents AGPL or commercial licensing. Extraction quality was not benchmarked here. | [1](https://github.com/pymupdf/pymupdf4llm) |
| [Repo Context Center](https://github.com/cyberturk01/repo-context-center)<br>`repository-context`, `change-impact`<br>`cli` | Creates repository maps and task routes, then suggests affected files, tests and verification commands from repository signals. | Helping a shell-capable coding assistant find likely files and plan focused verification. | Impact analysis is heuristic, not a complete dependency engine. Verify prints suggestions and does not execute tests or prove correctness. | [1](https://github.com/cyberturk01/repo-context-center) |
| [xlq](https://github.com/wms2537/aix)<br>`office-editing`, `structured-data`<br>`cli` | Provides JSON spreadsheet inspection and typed cell patches; upstream describes part-scoped OOXML writes that preserve other parts. | Applying a narrowly specified spreadsheet change through a native CLI with explicit coverage and refusal outputs. | Operations can refuse when formula coverage is unreliable. Part-preservation and calculation-coverage figures are maintainer-reported and were not retested here. | [1](https://github.com/wms2537/aix) |

### Available filters

Categories: `agent-runtime`, `budget-control`, `change-impact`, `cli-integration`, `context-transport`, `convergence`, `debugging`, `dependency-drift`, `document-data`, `document-review`, `file-transfer`, `human-input`, `json-reproduction`, `loop-control`, `office-editing`, `pdf-extraction`, `photo-input`, `process-control`, `repository-context`, `structured-data`, `task-queue`, `version-control`.

Interfaces: `cli`, `daemon`, `library`, `mcp`, `web`.

## CLI commands

| Command | Purpose |
| --- | --- |
| `list [--category X] [--interface X] [--json]` | List tools; combine category and interface filters. |
| `show ID [--json]` | Read a complete entry, including fit, limitations, and sources. |
| `compare ID ID [--json]` | Compare fit and tradeoffs for two entries. |
| `validate [--json]` | Check catalogue structure and consistency offline. |
| `check-links [--json]` | Check catalogue URLs over the network. |

Run each command after `node bin/agent-tool-map.mjs`. Only `check-links` uses the network. URL reachability describes the link; check tool behavior in your own workflow.

Exit codes: `0` success; `1` invalid catalogue or broken links (HTTP 404/410); `2` usage/read failures or unavailable links (for example HTTP 403/429, network errors, or timeouts). Broken links take priority when both broken and unavailable links occur.

## Review method

Entries reflect primary documentation at the recorded review dates, with explicit interfaces, fit, and limitations. This guide does not report installation or execution tests of third-party tools, and list order or star counts are not quality ratings. Review dates do not guarantee future behavior or compatibility; verify current sources against your requirements.

## Maintain and check

`data/tools.json` is the catalogue source. Both READMEs are generated by `scripts/build-readme.mjs`.

```sh
npm test
npm run validate
npm run docs
npm run docs:check
```

CI runs tests, catalogue validation, and generated-document checks on Windows and Linux with Node.js 22 and 24. Maintainers can run live link checks separately.

## License

MIT © 2026 bm1016bm-svg. See [LICENSE](LICENSE).
