# Using this repo with Cursor

This project includes a Cursor project rule so the TDD discipline is available when you implement a feature test-first here.

## In this repository

1. Open the folder in Cursor.
2. The rule [`.cursor/rules/tdd.mdc`](.cursor/rules/tdd.mdc) is committed with `alwaysApply: false`, so it applies situationally, when you tell Cursor you are implementing a feature or fixing a bug test-first. You can also attach it explicitly.
3. Confirm under Settings, Rules, where `tdd` should appear.

## Use the same rule in another project

Cursor (recommended): copy `.cursor/rules/tdd.mdc` into that project's `.cursor/rules/` directory (create the folders if needed). Merge with existing rules as you like.

Other AI coding tools: if a stack only supports a root instruction file, copy [`CLAUDE.md`](CLAUDE.md) into that project instead, or merge its contents into your existing instructions. Most modern AI coding tools (Claude Code, Continue, Cline, Windsurf, Aider) read a root-level instruction file.

## Optional: personal Agent Skills

If you want the same content as a reusable skill under `~/.cursor/skills`, use [`skills/tdd/SKILL.md`](skills/tdd/SKILL.md). Copy or symlink it into your personal skills directory.
