<div align="center">

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=26&duration=3000&pause=1000&color=A78BFA&center=true&vCenter=true&width=650&lines=Hi%2C+I'm+Reshal+%F0%9F%91%8B;I+quantize+models+until+they+almost+break;Sometimes+I+file+bugs+vLLM+maintainers+fix+same+day;Currently+teaching+a+1.7B+model+to+draft+for+an+8B+one" alt="Typing SVG" />

<a href="mailto:reshaldahima0@gmail.com"><img src="https://img.shields.io/badge/-reshaldahima0@gmail.com-A78BFA?style=flat-square&logo=gmail&logoColor=white&labelColor=1a1b27" alt="Email"></a>
<a href="#"><img src="https://img.shields.io/badge/-LinkedIn-1a1b27?style=flat-square&logo=linkedin&logoColor=0A66C2" alt="LinkedIn"></a>
<a href="https://github.com/vllm-project/vllm/issues/49893"><img src="https://img.shields.io/badge/-vLLM%20%2349893-1a1b27?style=flat-square&logo=github&logoColor=orange" alt="vLLM issue"></a>

</div>

<br>

```
🔭 building     → a quantized drafter for speculative decoding, because a fast
                  model that's wrong less often is more interesting than a
                  slow model that's always right
🌱 chasing      → the line where "more compressed" turns into "actively lying"
                  (currently: 2.158 bits/weight, PPL says I found it)
🐛 filed        → vllm-project/vllm#49893 — maintainer confirmed + PR'd it
                  within hours, which was genuinely a good day
💬 ask me about → quantization, speculative decoding, vLLM internals, or why
                  my desktop assistant wakes up when you clap twice
🏓 off-keyboard → national-level table tennis, 3x inter-college singles champ
```

<br>

## things i've built

### 🔬 [sgt-qat-draft](https://github.com/Resh19S/sgt-qat-draft) — can a quantized dense model draft for a bigger one?

vLLM ships EAGLE-3, a tiny purpose-built head, as its go-to speculative decoding
drafter. I wanted to know if a *quantized* full dense model could hang with it.
Short answer: it gets the *quality* right (draft acceptance basically ties
EAGLE-3, 2.443 vs. 2.474 tokens/step) and gets the *speed* very wrong (0.43x —
slower than not speculating at all). Turns out being a good predictor and being
architecturally cheap are two completely different problems.

Also spent a week in vLLM's source finding out *why* it couldn't load my
compressed checkpoint, filed it properly, and a maintainer had a fix PR open
before I woke up the next day.

<details>
<summary>the numbers, if you're into that</summary>
<br>

- Mean acceptance length: **2.443** (ours) vs. **2.474** (EAGLE-3) — near-parity
- Wall-clock speedup: **0.43x** — a real regression, not a wash
- Root cause: a full 28-layer dense model is too expensive per drafting step,
  independent of how well it's quantized — architecture, not precision
