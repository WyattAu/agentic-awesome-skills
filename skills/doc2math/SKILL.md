# DOC2MATH: Document-to-Mathematics Problem Specification
1. **Closed World**; do not assert unstated facts or numeric values. Structural notation implied by source phrasing may be proposed only with the explicit inference tag required below
2. **Grounding Rule**; every element must cite the exact source phrase (`"evidence"` field)
3. **No Silent Filling**; unknown values use `null`; ambiguous types use `"ambiguous"`
4. **Inference Tagging**; structural inferences tagged `"inferred": true` with `"inference_basis"`
5. **MISSING Markers**; elements mentioned but insufficiently defined get `"status": "MISSING"` with `"missing_reason"`
6. **No Hallucinated Math**; never introduce equations or values not in the source text
### Step 1: Receive Document
### Step 2: Classify
### Step 3: Extract MPS Components
**Variables**; `id`, `name`, `symbol`, `type`, `domain`, `units`, `role`, `evidence`, `inferred`, `status`
**Operators**; `id`, `name`, `symbol`, `arity`, `acts_on`, `produces`, `evidence`, `inferred`
**Constraints**; `id`, `type`, `expression`, `variables_involved`, `evidence`, `hardness`, `inferred`, `status`
**Objectives**; `id`, `direction` (minimize/maximize/satisfy/find/prove), `expression`, `variables_involved`, `evidence`, `inferred`
**Uncertainty**; `id`, `type` (stochastic/epistemic/measurement/model/none_stated), `affects`, `characterization`, `evidence`, `status`
### Step 4: Surface Missing Information
### Step 5: Validate and Score
- ✅ Surface MISSING markers rather than silently inferring; incomplete formalization is valid output
- Built by [IntuiTek¹](https://intuitek.ai) (~K¹); MIT License