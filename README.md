# ai-misc

## llama

Lighter than ollama I think.

```
winget install llama.cpp
```

```
Found llama.cpp [ggml.llamacpp] Version b10331
This application is licensed to you by its owner.
Microsoft is not responsible for, nor does it grant any licenses to, third-party packages.
This package requires the following dependencies:
  - Packages
      Microsoft.VCRedist.2015+.x64
Downloading https://github.com/ggml-org/llama.cpp/releases/download/b10331/llama-b10331-bin-win-vulkan-x64.zip
  ██████████████████████████████  32.5 MB / 32.5 MB
Successfully verified installer hash
Extracting archive...
Successfully extracted archive
Starting package install...
Path environment variable modified; restart your shell to use the new value.
Command line alias added: "llama-batched-bench"
Command line alias added: "llama-bench"
Command line alias added: "llama-cli"
Command line alias added: "llama-gguf-split"
Command line alias added: "llama-imatrix"
Command line alias added: "llama-mtmd-cli"
Command line alias added: "llama-perplexity"
Command line alias added: "llama-quantize"
Command line alias added: "llama-server"
Command line alias added: "llama-tokenize"
Command line alias added: "llama-tts"
Successfully installed
```
? Try cpu instead of vulkan due to small GPU memory on atacama?

Intel Graphics Command Center on atacama says that my Intel HD Graphics 620
has 128 MB of dedicated graphics memory and 16 MB of shared memory.

```
llama cli -hf ggml-org/Qwen3.5-0.8B-GGUF
```
Downloaded first time, just loaded after that.

```
llama serve -hf ggml-org/Qwen3.5-0.8B-GGUF
```
Web interface.

```
llama cli -hf ggml-org/gemma-3-1b-pt-GGUF
```

Coding!
```
llama cli -hf Qwen/Qwen2.5-Coder-1.5B-Instruct-GGUF
```

```
llama serve -hf google/gemma-4-E2B-it-qat-q4_0-gguf
```

Other possible small models to try
- google/gemma-4-E2B
- google/gemma-4-E2B-it-qat-q4_0-gguf
- Qwen/Qwen3-0.6B
- ggml-org/Qwen3-0.6B-GGUF
- ggml-org/Qwen3-1.7B-GGUF
- Qwen/Qwen3.5-4B
- Qwen/Qwen3.5-9B
- "Qwen 2.5 Coder 3B Instruct"
- Qwen/Qwen2.5-Coder-1.5B-Instruct-GGUF
- Qwen/Qwen3-Coder-30B-A3B-Instruct

Listing cached (downloaded) models (see ~/.cache/huggingface) with `llama fit-params -cl`
```
llama fit-params -cl
```

Listing models from Huggingface repository or cache with `hf` command.
```
PS D:\src\github> python -m venv .\ai-misc\
PS D:\src\github> .\ai-misc\Scripts\activate
(ai-misc) PS D:\src\github> python -m pip install -U "huggingface_hub[cli]"
(ai-misc) PS D:\src\github> hf models list --help
(ai-misc) PS D:\src\github> hf models list --author ggml-org --search Qwen --no-truncate | less -S
(ai-misc) PS D:\src\github> hf models list --search gemma --no-truncate | less -S
(ai-misc) PS D:\src\github> hf cache list
(ai-misc) PS D:\src\github> hf download Qwen/Qwen2.5-Coder-1.5B-Instruct-GGUF
```

```
llama fit-params -hf ggml-org/Qwen3.5-0.8B-GGUF
llama fit-params -hf ggml-org/Qwen3.5-0.8B-GGUF -fitp on
```

## llama.vscode extension

Hugging Face FIM (fill in the middle) recommended local model collection:

https://huggingface.co/collections/ggml-org/llamavim

## Model ideas

https://kimi.ai
- Non fim
```
llama cli -hf bartowski/google_gemma-4-E2B-it-GGUF:Q4_K_M
```
- Fim
```
llama serve -hf Qwen/Qwen2.5-Coder-3B-Instruct-GGUF:Q4_K_M
```

```
llama serve -hf ggml-org/Qwen2.5-Coder-0.5B-Q8_0-GGUF -c 12288 -ub 256 -b 256 --cache-reuse 256
```

