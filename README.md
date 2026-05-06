# nextjs-config-skills

A collection of reusable [Claude Code](https://claude.ai/code) skills for modern web development. Each skill encodes opinionated, production-ready configurations that Claude can apply to new or existing projects.

## Skills

### `nextjs-config-skills`

Sets up a TypeScript + Next.js project with:

- **`tsconfig.json`** — ES2022 target, bundler module resolution, `@/*` path alias, strict mode
- **`biome.json`** — replaces ESLint + Prettier with a single fast tool; single quotes, 100-char line width, organized imports
- **`package.json` scripts** — `dev`, `build`, `start`, `lint`, `format`, `check`, `typecheck`

**Trigger:** Use when creating a new Next.js project, configuring `tsconfig`, setting up Biome, or standardizing `package.json` scripts.

## Usage

Install the plugin via Claude Code:

```bash
claude plugin marketplace add gpocas/skills-maker
claude plugin install nextjs-config-skills@skills-maker
```

Then invoke the skill in any project conversation:

```
/nextjs-config-skills
```

Claude will apply the configuration files to the current project directory.

## Structure

```
skills-maker/
├── .claude-plugin/
│   ├── plugin.json        # Plugin metadata and skill registration
│   └── marketplace.json   # Marketplace listing
└── skills/
    └── nextjs-config-skills/
        └── SKILL.md       # Skill prompt with config templates
```

## Adding a skill

1. Create a directory under `skills/` with your skill name
2. Add a `SKILL.md` with a YAML frontmatter block (`name`, `description`) followed by the skill instructions
3. Register it in `.claude-plugin/plugin.json` under `"skills"`

## License

MIT
