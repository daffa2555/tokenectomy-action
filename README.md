# Tokenectomy Razor Action 🗡️

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-Tokenectomy%20Razor%20Action-blue.svg?logo=github&style=flat)](https://github.com/marketplace/actions/tokenectomy-razor-action)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Official MCP Registry](https://img.shields.io/badge/Official%20MCP%20Registry-Active-brightgreen)](https://registry.modelcontextprotocol.io/)
[![Main Repo](https://img.shields.io/badge/GitHub-daffa2555%2FTokenectomy-blue?logo=github)](https://github.com/daffa2555/Tokenectomy)

**Tokenectomy Razor Action** is a high-performance GitHub Action that surgically scrubs internal framework stack frames and redacts sensitive credentials from build/test failure logs in sub-milliseconds—preventing secret leaks and slashing 90%+ of token bloat before logs reach AI triage bots or workflow artifacts.

---

## ⚡ Quickstart

Add Tokenectomy Razor to any step in your CI/CD pipeline to sanitize failure logs:

```yaml
name: Test Suite & Autonomous Triage

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Build & Capture Errors
        id: build_step
        run: |
          npm test > build.log 2>&1 || true

      - name: Surgically Scrub Failure Log
        uses: daffa2555/tokenectomy-action@v1
        with:
          log-file: 'build.log'
          output-file: 'sanitized.log'

      - name: Upload Clean Artifact
        uses: actions/upload-artifact@v4
        with:
          name: sanitized-diagnostics
          path: sanitized.log
```

---

## ✨ Key Capabilities

- **🌐 Polyglot Frame Pruning**: Automatically filters thousands of lines of framework dependency noise across:
  - **Node.js / TypeScript**: `node_modules/`, `.next/`, `dist/`
  - **Python**: `site-packages/`, `dist-packages/`, `venv/`
  - **Rust**: `.cargo/registry/`, `.rustup/`, `target/debug/build/`
  - **Golang**: `go/src/` (stdlib), `go/pkg/mod/`, `vendor/`
  - **Java / Kotlin**: `.m2/repository/`, `.gradle/caches/`, `org.springframework`
  - **PHP**: `vendor/composer/`, `vendor/symfony/`, `vendor/laravel/`
  - **C / C++**: `/usr/include/`, `/usr/lib/`, `vcpkg_installed/`
- **🛡️ ReDoS-Safe Secret Redaction**: Strips database connection strings (`postgresql://`, `mysql://`, `mongodb://`), AWS keys (`AKIA...`), GitHub personal access tokens (`ghp_...`), Bearer tokens, and JWTs in $O(N)$ linear time.
- **⚡ Sub-Millisecond Speed**: Runs on verified native pre-compiled Rust binaries without requiring compilation on the CI runner.
- **🖥️ Cross-Platform**: Supports Linux (`ubuntu-latest`), macOS (`macos-latest` / Apple Silicon & Intel), and Windows (`windows-latest`).

---

## 📋 Inputs

| Input | Description | Required | Default |
| :--- | :--- | :---: | :--- |
| `log-file` | Path to the raw error log file to sanitize | No | `''` |
| `log-content` | Direct raw string content to sanitize (used if `log-file` is empty) | No | `''` |
| `output-file` | Target path to write the scrubbed output | No | `tokenectomy-sanitized.log` |
| `version` | Target release version of the Tokenectomy Razor binary | No | `v1.1.2` |

---

## 📤 Outputs

| Output | Description |
| :--- | :--- |
| `sanitized-file` | Path to the sanitized output file |
| `sanitized-content` | Content of the sanitized log (populated if output is < 20KB) |
| `original-lines` | Total line count before surgery |
| `sanitized-lines` | Total line count after surgery |
| `lines-pruned` | Total number of framework frames pruned |

---

## 🔗 Ecosystem Links

- **Core Engine (Rust Crate & MCP Server)**: [daffa2555/Tokenectomy](https://github.com/daffa2555/Tokenectomy)
- **Official MCP Registry Listing**: [`io.github.daffa2555/razor`](https://registry.modelcontextprotocol.io/)
- **Crates.io**: [crates.io/crates/tokenectomy](https://crates.io/crates/tokenectomy)
- **Autonomous Git Bridge**: [tokenectomy-git](https://github.com/daffa2555/tokenectomy-git)

---

## 📄 License

MIT License © 2026 Daffa Ananta
