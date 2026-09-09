# Security Policy / 資訊安全政策

This repository is a public integration component of StyTrix (www.stytrix.com):
it contains client-side tooling only (no backend source, no credentials).
本倉庫是 StyTrix 對外整合元件，僅含客戶端工具，不含後端原始碼或任何憑證。

## Reporting a vulnerability / 通報漏洞

| | |
|---|---|
| **Email / 信箱** | **security@stytrix.com** (fallback: hello@stytrix.com) |
| **Machine-readable / 機讀** | https://www.stytrix.com/.well-known/security.txt |
| **Acknowledgement / 回覆時限** | within 3 business days (Asia/Taipei) / 3 個工作天內 |

Please do **not** open a public GitHub issue for security reports.
請勿以公開 GitHub issue 通報安全問題。

Include the affected file or endpoint, reproduction steps and impact.
Reports about the StyTrix platform itself (API, MCP endpoint `https://www.stytrix.com/api/mcp`,
OAuth flow) are handled under the same policy.
請提供受影響的檔案／端點、重現步驟與影響範圍；平台本身（API、MCP 端點、OAuth 流程）的問題也適用本政策。

## Supported versions / 支援版本

Only the `main` branch and the latest tagged release receive security fixes.
僅 `main` 分支與最新標記版本會收到安全修補。

## Scope notes / 範圍說明

- Never commit tokens, client secrets or `.stytrix` credential files to this repository.
  不得將 token、client secret 或 `.stytrix` 憑證檔提交至本倉庫。
- Secret scanning and push protection are enabled on this repository.
  本倉庫已啟用 secret scanning 與 push protection。
