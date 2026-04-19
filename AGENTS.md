# Agent Directory

All agents across the BelichickGillisMusk organization. Use `agents/registry.json` for programmatic access.

## Quick Reference

| Agent | Type | Status | Source | Capabilities |
|-------|------|--------|--------|--------------|
| MILA | Autonomous | Active | demo-repository | Calendar, Gmail, Tasks, MCP Server |
| Samantha | Autonomous | Active | Vercel | Assistant (needs Google APIs) |
| Belichick Strategy | Skill | Available | belichick-margo-jesus | Strategy & Planning |
| Jon Jones Sales | Skill | Available | belichick-margo-jesus | Sales & Business Dev |
| MILA Carb CS | Skill | Available | belichick-margo-jesus | Customer Service |
| MILA Legal | Skill | Available | belichick-margo-jesus | Legal Assistance |
| Musk Creative | Skill | Available | belichick-margo-jesus | Creative Content |
| Slack Recon | Skill | Available | belichick-margo-jesus | Slack Monitoring |
| TPS Report | Skill | Available | belichick-margo-jesus | Report Generation |
| Kesha | OpenClaw | Available | demo-repository | OpenClaw Skill |
| Cloud Agent Starter | Template | Available | Claude-Gillis-Musk-Main | Starter Template |
| HR Agent | Function | Active | virtual-office-hub | HR Operations |
| Operation Agent | API | Active | Operation-Get-Shit-Done | Task Execution |
| Briefs Agents | Dashboard | Active | briefs | Meeting Coordination |

---

## Autonomous Agents

### MILA
**Status:** Active on Vercel  
**Source:** `BelichickGillisMusk/demo-repository`

The flagship autonomous agent with full Google Workspace integration.

**Key Files:**
- `MILA-VERCEL-SETUP.md` - Deployment guide
- `MILA-DEPLOYMENT-CHECKLIST.md` - Deployment checklist
- `MILA-GOVERNANCE-FRAMEWORK.md` - Governance rules
- `api/mila-mcp-server.js` - MCP server with Google APIs
- `api/mila-task.js` - Task handling
- `api/mila-status.js` - Status endpoint
- `mila-command-center.html` - Command center UI
- `.github/workflows/mila.yml` - GitHub Actions workflow
- `.github/workflows/mila-autonomous.yml` - Autonomous workflow

**Capabilities:**
- Google Calendar read/write
- Gmail send/receive
- Task management
- MCP server integration
- Autonomous workflows via GitHub Actions

**Integrations:**
- Google Calendar API
- Gmail API
- Vercel serverless functions
- GitHub Actions

---

### Samantha
**Status:** Active on Vercel (needs Google APIs)  
**Source:** Vercel deployment

Personal assistant agent for daily scheduling and weekly reviews.

**Setup Guide:** [agents/samantha/google-api-setup.md](./agents/samantha/google-api-setup.md)

**Required Integrations:**
- Google Calendar API (copy from MILA)
- Gmail API (copy from MILA)

---

## Skills (belichick-margo-jesus)

All skills located in `BelichickGillisMusk/belichick-margo-jesus/skills/`

### Belichick Strategy
**Path:** `skills/belichick-strategy/SKILL.md`  
**Purpose:** Strategic planning and decision-making

### Jon Jones Sales
**Path:** `skills/jonjones-sales/SKILL.md`  
**Purpose:** Sales assistance and business development

### MILA Carb CS
**Path:** `skills/mila-carb-cs/SKILL.md`  
**Purpose:** Customer service and code assistance  
**References:** `skills/mila-carb-cs/references/clean-truck-check-complete.md`

### MILA Legal
**Path:** `skills/mila-legal/SKILL.md`  
**Purpose:** Legal document assistance

### Musk Creative
**Path:** `skills/musk-creative/SKILL.md`  
**Purpose:** Creative content and innovation

### Slack Recon Agent
**Path:** `skills/slack-recon-agent/SKILL.md`  
**Purpose:** Slack channel monitoring and reconnaissance  
**References:** `skills/slack-recon-agent/references/slack-channel-map.md`  
**Implementation:** `src/slack-bot/agents.js`, `src/slack-bot/dispatch.js`

### TPS Report
**Path:** `skills/tps-report/SKILL.md`  
**Purpose:** Automated report generation

---

## OpenClaw Agents

Located in `BelichickGillisMusk/demo-repository/openclaw/workspace/`

### Kesha
**Path:** `openclaw/workspace/skills/kesha/SKILL.md`  
**Setup:** `OpenClaw/KESHA-SETUP.md`

### Overnight-Ops
**Path:** `openclaw/workspace/skills/overnight-ops/SKILL.md`

**Agent Registry:** `openclaw/workspace/AGENTS.md`

---

## Infrastructure Agents

### HR Agent
**Source:** `BelichickGillisMusk/virtual-office-hub`  
**Path:** `src/lib/hrAgent.ts`  
**Purpose:** Human resources and agent management

### Operation Agent
**Source:** `BelichickGillisMusk/Operation-Get-Shit-Done`  
**Path:** `app/api/agent/route.ts`  
**Purpose:** Task execution API endpoint

### Briefs Command Center
**Source:** `BelichickGillisMusk/briefs`  
**Path:** `command-center/`  
**Files:**
- `dashboard.js` - Dashboard logic
- `data.js` - Agent state data
- `index.html` - Main UI
- `styles.css` - Styling

**Purpose:** Morning and night meeting coordination

---

## Templates

### Cloud Agent Starter
**Source:** `BelichickGillisMusk/Claude-Gillis-Musk-Main`  
**Path:** `skills/cloud-agent-starter.md`  
**Purpose:** Template for creating new cloud-based agents

---

## Command Centers / Agent Boardrooms

### Briefs Command Center
**Repo:** `briefs`  
**URL:** `command-center/index.html`  
**Purpose:** Central dashboard for morning/night briefings

### Virtual Office Hub
**Repo:** `virtual-office-hub`  
**Components:**
- `HRDashboard.tsx` - Central agent dashboard
- `OfficeScene.tsx` - Visual agent representation
- `AgentChat.tsx` - Inter-agent communication
- `TeamManagement.tsx` - Agent team formation
- `officeData.ts` - Agent registry data
- `delegation.ts` - Task delegation logic

### Demo Repository Agents Page
**Repo:** `demo-repository`  
**Path:** `agents.html`  
**Purpose:** Agent visibility and discovery page

---

## Additional Resources

- **Skills Library:** `demo-repository/SKILLS-LIBRARY.md`
- **Agent Documentation:** `Claude-Gillis-Musk-Main/AGENTS.md`
- **OpenClaw Config:** `belichick-margo-jesus/openclaw-config.json5`

---

## Adding Google APIs to an Agent

See [integrations/google-apis/](./integrations/google-apis/) for step-by-step setup.

**Required:**
1. Google Cloud Project with Calendar API + Gmail API enabled
2. OAuth 2.0 credentials
3. Environment variables in Vercel

**Reference Implementation:** MILA in demo-repository
