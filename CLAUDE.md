# Claude Code Reference - OBxChat Agents

This document provides essential information for Claude Code when working with the OBxChat Agents repository.

## Project Overview

**OBxChat Agents** is a collection of specialized AI agents for digital marketing workflows, built on the Lobe Chat agents index framework. These agents are designed and maintained by OuterBox for internal use and client services.

## Technology Stack

- **Runtime**: Bun (v1.2.23+)
- **Language**: TypeScript
- **Package Manager**: npm/bun
- **Framework**: Lobe Chat Agents Index

## Critical Commands

### 1. Build Command
**Purpose**: Compiles all agent JSON definitions and generates localized versions across 18 languages.

```bash
bun run build
# or directly:
bun scripts/build.ts
```

**What it does**:
- Collects all agent definitions from `src/` directory
- Validates JSON schema
- Generates locale-specific versions (ar, bg-BG, zh-TW, en-US, ru-RU, ja-JP, zh-CN, ko-KR, fr-FR, tr-TR, es-ES, pt-BR, de-DE, it-IT, nl-NL, pl-PL, vi-VN, fa-IR)
- Outputs compiled agents to `public/` directory

**Expected Output**:
```
✔ build success
ℹ collected 74 agents (for each locale)
✔ build [locale]
```

### 2. Format Command
**Purpose**: Validates and formats all JSON agent configuration files for consistency.

```bash
bun run format
# or directly:
bun scripts/format.ts
```

**What it does**:
- Reads all JSON files in `src/` directory
- Validates against agent schema
- Formats with consistent spacing and structure
- Writes formatted versions back to source files

**Expected Output**:
```
✔ format success
```

**Important**: Requires `OPENAI_API_KEY` environment variable for some formatting operations.

### 3. Awesome Command
**Purpose**: Runs build and updates the agent showcase/listing.

```bash
bun run awesome
# or directly:
bun scripts/build.ts && bun scripts/updateAwesome.ts
```

**What it does**:
- Executes full build process
- Updates README with agent listings
- Generates agent showcase documentation

## Environment Variables

```bash
OPENAI_API_KEY=sk-xxxxxx...xxxxxx  # Required for format script
OPENAI_PROXY_URL=-                  # Optional proxy configuration
```

## File Structure Reference

```
obx-chat-agents/
├── src/                              # Source agent definitions (JSON)
│   ├── seo-performance-report.json
│   ├── obxchat-manus-assistant.json
│   └── [74 total agent files]
├── locales/                          # Translated metadata per agent
│   └── [agent-name]/
│       ├── index.ar.json
│       ├── index.zh-CN.json
│       └── [18 locale files]
├── public/                           # Built/compiled output
│   ├── index.json                    # Main agent index
│   └── agents.{locale}.json          # Locale-specific indexes
├── scripts/                          # Build tooling
│   ├── build.ts                      # Main build script
│   ├── format.ts                     # JSON formatter
│   └── updateAwesome.ts              # README updater
├── .i18nrc.js                        # i18n configuration
├── package.json                      # Project manifest
└── README.md                         # Project documentation
```

## Common Workflows

### Adding a New Agent
1. Create new JSON file in `src/` based on `agent-template.json`
2. Run `bun run format` to validate structure
3. Run `bun run build` to compile with locales
4. Commit changes including generated locale files

### Modifying an Existing Agent
1. Edit JSON file in `src/` directory
2. Run `bun run format` to validate changes
3. Run `bun run build` to recompile
4. Review and commit changes

### Full Rebuild Process
```bash
bun run format    # Validate and format all agents
bun run awesome   # Build and update documentation
```

## Vercel Deployment

### Critical Configuration

**⚠️ WORKSPACE REQUIREMENT**: This project MUST be deployed to the `nathanclay-trinitys-projects` workspace unless specifically instructed otherwise.

**Production URL**: https://marco-agents.vercel.app

**Workspace Details**:
- **Team**: nathanclay-trinitys-projects (OuterBox)
- **Team ID**: `team_NvG9x0jazsek3zrveMYkol1L`
- **Project ID**: `prj_VALQqQIfQydNddnPj041dDsUOah0`
- **Project Name**: marco-agents

### Complete Deployment Workflow

When deploying agent changes, follow this exact sequence:

```bash
# Step 1: Format and validate all agent JSON files
bun run format

# Step 2: Build agents across 18 locales
bun run build

# Step 3: Update README documentation
bun run awesome

# Step 4: Deploy to production (nathanclay-trinitys-projects)
vercel --prod
```

**Expected Build Time**: ~2 minutes for full deployment

### Vercel CLI Reference

