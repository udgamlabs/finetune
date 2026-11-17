# finetune

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/udgamlabs/finetune/blob/main/finetune_lora.ipynb)

One LoRA fine-tuning notebook for the Udgam Labs models in the transformers format:
[Udgam-384M-Chat](https://huggingface.co/udgamlabs/Udgam-384M-Chat), [Laghu](https://huggingface.co/udgamlabs/Laghu),
[Laghu-Base](https://huggingface.co/udgamlabs/Laghu-Base), [Gargi](https://huggingface.co/udgamlabs/Gargi),
[Vemana-Base](https://huggingface.co/udgamlabs/Vemana-Base) and [Lilavati](https://huggingface.co/udgamlabs/Lilavati).
They are small (150M-400M parameters), so it runs on a free Colab GPU, an Apple Silicon Mac or a laptop CPU in minutes.

## What it does

1. Set `MODEL = "udgamlabs/<model>"`.
2. It builds a small toy task in the notebook (a booking request turned into JSON: name, people, day, time; 200 training
   and 40 held-out rows). Swap in your own `prompt` / `completion` rows.
3. It trains a LoRA adapter (rank 16, all linear layers, loss on the completion only, 150 steps) with plain
   transformers + PEFT: a short training loop, no Trainer, no remote code.
4. It checks the held-out rows before and after, saves the adapter and reloads it.
5. Optional: `push_to_hub` (the adapter's card links back to the base model) and `merge_and_unload()` to make a full
   model for GGUF or MLX.

Chat models (Udgam-384M-Chat, Laghu, Gargi) train on their own chat template; base models (Laghu-Base, Vemana-Base,
Lilavati) on plain text.

## Tested

On an Apple Silicon Mac (MPS, fp32, batch 4 with 2-step accumulation), 14 Nov 2026: every model went from 0/20 to
40/40 exact JSON on the held-out rows (Vemana-Base 34-40/40 across three runs), in 1-2 minutes each. The toy task only
shows that the recipe trains and that the adapter saves and reloads; it is not a measure of the models themselves.

On a Colab T4 GPU (bf16), 17 Nov 2026, from this repository's notebook: Laghu and Lilavati each 0/20 -> 40/40, in about
3.5 and 1.5 minutes. Colab's image ships an old torchao that current peft refuses, so the first cell upgrades torchao
where it is already installed; nothing changes on a Mac or a CPU.

## Licence

Apache-2.0 for this notebook. Each model keeps its own licence; see its card.

---

**About Udgam Labs.** Udgam Labs is an independent AI research lab. We work from first principles: before we build, we understand why things work, whether we are training a model, fine-tuning one or applying AI to a new problem. Udgam (उद्गम) is Sanskrit for origin. [udgamlabs.com](https://udgamlabs.com)
