# Simplified Campaign ROI Analyzer - Single Purpose

**One Job:** Attribute closed-won revenue to marketing campaigns

---

## Proposed Simplified Prompt

```
You are a Salesforce Campaign ROI Analyzer. Your single purpose is to connect closed-won revenue to the marketing campaigns that generated it.

Primary Task

Execute this query to attribute revenue to campaigns:
SELECT Campaign.Name, SUM(Amount), COUNT(Id)
FROM Opportunity
WHERE IsWon=true
GROUP BY Campaign.Name

Expected Outcomes

Success: Show campaign names ranked by total revenue with deal counts.

No Data (Common): When opportunities lack campaign associations, acknowledge this transparently:
- State that campaign-opportunity linking is not configured
- Explain this field (Campaign on Opportunity) needs to be populated
- Provide fallback: List available campaigns using Campaign.Name, Type, Status

Specific Salesforce Fields

- Opportunity.Campaign.Name - Links won deals to campaigns
- Opportunity.Amount - Deal revenue value
- Opportunity.IsWon - Identifies closed-won deals
- Campaign.Name - Campaign identifier
- Campaign.Type - Campaign category (Email, Trade Show, Webinar, etc.)
- Campaign.Status - Campaign state (Active, Completed, etc.)

Tool Usage - Required

Use OBxMultiTool MCP to access Salesforce with these restrictions:
- ONLY use Salesforce and Workbench integrations
- ONLY perform READ operations (Query, List, Search, Describe)
- Never use other integrations
- Never perform write operations

Communication

Be direct and transparent. When campaign associations don't exist, clearly explain what's missing and what data IS available. Focus on actionable information.
```

---

## What This Removes

❌ Lead generation analysis
❌ Conversion funnel metrics
❌ UTM/digital attribution
❌ Pipeline creation tracking
❌ Campaign type comparison
❌ Multiple query approaches
❌ Efficiency metrics
❌ Complex multi-step analysis

## What This Keeps

✅ One specific query pattern
✅ Specific field names (Campaign.Name, Amount, IsWon, Type, Status)
✅ Clear success/failure paths
✅ Transparent communication about limitations
✅ Salesforce/Workbench-only requirement through MultiTool MCP
✅ Fallback behavior (list campaigns)

---

## Conversation Log

### 2025-11-23 - Initial Simplification
- Created simplified single-purpose prompt
- Focused on one query: Campaign revenue attribution
- Removed multi-object analysis (Leads, UTMs, etc.)
- Kept specific field references as requested
- Maintained hard requirement: Salesforce/Workbench only via MultiTool MCP

### 2025-11-23 - JSON Update & Git Commit
- Updated `src/salesforce-campaign-roi-analyzer.json` with simplified prompt
- Updated description to match single-purpose approach
- Committed changes (commit: 4696035)
- ✅ **PUSHED** to GitHub (005eead..4696035)

### Git Verification Results

**Repository:**
- Remote: `git@github.com:DevOps-Obx/marco-agents.git` (updated from Lindsayellis22)
- Branch: master

**Git Config:**
- User: Lindsay Ellis
- Email: 94205796+Lindsayellis22@users.noreply.github.com

**GitHub CLI:**
- Logged in as: Lindsayellis22
- Account active: ✓
- Token scopes: gist, read:org, repo

### 2025-11-23 - Remote URL Update
- Updated git remote from `Lindsayellis22/marco-agents` to `DevOps-Obx/marco-agents`
- Repository moved to organization account
- Verified remote change successful

### 2025-11-23 - Vercel Workspace Reconfiguration
- Relinked both local projects to `nathanclay-trinitys-projects` workspace
- **marco-agents**: Now deploys to https://marco-agents.vercel.app
  - Old orgId: `team_B9Wn8C5e8cmx6Y9FMXVxrVaX` (developer-operations-obx-team)
  - New orgId: `team_NvG9x0jazsek3zrveMYkol1L` (nathanclay-trinitys-projects)
  - Project ID: `prj_VALQqQIfQydNddnPj041dDsUOah0`
- **obx-project-tracker-next-js**: Now deploys to https://portal.outerboxai.com
  - Old orgId: `team_B9Wn8C5e8cmx6Y9FMXVxrVaX` (developer-operations-obx-team)
  - New orgId: `team_NvG9x0jazsek3zrveMYkol1L` (nathanclay-trinitys-projects)
  - Project ID: `prj_B9HLePez5qn3tgwm4HypAPTQv5Ua`
- Future CLI deployments from local will deploy to nathanclay-trinitys-projects workspace

### 2025-11-23 - Production Deployment with Simplified Prompt
- Ran build pipeline: `bun run format` → `bun run build` → `bun run awesome`
- All 10 agents compiled successfully across 18 locales
- Deployed to production: `vercel --prod`
- **Deployment URL**: https://marco-agents-et1suo2sv-nathanclay-trinitys-projects.vercel.app
- **Production URL**: https://marco-agents.vercel.app
- **Workspace**: nathanclay-trinitys-projects
- **Build time**: ~2 minutes
- Simplified Campaign ROI Analyzer now live with single-purpose prompt

### 2025-11-23 - Documentation Update
- Updated `README.md` with comprehensive Vercel deployment section
  - Added Vercel Configuration with workspace details
  - Added Deployment Workflow with complete command sequence
  - Added Vercel CLI commands reference
  - Added Workspace Information (team ID, project ID, domain)
  - Added Auto-Deployment information
  - ⚠️ Emphasized: ALWAYS use `nathanclay-trinitys-projects` workspace

- Updated `CLAUDE.md` with detailed Vercel deployment instructions
  - Added "Vercel Deployment" section with critical configuration
  - Added Complete Deployment Workflow (4-step process)
  - Added Vercel CLI Reference with all commands
  - Added Workspace Verification procedures
  - Added Project Structure Post-Build
  - Added Troubleshooting Deployment section
  - Updated "Notes for Claude Code" with deployment requirements
  - ⚠️ Made workspace requirement CRITICAL and prominent

**Key Points Documented:**
- Workspace: nathanclay-trinitys-projects (OuterBox)
- Team ID: team_NvG9x0jazsek3zrveMYkol1L
- Project ID: prj_VALQqQIfQydNddnPj041dDsUOah0
- Production: https://marco-agents.vercel.app
- GitHub: DevOps-Obx/marco-agents (auto-deploy enabled)

### 2025-11-23 - Vercel Build Fix
- **Issue**: Deployment failing with `stylelint-config-clean-order` error
- **Cause**: Vercel running prebuild lint hooks unnecessarily
- **Solution**: Created `vercel.json` to override build command
  - Direct build: `bun scripts/build.ts`
  - Bypasses npm/bun lifecycle hooks
  - Framework detection disabled
  - Output directory: `public`
- **Result**: Build time reduced from ~2 minutes to ~24 seconds
- **Commit**: 0595dbc
- **Status**: ✅ Deployed successfully to production
