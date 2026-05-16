# Setup & Installation

A complete guide to setting up your local development environment for the Solana.com monorepo.

## Prerequisites

Before you begin, ensure you have the following installed:

### Required
- **Node.js 22+** - [Download](https://nodejs.org/)
- **pnpm 10+** - [Installation Guide](https://pnpm.io/installation)
  
  ```bash
  # Install pnpm globally
  npm install -g pnpm
  ```

### Optional (But Recommended)
- **Git** - Version control
- **Visual Studio Code** - Code editor with extensions
- **Docker** - For dev container support (see Dev Container Setup below)

## Step 1: Clone the Repository

```bash
# Clone the repository
git clone https://github.com/DMENTERPRISES/solana.com.git

# Navigate to the directory
cd solana.com
```

## Step 2: Verify Node & pnpm

```bash
# Check Node version (should be 22+)
node --version

# Check pnpm version (should be 10+)
pnpm --version
```

## Step 3: Install Dependencies

```bash
# Install all dependencies across the monorepo
pnpm install
```

This command will:
- Install dependencies for all workspaces
- Create symlinks between local packages
- Generate lock files

## Step 4: Set Up Environment Variables

Depending on which apps you plan to work on, you may need environment variables:

### For Media App
Create `.env` in `apps/media/`:

```bash
# TinaCMS Cloud (optional - use local mode if not set)
NEXT_PUBLIC_TINA_CLIENT_ID=your_client_id
TINA_TOKEN=your_token
NEXT_PUBLIC_TINA_BRANCH=main
TINA_SEARCH_INDEXER_TOKEN=your_indexer_token

# Or use local mode (no cloud)
TINA_PUBLIC_IS_LOCAL=true
```

### For Web App
Create `.env.local` in `apps/web/` if needed for specific features.

## Step 5: Run Development Server

```bash
# Start all apps in development mode
pnpm dev

# Or start a specific app
pnpm --filter @solana-com-web dev
pnpm --filter @solana-com-media dev
```

The applications will be available at:
- **Web**: http://localhost:3000
- **Media**: http://localhost:3002
- **Docs**: http://localhost:3003

## Step 6: Verify Installation

```bash
# Check if all packages are properly linked
pnpm ls

# Run type checking
pnpm type-check

# Run linting
pnpm lint
```

---

## Alternative Setup Methods

### Using Dev Container (Docker)

If you have Docker installed, you can use the dev container for a consistent environment:

```bash
# Open in Dev Container using VS Code
# 1. Install the "Dev Containers" extension
# 2. Open the folder in a container (Command Palette: "Dev Containers: Reopen in Container")
```

The dev container configuration is located in `.devcontainer/`.

### Using NVM (Node Version Manager)

If you use NVM to manage Node versions:

```bash
# Install Node 22 via NVM
nvm install 22
nvm use 22

# Then proceed with the steps above
```

---

## Troubleshooting Installation

### Issue: `pnpm: command not found`

**Solution:** Install pnpm globally:
```bash
npm install -g pnpm@latest
```

### Issue: `node_modules` conflicts

**Solution:** Clean and reinstall:
```bash
pnpm clean
rm -rf node_modules pnpm-lock.yaml
pnpm install
```

### Issue: Port already in use

**Solution:** Use a different port:
```bash
pnpm dev -- --port 3001
```

### Issue: TypeScript errors after installation

**Solution:** Regenerate TypeScript files:
```bash
pnpm type-check
pnpm build
```

---

## Next Steps

Once installation is complete:
1. Review the [Project Structure](./02-Project-Structure.md)
2. Check out [Development Workflow](./03-Development-Workflow.md)
3. Start with [Apps Guide](./07-Apps-Guide.md) to understand specific applications

---

**Last Updated:** 2026-05-16
