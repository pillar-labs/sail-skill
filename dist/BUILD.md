# Rebuilding the distribution archives

The archives in this folder are built from `../sail/skills/sail/` (the skill source). Rebuild them after editing the skill.

## `sail-skill.zip` (plain zip — Cowork / Codex)

```bash
cd ../sail/skills
zip -r -X ../../dist/sail-skill.zip sail -x '*.DS_Store'
```

Unzips to a top-level `sail/` folder containing `SKILL.md` and `references/`.

## `sail.skill` (Claude skill package — Claude.ai / Desktop)

Same contents, `.skill` extension. If `skill-creator` and `pyyaml` are available:

```bash
python -m scripts.package_skill ../sail/skills/sail ../dist
```

Otherwise build it directly (both archives share the same layout — you can simply copy):

```bash
cp sail-skill.zip sail.skill
```

Both archives exclude `.DS_Store` and `__pycache__`.
