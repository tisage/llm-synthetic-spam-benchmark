# LLM-Generated Synthetic Spam Benchmark Dataset

**Status:** Accepted and presented at the 12th International Conference on Information Systems Security and Privacy (ICISSP 2026)

## Overview

This repository contains a benchmark dataset for evaluating Large Language Models' dual impact on spam detection: their utility for generating synthetic training data and their potential for creating AI-powered spam attacks.

## Dataset Description

This dataset supports systematic evaluation of LLM-generated synthetic spam across three experimental tracks:
- **Synthetic-to-Real Detection**: Can synthetic spam train effective classifiers for real spam detection?
- **Real-to-Synthetic Detection**: Can classifiers trained on real spam detect LLM-generated spam?
- **Mixed-to-Synthetic Detection**: Does cross-model synthetic augmentation improve detection?

### Data Statistics

- **Real Spam**: 2,000 samples (from CEAS-08 corpus)
- **Real Ham**: 18,000 samples (from CEAS-08 corpus)
- **Synthetic Spam (GPT-4.1-mini)**: 6,000 samples (3 prompt strategies × 2,000 each)
- **Synthetic Spam (Claude-3.5-Haiku)**: 6,000 samples (3 prompt strategies × 2,000 each)
- **Total**: 32,000 email samples

### Prompt Strategies

Three systematically varied prompt strategies explore generation quality:

1. **Original**: Faithful reproduction with natural lexical variation
2. **Strong**: Amplified spam indicators and explicit persuasive elements
3. **Weak**: Subtle spam with reduced explicit indicators

Complete prompt templates are available in the `prompts/` directory.

## Data Organization

```
data/
├── real/
│   ├── spam.jsonl          # 2,000 real spam emails
│   └── ham.jsonl           # 18,000 real ham emails
└── synthetic/
    ├── gpt41mini/
    │   ├── original.jsonl  # 2,000 samples
    │   ├── strong.jsonl    # 2,000 samples
    │   └── weak.jsonl      # 2,000 samples
    └── claude35haiku/
        ├── original.jsonl  # 2,000 samples
        ├── strong.jsonl    # 2,000 samples
        └── weak.jsonl      # 2,000 samples
```

### Data Format

Each JSONL file contains one JSON object per line:

```json
{
  "id": "unique_sample_id",
  "text": "email body content",
  "label": "spam or ham",
  "source": "real | gpt41mini | claude35haiku",
  "prompt_strategy": "original | strong | weak | null",
  "seed_id": "original_spam_id (for synthetic only)"
}
```

## Usage

### Loading Data (Python)

```python
import json

def load_jsonl(filepath):
    with open(filepath, 'r') as f:
        return [json.loads(line) for line in f]

# Load real spam
real_spam = load_jsonl('data/real/spam.jsonl')

# Load GPT synthetic spam
gpt_original = load_jsonl('data/synthetic/gpt41mini/original.jsonl')
```

### Reproducing Experiments

The dataset supports replication of all experimental tracks:

1. **Track 1 (Synthetic-to-Real)**: Train on varying mixtures of real + synthetic spam, test on real spam
2. **Track 2a (Real-to-Synthetic)**: Train on real spam only, test on synthetic spam
3. **Track 2b (Mixed-to-Synthetic)**: Train on real + LLM_B synthetic, test on LLM_A synthetic

## Citation

If you use this database in your research, please cite our paper:

```bibtex
@inproceedings{wang2026synthetic,
  title={Can synthetic spam beat real-world detectors? Evaluating LLMs' dual role in spam generation and detection},
  author={Wang, T. and Zhou, N. and Chen, Z.},
  booktitle={Proceedings of the 12th International Conference on Information Systems Security and Privacy},
  volume={1},
  pages={73--84},
  year={2026},
  isbn={978-989-758-800-6}
}
```

## License

This dataset is released under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) for research purposes only.

## Ethical Considerations

**Defensive Use Only**: This dataset is intended for security research to improve spam detection systems. We do not condone using this data or methodology for malicious purposes including spam generation or deployment.

**Data Privacy**: Real spam samples are sourced from the publicly available CEAS-08 corpus. All personally identifiable information has been removed.

## Contact

For questions about this dataset, please open an issue in this repository.

---

**Note**: Experimental settings, detailed methodology, and comprehensive results are detailed in our ICISSP 2026 paper.
