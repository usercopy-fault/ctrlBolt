# ctrlBolt quick start

## Install

```bash
cd ~/Downloads
unzip -o ctrlBolt_kit.zip
mkdir -p ~/.local/bin
cp ctrlBolt_kit/ctrlBolt ~/.local/bin/ctrlBolt
chmod +x ~/.local/bin/ctrlBolt
ln -sf ~/.local/bin/ctrlBolt ~/.local/bin/ctrlbolt
```

## Verify

```bash
ctrlBolt providers
```

## Run macsvg prompts with Codex

```bash
cd ~/bb/projects

ctrlBolt \
  --provider codex \
  --prompt-dir ~/bb/projects/macsvg_prompt_kit \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/macsvg \
  --danger
```

## Resume

```bash
ctrlBolt --resume --project-dir ~/bb/projects/macsvg
```

## Run deepMerge prompts with Codex

```bash
cd ~/bb/projects

ctrlBolt \
  --provider codex \
  --prompt-dir ~/bb/projects/deepMerge_prompt_kit \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/deepMerge \
  --danger
```

## Run any future kit

```bash
ctrlBolt \
  --provider codex \
  --prompt-dir ~/bb/projects/NEW_PROMPT_KIT \
  --root-dir ~/bb/projects \
  --project-dir ~/bb/projects/NEW_PROJECT \
  --danger
```
