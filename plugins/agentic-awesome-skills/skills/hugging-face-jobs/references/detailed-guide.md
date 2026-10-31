Run any workload on fully managed Hugging Face infrastructure. No local setup required-jobs run on cloud CPUs, GPUs, or TPUs and can persist results to the Hugging Face Hub.
**⚠️ EPHEMERAL ENVIRONMENT-MUST PERSIST RESULTS**
### Pattern 1: Dataset → Model Responses (vLLM): `scripts/generate-responses.py`
### Pattern 2: CoT Self-Instruct Synthetic Data: `scripts/cot-self-instruct.py`
### Pattern 3: Streaming Dataset Stats (Polars + HF Hub): `scripts/finepdfs-stats.py`
2. Verify token in secrets; MCP: `"$HF_TOKEN"`, Python API: `get_token()` (NOT `"$HF_TOKEN"`)
5. **Use tokens securely** - MCP: `secrets={"HF_TOKEN": "$HF_TOKEN"}`, Python API: `secrets={"HF_TOKEN": get_token()}`; `"$HF_TOKEN"` only works with MCP tool