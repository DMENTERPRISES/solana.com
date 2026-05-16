# Monorepo Architecture

Deep dive into how the Solana.com monorepo is structured and how different parts interact.

## Architecture Overview

The Solana.com monorepo uses a **pnpm workspaces** structure with **Turbo** for build orchestration.

```
┌─────────────────────────────────────────────────────────┐
│                    Root Workspace                        │
│  (pnpm-workspace.yaml + turbo.json)                      │
└─────────────────────────────────────────────────────────┘
         │
    ┌────┴────┬────────────────┐
    │         │                │
    ▼         ▼                ▼
┌────────┐ ┌────────┐     ┌─────────────┐
│ apps/  │ │package │     │Configuration│
│        │ │  /     │     │   & Config  │
└────────┘ └────────┘     └─────────────┘
    │         │
    ├─web     ├─ui
    ├─media   ├─ui-chrome
    ├─docs    ├─i18n
    ├─template├─config-eslint
    │  s      └─config-typescript
    └─accelerate
```

---

## Workspace Hierarchy

### Level 1: Root Workspace

**Purpose:** Manage all workspaces and shared configuration

**Key Files:**
- `pnpm-workspace.yaml` - Defines all workspaces
- `turbo.json` - Turbo build configuration
- `package.json` - Root dependencies and scripts
- `tsconfig.json` - Shared TypeScript config
- `.eslintrc.js` - Shared ESLint config

**Responsibilities:**
- Install dependencies for all workspaces
- Define global build tasks
- Set shared development standards

### Level 2: Apps (Next.js Applications)

**Purpose:** Independent applications served on different ports

**Characteristics:**
- Each is a complete Next.js application
- Has its own `package.json` and `next.config.ts`
- Contains app-specific routes and components
- Can be deployed independently

**Apps:**
1. **Web** (`apps/web`) - Main Solana.com website
2. **Docs** (`apps/docs`) - Developer documentation
3. **Media** (`apps/media`) - News and podcasts
4. **Templates** (`apps/templates`) - Developer templates
5. **Accelerate** (`apps/accelerate`) - Accelerate program

### Level 3: Packages (Shared Libraries)

**Purpose:** Reusable code and configurations

**Characteristics:**
- Consumed by apps via monorepo linking
- Have their own `package.json`
- Export TypeScript types and components
- Updated in one place, used everywhere

**Packages:**
1. **ui** - React components
2. **ui-chrome** - Header/footer components
3. **i18n** - Internationalization utilities
4. **config-eslint** - ESLint configuration
5. **config-typescript** - TypeScript configuration

---

## Workspace Linking

### How Workspaces Connect

When you install dependencies, pnpm:
1. Creates `node_modules` at root level
2. Symlinks workspace packages locally
3. Allows imports without publishing

**Example:**
```typescript
// In apps/web/package.json
{
  "dependencies": {
    "@solana-com/ui": "*",
    "@solana-com/ui-chrome": "*",
    "@workspace/i18n": "*"
  }
}
```

The `*` means "use the local version from packages/".

### Import Pattern
```typescript
// In apps/web
import { Button } from "@solana-com/ui";
import { Header } from "@solana-com/ui-chrome";
import { useTranslation } from "@workspace/i18n";
```

---

## Build Orchestration with Turbo

### Turbo's Role

**Turbo** manages task execution across workspaces:
- Runs tasks in optimal order
- Caches results
- Parallelizes where possible
- Tracks dependencies

### Turbo Configuration

**File:** `turbo.json`

```json
{
  "pipeline": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [".next/**", "dist/**"]
    },
    "dev": {
      "cache": false
    },
    "lint": {
      "outputs": []
    },
    "type-check": {
      "outputs": []
    }
  }
}
```

### Task Dependency Graph

```
lint ─────┐
          ├──> build ──> deploy
type-check┘
```

- **lint** - Runs independently
- **type-check** - Runs independently
- **build** - Depends on lint and type-check completing
- **deploy** - Depends on build completing

---

## Dependency Resolution

### Root Dependencies

```
pnpm install
│
├─ Installs root dependencies (turbo, etc)
│
└─ Installs all workspace dependencies
   ├─ apps/web/package.json
   ├─ apps/media/package.json
   ├─ packages/ui/package.json
   └─ ... others
```

### Workspace-to-Workspace

```
apps/media
  ├─ Imports from packages/ui
  │  └─ Resolves via symlink
  │
  └─ Imports from packages/i18n
     └─ Resolves via symlink
```

### External Dependencies

```
pnpm-lock.yaml
  ├─ Tracks all versions
  ├─ Ensures consistency across workspaces
  └─ Enables reproducible installs
```

---

## Data Flow

### Development Runtime

