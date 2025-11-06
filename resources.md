# Phase 0: Research Resources & Findings

## Literature Review Summary

### Key Papers Reviewed

1. **"If Pigs Could Fly… Can LLMs Logically Reason Through Counterfactuals?"** (2025)
   - Source: arXiv 2505.22318
   - Tested 11 SOTA LLMs on counterfactual reasoning
   - Key metric: 27% performance gap between factual and counterfactual scenarios
   - Introduced CounterLogic dataset (1,800 examples, 9 logical schemas)
   - Self-segregation intervention reduced gap to 11%

2. **"On the Eligibility of LLMs for Counterfactual Reasoning"** (2025)
   - Decompositional approach to counterfactual reasoning
   - Identified specific stages where LLMs fail
   - Proposed actionable strategies for improvement

3. **"Can LLMs Truly Perform Analogical Reasoning?"** (ACL 2025)
   - LLMs match humans on some analogical tasks
   - Different response patterns to semantic distractors
   - Heavily prompt-sensitive

4. **Chess Reasoning Papers** (2024-2025)
   - Multiple papers use chess as reasoning testbed
   - LLMs plateau below expert level
   - Limited by pretrained model's internal understanding

### Key Insights for This Research

**Research Gap**: No direct study of "conditional forgetting" - the ability to suppress existing domain knowledge and apply modified rules

**Cognitive Phenomenon**: Humans can conditionally forget chess rules and apply hypothetical ones (e.g., "bishops move like rooks")

**LLM Challenge**: Knowledge conflict between parametric knowledge and counterfactual instructions

## Resources Selected

### No Pre-existing Dataset
- **Decision**: Create custom test set for modified chess rules
- **Rationale**: No existing benchmark for this specific phenomenon
- **Approach**: Design synthetic scenarios with rule modifications

### Baselines Identified
1. Standard chess rule understanding (factual baseline)
2. Random/majority baseline for controlled comparison
3. Counterfactual reasoning benchmarks as reference

### Models to Test (2024-2025 SOTA)
Based on domain-specific guidelines:
- **GPT-4o** or **GPT-4.1** (OpenAI, latest available)
- **Claude Sonnet 4.5** (Anthropic, excellent for reasoning)
- **Gemini 2.0 Pro** or latest (Google, multimodal capabilities)
- Possibly via OpenRouter for additional models

### Evaluation Metrics
1. **Accuracy**: Correct application of modified rules
2. **Error Categories**:
   - Original rule intrusion (applying real chess rules)
   - Novel rule understanding
   - Logical consistency
3. **Comparative**: Factual vs. counterfactual performance gap

## Time Allocation (Phase 0)

**Time Spent**: ~20 minutes
- Literature search: 10 min
- Paper review: 7 min
- Documentation: 3 min

**Status**: Complete ✓

## Next Steps

Move to **Phase 1: Planning** to design:
1. Specific test scenarios (chess rule modifications)
2. Experimental protocol
3. Statistical analysis plan
4. Implementation timeline
