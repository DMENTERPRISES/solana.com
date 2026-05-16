# Development Workflow

Common tasks and commands for day-to-day development in the Solana.com monorepo.

## Starting Development

### Run All Apps
```bash
pnpm dev
```
This starts all applications in development mode simultaneously.

### Run Specific App
```bash
# Media app
pnpm --filter @solana-com-media dev

# Web app
pnpm --filter @solana-com-web dev

# Docs app
pnpm --filter @solana-com-docs dev

# Templates app
pnpm --filter solana-templates dev

# Accelerate app
pnpm --filter solana-accelerate dev
```

### Run from App Directory
```bash
cd apps/web
pnpm dev
```

---

## Building & Production

### Build All Apps
```bash
pnpm build
```

### Build Specific App
```bash
pnpm --filter @solana-com-web build
pnpm --filter @solana-com-media build
```

### Build Specific Package
```bash
pnpm --filter @solana-com/ui build
```

### Run Production Build Locally
```bash
# Build
pnpm build

# Start production server
pnpm start
```

---

## Code Quality & Formatting

### Lint Code
```bash
# Lint all files
pnpm lint

# Lint specific app
pnpm --filter @solana-com-web lint

# Fix lint errors
pnpm lint:fix
```

### Format Code
```bash
# Format all files with Prettier
pnpm format

# Format specific file
pnpm format -- path/to/file.ts

# Check formatting without changing
pnpm format:check
```

### Type Checking
```bash
# Check TypeScript types
pnpm type-check

# Watch mode
pnpm type-check:watch
```

### All Quality Checks
```bash
# Run lint, format, and type checks
pnpm check
```

---

## Testing

### Run Tests
```bash
# Run all tests
pnpm test

# Run specific app tests
pnpm --filter @solana-com-web test

# Watch mode
pnpm test:watch

# Coverage report
pnpm test:coverage
```

---

## Working with Content (Media App)

### Start TinaCMS Admin
```bash
cd apps/media
pnpm dev
# Visit http://localhost:3002/admin
```

### Using Local TinaCMS
```bash
# Set environment variable
export TINA_PUBLIC_IS_LOCAL=true

# Then run dev
pnpm dev
```

### Build with Local Mode
```bash
pnpm build-local
```

---

## Database & Data Management

### Reset Data
```bash
# Clear build cache
pnpm clean

# Clear node_modules and reinstall
pnpm install

# Clear specific app
pnpm --filter @solana-com-web clean
```

---

## Git Workflow

### Pre-commit Hooks
Husky automatically runs:
- ESLint (fixes errors)
- Prettier (formats code)
- TypeScript checks

```bash
# Manually run pre-commit hooks
pnpm husky install

# Run hook manually
pnpm husky pre-commit
```

### Creating a Feature Branch
```bash
# Create and switch to new branch
git checkout -b feature/my-feature

# Make changes, commit, and push
git add .
git commit -m "feat: add my feature"
git push -u origin feature/my-feature
```

### Committing Changes
```bash
# Stage changes
git add .

# Commit (hooks will run automatically)
git commit -m "feat: add new feature"

# If hooks fail, fix and commit again
git commit -m "fix: resolve lint errors"
```

---

## Package Management

### Add Dependency to App
```bash
# Add to specific app
pnpm --filter @solana-com-web add package-name

# Add dev dependency
pnpm --filter @solana-com-web add -D package-name
```

### Add Dependency to Package
```bash
# Add to shared package
pnpm --filter @solana-com/ui add package-name
```

### Update All Dependencies
```bash
# Check for outdated packages
pnpm outdated

# Update specific package
pnpm update package-name

# Update all
pnpm update
```

### Remove Dependency
```bash
pnpm --filter @solana-com-web remove package-name
```

---

## Monorepo-Specific Commands

### List All Workspaces
```bash
pnpm ls --depth -1
```

### Run Task in All Workspaces
```bash
# Run build in all workspaces
pnpm run build --recursive

# Run specific task
pnpm run lint --recursive
```

### Filter by Package
```bash
# Run command only in packages directory
pnpm --filter "./packages/**" build

# Run only in apps
pnpm --filter "./apps/**" dev
```

---

## Turbo-Specific Commands

### Turbo Build
```bash
# Build with Turbo (optimized)
pnpm turbo build

# Build specific app
pnpm turbo build --filter @solana-com-web

# Build with cache
pnpm turbo build --cache

# Skip cache
pnpm turbo build --no-cache
```

### Turbo Run
```bash
# Run specific task across workspaces
pnpm turbo run build

# Parallel execution
pnpm turbo run build --concurrency 4

# See execution plan
pnpm turbo build --dry
```

### Turbo Graph
```bash
# Visualize dependency graph
pnpm turbo run dev --graph

# Generate graph file
pnpm turbo run build --graph=graph.svg
```

---

## Debugging

### Debug Next.js App
```bash
# Start with Node debugger
NODE_OPTIONS='--inspect' pnpm dev

# Visit chrome://inspect in Chrome
```

### Debug Specific App
```bash
cd apps/web
NODE_OPTIONS='--inspect' pnpm dev
```

### View Build Output
```bash
# Verbose logging
pnpm dev --verbose

# Debug Turbo
TURBO_LOG_LEVEL=debug pnpm build
```

---

## Clean Up & Maintenance

### Full Clean
```bash
# Remove all generated files
pnpm clean

# Remove all dependencies
rm -rf node_modules pnpm-lock.yaml

# Reinstall everything
pnpm install

# Run builds
pnpm build
```

### Cache Management
```bash
# Clear Turbo cache
pnpm turbo prune --scope=@solana-com-web

# Clear Next.js cache
rm -rf apps/*/. next

# Clear ESLint cache
pnpm lint -- --fix
```

---

## Environment Variables

### Loading Variables
Create `.env` or `.env.local` in app directories:

```bash
# apps/media/.env
NEXT_PUBLIC_TINA_CLIENT_ID=your_id
TINA_TOKEN=your_token
```

### Using in Code
```typescript
// Access in Next.js
const clientId = process.env.NEXT_PUBLIC_TINA_CLIENT_ID;
```

---

## Performance Tips

### Faster Installs
```bash
# Use lockfile, skip scripts
pnpm install --offline

# Frozen lockfile
pnpm install --frozen-lockfile
```

### Faster Builds
```bash
# Use Turbo caching
pnpm turbo build

# Filter specific apps to build
pnpm turbo build --filter @solana-com-web
```

### Memory Issues
```bash
# Increase Node memory
NODE_OPTIONS='--max-old-space-size=4096' pnpm build
```

---

## Useful Aliases

Add to your shell configuration (`.bashrc`, `.zshrc`, etc.):

```bash
# Development
alias dev='pnpm dev'
alias build='pnpm build'
alias lint='pnpm lint'
alias format='pnpm format'

# Specific apps
alias dev-web='pnpm --filter @solana-com-web dev'
alias dev-media='pnpm --filter @solana-com-media dev'
alias dev-docs='pnpm --filter @solana-com-docs dev'
```

---

## Next Steps

- Check [Code Quality](./10-Code-Quality.md) for standards
- Review [Apps Guide](./07-Apps-Guide.md) for app-specific workflows
- See [Troubleshooting](./12-Troubleshooting.md) if issues arise

---

**Last Updated:** 2026-05-16