```
┌─────────────────────────────────────────┐
│       Development Server (pnpm dev)      │
└──────────────┬──────────────────────────┘
               │
         ┌─────┴─────┐
         ▼           ▼
    ┌────────┐   ┌────────┐
    │ App    │   │Shared  │
    │Server  │──→│Package │
    │        │   │(Symlink)
    └────────┘   └────────┘
         ▼
    ┌────────┐
    │ Content│ (MDX/TinaCMS)
    │Files  │
    └────────┘
```

### Build Process

```
pnpm build
  │
  ├─ Turbo analyzes dependencies
  │
  ├─ Builds packages first
  │   ├─ config-eslint
  │   ├─ config-typescript
  │   ├─ ui
  │   ├─ ui-chrome
  │   └─ i18n
  │
  └─ Builds apps (can run in parallel)
      ├─ web
      ├─ media
      ├─ docs
      ├─ templates
      └─ accelerate
```

---

## Content Management Flow

### MDX Content

```
Source (content/*.mdx)
         │
         ▼
   ├─ Webpack Loader
   │  └─ Converts to React components
   │
   └─ Processed by Next.js
      └─ Served as HTML
```

### TinaCMS Content (Media App)

```
TinaCMS Admin (/)
        │
        ├─ Edit content in UI
        │
        ├─ Save to Git/Cloud
        │
        ▼
Content Files (content/*.mdx)
        │
        ▼
CMS Query (getContentBySlug)
        │
        ▼
Next.js Page Component
```

---

## Internationalization (i18n) Flow

```
@workspace/i18n
  │
  ├─ next-intl setup
  │
  ├─ Translation files (locales/*.json)
  │
  └─ useTranslation() hook
     │
     └─ Used by all apps for multi-language support
```

---

## Component Hierarchy

### UI Components Hierarchy

```
@solana-com/ui-chrome
  ├─ Header component
  │  └─ Navigation
  │
  └─ Footer component
     └─ Links

@solana-com/ui
  ├─ Button
  ├─ Card
  ├─ Modal
  └─ ... other components

Apps use both:
  app-specific
    ├─ Page components
    ├─ Layout components
    └─ Uses shared UI
```

---

## Configuration Inheritance

### TypeScript Configuration

```
Root tsconfig.json (base)
        │
        ├─ packages/config-typescript/
        │  └─ Defines standard config
        │
        └─ Apps inherit and extend
           ├─ apps/web/tsconfig.json
           ├─ apps/media/tsconfig.json
           └─ ... others
```

### ESLint Configuration

```
Root .eslintrc.js (base)
        │
        ├─ packages/config-eslint/
        │  └─ Defines standard rules
        │
        └─ Apps can override
           ├─ apps/web/.eslintrc.js
           └─ ... others
```

---

## Performance Considerations

### Monorepo Advantages
- ✅ Shared code in one place
- ✅ Consistent dependencies
- ✅ Atomic commits
- ✅ Easier refactoring
- ✅ Turbo caching speeds builds

### Monorepo Challenges
- ⚠️ Large repository size (~285 MB)
- ⚠️ Complex dependency graph
- ⚠️ All tests run in CI
- ⚠️ Careful version management needed

### Optimization Strategies
1. **Use Turbo caching** - Speeds up repeated builds
2. **Filter tasks** - Only build/test what changed
3. **Parallel execution** - Run independent tasks together
4. **Workspace-level caching** - Each app caches separately

---

## Deployment Architecture

### Multi-App Deployment

```
solana.com Domain
  │
  ├─ /         → apps/web (main)
  ├─ /news     → apps/media (rewrite)
  ├─ /podcasts → apps/media (rewrite)
  ├─ /docs     → apps/docs (rewrite)
  ├─ /validators
  └─ ...
```

Each app can be deployed to:
- **Vercel** (Recommended for Next.js)
- **Self-hosted** (Docker/K8s)
- **Edge networks** (Cloudflare, etc)

---

## Maintenance & Scalability

### Adding New App
1. Create directory in `apps/`
2. Create `package.json` with workspace name
3. Create `next.config.ts`
4. Update `pnpm-workspace.yaml` (auto-discovered)
5. Create build/dev scripts

### Adding New Package
1. Create directory in `packages/`
2. Create `package.json` with @solana-com/ scope
3. Export from `src/index.ts`
4. Add to import paths in tsconfig
5. Apps can import immediately

### Updating Dependencies
```bash
# Update across all workspaces
pnpm update package-name

# Verify builds still work
pnpm build
```

---

## Next Steps

- Review [Tech Stack Overview](./06-Tech-Stack.md) for technology details
- Check [Build System](./05-Build-System.md) for Turbo specifics
- See [Apps Guide](./07-Apps-Guide.md) for app-specific architecture

---

**Last Updated:** 2026-05-16
