# Auri/EarLLM: Contexto Completo para Análise Matemática
├── bluetooth (conectividade BT; A2DP, HFP, SCO)
⚠️  SPEAKING não tem cancel; bloqueio possível se TTS travar
### AuriToolExecutor: Análise de Idempotência
1. alarm   ; NÃO idempotente (cria alarmes duplicados)
2. calendar; NÃO idempotente (cria eventos duplicados)
3. reminder; NÃO idempotente
4. time    ; Idempotente (read-only)
5. email   ; NÃO idempotente (pode enviar duplicado)
6. draft   ; Quase idempotente (draft com mesmo conteúdo)
7. call    ; NÃO idempotente (inicia chamada)
8. whatsapp; NÃO idempotente
9. app     ; Idempotente se app já aberto
STT (Vosk offline)  N/A (stub)         -
4. LAN access: cleartext permitido para 192.168.*.*; risco em redes públicas ❌
5. Gmail OAuth2 tokens: persistidos em token store; verificar criptografia
6. Audio recording: exige permissão RECORD_AUDIO; verificar escopo temporal