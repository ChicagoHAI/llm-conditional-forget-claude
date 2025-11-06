# LLMs and "Conditional Forgetting" - Research Project

## Overview

This project investigates whether large language models can "conditionally forget" existing knowledge when presented with modified rules, similar to how humans can reason hypothetically (e.g., "imagine chess where bishops move like rooks").

## Key Findings

**Primary Discovery**: GPT-4o demonstrated surprisingly poor baseline chess knowledge (0% accuracy on standard chess questions), which complicated testing of the conditional forgetting hypothesis.

**Evidence of Difficulty**: Despite weak baseline knowledge, the model showed 15-30% rule intrusion errors - applying standard chess rules even when explicitly instructed to use modified rules.

**Implications**:
- LLMs struggle to suppress default patterns even when those patterns are weak or incorrect
- Testing "conditional forgetting" requires validating baseline knowledge first
- Explicit "forget old rules" instructions may backfire by activating baseline knowledge

### Summary Statistics

| Condition | Accuracy | Rule Intrusion Rate |
|-----------|----------|---------------------|
| Standard Chess | 0% (0/20) | N/A |
| Modified Rules | 5% (1/20) | 15% |
| Metacognitive Prompt | 5% (1/20) | 30% |

**Total Cost**: $0.18 (60 API calls to GPT-4o)

## Repository Structure

```
.
├── README.md                          # This file
├── REPORT.md                          # Comprehensive research report with full analysis
├── planning.md                        # Detailed research plan and methodology
├── resources.md                       # Literature review and resource gathering
├── results/
│   ├── test_scenarios.json           # 20 test scenarios (standard + modified)
│   ├── experiment_results_rescored.json  # All 60 experimental results
│   └── figures/
│       ├── accuracy_by_condition.png  # Main accuracy comparison
│       ├── error_breakdown.png        # Error category analysis
│       └── accuracy_by_category.png   # Performance by rule type
├── notebooks/
│   └── 2025-11-06-16-16_ConditionalForgetting.ipynb  # Full experiment code
└── pyproject.toml                     # Project dependencies
```

## How to Reproduce

### 1. Environment Setup

```bash
# Create virtual environment
uv venv

# Activate environment
source .venv/bin/activate

# Install dependencies
uv pip install openai requests pandas matplotlib scipy
```

### 2. Set API Key

```bash
export OPENAI_API_KEY="your-api-key-here"
```

### 3. Run Experiments

Open the Jupyter notebook:
```bash
jupyter notebook notebooks/2025-11-06-16-16_ConditionalForgetting.ipynb
```

Or run programmatically:
- Load test scenarios from `results/test_scenarios.json`
- Use the `ConditionalForgettingEvaluator` class from the notebook
- Results will match those in `results/experiment_results_rescored.json`

### 4. View Results

See `REPORT.md` for comprehensive analysis, or view:
- Visualizations in `results/figures/`
- Raw data in `results/experiment_results_rescored.json`

## Research Design

### Hypothesis
LLMs will struggle to "conditionally forget" existing knowledge (chess rules) when applying modified rules, showing:
1. Lower accuracy on modified-rule problems
2. Errors primarily from "rule intrusion" (applying original rules)
3. Some improvement with metacognitive prompting

### Methodology

**Within-subjects experimental design** with 20 chess scenarios evaluated under 3 conditions:

1. **Standard**: Baseline chess knowledge (control)
2. **Modified**: Explicitly modified rules (experimental)
3. **Metacognitive**: Modified rules + "forget old rules" instruction (intervention)

**Domain**: Chess (bishops move like rooks, knights move like bishops, etc.)

**Model**: GPT-4o (OpenAI, 2024)

**Parameters**: temperature=0.0 (deterministic), max_tokens=200

### Example Scenario

**Standard Chess**:
- Question: "Can the bishop on c1 capture the pawn on h6?"
- Expected: Yes (c1-to-h6 is diagonal)
- GPT-4o: No ❌ (incorrect spatial reasoning)

**Modified Rules** (bishops move like rooks):
- Question: "With modified rules where bishops move like rooks, can the bishop on c1 capture h6?"
- Expected: No (h6 requires diagonal, bishops now only move straight)
- GPT-4o: No ✓ (correct)

## Key Insights

### 1. Methodological Challenge
Cannot cleanly test "conditional forgetting" when baseline knowledge is absent. The study reveals importance of validating baseline competence before testing advanced reasoning.

### 2. Rule Intrusion Despite Weak Knowledge
Even with 0% baseline accuracy, model still applied standard chess rules in 15-30% of modified scenarios, suggesting:
- Pattern activation occurs even for weak/incorrect knowledge
- Interference mechanism differs from human "forgetting"
- Explicit "don't use old rules" may activate those rules paradoxically

### 3. Spatial Reasoning Deficit
Many errors involved basic geometric failures (e.g., unable to determine if two squares are diagonal), pointing to fundamental capability gaps beyond knowledge interference.

## Future Directions

### Immediate Next Steps

1. **Test domain with strong baseline** (e.g., arithmetic: "In this variant, + means multiply")
2. **Test multiple SOTA models** (Claude, Gemini, GPT-4.1)
3. **Visual board representation** (test if notation is the bottleneck)
4. **Few-shot learning** (provide examples of modified rule application)

### Broader Research Questions

- Does conditional forgetting ability emerge with model scale?
- What prompting strategies effectively mitigate rule intrusion?
- Do some domains show worse conditional forgetting than others?
- Can we quantify relationship between knowledge strength and forgetting difficulty?

## Citation

If you use this research or dataset, please cite:

```bibtex
@techreport{llm_conditional_forgetting_2025,
  title={LLMs and Conditional Forgetting: Evidence from Modified Chess Rules},
  author={Automated Research System},
  year={2025},
  month={November},
  institution={AI Research},
  note={Experimental study testing GPT-4o's ability to apply modified rules}
}
```

## License

This research is released for educational and research purposes. Test scenarios and analysis code are freely available for reuse.

## Contact

For questions about this research, see the comprehensive analysis in `REPORT.md` or review the experimental code in the Jupyter notebook.

---

**Research Duration**: ~4.5 hours (automated, end-to-end)
**Cost**: $0.18
**Date**: November 6, 2025
