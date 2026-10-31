# Rate Limits: Instagram Graph API
| Stories | Sem limite oficial |; | Mas recomenda-se < 25/dia |
- **80%**: Info log; "Approaching rate limit"
- **90%**: Warning; "Near rate limit, consider slowing down"
- **100%**: Block; Retorna erro com tempo de espera estimado
### api_client.py: Exponential Backoff
O `db.py` persiste dados em SQLite; evita refazer chamadas para dados recentes.