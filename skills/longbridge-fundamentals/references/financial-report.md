- Financial statements (income statement, balance sheet, cash flow); single statement or all three at once
- Period selection (annual, semi-annual, quarterly; exact flag values differ by CLI version; check `--help`)
# Then fetch financials: example structure (verify flags with --help):
{Symbol} ({code}) Financial Statements; Source: Longbridge Securities
- Accrual ratio: X%; {low / medium / high} accrual, earnings are {cash-backed / partly accrual-driven / accrual-heavy}
| Other stderr                        | 直接显示原始错误，不静默重试。                                | 顯示原始錯誤。/ Surface a redacted error summary; do not retry silently.                                                                                   |