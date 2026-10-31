Prompt-only skill. Provides a **fund-manager–centric** view of SEC 13F filings via the Longbridge CLI; who the biggest players are, what a specific manager holds, and how their portfolio changed quarter over quarter.
**Ranking**; _Top-50 institutional investors_
**Portfolio snapshot**; _Berkshire Hathaway as of {filing date}_
**Changes**; _Berkshire Hathaway: Q4 2024 → Q1 2025_
| `investors` returns empty       | "暂无 13F 数据，CIK 可能有误，请用 `longbridge investors` 列出前50名确认。" | "暫無 13F 數據，請確認 CIK。" / "No 13F data; verify CIK via `longbridge investors`."                               |
| Other stderr                    | 直接显示原始错误，不静默重试。                                              | 顯示原始錯誤。/ Surface a redacted error summary; do not retry silently.                                                            |