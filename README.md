# ctrlBolt

`ctrlBolt` is a reusable numbered-prompt chain runner for AI coding CLIs.

It replaces one-off scripts like `run_codex_chain.sh` with a standardized runner that supports multiple providers, per-project state, resume, logs, git checkpoints, tests, and clean usage-limit exits.

## Supported adapters

| Provider | Default command strategy |
|---|---|
| `codex` | `codex exec -` |
| `claude` | `claude -p "<prompt>"` |
| `gemini` | `gemini -p "<prompt>"` |
| `aider` | `aider --yes-always --no-auto-commits --message "<prompt>"` |
| `ollama` | `ollama run <model>` with prompt on stdin |
| `custom` | Your command template |

The `custom` adapter is the universal escape hatch.

## Install

```bash
mkdir -p ~/.local/bin
cp ctrlBolt ~/.local/bin/ctrlBolt
chmod +x ~/.local/bin/ctrlBolt

# optional lowercase alias
ln -sf ~/.local/bin/ctrlBolt ~/.local/bin/ctrlbolt
```

Make sure `~/.local/bin` is on PATH:

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

## Use with macsvg

```bash
cd ~/bb/projects

ctrlBolt \
  --provider codex \
  --prompt-dir ~/bb/projects/macsvg_prompt_kit \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/macsvg \
  --danger
```

Resume:

```bash
ctrlBolt --resume --project-dir ~/bb/projects/macsvg
```

## Use with Claude Code

```bash
ctrlBolt \
  --provider claude \
  --prompt-dir ~/bb/projects/macsvg_prompt_kit \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/macsvg \
  --danger
```

`--danger` maps to Claude Code's bypass permission mode.

## Use with Gemini CLI

```bash
ctrlBolt \
  --provider gemini \
  --prompt-dir ~/bb/projects/macsvg_prompt_kit \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/macsvg
```

## Use with Aider

```bash
ctrlBolt \
  --provider aider \
  --prompt-dir ~/bb/projects/macsvg_prompt_kit \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/macsvg
```

## Use with Ollama local model

Ollama alone is usually advisory/review output, not a full file-editing coding agent. It can still run prompt chains and save logs.

```bash
ctrlBolt \
  --provider ollama \
  --model qwen2.5-coder:7b \
  --prompt-dir ~/bb/projects/macsvg_prompt_kit \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/macsvg
```

## Use any local/custom agent

```bash
ctrlBolt \
  --provider custom \
  --provider-cmd 'my-agent --project {project_dir} --prompt-file {prompt_file}' \
  --prompt-dir ~/bb/projects/my_prompt_kit \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/mytool
```

Available placeholders:

```text
{prompt_file}
{prompt_text_file}
{project_dir}
{root_dir}
{log_file}
{state_dir}
{model}
```

## Per-project state

Default state location:

```text
<root-dir>/.ctrlBolt/projects/<project-name>/
  state.json
  state.env
  logs/
```

Examples:

```text
~/bb/projects/.ctrlBolt/projects/macsvg/
~/bb/projects/.ctrlBolt/projects/deepMerge/
~/bb/projects/.ctrlBolt/projects/apdif/
```

That means multiple projects can be paused/resumed independently.

## Prompt kit format

Number prompt files like this:

```text
00_MASTER_ARCHITECTURE.md
01_CORE.md
02_FEATURES.md
03_REFINER.md
```

`ctrlBolt` auto-sorts and runs them in order.

## Useful flags

```bash
--start-at 03_REFINER.md
--run-tests
--no-commit
--continue-on-error
--timeout 3600
--dry-run
--extra-arg VALUE
```

## Usage-limit behavior

If the provider output includes quota/rate/usage-limit language, `ctrlBolt` saves state and exits with code `75`.

Resume later:

```bash
ctrlBolt --resume --project-dir ~/bb/projects/macsvg
```
