## Modern RL environments: A light Reading List

VR: Pulled a few papers I'd read a while back and merged with an AI generated list I made to get smarter about RL environments. May update lightly from time to time. I'd focus on the idea behind the method more than a deep read

## Part 1: Modern RL basics

### Deep Q-Networks
#### Playing Atari with Deep Reinforcement Learning (Mnih et al., 2013)
[arXiv 1312.5602](https://arxiv.org/abs/1312.5602)

VR: Classic for the very basic idea, not super relevant to today's methods

The paper that put deep nets inside an RL loop. Learns directly from pixels with Q-learning and a replay buffer. Value-based RL is no longer the main story for LLMs, but it is the vocabulary everything else builds on.


#### High-Dimensional Continuous Control Using Generalized Advantage Estimation (Schulman et al., 2015)
[arXiv 1506.02438](https://arxiv.org/abs/1506.02438)

VR: Agent found this, never read

GAE, the standard way to trade bias against variance when estimating advantages. "Advantage" is the key term for everything that follows, including GRPO.

#### Proximal Policy Optimization Algorithms (Schulman et al., 2017)
[arXiv 1707.06347](https://arxiv.org/abs/1707.06347)

VR: This is a key idea, but I only skimmed this one a bit back

A simpler, clipped version of TRPO that became the default RL algorithm for years. The clipped objective is still the core of most LLM RL recipes.


#### Training Language Models to Follow Instructions with Human Feedback (InstructGPT) (Ouyang et al., 2022)
[arXiv 2203.02155](https://arxiv.org/abs/2203.02155)

VR: The OG method

The canonical recipe: SFT, then a reward model, then PPO. Gives the baseline pipeline that the newer verifiable-reward methods simplify.

#### DeepSeekMath (Shao et al., 2024)
[arXiv 2402.03300](https://arxiv.org/abs/2402.03300)

VR: The Deepseek papers are my fav, bonkers clever ideas in each one

Introduces GRPO, which drops the value network and uses the mean reward of a group of sampled answers as the baseline. Section 4 is the part to read.

#### DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning (2025)
[arXiv 2501.12948](https://arxiv.org/abs/2501.12948)

VR: Literally the single coolest paper IMHO in terms of what it showed was possible / and new methods

The paper that made "GRPO with verifiable rewards" the default recipe. Read this right after DeepSeekMath. R1-Zero (pure RL, no SFT) is the interesting part.

#### DAPO: An Open-Source LLM Reinforcement Learning System at Scale (2025)
[arXiv 2503.14476](https://arxiv.org/abs/2503.14476)

This is the paper with **dynamic sampling** (drop groups where every rollout gets the same reward), clip-higher, token-level loss and overlong reward shaping. It comes from ByteDance and Tsinghua, not DeepSeek.

## Part 2: Terminal and agentic benchmarks

### Terminal-Bench
#### Terminal-Bench: Benchmarking Agents on Hard, Realistic Tasks in Command Line Interfaces (Merrill et al., 2026)
[arXiv 2601.11868](https://arxiv.org/abs/2601.11868)

89 hard terminal tasks, each with its own environment, a reference solution and tests. Frontier agents resolve under 65%. Includes a useful failure-mode taxonomy (execution, coherence, verification).

#### Terminal-Bench 4.0 (tbench.ai news post)
[tbench.ai/news/terminal-bench-4-0](https://www.tbench.ai/news/terminal-bench-4-0)

Short update on how the benchmark is maintained: calibrated resources, an 8-hour agent timeout, and tasks removed for saturation or leaked solutions. Good for seeing how benchmarks get hardened over time.

### Real-world terminal work
#### TerminalWorld: Benchmarking Agents on Real-World Terminal Tasks (Chu et al., 2026)
[arXiv 2605.22535](https://arxiv.org/abs/2605.22535)

Builds 1,530 validated tasks from about 80k real terminal recordings. Best pass rate is 62.5%, and the correlation with Terminal-Bench is weak, so the two measure different things.

#### From Plan to Action: How Well Do Agents Follow the Plan? (Liu et al., 2026)
[arXiv 2604.12147](https://arxiv.org/abs/2604.12147)

Analyzes 21k agent trajectories. Agents drift from plans toward whatever the latest tool output suggests. Explicit plans and periodic reminders help; bad plans hurt.

### Performance optimization
#### FormulaCode: Evaluating Agentic Optimization on Large Codebases (Sehgal et al., 2026)
[arXiv 2603.16011](https://arxiv.org/abs/2603.16011)

957 real performance bottlenecks from scientific Python repos, each with an expert patch. Repository-scale optimization is still hard for frontier agents.

#### PERFOPT-Bench: Evaluating Coding Agents on Software Performance Optimization (Cui et al., 2026)
[arXiv 2607.07744](https://arxiv.org/abs/2607.07744)

Long-horizon optimization tasks scored by verified speedup. Notable for documenting how agents game benchmarks, which is a recurring theme in RL environments.

## Part 3: How RL environments are built

#### Training Software Engineering Agents and Verifiers with SWE-Gym (Pan et al., 2024)
[arXiv 2412.21139](https://arxiv.org/abs/2412.21139)

An early executable environment of real GitHub issues, each with a runnable repo and tests. Shows that agents trained on trajectories from these environments improve, and that verifiers help at inference time.

#### R2E-Gym: Procedural Environments and Hybrid Verifiers for Scaling Open-Weights SWE Agents (Jain et al., 2025)
[arXiv 2504.07164](https://arxiv.org/abs/2504.07164)

Generates environments procedurally from commits instead of hand-curating them, and combines test-based and model-based verification. A good look at where tasks come from and how they are checked.

#### SWE-smith: Scaling Data for Software Engineering Agents (Yang et al., 2025)
[arXiv 2504.21798](https://arxiv.org/abs/2504.21798)

Mass-produces executable training tasks from ordinary repos by synthesizing bugs, with one environment per repo. The cleanest recipe for scaling task count cheaply.

## Part 4: What to read next

Suggested, in this order.

### Modern GRPO fixes
#### DeepSeek-V3.2 technical report (2025)
[arXiv 2512.02556](https://arxiv.org/abs/2512.02556)

VR: Also was a thrilling paper when it came out

The DeepSeek-side follow-up, with their own stabilization changes to GRPO for large-scale RL. Read it alongside DAPO to see where the two groups agree.

### A live look at large-scale RL
#### MiMo-V2.6 live RL training dashboard
[mimo.xiaomi.com/rl](https://mimo.xiaomi.com/rl)

VR: Was absolutely facinating to watch live. My next blog post will likely be going into details about all the clever ideas pulled together here

Xiaomi streams reward curves, rollout counts and step timings for its V2.6 Pro and Flash runs. Pair it with the [MiMo-V2.6 technical report](https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Pro-RL/blob/main/MiMo_V2_6_technical_report.pdf), which describes fully asynchronous GRPO at 1,568 prompts × 16 rollouts per step.
