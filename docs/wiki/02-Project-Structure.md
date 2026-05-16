# Project Structure

Understand the organization and layout of the Solana.com monorepo.

## Directory Tree Overview

```
solana.com/
├── .github/                      # GitHub workflows & CI/CD
├── .devcontainer/                # Dev container configuration
├── .cursor/                       # Cursor IDE settings
├── .husky/                        # Git hooks
├── .vscode/                       # VS Code settings
├── docs/
│   └── wiki/                      # This documentation
├── apps/                          # Next.js applications
│   ├── web/                       # Main website (port 3000)
│   ├── media/                     # News & podcasts (port 3002)
│   ├── docs/                      # Developer docs (port 3003)
│   ├── templates/                 # Developer templates (port 3001)
│   └── accelerate/                # Accelerate program
├── packages/                      # Shared packages
│   ├── ui/                        # React components
│   ├── ui-chrome/                 # Header/footer components
│   ├── i18n/                      # Internationalization
│   ├── config-eslint/             # ESLint configuration
│   └── config-typescript/         # TypeScript configuration
├── turbo/                         # Turbo configuration
├── pnpm-workspace.yaml            # Workspace definition
├── turbo.json                     # Turbo build config
├── package.json                   # Root dependencies
├── tsconfig.json                  # Root TypeScript config
├── .eslintrc.js                   # Root ESLint config
├── .prettierrc                    # Prettier config
├── pnpm-lock.yaml                 # Dependency lock file
└── README.md                      # Main documentation
```

---

## Apps Directory

Each app is a standalone Next.js application with its own configuration.

### apps/web
**Main website for Solana.com**

```
apps/web/
├── public/                        # Static assets
├── src/
│   ├── app/                       # Next.js App Router pages
│   │   ├── layout.tsx             # Root layout
│   │   ├── page.tsx               # Home page
│   │   ├── [locale]/              # i18n wrapper
│   │   │   ├── page.tsx
│   │   │   ├── validators/
│   │   │   ├── ecosystem/
│   │   │   └── ...
│   │   └── api/                   # API routes
│   ├── components/                # React components
│   ├── lib/                       # Utilities
│   └── styles/                    # CSS/Tailwind
├── next.config.ts                 # Next.js configuration
├── package.json                   # App dependencies
└── tsconfig.json                  # TypeScript config
```

**Key Features:**
- Main landing page
- Validators directory
- Ecosystem information
- Multi-language support

### apps/media
**News articles and podcasts**

```
apps/media/
├── content/                       # MDX & JSON content
│   ├── posts/                     # News articles
│   ├── podcasts/                  # Podcast metadata
│   ├── authors/                   # Author profiles
│   ├── tags/                      # Content tags
│   ├── categories/                # Categories
│   └── global/                    # Global settings
├── public/                        # Static assets
├── src/
│   ├── app/
│   │   ├── layout.tsx
│   │   ├── [locale]/
│   │   │   ├── news/              # News listing/detail
│   │   │   └── podcasts/          # Podcast pages
│   │   └── admin/                 # TinaCMS admin
│   ├── components/
│   ├── lib/
│   └── styles/
├── tina/                          # TinaCMS configuration
├── next.config.ts
├── package.json
└── tsconfig.json
```

**Key Features:**
- TinaCMS content management
- MDX content rendering
- RSS feed integration
- Multi-language support

### apps/docs
**Developer documentation**

```
apps/docs/
├── content/                       # Documentation files
│   ├── getting-started/
│   ├── api-reference/
│   ├── guides/
│   └── ...
├── src/
│   ├── app/
│   │   ├── [locale]/
│   │   │   ├── docs/
│   │   │   └── api/
│   ├── components/
│   └── lib/
├── next.config.ts
├── package.json
└── tsconfig.json
```

**Key Features:**
- Developer guides
- API documentation
- Code examples
- Search functionality

### apps/templates
**Developer template showcase**

```
apps/templates/
├── src/
│   ├── app/
│   │   ├── [locale]/
│   │   │   ├── page.tsx           # Template list
│   │   │   └── [id]/              # Template detail
│   ├── components/
│   ├── lib/
│   │   └── github.ts              # GitHub integration
│   └── styles/
├── next.config.ts
├── package.json
└── tsconfig.json
```

**Key Features:**
- GitHub template integration
- Dynamic OG image generation
- Multi-language routes
- Template showcase

### apps/accelerate
**Accelerate program information**

```
apps/accelerate/
├── src/
│   ├── app/
│   ├── components/
│   └── lib/
├── next.config.ts
├── package.json
└── tsconfig.json
```

---

## Packages Directory

Shared libraries used across multiple apps.

### packages/ui
**React component library**

```
packages/ui/
├── src/
│   ├── components/
│   │   ├── Button/
│   │   ├── Card/
│   │   ├── Modal/
│   │   ├── Input/
│   │   └── ...
│   ├── hooks/                     # Custom React hooks
│   ├── types/                     # TypeScript types
│   ├── styles/                    # CSS/Tailwind
│   └── index.ts                   # Exports
├── package.json
│   └── "name": "@solana-com/ui"
└── tsconfig.json
```

