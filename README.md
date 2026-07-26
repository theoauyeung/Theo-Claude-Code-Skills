# Theo-Claude-Code-Skills

Personal Claude Code skills for global installation. Each skill lives in its own directory with a `SKILL.md`.

## Installing a skill globally

```bash
# Clone the repo
git clone https://github.com/theoauyeung/Theo-Claude-Code-Skills.git /tmp/theo-skills

# Copy the skill you want
mkdir -p ~/.claude/skills/<skill-name>
cp /tmp/theo-skills/<skill-name>/SKILL.md ~/.claude/skills/<skill-name>/SKILL.md
```

Restart Claude Code to pick it up. The skill will be invokable as `/<skill-name>` or via `use skill <skill-name>`.

## Skills

### `code-explain`

Explains what a piece of code does — requires reading the full project scope first, then produces a two-layer explanation (technical + domain layman's terms) in bullet-point form, grounded in whole-project context. Designed for sports analytics projects where the audience knows baseball but not the codebase internals.

Invoke with `/code-explain` or `use skill code-explain`.
