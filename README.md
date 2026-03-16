# hf-agents

HF CLI extension that bridges [llmfit](https://github.com/Mozilla-Ocho/llmfit) hardware detection with [Pi](https://github.com/plandex-ai/pi) coding agent via a local [llama.cpp](https://github.com/ggml-org/llama.cpp) server.

Go from "what can my machine run?" to "running a local coding agent" in one command.

## Requirements & References

- [llmfit](https://github.com/AlexsJones/llmfit) : hardware detection & model recommendations
- [llama.cpp](https://github.com/ggml-org/llama.cpp) : `llama-server` for local inference
- [Pi](https://github.com/plandex-ai/pi) : coding agent
- [huggingface_hub](https://github.com/huggingface/huggingface_hub) : CLI (`hf`)
- `jq`, `fzf`, `curl`

## Installation

```bash
hf extension install hf-agents
```

## Usage

### Hardware detection & model recommendations

Passes through to `llmfit` — all llmfit subcommands work:

```bash
hf agents fit recommend -n 5      # top 5 models for your hardware
hf agents fit system              # show detected hardware
hf agents fit search "qwen"       # search models
hf agents fit recommend --use-case coding --min-fit good
```

### Run a local coding agent

Detects your hardware, lets you pick a model, starts llama-server, and launches Pi:

```bash
hf agents run pi                  # interactive model selection + coding agent
hf agents run pi --print "hello"  # non-interactive mode, forward args to Pi
```

If llama-server is already running on the target port, it reuses it and skips model selection.

## Environment Variables

| Variable | Default | Purpose |
|----------|---------|---------|
| `LLAMA_SERVER_PORT` | `8080` | Port for llama-server |
| `HF_TOKEN` | *(from `hf` CLI)* | For downloading gated models |

