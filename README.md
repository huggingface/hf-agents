# hf-agents : `hf` CLI extension to detect the best model/quant for a user's hardware and then spins up a local coding agent

hf-agents is a HF CLI extension that uses [llmfit](https://github.com/AlexsJones/llmfit) to detect the user's hardware and recommend models they can actually run, then spins up a local [llama.cpp](https://github.com/ggml-org/llama.cpp) server with the most suited model and launches a coding agent with [Pi](https://github.com/badlogic/pi-mono).

Go from "what can my machine run?" to "running a local coding agent" in one command.

## Requirements & References
- How to create a HF CLI extension guide: https://huggingface.co/docs/huggingface_hub/en/guides/cli-extensions
- [llmfit](https://github.com/AlexsJones/llmfit) : hardware detection & model recommendations
- [llama.cpp](https://github.com/ggml-org/llama.cpp) : `llama-server` for local inference
- [Pi](https://github.com/badlogic/pi-mono) : coding agent
- `jq`, `fzf`, `curl`

## Installation

```bash
curl -LsSf https://hf.co/cli/install.sh | bash
hf extensions install hf-agents
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