```
llama serve -hf bartowski/google_gemma-4-E2B-it-GGUF:Q4_K_M -ub 1024 -b 1024 --ctx-size 0 --cache-reuse 256 -np 2 --port 8011 --host 127.0.0.1
```

## From https://kimi.ai

Here are both tables with a **llama `-hf` command** column added. I preserved the original recommendations as closely as possible, but I corrected two model names to their actual released counterparts so the HF repos are real and reachable:

---

### General Chat / Instruction Models

| Model | Size (Q4_K_M) | RAM Used | Speed* | Best For | `llama -hf` parameter |
|-------|--------------|----------|--------|----------|----------------------|
| **Gemma 4 E2B** | ~1.5 GB | ~3 GB | Fastest (~8–12 tok/s) | Quick answers, low latency | `bartowski/google_gemma-4-E2B-it-GGUF:Q4_K_M` |
| **TinyLlama 1.1B** | ~0.7 GB | ~2 GB | Very fast | Simple tasks, autocomplete | `TheBloke/TinyLlama-1.1B-Chat-v1.0-GGUF:Q4_K_M` |
| **Llama 3.2 3B** | ~2.0 GB | ~3.5 GB | Moderate (~5–8 tok/s) | General chat, broad compatibility | `unsloth/Llama-3.2-3B-Instruct-GGUF:Q4_K_M` |
| **SmolLM2 1.7B** | ~1.1 GB | ~2.5 GB | Moderate | Multilingual, fast reasoning | `HuggingFaceTB/SmolLM2-1.7B-Instruct-GGUF:Q4_K_M` |
| **Phi-4 Mini** (3.8B) | ~2.3 GB | ~4 GB | Moderate (~4–8 tok/s) | STEM, coding, structured output | `bartowski/Phi-4-mini-instruct-GGUF:Q4_K_M` |
| **Qwen3 4B** | ~2.5 GB | ~4.5 GB | Moderate | Best all-rounder for CPU | `Qwen/Qwen3-4B-GGUF:Q4_K_M` |
| **Mistral 7B** | ~4.5 GB | ~6–7 GB | Slow (~2–4 tok/s) | Better quality if you're patient | `TheBloke/Mistral-7B-Instruct-v0.2-GGUF:Q4_K_M` |

---

### FIM (Fill-in-the-Middle) Coding Models

| Model | Size | FIM Format | Notes | `llama -hf` parameter |
|-------|------|-----------|-------|----------------------|
| **Qwen2.5-Coder 1.5B** | ~1 GB (Q4) | Yes, `<fim_prefix>` etc. | Excellent code quality for its size; runs very fast on your hardware | `Qwen/Qwen2.5-Coder-1.5B-Instruct-GGUF:Q4_K_M` |
| **Qwen2.5-Coder 3B** | ~2 GB (Q4) | Yes | Best balance of quality and speed for your machine | `Qwen/Qwen2.5-Coder-3B-Instruct-GGUF:Q4_K_M` |
| **CodeLlama 7B** | ~4 GB (Q4) | Yes, `<PRE>`, `<SUF>`, `<MID>` | Older but solid; 7B might be slow on 2017 CPU | `TheBloke/CodeLlama-7B-Instruct-GGUF:Q4_K_M` |
| **DeepSeek-Coder 1.3B** | ~0.8 GB (Q4) | Yes | Tiny but surprisingly capable for simple completions | `TheBloke/deepseek-coder-1.3b-instruct-GGUF:Q4_K_M` |
| **StarCoder2 3B** | ~2 GB (Q4) | Yes | Good multilingual coding, but Qwen2.5-Coder 3B generally outperforms it now | `TheBloke/starcoder2-3b-GGUF:Q4_K_M` |

---

### Full command template

Append the same flags to any row above:

```bash
./llama-cli \
  -hf <repo-from-table> \
  -ngl 0 \
  -t 4 \
  -c 4096 \
  -cnv
```

**Notes:**
- **`-ngl 0`** is required on your machine to force CPU-only mode.
- **`-t 4`** assumes a 4-thread CPU; adjust to your physical core count.
- The repos listed are **community GGUF conversions** (bartowski, TheBloke, unsloth, HuggingFaceTB, Qwen official). If a specific repo is ever unreachable, search Hugging Face for the model name + `GGUF` + `Q4_K_M` — there are usually multiple mirrors.