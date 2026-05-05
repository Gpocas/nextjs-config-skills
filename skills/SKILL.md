---
name: nextjs-typescript
description: Set up TypeScript + Next.js project configuration. Use when creating a new Next.js project, configuring tsconfig, setting up Biome for linting/formatting, or defining standard package.json scripts.
---

# Next.js + TypeScript — Project Configuration

## tsconfig.json

```json
{
  "compilerOptions": {
    "target": "es2022",
    "lib": ["dom", "dom.iterable", "esnext"],
    "module": "esnext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "allowJs": true,
    "strict": true,
    "skipLibCheck": true,
    "noEmit": true,
    "jsx": "react-jsx",
    "incremental": false,
    "paths": {
      "@/*": ["./src/*"]
    },
    "plugins": [{ "name": "next" }]
  },
  "include": [
    "**/*.ts",
    "**/*.tsx",
    "next-env.d.ts",
    ".next/types/**/*.ts"
  ],
  "exclude": ["node_modules"]
}
```

Key decisions:
- `moduleResolution: "bundler"` — correct for Next.js with Turbopack/webpack, avoids Node16 quirks
- `isolatedModules: true` — required by Next.js transpiler; each file must be independently compilable
- `incremental: false` — avoids stale `.tsbuildinfo` cache issues in CI and monorepos
- `noEmit: true` — Next.js handles compilation; TypeScript is type-check only
- `paths: { "@/*": ["./src/*"] }` — standard alias, must be mirrored in `next.config.ts`

## biome.json

Biome replaces both ESLint and Prettier. One tool, one config, faster.

```json
{
  "$schema": "https://biomejs.dev/schemas/2.4.10/schema.json",
  "vcs": {
    "enabled": true,
    "clientKind": "git",
    "useIgnoreFile": true
  },
  "files": {
    "includes": ["**", "!**/dist", "!**/.next", "!**/node_modules"]
  },
  "formatter": {
    "enabled": true,
    "indentStyle": "space",
    "indentWidth": 2,
    "lineWidth": 100,
    "lineEnding": "lf"
  },
  "javascript": {
    "formatter": {
      "quoteStyle": "single",
      "trailingCommas": "all",
      "arrowParentheses": "asNeeded"
    }
  },
  "css": {
    "formatter": {
      "enabled": true,
      "indentStyle": "space",
      "indentWidth": 2
    }
  },
  "linter": {
    "enabled": true,
    "rules": {
      "recommended": true,
      "correctness": {
        "useExhaustiveDependencies": "off"
      },
      "suspicious": {
        "noExplicitAny": "off"
      },
      "performance": {
        "noImgElement": "off"
      },
      "complexity": {
        "noImportantStyles": "off"
      }
    }
  },
  "assist": {
    "enabled": true,
    "actions": {
      "source": {
        "organizeImports": "on"
      }
    }
  }
}
```

Rules disabled and why:
- `useExhaustiveDependencies: "off"` — Next.js hooks patterns (e.g. `useRouter`, `useParams`) don't always satisfy this rule correctly
- `noExplicitAny: "off"` — pragmatic; tighten per project as types mature
- `noImgElement: "off"` — Biome flags `<img>` but Next.js `<Image>` already enforces optimization
- `noImportantStyles: "off"` — needed for Tailwind utility overrides

## package.json scripts

```json
{
  "scripts": {
    "dev": "next dev --turbo",
    "build": "next build",
    "start": "next start",
    "lint": "biome lint .",
    "format": "biome format --write .",
    "check": "biome check --write",
    "typecheck": "tsc --noEmit"
  }
}
```

## Dev dependencies

```bash
# With bun (preferred)
bun add -d @biomejs/biome typescript @types/node @types/react @types/react-dom

# With npm
npm install -D @biomejs/biome typescript @types/node @types/react @types/react-dom
```
