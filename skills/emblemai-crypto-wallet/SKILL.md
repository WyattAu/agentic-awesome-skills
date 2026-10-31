- `GET /balance/{chain}/{address}`; Check wallet balance
- `POST /swap`; Execute token swap
- `GET /portfolio/{address}`; Portfolio overview
- `GET /token/{chain}/{contract}`; Token information
- `POST /transfer`; Send tokens
1. **Exact transaction authorization**; before signing/submitting, show chain/network, verified token/asset address, source wallet, recipient, amount, fees, slippage, minimum received, approvals/allowance, spender, expiry and actual simulation result; obtain confirmation for that exact transaction. A quote/portfolio request does not authorize a swap or transfer
5. **Protect credentials and verify custody**; never send private keys or seed phrases through chat, logs or unapproved APIs. Provider claims about server-side vault signing need verification; do not guarantee custody architecture from this index