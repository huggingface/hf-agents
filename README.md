# hf-agents

A [HF CLI](https://huggingface.co/docs/huggingface_hub/en/guides/cli) extension that detects your hardware, picks the best model you can run locally, and launches a coding agent — in one command.

Uses [llmfit](https://github.com/AlexsJones/llmfit) for hardware detection, [llama.cpp](https://github.com/ggml-org/llama.cpp) for inference, and [Pi](https://github.com/badlogic/pi-mono) as the coding agent.

## Quickstart

```bash
# install the HF CLI
curl -LsSf https://hf.co/cli/install.sh | bash
# install the hf-agents extension
hf extensions install hf-agents
# run a local coding agent
hf agents run pi
```

That's it. The script auto-detects your hardware, picks the best coding model, downloads it, starts a local server, and drops you into a coding agent.

## What happens when you run `hf agents run pi`

1. **Installs missing dependencies** — auto-installs via Homebrew on macOS; prints install hints on Linux
2. **Detects your hardware** via llmfit (RAM, VRAM, CPU/GPU)
3. **Picks the best GGUF model** for coding that fits your machine
4. **Downloads and starts llama-server** — shows download progress and loading status
5. **Configures and launches Pi** — connected to your local server
6. **Cleans up on exit** — kills the server and restores Pi's config

## Usage

### Run a coding agent (auto-picks the best model)

```bash
hf agents run pi
```

### Pick from the top N models interactively

```bash
hf agents run pi --top 5
```

Opens an interactive picker ([fzf](https://github.com/junegunn/fzf)) showing model name, quantization, parameters, speed, and fit score.

### Use a specific model

```bash
hf agents run pi unsloth/Qwen3-Coder-Next-GGUF:q4_k_m
```

Skips hardware detection entirely and goes straight to download + launch.

### Forward arguments to Pi

Any flags starting with `-` (other than `--top`) are passed through to Pi:

```bash
hf agents run pi --print "explain this codebase"
```

### Hardware detection (llmfit passthrough)

```bash
hf agents fit system                # show detected hardware
hf agents fit recommend -n 5        # top 5 models for your hardware
hf agents fit search "qwen"         # search models
hf agents fit recommend --use-case coding --min-fit good
```

All [llmfit](https://github.com/AlexsJones/llmfit) subcommands work directly.

## Dependencies

| Tool | macOS | Linux |
|------|-------|-------|
| [llmfit](https://github.com/AlexsJones/llmfit) | Auto-installed via Homebrew | Auto-installed via install script |
| [llama-server](https://github.com/ggml-org/llama.cpp) | Auto-installed via Homebrew | Manual install required |
| [Pi](https://github.com/badlogic/pi-mono) | Auto-installed via npm | Auto-installed via npm |
| `jq` | Auto-installed via Homebrew | Manual install required |
| `fzf` | Auto-installed via Homebrew | Manual install required (only needed with `--top`) |
| `curl` | Pre-installed | Pre-installed |
| `node`/`npm` | Auto-installed via Homebrew | Manual install required |

On Linux, the script detects your package manager (apt, dnf, pacman, zypper, apk) and prints the exact install command for any missing dependency.

## Environment variables

| Variable | Default | Description |
|----------|---------|-------------|
| `LLAMA_SERVER_PORT` | `8080` | Port for llama-server |
| `LLAMA_SERVER_TIMEOUT` | `600` | Max seconds to wait for server startup |
| `HF_TOKEN` | *(from `hf auth token`)* | For downloading gated models |

## Good to know

- **Server reuse**: if llama-server is already running on the target port, it's reused — no model selection or download happens.
- **Logs**: llama-server output goes to `/tmp/hf-agents-llama-server.log`.
- **Pi config**: temporarily writes to `~/.pi/agent/models.json` during the session, restores the original on exit.
- **Press Esc** during server startup to cancel.

## License

Apache 2.0
