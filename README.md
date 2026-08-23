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