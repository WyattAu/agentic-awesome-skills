# 007: Licenca para Auditar - Detailed Guide
## 007: Licenca Para Auditar
## Processo De Analise: 6 Fases
#### STRIDE (Tecnico: por componente)
#### PASTA (Negocio: orientado a risco)
1. **Usuario malicioso**; tem conta legitima, quer escalar privilegios
2. **Bot abusivo**; automacao hostil tentando explorar APIs
3. **Agente comprometido**; um agente do ecossistema foi manipulado
4. **API externa hostil**; servico de terceiro retorna dados maliciosos
5. **Operador descuidado**; erro humano com consequencias de seguranca
6. **Insider malicioso**; tem acesso ao codigo/infra e ma intencao
7. **Supply chain attacker**; dependencia maliciosa inserida
1. **Arquitetura**; mudancas estruturais que eliminam classes de vulnerabilidade
2. **Guardrails Tecnicos**; limites codificados que impedem abuso
3. **Sandboxing**; isolamento que contem dano em caso de comprometimento
4. **Monitoramento**; visibilidade para detectar e responder
5. **Resposta**; procedimentos para quando algo da errado
- **90-100**: Aprovado; indicador heuristico; revisao humana obrigatoria
- **70-89**: Aprovado com ressalvas; requer revisao humana, sem autorizacao de deploy
- **50-69**: Bloqueado parcial; precisa correcoes antes de producao
- **0-49**: Bloqueado total; inseguro, requer redesign
1. **Zero Trust**: nunca confiar em input externo; humano, API, agente ou IA
- `references/stride-pasta-guide.md`; Guia completo de threat modeling
- `references/owasp-checklists.md`; OWASP Top 10 Web, API e LLM com exemplos
- `references/api-security-patterns.md`; Padroes de seguranca para APIs
- `references/ai-agent-security.md`; Seguranca de IA, agentes e LLM pipelines
- `references/incident-playbooks.md`; Playbooks completos de resposta a incidente