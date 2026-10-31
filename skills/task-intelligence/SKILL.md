description: "Protocolo de Inteligência Pré-Tarefa; planeja a tarefa e consulta apenas agentes disponíveis, relevantes e autorizados quando o benefício justificar o custo."
# Task Intelligence: Protocolo de Amplificação Pré-Tarefa
Protocolo de Inteligência Pré-Tarefa; planeja a tarefa e consulta apenas agentes disponíveis, relevantes e autorizados quando o benefício justificar o custo. Enriquece o contexto com análise paralela multi-agente, produz estimativa real de tempo (início→fim), mapeia problemas prováveis e improvável, e formula um plano de execução antecipado com estratégias de contingência.
1. **Seleciona consultas limitadas e autorizadas**; cada um analisa a tarefa pela sua ótica
## Fase 1: Classificação Da Tarefa (5-10 Segundos)
## Fase 2: Scan E Match Paralelo
## Fase 3: Briefing Dos Agentes Especializados
Não consulte todos os agentes cegamente; escolha os **3-5 mais relevantes** para a tarefa.
## Fase 4: Estimativa De Tempo Real
ESTIMATIVA DE TEMPO; [Nome da Tarefa]
Confiança: Alta/Média/Baixa; [justificativa]
- Nunca subestime para agradar; o usuário precisa saber o tempo real
## Fase 5: Mapa De Problemas (Antecipação Proativa)
MAPA DE PROBLEMAS; [Nome da Tarefa]
## Fase 6: Plano De Execução Enriquecido
BRIEFING PRÉ-EXECUÇÃO; [Nome da Tarefa]
  1. [etapa] (~Xmin); [por quê esta ordem]
  2. [etapa] (~Xmin); [dependência da anterior]
  3. [etapa] (~Xmin); [verificação de qualidade]
Este agente **complementa** o agent-orchestrator; não substitui:
O objetivo não é burocracia; é inteligência a serviço da velocidade real.
- `references/problem-catalog.md`; Catálogo de problemas típicos por domínio
- `references/time-patterns.md`; Padrões históricos de tempo por tipo de tarefa
- `scripts/pre_task_check.py`; referência upstream ausente, não executável neste pacote
BRIEFING PRÉ-EXECUÇÃO; Skill: stripe-integration
  • 007: CRÍTICO; API keys do Stripe NÃO devem ir para SKILL.md ou git.
   ; comparar contratos, sem assumir equivalência. Estrutura hipotética:: config.py + client.py + SKILL.md.
  1. Criar estrutura de diretórios (~2min); base para os demais arquivos
  2. Escrever SKILL.md com workflow (~5min); define comportamento do agente
  3. Criar config.py com variáveis de ambiente (~3min); sem hardcode de keys
  4. Criar stripe_client.py com autenticação (~10min); métodos principais
  5. Criar webhook_handler.py com HMAC-SHA256 (~5min); implementar o contrato específico de assinatura Stripe
  6. Instalar via skill-installer (~2min); validação + registro
  7. Gerar ZIP (~1min); para backup/upload manual