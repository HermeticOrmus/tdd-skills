<p align="center">
  <img src="https://ormus.solutions/mascot/pixellab_liquid_to_ouroboros.gif" alt="TDD Skills" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">TDD Skills</h1>

<p align="center">
  <em>A Claude Code skill for test-driven development — the Red-Green-Refactor cycle, test templates, and anti-patterns, for JavaScript/TypeScript and Python.</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/tdd-skills/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/tdd-skills?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/tdd-skills/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/tdd-skills?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/tdd-skills/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/tdd-skills?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/Claude_Code-aa8142?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
</p>

---

## The problem

When code is generated faster than it is read, untested code paths accumulate quietly. A function that looks right and compiles is not the same as a function whose behavior is pinned by a test. Test-driven development inverts the order: the test comes first, so the code that follows has a definition of done before it exists.

TDD gives you:

- Clear requirements, because the test states what the code must do before the code is written.
- Better design, because writing the test first exposes awkward APIs early.
- Coverage as a byproduct, because every behavior arrives with its test.
- Confidence to refactor, because green tests prove behavior did not change.
- Documentation, because the tests show how the code is meant to be used.

## Red-Green-Refactor

| Phase | What you do | Why |
|---|---|---|
| Red | Write one failing test for the simplest unhandled case | A test that never fails proves nothing |
| Green | Write the minimal code that makes it pass | Over-building hides untested paths |
| Refactor | Improve the code without changing behavior; tests stay green | Green code can always be cleaned up safely |

Repeat the cycle one test at a time until the feature is complete. The order is the discipline.

Full discipline: [`CLAUDE.md`](CLAUDE.md). Worked walkthrough and patterns: [`EXAMPLES.md`](EXAMPLES.md).

## Install

### As a project CLAUDE.md

Drop [`CLAUDE.md`](CLAUDE.md) at the root of your repository. Claude Code picks it up automatically. Merge with existing project instructions if any.

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/HermeticOrmus/tdd-skills/main/CLAUDE.md
```

### As a Claude Code skill

The same content is packaged as a skill under [`skills/tdd/`](skills/tdd/) for `~/.claude/skills/`. Copy or symlink the folder there and Claude Code will load it on demand.

```bash
cp -r skills/tdd ~/.claude/skills/tdd
```

### As a `/tdd` slash command

Drop the discipline into `~/.claude/commands/tdd.md` to invoke it as `/tdd` from any session.

```bash
curl -o ~/.claude/commands/tdd.md https://raw.githubusercontent.com/HermeticOrmus/tdd-skills/main/CLAUDE.md
```

### In Cursor

See [`CURSOR.md`](CURSOR.md) for the Cursor-rule equivalent at [`.cursor/rules/tdd.mdc`](.cursor/rules/tdd.mdc).

### In other AI coding tools

If your tool reads a single instruction file at the project root, copy `CLAUDE.md` to whatever name your tool expects (`AGENTS.md`, `INSTRUCTIONS.md`, etc.).

## See also

- [`riper-workflow-skills`](https://github.com/HermeticOrmus/riper-workflow-skills): Research, Innovate, Plan, Execute, Review. TDD lives inside its Execute phase.
- [`vibe-engineer-skills`](https://github.com/HermeticOrmus/vibe-engineer-skills): how to direct AI codegen well. A failing test first is the sharpest scoped prompt there is.

## Contributing

PRs welcome, especially additional worked examples in [`EXAMPLES.md`](EXAMPLES.md), templates for more languages (Go, Rust, Java), and adaptations of `CURSOR.md` for other AI coding tools (Windsurf, Cline, Aider, Continue).

## License

MIT. Use it, fork it, merge it into your own CLAUDE.md.