**Consumed by:** All apps

### packages/ui-chrome
**Header and footer components**

```
packages/ui-chrome/
├── src/
│   ├── components/
│   │   ├── Header/
│   │   ├── Footer/
│   │   └── Navigation/
│   ├── url-config.ts              # URL routing logic
│   ├── types/
│   └── index.ts
├── package.json
│   └── "name": "@solana-com/ui-chrome"
└── tsconfig.json
```

**Consumed by:** All apps (provides consistent navigation)

### packages/i18n
**Internationalization utilities**

```
packages/i18n/
├── src/
│   ├── config.ts                  # i18n configuration
│   ├── locales/
│   │   ├── en.json                # English translations
│   │   ├── es.json                # Spanish translations
│   │   └── ...
│   ├── hooks/
│   │   └── useTranslation.ts
│   └── index.ts
├── package.json
│   └── "name": "@workspace/i18n"
└── tsconfig.json
```

**Consumed by:** All apps (multi-language support)

### packages/config-eslint
**Shared ESLint configuration**

```
packages/config-eslint/
├── index.js                       # ESLint config
├── package.json
│   └── "name": "@solana-com/eslint-config"
└── tsconfig.json
```

**Consumed by:** Root and all apps

### packages/config-typescript
**Shared TypeScript configuration**

```
packages/config-typescript/
├── base.json                      # Base TypeScript config
├── next.json                      # Next.js-specific config
├── package.json
│   └── "name": "@solana-com/tsconfig"
└── README.md
```

**Consumed by:** Root and all apps (via extends)

---

## Root Configuration Files

### Workspace Configuration
```
pnpm-workspace.yaml
  └─ Defines which directories are workspaces
```

```yaml
packages:
  - "apps/*"
  - "packages/*"
```

### Build Configuration
```
turbo.json
  └─ Turbo task pipeline & caching rules
```

### Package Management
```
package.json (root)
  ├─ Scripts (dev, build, lint, etc)
  ├─ Dependencies (Turbo, etc)
  └─ Workspace configuration

pnpm-lock.yaml
  └─ Lock file for all dependencies
```

### Code Quality
```
.eslintrc.js (root)
  └─ Base ESLint configuration

.prettierrc
  └─ Prettier formatting rules

tsconfig.json (root)
  └─ Base TypeScript configuration
```

---

## Environment-Specific Files

### Development
```
.devcontainer/
  ├─ devcontainer.json             # Docker dev environment
  └─ Dockerfile

.vscode/
  ├─ settings.json                 # VS Code settings
  ├─ extensions.json               # Recommended extensions
  └─ launch.json                   # Debug configuration

.cursor/
  └─ cursor.rules                  # Cursor IDE rules
```

### CI/CD
```
.github/
├─ workflows/
│   ├─ test.yml                    # Run tests on PR
│   ├─ lint.yml                    # Lint on PR
│   └─ deploy.yml                  # Deploy on merge
├─ ISSUE_TEMPLATE/
└─ PULL_REQUEST_TEMPLATE/
```

### Git
```
.husky/
├─ pre-commit                      # Run linting before commit
├─ pre-push                        # Run tests before push
└─ commit-msg                      # Validate commit messages

.gitignore
  └─ Define ignored files
```

---

## Content Structure (Media App Example)

```
apps/media/content/
├── posts/
│   ├── 2024-01-15-solana-update.mdx
│   ├── 2024-01-10-ecosystem-news.mdx
│   └── ...
├── podcasts/
│   ├── breakpoint.mdx             # Podcast metadata
│   ├── solana-cast.mdx
│   └── ...
├── authors/
│   ├── alice.md
│   └── bob.md
├── tags/
│   ├── ecosystem.mdx
│   └── development.mdx
├── categories/
│   ├── news.mdx
│   └── announcements.mdx
└── global/
    ├── config.json                # Global site settings
    └── authors.json               # Author list
```

---

## File Type Reference

| Extension | Purpose | Example |
|-----------|---------|---------|
| `.tsx` | React components with TypeScript | Button.tsx |
| `.ts` | TypeScript utilities | utils.ts |
| `.json` | Configuration & data | config.json |
| `.mdx` | Markdown with React components | blog-post.mdx |
| `.yaml` | Configuration | pnpm-workspace.yaml |
| `.js` | JavaScript config | next.config.js |
| `.css` | CSS styles | styles.css |

---

## Import Paths Reference

```typescript
// From shared packages
import { Button } from "@solana-com/ui";
import { Header } from "@solana-com/ui-chrome";
import { useTranslation } from "@workspace/i18n";

// Within app
import { MyComponent } from "@/components/MyComponent";
import { myUtil } from "@/lib/myUtil";

// Relative imports
import { helper } from "../helpers";
```

---

## Next Steps

- Review [Setup & Installation](./01-Setup-Installation.md) to get started
- Check [Development Workflow](./03-Development-Workflow.md) for daily tasks
- See [Monorepo Architecture](./04-Monorepo-Architecture.md) for deep dives

---

**Last Updated:** 2026-05-16
