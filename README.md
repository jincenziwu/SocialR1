# Social-R1: Towards Human-like Social Reasoning in LLMs

<a href="https://arxiv.org/abs/2603.09249" target="_blank"><img alt="Paper" src="https://img.shields.io/badge/📄 Paper-28a745?color=28a745" /></a>
<a href="https://huggingface.co/datasets/Jincenzi/ToMBench_Hard" target="_blank"><img alt="Dataset" src="https://img.shields.io/badge/🤗 Dataset-8e44ad?color=8e44ad" /></a>
<a href="https://huggingface.co/Jincenzi/SocialR1-4B" target="_blank"><img alt="SocialR1-4B" src="https://img.shields.io/badge/🤗 SocialR1--4B-2980b9?color=2980b9" /></a>
<a href="https://huggingface.co/Jincenzi/SocialR1_8B" target="_blank"><img alt="SocialR1-8B" src="https://img.shields.io/badge/🤗 SocialR1--8B-2980b9?color=2980b9" /></a>

![Social-R1](asset/socialr1.png)

## TLDR
We propose **Social-R1**, a reinforcement learning framework that aligns LLM reasoning at the trajectory level for **human-like social reasoning**. We identify two core failure modes—**Reasoning Parasitism** (models anchor on answer options and backfill justifications) and the **Interpretation Bottleneck** (models fail to map social cues to latent mental states)—and address them through multi-dimensional rewards inspired by Social Information Processing theory. We also construct **ToMBench-Hard**, a challenging ToM dataset for training genuine social inference. Extensive experiments across static MCQ benchmarks, open-ended generation (FanToM), and interactive settings (SOTOPIA) show that Social-R1 enables smaller models to match or outperform substantially larger baselines.

## Key Features

- 🧠 **ToMBench-Hard Benchmark**: 800 expert-annotated adversarial questions covering six Theory-of-Mind dimensions (Belief, Desire, Emotion, Intention, Knowledge, Non-literal Communication) designed to expose reasoning shortcuts
- 🎯 **Multi-Dimensional Reward System**: Combines structural alignment ($R_\text{struct}$), content integrity ($R_\text{content}$), and inference efficiency ($R_\text{len}$) to supervise the entire reasoning trajectory
- 🔬 **SIP-Guided Reasoning**: Enforces stage-consistent social inference following the Social Information Processing framework: Cue Encoding → Cue Interpretation → Goal Clarification → Response Generation
- 🚀 **Cross-Family Generalisation**: Applied to Qwen3-4B, Qwen3-8B, and Llama-3.1-8B-Instruct, demonstrating backbone-agnostic improvements
- 📊 **Comprehensive Evaluation**: Validated across three settings — static MCQ (8 benchmarks), open-ended generation ([FanToM](https://arxiv.org/abs/2310.15421)), and interactive social intelligence ([SOTOPIA](https://arxiv.org/abs/2310.11667))

## 📢 News
- **[2026.05.11]** 🎉 Model checkpoints released: [SocialR1-4B](https://huggingface.co/Jincenzi/SocialR1-4B) | [SocialR1-8B](https://huggingface.co/Jincenzi/SocialR1_8B)
- **[2026.03.10]** 🎉 Social-R1 paper released on arXiv!

## TODO List
- [x] Release the paper.
- [x] Release ToMBench-Hard evaluation set.
- [x] Release training and evaluation code.
- [x] Release model checkpoints (SocialR1-4B, SocialR1-8B).


## Installation

### 0. Initialise Submodules and Conda Environments

```bash
bash ./scripts/setup/init_submodules.sh
```
This script initialises all required submodules and creates the `verl` conda environment.

### 1. Clone verl Repository

```bash
cd submodules
git clone https://github.com/verl-project/verl.git
```

### 2. Overwrite verl with Project Modifications

```bash
bash ./scripts/setup/overwrite_verl.sh
```

### 3. Start LLM Server (for reward evaluation)

```bash
bash ./scripts/setup/llm_server_gpt.sh
```

## Training

Social-R1 is trained using [verl](https://github.com/verl-project/verl) with Group Relative Policy Optimization (GRPO). Training scripts are located in `verl_overwrite/grpo_trainer/`.

### Full Social-R1 Training

```bash
bash run_socialR1_4B.sh
```

 

> **Note:** All training scripts are configured for **4B models** (Qwen3-4B) by default. Training is performed for 600 steps on 8× NVIDIA A100 (80GB) GPUs with group size 5, KL coefficient 0.04, and learning rate 5×10⁻⁷.

 

## Project Structure

```
SocialR1/
├── asset/                        # Figures and assets
├── dataset/
│   └── ToMBench_Hard_val.csv     # ToMBench-Hard validation set
├── scripts/
│   └── setup/
│       ├── init_submodules.sh    # Environment setup
│       ├── llm_server_gpt.sh    # LLM server for reward
│       └── overwrite_verl.sh    # Apply verl modifications
└── verl_overwrite/
    ├── grpo_trainer/             # Training scripts
    │   ├── run_socialR1_4B.sh
    │   ├── run_socialR1_4B_GRPO.sh
    │   ├── run_socialR1_4B_content.sh
    │   ├── run_socialR1_4B_len.sh
    │   └── run_socialR1_4B_structure.sh
    ├── reward_score/             # Reward computation
    │   ├── qa_em_and_format.py   # Format & outcome reward
    │   ├── llm_evaluate.py       # LLM-based evaluation
    │   ├── llm_eval_prompt.py    # Evaluation prompts
    │   └── utils_metric.py       # Metric utilities
    ├── fs.py                     # Few-shot utilities
    ├── naive.py                  # Naive baseline
    └── ray_trainer.py            # Ray-based trainer
```

## Citation
```BibTeX
@article{wu2026socialr1,
  title={Social-R1: Towards Human-like Social Reasoning in LLMs},
  author={Wu, Jincenzi and Lei, Yuxuan and Lian, Jianxun and Huang, Yitian and Zhou, Lexin and Li, Haotian and Yang, Deng and Xie, Xing and Meng, Helen},
  journal={arXiv preprint arXiv:2603.09249},
  year={2026}
}
```

## Contact
If you have any questions or would like to discuss this work, please contact the authors at [jincenziwu@gmail.com](mailto:jincenziwu@gmail.com).
