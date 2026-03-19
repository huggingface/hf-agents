# hf-agents

A HF CLI extension that uses [llmfit](https://github.com/AlexsJones/llmfit) to detect your hardware and recommend models you can actually run, then spins up a local [llama.cpp](https://github.com/ggml-org/llama.cpp) server with the most suited model and launches a coding agent with [Pi](https://github.com/badlogic/pi-mono).

## Quickstart

```bash
# install the HF CLI
curl -LsSf https://hf.co/cli/install.sh | bash
# install the hf-agents extension
hf extensions install hf-agents
# run a local coding agent
hf agents run pi
```

First run downloads a model and starts a server — takes a few minutes depending on your connection. Subsequent runs reuse the cached model and start in seconds. On macOS, dependencies auto-install. On Linux, the script tells you what to install.

## Usage

```bash
hf agents run pi                                        # auto-pick best model
hf agents run pi --top 5                                # interactive model picker
hf agents run pi unsloth/Qwen3-Coder-Next-GGUF:q4_k_m  # use a specific model
hf agents run pi --print "explain this codebase"        # pass args through to Pi

hf agents fit system                                    # show your hardware
hf agents fit recommend -n 5                            # top 5 recommended models
```

Any flags starting with `-` (other than `--top`) are forwarded to Pi.

## Configuration

<details>
<summary>Environment variables & operational notes</summary>

**Environment variables**

| Variable | Default | Description |
|----------|---------|-------------|
| `LLAMA_SERVER_PORT` | `8080` | Port for llama-server |
| `LLAMA_SERVER_TIMEOUT` | `600` | Max seconds to wait for server startup |
| `HF_TOKEN` | *(from `hf auth token`)* | For downloading gated models |

**Good to know**

- **Server reuse**: if llama-server is already running on the target port, it's reused — no model selection or download happens.
- **Logs**: llama-server output goes to `/tmp/hf-agents-llama-server.log`.
- **Pi config**: temporarily writes to `~/.pi/agent/models.json` during the session, restores the original on exit.
- **Press Esc** during server startup to cancel.

</details>

---

Apache 2.0
