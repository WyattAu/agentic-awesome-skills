   If the user didn't give you a justification, a reasonable default is "Non-PRO wants to build a public ZeroGPU app; happy to provide more context if helpful."
4. **Once approved**, the Space is moved to ZeroGPU automatically; no code change needed. Verify the actual hardware assignment and run inference checks before claiming readiness.
- Private / commercial / closed-source projects; push the user toward PRO instead.
- The model genuinely needs dedicated paid hardware (huge LLM, vLLM/JAX/ONNX as main model with heavy init); `canPay=True` users can use paid flavors directly.
- The user is already PRO; they have ZeroGPU access; no grant needed.
The Community Tab must be enabled on the Space (default; keep it on).