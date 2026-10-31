# DCGM: production monitoring daemon (for clusters)
# Check GPU errors (XID errors: important for stability)
# 1g.10gb  : 1 slice,  10GB (max 7 instances)
# 2g.20gb  : 2 slices, 20GB (max 3 instances)
# 3g.40gb  : 3 slices, 40GB (max 2 instances)
# 7g.80gb  : full GPU, 80GB (max 1 instance)
- Always enable persistence mode (`nvidia-smi -pm 1`); reduces first-request latency.