- Filed [vllm#49893](https://github.com/vllm-project/vllm/issues/49893)
  (mixed-precision `compressed-tensors` checkpoints failing to load as
  draft models) → maintainer-confirmed → [PR #49900](https://github.com/vllm-project/vllm/pull/49900)
- Pushed to 2.158 bits/weight to chase the memory story specifically: standalone
  VRAM beats EAGLE-3's in-serving footprint (0.646 GiB vs. 1.047 GiB) — but
  perplexity collapses ~12x getting there. Memory win, quality loss.

</details>

### 📊 mixed-precision-guided targeted QAT — a quantization recovery method, plus a bug story

The pitch: protect the ~15% most damage-prone layers at 4-bit in one calibrated
pass, then spend all your QAT budget only on the layers still stuck at 3-bit,
instead of fine-tuning everything uniformly. It recovers more damage while
training ~30% fewer parameters, and the gap *widens* on a second seed.

The more fun part is the bug hunt buried in the same project: a `random.seed()`
call that seeded a module nothing downstream actually used, silently
producing a fake 411% perplexity swing that survived two separate rounds
of mechanistic analysis before a failed baseline reproduction finally
outed it. Wrote the whole chain up because burying your own dead ends is
how the next person (or you, in six months) re-discovers them the hard way.

<details>
<summary>the numbers, if you're into that</summary>
<br>

- **70.3%** recovery of GPTQ-W3 quantization damage on Qwen3-1.7B vs. full-
  parameter QAT baseline, at ~30% fewer trainable parameters
- Advantage confirmed across two calibration seeds (+4.5pts → +13.5pts)
- Found and corrected a domain-adaptation confound in the standard PTQ→QAT
  recovery metric — the naive formula produced a >100% recovery result,
  which is how the bug surfaced in the first place
- Full reproducibility case study on the `random.seed()` bug: weight-space
  analysis found nothing, output-space analysis found a convincing but
  *wrong* localized signal, and a routine causal-ablation baseline is what
  actually caught it

</details>

### 🎙️ [NOVA](https://github.com/Resh19S/Neural-Orchestrator-for-Virtual-Assistance-NOVA-) — a voice assistant that lives on my desktop, not in a browser tab

Not a wrapper around an API — a three-stage router (regex → fuzzy match →
LLM fallback) that resolves ~80% of what I say in under 10ms without ever
touching the network, plus a dual local/cloud LLM backend, a real audio
pipeline (Whisper, wake-word detection, offline TTS), and enough persistent
memory that it stops asking me where my own folders are after the first time.

<details>
<summary>the numbers, if you're into that</summary>
<br>

- Three-stage command router: regex → semantic fuzzy match → Gemini LLM —
  ~80% of commands resolve locally, <10ms, zero API calls
- Dual-backend LLM routing (local Ollama + Gemini 2.5 Flash) with a real
  multi-turn dialogue state machine for confirmations and follow-ups
- Whisper large-v3 (GPU/FP16) STT, clap/Vosk wake-word detection, offline TTS
- Autonomous research pipeline: one spoken sentence → multi-source search →
  screen-read via Gemini Vision → synthesized report saved to disk

</details>

<br>

## outside the terminal

Selected for **Amazon ML Summer School 2026** (2.1% acceptance, 3,000 out of
150,000+ applicants), and picked for two international Edge AI research
programs — **KTH Royal Institute of Technology** (Stockholm, ~500 applicants
→ 35 seats) and the **EdgeGen PhD Summer School** at the **University of
Helsinki** (with NTNU and Aarhus). Presented desktop-automation research at the
**SVIMS International Research Conference**. Also a fairly serious table tennis
player — semifinalist at nationals, captain of my college team.

<br>

## snake eating my contribution graph, as one does

<div align="center">

![snake animation](https://raw.githubusercontent.com/Resh19S/Resh19S/output/github-contribution-grid-snake-dark.svg#gh-dark-mode-only)
![snake animation](https://raw.githubusercontent.com/Resh19S/Resh19S/output/github-contribution-grid-snake.svg#gh-light-mode-only)

</div>

<br>

<div align="center">

<img src="https://github-readme-stats.vercel.app/api?username=Resh19S&show_icons=true&theme=radical&hide_border=true&count_private=true" alt="GitHub Stats" height="165">
<img src="https://github-readme-streak-stats.herokuapp.com/?user=Resh19S&theme=radical&hide_border=true" alt="GitHub Streak" height="165">

<img src="https://github-readme-activity-graph.vercel.app/graph?username=Resh19S&theme=react-dark&hide_border=true" alt="Activity graph" width="850">

</div>

<br>

## toolbox

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

*quantization (PTQ/QAT) · speculative decoding · vLLM internals · RAG ·
embeddings & vector DBs · ASR (Whisper) · edge AI*

</div>

<br>

<div align="center">

<img src="https://github-readme-quotes.vercel.app/api?type=horizontal&theme=radical" alt="Random dev quote" />

</div>
