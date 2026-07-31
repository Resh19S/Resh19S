<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=24&duration=3000&pause=1000&color=A78BFA&center=true&vCenter=true&width=650&lines=Hi%2C+I'm+Reshal+%F0%9F%91%8B;ML+Systems+%C2%B7+Quantization+Research+%C2%B7+Edge+AI;Currently+teaching+a+1.7B+model+to+draft+for+an+8B+one" alt="Typing SVG" />

<a href="mailto:reshaldahima0@gmail.com"><img src="https://img.shields.io/badge/Email-reshaldahima0%40gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white" alt="Email"></a>
<a href="#"><img src="https://img.shields.io/badge/LinkedIn-Reshal%20Dahima-0A66C2?style=flat-square&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
<a href="https://github.com/vllm-project/vllm/issues/49893"><img src="https://img.shields.io/badge/vLLM-Bug%20%2349893%20filed-orange?style=flat-square&logo=github" alt="vLLM issue"></a>

B.Tech Computer Science· Class of 2028

</div>

<br>

## About

I work on the layer under the model quantization, speculative decoding, and inference
serving and separately build full end-to-end systems (a voice-driven desktop assistant with
its own hybrid command router and audio pipeline). Currently a software intern at **Crave
Infotech**

I write findings down even when they're negative. my project docs keep failed hypotheses,
disproven anomalies, and the debugging trail alongside the results that worked. A quantization
bug I filed and reproduced for vLLM was confirmed by a maintainer within hours ([#49893](https://github.com/vllm-project/vllm/issues/49893) → [PR #49900](https://github.com/vllm-project/vllm/pull/49900)).

<br>

## Featured Work

### 🔬 Architecture vs. Precision in Speculative Decoding
**[sgt-qat-draft →](https://github.com/Resh19S/sgt-qat-draft)**

A real speculative-decoding benchmark, not a simulated one: does a *quantized dense model*
make a viable drafter against vLLM's purpose-built EAGLE-3, on Qwen3-8B with real mt-bench
prompts?

- **Near-parity draft quality**: mean acceptance length **2.443** (SGT-QAT drafter) vs. **2.474**
  (EAGLE-3) — but a **0.43x wall-clock regression**, because a full 28-layer dense model is
  architecturally too heavy per drafting step, no matter how well-quantized. Quality and speed
  turned out to be separable outcomes.
- Isolated a **vLLM bug**: `SpeculativeConfig(method="draft_model")` couldn't load
  mixed-precision `compressed-tensors` checkpoints. Filed with a minimal repro
  ([#49893](https://github.com/vllm-project/vllm/issues/49893)), confirmed by a maintainer,
  fix opened as [PR #49900](https://github.com/vllm-project/vllm/pull/49900).
- Pushed quantization to **2.158 bits/weight** to test the memory story directly: standalone
  VRAM drops to 0.646 GiB (beating EAGLE-3's 1.047 GiB in-serving footprint) — but perplexity
  collapses ~12x getting there. A memory win that isn't a viable drafter.

### 📊 Mixed-Precision-Guided Targeted QAT
LLM quantization recovery framework, built on a fully seed-verified GPTQ-W3 protocol for Qwen3.

- **70.3% recovery** of GPTQ-W3 damage on Qwen3-1.7B vs. full-parameter QAT, training **~30%
  fewer parameters** — confirmed across two calibration seeds, advantage widens at the second.
- Identified and corrected a **domain-adaptation confound** in the standard PTQ→QAT recovery
  metric (naive formula produced a mathematically impossible >100% recovery) — supplied a
  corrected metric with a control experiment.
- **Reproducibility case study**: a dead `random.seed()` call (seeded a module nothing
  downstream used) produced a false 411% PPL-swing finding that survived weight-space *and*
  output-space mechanistic analysis before a failed causal-ablation baseline exposed it.
  Documented in full as a lesson in what actually catches this class of bug.

### 🎙️ NOVA — Neural Orchestrator for Virtual Assistance
**[Neural-Orchestrator-for-Virtual-Assistance-NOVA- →](https://github.com/Resh19S/Neural-Orchestrator-for-Virtual-Assistance-NOVA-)**

A voice-first Windows desktop assistant, built from scratch — not a wrapper around an API.

- **Three-stage command router** (regex → semantic fuzzy match → Gemini LLM fallback): ~80% of
  commands resolve locally in <10ms, zero API calls.
- **Dual-backend LLM routing** (local Ollama + Gemini 2.5 Flash) with a multi-turn dialogue
  state machine for confirmations, ambiguity resolution, and contextual follow-ups.
- Real-time audio pipeline: Whisper large-v3 (GPU, FP16) STT, clap/Vosk wake-word detection,
  offline TTS, persistent memory across five JSON-backed stores (locations, apps, protocols,
  workspaces, profile).
- Autonomous multi-step planning and a full research pipeline (topic → search → screen-read via
  Gemini Vision → synthesized report saved to disk) from a single spoken sentence.

<br>

## Research & Achievements

| | |
|---|---|
| 🏆 **Amazon ML Summer School 2026** | Selected cohort member — 2.1% acceptance (3,000 / 150,000+ applicants); six-module ML curriculum |
| 🌍 **EdgeGen PhD Summer School** — University of Helsinki | Generative Edge Intelligence, jointly organized with KTH, NTNU, Aarhus (NUEI) |
| 🌍 **Edge AI Research Program** — KTH Royal Institute of Technology Stockholm Sweden | EDGE AI |
| 📄 **SVIMS International Research Conference** | Presented: *Enhancing Desktop Productivity with Generative AI Automation* |
| 🏓 **National Table Tennis Championship** | Semifinalist · College team captain · 3× Inter-College Singles Champion |

<br>

## snake omgg

<div align="center">

![snake animation](https://raw.githubusercontent.com/Resh19S/Resh19S/output/github-contribution-grid-snake-dark.svg#gh-dark-mode-only)
![snake animation](https://raw.githubusercontent.com/Resh19S/Resh19S/output/github-contribution-grid-snake.svg#gh-light-mode-only)

</div>

<br>

## Stack

<div align="center">

![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/-PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![TensorFlow](https://img.shields.io/badge/-TensorFlow-FF6F00?style=flat-square&logo=tensorflow&logoColor=white)
![Transformers](https://img.shields.io/badge/-🤗%20Transformers-FFD21E?style=flat-square)
![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Java](https://img.shields.io/badge/-Java-007396?style=flat-square&logo=openjdk&logoColor=white)
![C++](https://img.shields.io/badge/-C%2FC%2B%2B-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![Git](https://img.shields.io/badge/-Git-F05032?style=flat-square&logo=git&logoColor=white)
![SQLite](https://img.shields.io/badge/-SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white)

**Focus areas:** Quantization (PTQ/QAT) · Speculative Decoding · vLLM internals · RAG ·
Embeddings & Vector Databases · ASR (Whisper) · Edge AI

</div>

<br>

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=Resh19S&show_icons=true&theme=radical&hide_border=true&count_private=true" alt="GitHub Stats" height="165">
<img src="https://github-readme-streak-stats.herokuapp.com/?user=Resh19S&theme=radical&hide_border=true" alt="GitHub Streak" height="165">

<img src="https://github-readme-activity-graph.vercel.app/graph?username=Resh19S&theme=react-dark&hide_border=true" alt="Activity graph" width="850">

</div>

<br>

<div align="center">

<img src="https://github-readme-quotes.vercel.app/api?type=horizontal&theme=radical" alt="Random dev quote" />

<br><br>

*Reach out about quantization, speculative decoding, or vLLM internals. always happy to talk shop.*

</div>
