# PurifyLLM

CLI and pre-commit hook to normalize “smart” punctuation and invisible Unicode produced by LLMs.

## What it does

Replaces common UTF-8 special characters with safe equivalents:

- smart quotes “ ” ‘ ’ -> " and '
- dashes – — − -> -
- ellipsis … -> ...
- non-breaking spaces and thin spaces -> regular space
- zero-width and BOM characters -> removed

You can add your own mappings in the hook config.

## Install (as a hook consumer)

Add this repo to your `.pre-commit-config.yaml`:

```yaml
repos:
- repo: https://github.com/wdroz/PurifyLLM
  rev: v0.0.1
  hooks:
  - id: purify-llm
    # optional: add extra replacements
    args:
    - --map
    - "\u00AB=\""   # « to "
    - --map
    - "\u00BB=\""   # » to "
```

Then install hooks:

```bash
pre-commit install
```

Run on all files at any time:

```bash
pre-commit run --all-files
```

## CLI usage

```bash
purifyllm [--no-defaults] [--map KEY=VALUE ...] [FILES ...]
```

Examples:

```bash
purifyllm README.md
purifyllm --map "\u00B7=-" file.txt
purifyllm --no-defaults --map "…=..." --map "—=-" src/
```

Exit codes:

- 0: no changes needed
- 1: files were modified or an error occurred
