CC 11-20: complexo, difícil de testar; refatorar
// CC ≈ 1 + 10 + 3 = 14; deve ser refatorado
  app: Ca=0, Ce=6 → I = 1 (INSTÁVEL; esperado: é a camada mais volátil)
  CREATED → DESTROYED (onDestroy; sem start, raro)
   - Latência: O(total_tokens / bandwidth); alta
   - Memória: O(total_tokens); linear
   - Latência percebida: O(primeira_sentença / bandwidth); baixa
MainViewModel.kt: estimado V ≈ 5000-10000, D ≈ 15-25; COMPLEXO
LlmProvider.kt: estimado V ≈ 500-1000, D ≈ 5-10; SIMPLES