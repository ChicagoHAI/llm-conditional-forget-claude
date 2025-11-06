# Research Plan: LLMs and Conditional Forgetting

## Research Question

**Primary**: Can large language models "conditionally forget" existing knowledge (e.g., chess rules) and apply new hypothetical rules as effectively as humans can?

**Specific**: When presented with modified rules for a familiar domain (chess), do LLMs struggle to suppress their parametric knowledge and apply the new rules, resulting in performance degradation compared to baseline scenarios?

## Background and Motivation

### Why This Matters

**Cognitive Phenomenon**: Humans excel at hypothetical and analogical reasoning. When told "imagine chess where bishops move like rooks," humans can temporarily suspend their knowledge of standard chess and reason within the modified rule system.

**Potential LLM Limitation**: Recent research shows LLMs struggle with counterfactual reasoning when it conflicts with parametric knowledge (27% performance gap in recent studies). However, no research directly tests "conditional forgetting" - the ability to suppress domain-specific knowledge.

**Practical Implications**:
- Understanding this limitation affects LLM applications in creative problem-solving
- Informs prompt engineering strategies for hypothetical scenarios
- Relates to broader questions about LLM flexibility and knowledge representation

## Hypothesis Decomposition

### H1: Knowledge Interference (Primary Hypothesis)
**Statement**: LLMs will show higher error rates on modified-rule chess problems compared to standard chess problems, with errors primarily consisting of applying original chess rules instead of modified rules.

**Testable**: Compare accuracy on standard vs. modified-rule scenarios

**Success Criteria**: Statistically significant accuracy drop (p < 0.05) with error analysis showing ≥50% of errors involve original rule intrusion

### H2: Explicit Instruction Helps (Secondary)
**Statement**: Explicit metacognitive prompting (e.g., "forget the standard rules, only use these new rules") will reduce but not eliminate the performance gap.

**Testable**: Compare modified-rule performance across prompt variants

**Success Criteria**: Improvement with metacognitive prompting, but gap remains significant

### H3: Domain Familiarity Effect
**Statement**: The performance gap will be larger for well-known rules (e.g., pawn movement) than obscure rules (e.g., en passant), suggesting stronger interference from frequently-accessed knowledge.

**Testable**: Measure error rates by rule type

## Proposed Methodology

### High-Level Approach

**Experimental Design**: Within-subjects comparison
- **Control Condition**: Standard chess rule application
- **Experimental Condition**: Modified chess rules
- **Intervention Condition**: Modified rules + metacognitive prompting

**Domain Choice Rationale**: Chess is ideal because:
1. Clear, well-defined rules
2. Recent LLM chess research provides context
3. Easy to modify rules systematically
4. Mix of simple and complex rules
5. LLMs have substantial chess knowledge in training data

### Experimental Steps

#### Step 1: Design Test Scenarios (30 min)
Create matched pairs of chess problems:
- **Standard**: "Can the bishop on c1 capture the pawn on h6?" (diagonal movement)
- **Modified**: "In this variant, bishops move like rooks. Can the bishop on c1 capture the pawn on h6?" (requires straight-line movement check)