#### Check Workspace Configuration

```bash
# Verify current user
vercel whoami
# Expected: devops-8822

# Check project linking
cat .vercel/project.json
# Expected orgId: "team_NvG9x0jazsek3zrveMYkol1L"
```

#### Deployment Commands

```bash
# Preview deployment (generates unique URL)
vercel

# Production deployment (updates marco-agents.vercel.app)
vercel --prod

# Deploy with confirmation prompt disabled
vercel --prod --yes
```

#### Post-Deployment Commands

```bash
# View deployment logs
vercel logs

# Inspect specific deployment
vercel inspect [deployment-url] --logs

# List all deployments
vercel list

# Remove old deployments
vercel remove [deployment-url]
```

### Workspace Verification

Before deploying, ALWAYS verify the correct workspace:

```bash
# List all accessible workspaces
vercel teams list

# Should show:
# ✔ developer-operations-obx-team
#   nathanclay-trinitys-projects  ← USE THIS ONE
```

If the local project is linked to the wrong workspace, relink it:

```bash
# Remove current linking
rm -rf .vercel

# Relink to correct workspace
vercel link --scope nathanclay-trinitys-projects --yes
```

### Auto-Deployment from GitHub

Pushes to the `master` branch automatically trigger Vercel production deployments. The GitHub repository is connected to:
- **Repo**: DevOps-Obx/marco-agents
- **Branch**: master → Production
- **Auto-deploy**: Enabled

### Project Structure Post-Build

```
marco-agents/
├── public/                         # Deployed directory (output)
│   ├── index.json                  # Main agent index
│   ├── index.en-US.json           # English index
│   ├── index.[locale].json        # 18 locale-specific indexes
│   ├── [agent-id].en-US.json      # Individual agent files
│   └── schema/                     # JSON schemas
└── .vercel/                        # LOCAL ONLY - DO NOT COMMIT
    ├── project.json                # Project linking config
    └── README.txt                  # Vercel info
```

**Note**: Vercel serves the `public/` directory as the root of marco-agents.vercel.app

### Troubleshooting Deployment

**Issue**: Deployment goes to wrong workspace
```bash
# Solution: Verify and relink
cat .vercel/project.json
rm -rf .vercel
vercel link --scope nathanclay-trinitys-projects --yes
```

**Issue**: Build fails on Vercel
```bash
# Solution: Test build locally first
bun run build
# Fix any errors, then deploy
```

**Issue**: Changes not reflected after deployment
```bash
# Solution: Check build actually ran
vercel inspect [deployment-url] --logs
# Verify public/ directory was updated
```

## Agent Schema Structure

Each agent JSON file contains:
- `author`: "OuterBox"
- `config.systemRole`: The agent's system prompt
- `createdAt`: Creation date (YYYY-MM-DD)
- `homepage`: OuterBox website URL
- `identifier`: Unique agent ID (kebab-case)
- `meta`: Agent metadata
  - `avatar`: Emoji representation
  - `description`: Brief agent description
  - `tags`: Array of category tags
  - `title`: Agent display name

## Troubleshooting

### Bun Not Found
If Bun is not installed:
```bash
# Windows (PowerShell)
powershell -c "irm bun.sh/install.ps1|iex"

# macOS/Linux
curl -fsSL https://bun.sh/install | bash
```

### Dependencies Missing
```bash
npm install
```

### Build Failures
- Check JSON syntax in `src/` files
- Ensure `.i18nrc.js` is properly configured
- Verify all required dependencies are installed

## Notes for Claude Code

### Agent Development
- Always run `bun run format` before committing agent changes
- The build process generates ~10 agents across 18 locales (180 total files)
- Agent modifications in `src/` require rebuild to update `public/` output
- Locale files in `locales/` are auto-generated, don't edit manually
- The project uses Bun for better TypeScript performance, but npm fallback is available

### Deployment Requirements
- **CRITICAL**: ALWAYS deploy to `nathanclay-trinitys-projects` workspace unless explicitly instructed otherwise
- Verify workspace before deployment: `cat .vercel/project.json` should show `team_NvG9x0jazsek3zrveMYkol1L`
- Complete workflow: `format` → `build` → `awesome` → `vercel --prod`
- Production URL: https://marco-agents.vercel.app
- Auto-deployment enabled for `master` branch pushes

### Workspace Switching (Rarely Needed)
If you need to switch workspaces (only when explicitly requested):
```bash
rm -rf .vercel
vercel link --scope [target-workspace] --yes
```

### Common Deployment Pattern
```bash
# After modifying agents
bun run format && bun run build && bun run awesome && vercel --prod
```