**Categories of Rule Modifications**:
1. Piece movement changes (bishop moves like rook, knight moves like bishop)
2. Capture rule changes (pawns can't capture, only move)
3. Win condition changes (checkmate requires capturing king)
4. Combination rules (multiple modifications)

**Rationale**: Matched pairs control for problem difficulty

#### Step 2: Create Evaluation Dataset (30 min)
- 60 total problems (20 pairs × 3 conditions)
  - 20 standard chess problems (baseline)
  - 20 modified-rule problems (simple prompt)
  - 20 modified-rule problems (metacognitive prompt)
- Mix of yes/no questions and move selection
- Balanced difficulty (easy, medium, hard based on moves ahead)

**Rationale**: Sufficient for statistical power (n=20 per condition) while remaining within time/budget constraints

#### Step 3: Implement Evaluation Pipeline (45 min)
```python
# Pseudo-code structure
1. Load test scenarios
2. For each model:
   - Generate responses for all conditions
   - Parse and validate outputs
   - Record: answer, reasoning, latency
3. Score responses (binary correct/incorrect)
4. Categorize errors (rule intrusion, misunderstanding, etc.)
```

**Rationale**: Automated pipeline enables consistent evaluation

#### Step 4: Run Experiments (60 min)
- Test 2-3 SOTA models (GPT-4o, Claude Sonnet 4.5, Gemini)
- 3 temperature settings (0, 0.7) for robustness check
- Track API costs in real-time

**Rationale**: Multiple models show generalizability; temperature=0 for determinism

#### Step 5: Error Analysis (30 min)
Manually categorize errors:
1. **Original Rule Intrusion**: Applied standard chess rule instead of modified
2. **Misunderstood Modified Rule**: Understood modification but applied incorrectly
3. **Logical Error**: Correct rule understanding but faulty reasoning
4. **Parsing Error**: Failed to understand question

**Rationale**: Qualitative analysis reveals mechanism of failure

#### Step 6: Statistical Analysis (20 min)
- Paired t-test for accuracy differences (standard vs. modified)
- Chi-square for error category distribution
- Cohen's d for effect size
- Confidence intervals for all estimates

**Rationale**: Appropriate tests for paired within-subjects design

### Baselines

1. **Random Baseline**: 50% accuracy (yes/no questions)
2. **Standard Chess Performance**: Model's baseline chess ability
3. **Literature Baseline**: 27% performance gap from counterfactual reasoning research

**Why These Baselines**:
- Random shows floor performance
- Standard chess shows ceiling (no interference)
- Literature baseline contextualizes findings

### Evaluation Metrics

#### Primary Metrics

1. **Accuracy**: % correct responses
   - **Why**: Direct measure of task performance
   - **Interpretation**: Lower is worse

2. **Performance Gap**: Accuracy(standard) - Accuracy(modified)
   - **Why**: Quantifies interference effect
   - **Interpretation**: Larger gap = stronger conditional forgetting deficit

#### Secondary Metrics

3. **Error Rate by Category**: % of errors that are rule intrusions
   - **Why**: Tests mechanism (H1)
   - **Interpretation**: High % supports knowledge interference hypothesis

4. **Prompt Sensitivity**: Accuracy(metacognitive) - Accuracy(simple)
   - **Why**: Tests intervention effectiveness (H2)
   - **Interpretation**: Improvement shows prompt engineering can help

### Statistical Analysis Plan

**Tests**:
1. Paired t-test: Standard vs. Modified accuracy (α = 0.05)
2. Repeated measures ANOVA: Across all 3 conditions
3. Chi-square: Error category distributions
4. Cohen's d: Effect size for performance gap

**Multiple Comparison Correction**: Bonferroni correction for post-hoc tests

**Sample Size**: n=20 problems per condition (power ≥ 0.8 for medium effect size)

## Expected Outcomes

### Outcome 1: Hypothesis Supported
**If**: Accuracy(modified) < Accuracy(standard) with p < 0.05 AND ≥50% errors are rule intrusions

**Conclusion**: LLMs exhibit conditional forgetting deficit - they cannot fully suppress parametric knowledge

**Implications**: Fundamental limitation for counterfactual/hypothetical reasoning

### Outcome 2: Hypothesis Partially Supported
**If**: Significant gap exists BUT metacognitive prompting eliminates it

**Conclusion**: LLMs can conditionally forget with explicit scaffolding

**Implications**: Prompt engineering strategies can mitigate this limitation

### Outcome 3: Hypothesis Not Supported
**If**: No significant difference between conditions

**Conclusion**: LLMs may handle conditional forgetting better than expected

**Implications**: Re-examine hypothesis or experimental design

## Timeline and Milestones

**Total Time Budget**: 3.5 hours

| Phase | Task | Duration | Cumulative |
|-------|------|----------|------------|
| 0 | Literature review & resource gathering | 30 min | 0:30 |
| 1 | Planning (this document) | 20 min | 0:50 |
| 2 | Design test scenarios & dataset | 60 min | 1:50 |
| 3 | Implementation | 45 min | 2:35 |
| 4 | Run experiments | 60 min | 3:35 |
| 5 | Analysis | 30 min | 4:05 |
| 6 | Documentation | 30 min | 4:35 |
| | **Buffer** | 25 min | 5:00 |

**Critical Path**: Dataset creation → Implementation → Experiments → Analysis

## Potential Challenges

### Challenge 1: API Costs Exceed Budget
**Risk**: High
**Mitigation**:
- Start with 1 model (cheapest: GPT-4o-mini or Claude Haiku)
- Reduce sample size if needed (minimum n=15)
- Use temperature=0 only (no multiple samples)

### Challenge 2: Models Perform Perfectly
**Risk**: Medium
**Impact**: Null result
**Mitigation**:
- Increase difficulty of modified rules
- Test more complex scenarios
- Still valuable negative result

### Challenge 3: Parsing Errors Dominate
**Risk**: Medium
**Impact**: Can't distinguish from reasoning errors
**Mitigation**:
- Use structured output (JSON)
- Multiple-choice format
- Clear prompting

### Challenge 4: Time Constraints
**Risk**: Medium
**Mitigation**:
- Prioritize core conditions (standard vs. modified)
- Skip metacognitive condition if needed
- Document what was completed

## Success Criteria

**Minimum Viable Research**:
- ✓ Test at least 1 SOTA model
- ✓ At least 30 problems (15 standard, 15 modified)
- ✓ Statistical test of primary hypothesis
- ✓ Error analysis with examples
- ✓ Complete documentation

**Full Success**:
- ✓ Test 2-3 models
- ✓ 60 problems across 3 conditions
- ✓ Comprehensive error analysis
- ✓ Publication-quality documentation
- ✓ Reproducible code

## Research Ethics & Transparency

- All prompts will be documented verbatim
- API versions and timestamps recorded
- Negative results will be reported
- Limitations clearly acknowledged
- No cherry-picking of results

## Contingency Plans

**If H1 not supported**: Investigate whether:
- Problems too easy
- Models truly can conditionally forget
- Chess domain inappropriate

**If time runs out**: Prioritize:
1. Core experiment (standard vs. modified)
2. Error analysis
3. Documentation
4. Skip: Multiple models, metacognitive condition

**If budget exceeded**:
- Use smaller models
- Reduce sample size
- Document actual costs

---

**Plan Status**: Complete ✓
**Next Phase**: Phase 2 - Dataset Creation and Implementation
