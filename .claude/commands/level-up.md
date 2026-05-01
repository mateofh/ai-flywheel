---
description: AI Flywheel — measure how much work your AI can do without you
---

# AI Flywheel

Measure how much work your AI can do without you. Works for everyone — from ChatGPT in the browser to autonomous agent swarms.

**Usage:**
- `/level-up` — Full assessment: scan + score + dashboard + share copy
- `/level-up --assess` — Score only, no dashboard
- `/level-up --history` — Show timeline of past runs
- `/level-up --diff` — Show what changed since last run
- `/level-up --cleanup` — Generate cleanup script for dead skills/stale memory

---

## The 5 Dimensions

| Dimension | What It Measures |
|-----------|-----------------|
| Context Depth | How well does your AI know you? |
| Data Reach | How many real data sources can your AI access? |
| Workflow Power | How much repetitive work have you automated? |
| Output Quality | Is what your AI produces ready to use? |
| Autonomy | How much can your AI do without you? |

Each dimension: 0-5. Total: 30 points max.

## The 6 Tiers

| Score | Tier | Name | Who This Is |
|-------|------|------|-------------|
| 0-5 | 1 | Tourist | Using AI like Google. Ask, get answer, move on |
| 6-10 | 2 | Power User | Customized AI with saved prompts or GPTs. Better output, still manual |
| 11-15 | 3 | Operator | AI executes real workflows. Connected to your data. You direct, AI does |
| 16-20 | 4 | Architect | AI is a team. Complex systems, sub-agents, quality gates |
| 21-25 | 5 | Founder Mode | AI is infrastructure. Scheduled, autonomous, integrated everywhere |
| 26-30 | 6 | Singularity | AI manages AI. Autonomous loops. You set vision, AI builds toward it |

---

## Phase 0: Load History (NEW in v2.1)

Before scanning, check for prior runs:

```bash
mkdir -p ~/.config/ai-flywheel/runs
ls -t ~/.config/ai-flywheel/runs/*.json 2>/dev/null | head -5
```

If prior runs exist, load the most recent one to compare scores. This enables:
- Diff section in dashboard ("Up from 18 last month")
- `--history` and `--diff` flags
- Anti-gaming validation (sudden score jump from 8 → 24 = suspicious)

Store reference to `LAST_RUN` (timestamp + scores per dimension + counts).

---

## Phase 1: Environment Scan (Inventory + Vitality)

**Critical:** This phase has TWO parts. Inventory (what exists) AND vitality (what's actually used). Past versions only counted inventory — that's gameable. Now we measure both.

### 1A. INVENTORY signals (raw counts)

```
1. CONTEXT inventory
   - CLAUDE.md: ~/.claude/CLAUDE.md, .claude/CLAUDE.md, CLAUDE.local.md
   - MEMORY.md: ~/.claude/projects/*/memory/MEMORY.md (line count)
   - Auto-memory files: count files in ~/.claude/projects/*/memory/
   - Settings: ~/.claude/settings.json (hooks count by type)

2. DATA REACH inventory
   - Local MCPs: ~/.claude.json mcpServers + .mcp.json
   - Cloud MCPs: count distinct prefixes mcp__<server>__ in available tools
   - Categorize: data, comms, dev, browser, automation, scraping, design

3. WORKFLOW POWER inventory
   - Skills: ls ~/.claude/skills/ + project .claude/skills/
   - Commands: ls ~/.claude/commands/ + project .claude/commands/
   - Agents: ls ~/.claude/agents/ + project .claude/agents/
   - Plugins: ~/.claude/plugins/ subdirs

4. OUTPUT QUALITY inventory
   - Deliverable skills (grep skill descriptions for: html, pdf, pptx, figma, png, mp4, csv)
   - Voice/brand skills (voz-*, brand-*, *-review)
   - Reference templates (examples/, templates/, patterns/, references/)

5. AUTONOMY inventory
   - Hooks: settings.json keys under "hooks" (PreToolUse, PostToolUse, SessionStart, Stop)
   - Headless scripts: grep -rl "claude -p" + "claude --print"
   - Cron/launchd user agents: crontab -l + ls ~/Library/LaunchAgents/ (filter user-created, exclude vendor like com.google.*, ai.perplexity.*)
   - Scheduled remote agents: presence of /schedule skill + recent invocations
   - Subagent count: ~/.claude/agents/ + project agents
   - n8n/Make scenarios accessible via MCP
```

### 1B. VITALITY signals (last 30 days activity)

Parse Claude Code transcripts for actual usage:

```bash
# Skill/command invocations in last 30 days
find ~/.claude/projects -name "*.jsonl" -mtime -30 -exec \
  grep -hoE '<command-name>[^<]+</command-name>' {} + 2>/dev/null \
  | sed 's/<[^>]*>//g' | sort | uniq -c | sort -rn

# MCP tool calls in last 30 days
find ~/.claude/projects -name "*.jsonl" -mtime -30 -exec \
  grep -hoE 'mcp__[a-zA-Z_]+__[a-zA-Z_]+' {} + 2>/dev/null \
  | awk -F'__' '{print $2}' | sort | uniq -c | sort -rn

# Memory file freshness
find ~/.claude/projects/*/memory -name "*.md" -mtime -30 | wc -l  # living
find ~/.claude/projects/*/memory -name "*.md" -mtime +90 | wc -l  # stale
```

Classify each asset:
- **Active** = invoked in last 30 days (or modified, for memory)
- **Dormant** = invoked 31-90 days ago
- **Dead** = >90 days ago, or never invoked

Compute:
- `ACTIVE_SKILLS / TOTAL_SKILLS` ratio
- `LIVE_MCPS / CONFIGURED_MCPS` ratio (MCPs with calls in 30d)
- `LIVING_MEMORY / TOTAL_MEMORY` ratio
- `TOP_3_INVOKED` (most-called skills, for signature workflow)

### 1C. SIGNATURE WORKFLOW (data-driven)

Pick from `TOP_3_INVOKED` (last 30 days). Among those:
- Prefer orchestrators (skills that invoke other skills)
- Prefer skills with multi-MCP integrations
- Tie-breaker: longest skill file

If no JSONL data available (new user, fresh install), fallback to longest+most-complex skill (legacy v2.0 method).

---

## Phase 2: Score Each Dimension (0-5)

Scoring uses **inventory × vitality**. Pure inventory scores are capped at 4/5. To reach 5/5 you need usage evidence.

### Context Depth
| Score | Criteria |
|-------|----------|
| 0 | No CLAUDE.md, no memory, no persistent context |
| 1 | CLAUDE.md OR MEMORY.md exists, basic info (<50 lines) |
| 2 | Structured memory: 5-15 files. CLAUDE.md references them. **At least 1 file modified in last 90 days** |
| 3 | Deep memory: 15-30 files, cross-referenced. **>30% modified in last 30 days** |
| 4 | Living system: 30+ files, auto-updating MEMORY.md index, feedback memories. **>50% touched in 30d** |
| 5 | Institutional memory: 50+ files. MEMORY.md updated in last 7 days. Feedback loops evident (corrections + confirmations both saved). Every active client/project has a memory file |

### Data Reach
| Score | Criteria |
|-------|----------|
| 0 | No MCPs |
| 1 | 1-2 MCPs configured |
| 2 | 3-5 MCPs configured, **at least 1 with recent tool calls** |
| 3 | 6-10 MCPs across 3+ categories, **>50% have calls in last 30d** |
| 4 | 10-15 MCPs incl. browser+scraping, **>60% live (called in 30d)** |
| 5 | 15+ MCPs covering CRM+email+docs+chat+analytics+web, **>70% live**. Configured MCPs without recent calls penalize toward 4/5 |

### Workflow Power
| Score | Criteria |
|-------|----------|
| 0 | No custom skills |
| 1 | 1-5 simple skills (<30 lines) |
| 2 | 6-20 skills with multi-step logic, **at least 3 invoked in last 30d** |
| 3 | 20-50 skills incl. orchestrators+agents, **>15 active in 30d** |
| 4 | 50-100 skills, orchestrators chain others, sub-agents used, **>25 active in 30d** |
| 5 | 100+ skills covering every business process, orchestrators compose smaller skills. **>40 active skills in last 30d. Active/Total ratio >35%** |

**Anti-gaming:** if ACTIVE_SKILLS / TOTAL_SKILLS < 25%, cap Workflow Power at 4/5 regardless of total count.

### Output Quality
| Score | Criteria |
|-------|----------|
| 0 | No output templates or deliverable skills |
| 1 | Some skills with basic format instructions |
| 2 | Skills with structured output templates and voice/style matching |
| 3 | Skills generating professional deliverables (HTML dashboards, formatted emails, reports). **At least 1 voice/brand skill** |
| 4 | Client-ready output: PPTX, complete campaigns, interactive dashboards. Quality hooks active. Brand-review skills present |
| 5 | Production-grade across all workflows. Voice consistency (voz-*, brand-* skills used regularly). Quality hooks gating output. Reference templates exist (examples/, patterns/) |

### Autonomy (Modernized)
| Score | Criteria |
|-------|----------|
| 0 | Every task requires manual prompting |
| 1 | Multi-step tasks execute within session with checkpoints |
| 2 | Complete workflows with approval gates run end-to-end |
| 3 | Programmatic automation: 1+ Claude Code hook active OR MCP-based workflows (n8n/Make/Apify) OR headless scripts (`claude -p`) |
| 4 | Multiple autonomy signals: hooks (2+ types) + scheduled jobs (Routines/cron/launchd) + subagent orchestration (10+ agents). Background jobs running on timers |
| 5 | Full infra: hooks (3+ types) + scheduled remote agents + autonomous loops + 25+ subagents. Agents spawning subagents. Self-monitoring loops (e.g., monitors that auto-trigger cleanup) |

**Note:** Vendor launchd plists (com.google.*, ai.perplexity.*, com.coccoc.*) do NOT count — only user-created automation.

---

## Phase 3: Present the Assessment

### Step 3.1: The Score

Format:

```
## AI Flywheel

### Your Tier: [TIER_NAME] ([TOTAL]/30)

  Context Depth:   [bar] [SCORE]/5 — [one-line why, reference vitality]
  Data Reach:      [bar] [SCORE]/5 — [N live / M configured]
  Workflow Power:  [bar] [SCORE]/5 — [A active / T total skills]
  Output Quality:  [bar] [SCORE]/5 — [one-line]
  Autonomy:        [bar] [SCORE]/5 — [one-line]

### Your Signature Workflow:
/[name] — invoked [X]x in last 30 days. [What it does. Inputs → outputs.]

### Top 3 Workhorses (last 30 days):
1. /[name] — [N] invocations
2. /[name] — [N] invocations
3. /[name] — [N] invocations

### What Your Setup Can Do Today:
[5-7 concrete capabilities — what you DO, not what you have]
```

### Step 3.2: The Gap

```
### Your Biggest Unlock: [WEAKEST DIMENSION]: [CURRENT]/5 → [TARGET]/5

[Plain language: what's missing, why it matters, what it would unlock.
Reference THEIR specific skills and tools by name.
For Workflow Power gap: name dead skills they could revive or archive.
For Autonomy gap: point to their existing /schedule or hooks they could activate.]

### 3 Steps to Level Up:
1. [Step referencing their actual setup]
2. [Step referencing their actual setup]
3. [Step referencing their actual setup]
```

### Step 3.3: Cleanup Opportunities (NEW)

```
### Dead Weight Detected

- [N] skills not used in 90+ days: [list 3-5 names]
- [N] memory files not touched in 90+ days
- [N] MCPs configured but no tool calls in last 30 days: [names]

→ Run `/level-up --cleanup` to archive dead skills and review stale memory.
```

If counts are zero or very low, skip this section entirely.

### Step 3.4: Tier-Specific Deliverable (NEW)

Each tier gets a CONCRETE deliverable, not just a CTA. See "Tier Deliverables" section below.

**Tone:** Knowledgeable friend. Reference their skills by name. Honest about gaming risk if scores look inflated. Insightful about gaps, not critical.

If `--assess` was used, stop here. Otherwise proceed to Phase 4-6.

---

## Phase 4: Interview (after results)

3 questions to personalize the dashboard:

1. **What industry?** (marketing / consulting / dev / operations / education / other)
2. **What's one thing your AI does that surprises people?**
3. **Submit to leaderboard?** (yes → ask: name, handle, LinkedIn, email — note: leaderboard backend coming in v3.0)

---

## Phase 5: Save Run

Persist the run for history tracking:

```bash
# Compute timestamp
TS=$(date -u +"%Y-%m-%dT%H:%M:%SZ")

# Write JSON
cat > ~/.config/ai-flywheel/runs/${TS}.json <<EOF
{
  "timestamp": "${TS}",
  "version": "2.1.0",
  "scores": {
    "context_depth": N,
    "data_reach": N,
    "workflow_power": N,
    "output_quality": N,
    "autonomy": N
  },
  "total": N,
  "tier": "TIER_NAME",
  "inventory": {
    "skills_total": N,
    "mcps_total": N,
    "agents_total": N,
    "memory_total": N
  },
  "vitality": {
    "skills_active_30d": N,
    "mcps_live_30d": N,
    "memory_touched_30d": N
  },
  "signature": "/skill-name",
  "top_3": ["/a", "/b", "/c"]
}
EOF
```

This enables `--history`, `--diff`, and progression tracking on the dashboard.

---

## Phase 6: Generate Dashboard

ALWAYS generate and open an HTML dashboard.

### Instructions
1. Build self-contained HTML, replace all `{{TOKENS}}`
2. Include radar chart SVG (5 axes, 0-5 scale)
3. If prior run exists: include "Progress Since Last Run" section with delta
4. Write to `~/Desktop/ai-flywheel-results.html`
5. Open with `open ~/Desktop/ai-flywheel-results.html` (macOS)

### HTML Template

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Flywheel: {{TIER_NAME}} ({{TOTAL_SCORE}}/30)</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body { background: #0a0a0a; color: #fff; font-family: 'SF Mono', Consolas, 'Courier New', monospace; min-height: 100vh; }
  .container { max-width: 900px; margin: 0 auto; padding: 48px 40px 64px; }
  .header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 48px; }
  .header h1 { font-size: 20px; font-weight: 700; letter-spacing: 3px; text-transform: uppercase; }
  .header h1 .accent { color: #f59e0b; }
  .header .version { font-size: 11px; color: #444; }
  .hero { display: flex; gap: 48px; align-items: flex-start; margin-bottom: 48px; }
  .score-display { flex-shrink: 0; text-align: center; min-width: 180px; }
  .score-number { font-size: 120px; font-weight: 400; color: #f59e0b; line-height: 1; }
  .score-label { font-size: 12px; letter-spacing: 3px; text-transform: uppercase; color: #888; margin-bottom: 4px; }
  .tier-name { font-size: 22px; color: #f59e0b; font-weight: 700; letter-spacing: 1px; }
  .score-sub { font-size: 12px; color: #555; margin-top: 4px; }
  .delta { display: inline-block; padding: 2px 8px; border-radius: 3px; font-size: 11px; margin-left: 6px; }
  .delta.up { background: #14532d; color: #4ade80; }
  .delta.down { background: #4c1d24; color: #f87171; }
  .delta.flat { background: #1f2937; color: #9ca3af; }
  .summary { flex: 1; border: 1px solid #1a1a1a; padding: 24px; color: #bbb; font-size: 14px; line-height: 1.7; }
  .stats { display: flex; gap: 16px; justify-content: center; margin-bottom: 48px; padding: 20px; border: 1px solid #1a1a1a; background: #111; }
  .stat { text-align: center; flex: 1; }
  .stat-num { font-size: 24px; color: #f59e0b; font-weight: 700; }
  .stat-num .total { color: #555; font-size: 16px; }
  .stat-label { font-size: 10px; color: #555; text-transform: uppercase; letter-spacing: 1px; margin-top: 4px; }
  .section-title { font-size: 18px; letter-spacing: 3px; text-transform: uppercase; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 1px solid #222; font-weight: 700; }
  .radar-wrap { display: flex; gap: 32px; align-items: center; margin-bottom: 48px; padding: 24px; border: 1px solid #1a1a1a; background: #111; }
  .radar-wrap svg { flex-shrink: 0; }
  .legend { display: flex; flex-direction: column; gap: 8px; font-size: 12px; color: #999; }
  .legend-item { display: flex; align-items: center; gap: 8px; }
  .legend-swatch { width: 12px; height: 12px; border-radius: 2px; }
  .dimensions { margin-bottom: 48px; }
  .dim-row { display: flex; align-items: center; gap: 16px; padding: 14px 0; border-bottom: 1px solid #111; }
  .dim-name { width: 160px; font-size: 13px; color: #ccc; font-weight: 600; }
  .dim-bar { flex: 1; height: 8px; background: #1a1a1a; }
  .dim-fill { height: 100%; }
  .dim-fill.s0 { width: 0%; }
  .dim-fill.s1 { width: 20%; background: #ef4444; }
  .dim-fill.s2 { width: 40%; background: #f97316; }
  .dim-fill.s3 { width: 60%; background: #f59e0b; }
  .dim-fill.s4 { width: 80%; background: #84cc16; }
  .dim-fill.s5 { width: 100%; background: #4ade80; }
  .dim-score { width: 56px; text-align: right; font-size: 14px; color: #888; font-weight: 700; }
  .signature { margin-bottom: 48px; padding: 24px; border: 1px solid #222; background: #111; }
  .sig-label { font-size: 11px; color: #555; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 8px; }
  .sig-name { font-size: 18px; color: #f59e0b; font-weight: 700; margin-bottom: 8px; }
  .sig-desc { font-size: 14px; color: #bbb; line-height: 1.6; margin-bottom: 16px; }
  .workhorses { display: flex; flex-direction: column; gap: 6px; padding-top: 16px; border-top: 1px solid #1a1a1a; }
  .workhorse { display: flex; justify-content: space-between; font-size: 12px; color: #999; }
  .workhorse-name { color: #ccc; }
  .workhorse-count { color: #f59e0b; font-weight: 600; }
  .capabilities { margin-bottom: 48px; }
  .cap-list { list-style: none; }
  .cap-list li { padding: 8px 0; border-bottom: 1px solid #111; font-size: 13px; color: #bbb; }
  .cap-list li::before { content: '>'; color: #4ade80; margin-right: 8px; }
  .cleanup { margin-bottom: 48px; padding: 20px; border: 1px solid #4c1d24; background: #1f0a0e; }
  .cleanup-title { font-size: 14px; color: #f87171; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 12px; font-weight: 700; }
  .cleanup-list { list-style: none; font-size: 13px; color: #bbb; }
  .cleanup-list li { padding: 6px 0; }
  .cleanup-list li::before { content: '×'; color: #f87171; margin-right: 8px; }
  .cleanup-cta { margin-top: 12px; font-size: 12px; color: #888; font-style: italic; }
  .gap { margin-bottom: 48px; }
  .gap-title { font-size: 16px; color: #f59e0b; margin-bottom: 12px; font-weight: 700; }
  .gap-text { font-size: 14px; color: #bbb; line-height: 1.7; margin-bottom: 20px; }
  .steps { display: flex; flex-direction: column; gap: 12px; }
  .step { display: flex; gap: 16px; padding: 16px; border: 1px solid #1a1a1a; background: #111; }
  .step-num { font-size: 24px; color: #f59e0b; font-weight: 700; flex-shrink: 0; width: 32px; }
  .step-title { font-size: 14px; color: #fff; font-weight: 600; margin-bottom: 4px; }
  .step-desc { font-size: 13px; color: #999; line-height: 1.5; }
  .deliverable { margin-bottom: 48px; padding: 24px; border: 1px solid #f59e0b; background: #1a1208; }
  .deliverable-label { font-size: 11px; color: #f59e0b; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 8px; font-weight: 700; }
  .deliverable-title { font-size: 18px; color: #fff; font-weight: 700; margin-bottom: 12px; }
  .deliverable-body { font-size: 14px; color: #ccc; line-height: 1.7; }
  .deliverable-body code { background: #0a0a0a; padding: 2px 6px; font-size: 12px; color: #f59e0b; }
  .progress { margin-bottom: 48px; padding: 20px; border: 1px solid #14532d; background: #0a1f12; }
  .progress-title { font-size: 14px; color: #4ade80; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 12px; font-weight: 700; }
  .progress-text { font-size: 13px; color: #ccc; line-height: 1.7; }
  .share { margin-bottom: 48px; }
  .share-copy { background: #111; border: 1px solid #222; padding: 16px; font-size: 12px; color: #888; line-height: 1.6; white-space: pre-wrap; position: relative; }
  .copy-btn { position: absolute; top: 8px; right: 8px; background: #222; border: 1px solid #333; color: #888; font-size: 11px; font-family: inherit; padding: 4px 10px; cursor: pointer; letter-spacing: 1px; text-transform: uppercase; }
  .copy-btn:hover { color: #f59e0b; border-color: #f59e0b; }
  .copy-btn.copied { color: #4ade80; border-color: #4ade80; }
  .cta { text-align: center; padding: 32px; border: 1px solid #222; background: #111; margin-bottom: 48px; }
  .cta-title { font-size: 16px; color: #fff; margin-bottom: 8px; font-weight: 700; }
  .cta-text { font-size: 14px; color: #bbb; line-height: 1.6; }
  .footer { text-align: center; padding-top: 24px; border-top: 1px solid #1a1a1a; color: #333; font-size: 11px; }
  .footer a { color: #555; text-decoration: none; } .footer a:hover { color: #f59e0b; }
  .disclaimer { font-size: 10px; color: #444; line-height: 1.5; max-width: 600px; margin: 0 auto 24px; text-align: center; font-style: italic; }
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <h1><span class="accent">AI</span> Flywheel</h1>
    <div class="version">v2.1</div>
  </div>
  <div class="hero">
    <div class="score-display">
      <div class="score-label">Your Score</div>
      <div class="score-number">{{TOTAL_SCORE}}</div>
      <div class="tier-name">{{TIER_NAME}} {{DELTA_BADGE}}</div>
      <div class="score-sub">out of 30</div>
    </div>
    <div class="summary">{{SUMMARY_TEXT}}</div>
  </div>

  {{PROGRESS_SECTION}}

  <div class="stats">
    <div class="stat"><div class="stat-num">{{ACTIVE_SKILLS}}<span class="total">/{{TOTAL_SKILLS}}</span></div><div class="stat-label">Skills Active</div></div>
    <div class="stat"><div class="stat-num">{{LIVE_MCPS}}<span class="total">/{{TOTAL_MCPS}}</span></div><div class="stat-label">MCPs Live</div></div>
    <div class="stat"><div class="stat-num">{{AGENT_COUNT}}</div><div class="stat-label">Agents</div></div>
    <div class="stat"><div class="stat-num">{{LIVING_MEMORY}}<span class="total">/{{TOTAL_MEMORY}}</span></div><div class="stat-label">Memory Live</div></div>
  </div>

  <div class="radar-wrap">
    {{RADAR_SVG}}
    <div class="legend">
      <div class="legend-item"><div class="legend-swatch" style="background:#f59e0b"></div>Your score</div>
      <div class="legend-item"><div class="legend-swatch" style="background:#4ade80;opacity:0.3"></div>Reference: top 10% setup</div>
      <div class="disclaimer" style="margin:8px 0 0;text-align:left">Reference benchmark is illustrative (4.5 avg per axis). Real population data requires leaderboard (v3.0).</div>
    </div>
  </div>

  <div class="dimensions">
    <div class="section-title">Dimension Detail</div>
    {{DIMENSION_ROWS}}
  </div>

  <div class="signature">
    <div class="sig-label">Signature Workflow</div>
    <div class="sig-name">{{SIGNATURE_NAME}}</div>
    <div class="sig-desc">{{SIGNATURE_DESC}}</div>
    <div class="workhorses">
      <div class="sig-label" style="margin-bottom:8px">Top 3 Workhorses · Last 30 Days</div>
      {{WORKHORSES_LIST}}
    </div>
  </div>

  <div class="capabilities">
    <div class="section-title">What Your Setup Can Do</div>
    <ul class="cap-list">{{CAPABILITY_ITEMS}}</ul>
  </div>

  {{CLEANUP_SECTION}}

  <div class="gap">
    <div class="section-title">Your Biggest Unlock</div>
    <div class="gap-title">{{GAP_DIMENSION}}: {{GAP_CURRENT}}/5 &rarr; {{GAP_TARGET}}/5</div>
    <div class="gap-text">{{GAP_EXPLANATION}}</div>
    <div class="steps">{{ROADMAP_STEPS}}</div>
  </div>

  <div class="deliverable">
    <div class="deliverable-label">{{DELIVERABLE_LABEL}}</div>
    <div class="deliverable-title">{{DELIVERABLE_TITLE}}</div>
    <div class="deliverable-body">{{DELIVERABLE_BODY}}</div>
  </div>

  <div class="share">
    <div class="section-title">Share Your Results</div>
    <div class="share-copy"><button class="copy-btn" onclick="copyShare(this)">Copy</button>{{SHARE_COPY}}</div>
  </div>

  <div class="cta">
    <div class="cta-title">{{CTA_TITLE}}</div>
    <div class="cta-text">{{CTA_TEXT}}</div>
  </div>

  <div class="disclaimer">Distribution percentiles shown are estimates from observed Claude Code adoption patterns. Real percentiles will be available once the leaderboard launches in v3.0.</div>

  <div class="footer">AI Flywheel v2.1<br><a href="https://github.com/mateofh/ai-flywheel">github.com/mateofh/ai-flywheel</a></div>
</div>
<script>
function copyShare(btn) {
  const text = btn.parentElement.textContent.replace('Copy','').trim();
  navigator.clipboard.writeText(text).then(() => { btn.textContent='Copied'; btn.classList.add('copied'); setTimeout(() => { btn.textContent='Copy'; btn.classList.remove('copied'); }, 2000); });
}
</script>
</body>
</html>
```

### Radar Chart SVG (build inline)

5 axes, 0-5 scale. Pentagon shape with concentric rings at 1, 2, 3, 4, 5.

```svg
<svg width="280" height="280" viewBox="-140 -140 280 280" xmlns="http://www.w3.org/2000/svg">
  <!-- Concentric pentagons (rings 1-5) -->
  <g stroke="#1a1a1a" fill="none" stroke-width="1">
    <polygon points="{{RING_5_POINTS}}" />
    <polygon points="{{RING_4_POINTS}}" />
    <polygon points="{{RING_3_POINTS}}" />
    <polygon points="{{RING_2_POINTS}}" />
    <polygon points="{{RING_1_POINTS}}" />
  </g>
  <!-- Axes -->
  <g stroke="#1a1a1a" stroke-width="1">
    <line x1="0" y1="0" x2="0" y2="-110" />
    <line x1="0" y1="0" x2="105" y2="-34" />
    <line x1="0" y1="0" x2="65" y2="89" />
    <line x1="0" y1="0" x2="-65" y2="89" />
    <line x1="0" y1="0" x2="-105" y2="-34" />
  </g>
  <!-- Reference benchmark (top 10% illustrative: 4.5 each) -->
  <polygon points="{{REFERENCE_POINTS}}" fill="#4ade80" fill-opacity="0.15" stroke="#4ade80" stroke-opacity="0.4" stroke-width="1" />
  <!-- Your score -->
  <polygon points="{{YOUR_POINTS}}" fill="#f59e0b" fill-opacity="0.35" stroke="#f59e0b" stroke-width="2" />
  <!-- Axis labels -->
  <g fill="#888" font-family="monospace" font-size="10" text-anchor="middle">
    <text x="0" y="-122">CONTEXT</text>
    <text x="118" y="-34">DATA</text>
    <text x="73" y="105">WORKFLOW</text>
    <text x="-73" y="105">OUTPUT</text>
    <text x="-118" y="-34">AUTONOMY</text>
  </g>
</svg>
```

**Coordinate math:**
- 5 axes at angles: -90° (top), -18°, 54°, 126°, 198° (i.e., 90°+72°k)
- Each unit = 22 pixels (so radius 5 = 110)
- For score `s` on axis `i`: `x = 22*s * cos(angle_i)`, `y = 22*s * sin(angle_i)`
- `RING_N_POINTS` = pentagon at radius N*22 (use angles for all 5 vertices)
- `REFERENCE_POINTS` = pentagon at radius 4.5*22 = 99 (all 5 axes)
- `YOUR_POINTS` = pentagon connecting (axis_i, score_i*22) for each dimension

Order of axes (CONTEXT, DATA, WORKFLOW, OUTPUT, AUTONOMY) matches dimension order.

### Dimension Row Format
```html
<div class="dim-row">
  <div class="dim-name">Context Depth</div>
  <div class="dim-bar"><div class="dim-fill s4"></div></div>
  <div class="dim-score">4/5</div>
</div>
```
Use class `s0` through `s5` matching the score.

### Workhorses List Format
```html
<div class="workhorse"><span class="workhorse-name">/cold-email-weekly</span><span class="workhorse-count">23x</span></div>
```

### Cleanup Section (only render if dead weight > 0)
```html
<div class="cleanup">
  <div class="cleanup-title">Dead Weight Detected</div>
  <ul class="cleanup-list">
    <li>{{N}} skills not used in 90+ days: {{LIST}}</li>
    <li>{{N}} memory files stale (>90d): use mtime to surface</li>
    <li>{{N}} MCPs configured but no recent calls: {{LIST}}</li>
  </ul>
  <div class="cleanup-cta">→ Run <code>/level-up --cleanup</code> to archive dead skills.</div>
</div>
```
If everything is fresh, omit this section entirely (`{{CLEANUP_SECTION}}` = empty).

### Progress Section (only if prior run exists)
```html
<div class="progress">
  <div class="progress-title">Progress Since Last Run · {{DAYS_AGO}} days ago</div>
  <div class="progress-text">{{PROGRESS_TEXT}}</div>
</div>
```
`PROGRESS_TEXT` example: "Up from 18 → 24. Workflow Power +1 (added 12 skills, 8 active). New MCPs: Notion, Supabase. Stale skills cleaned: 14."
If first run ever, omit this section.

### Delta Badge (next to tier name)
- Up: `<span class="delta up">+{{N}}</span>`
- Down: `<span class="delta down">−{{N}}</span>`
- Flat: `<span class="delta flat">±0</span>`
Omit if no prior run.

### Share Copy Format (v2.1)
```
My AI does the work of {{ESTIMATED_HUMANS}} people.

Tier: {{TIER_NAME}} ({{TOTAL_SCORE}}/30){{DELTA_TEXT}}

Context Depth:   {{BARS}} {{SCORE}}/5
Data Reach:      {{BARS}} {{SCORE}}/5
Workflow Power:  {{BARS}} {{SCORE}}/5
Output Quality:  {{BARS}} {{SCORE}}/5
Autonomy:        {{BARS}} {{SCORE}}/5

{{ACTIVE_SKILLS}}/{{TOTAL_SKILLS}} skills active · {{LIVE_MCPS}} MCPs live · {{AGENT_COUNT}} agents

Signature: {{SIG_NAME}} — {{SIG_INVOCATIONS}}x/month

Take the assessment: github.com/mateofh/ai-flywheel
#AIFlywheel
```

- `ESTIMATED_HUMANS` = `round(TOTAL_SCORE / 6)` — rough heuristic
- `DELTA_TEXT` if prior run: ` · up from {{LAST_SCORE}}` (else empty)
- Bar characters: `█████` 5/5, `████░` 4/5, `███░░` 3/5, `██░░░` 2/5, `█░░░░` 1/5, `░░░░░` 0/5

### Tier Deliverables (replace generic CTAs)

Each tier gets a CONCRETE deliverable in the dashboard. Render in `.deliverable` block.

**Tier 1 — Tourist (0-5)**
- Label: `STARTER PACK`
- Title: "3 skills + 3 MCPs to install today"
- Body: Specific install commands. Example:
  ```
  Skills (run in terminal):
  - /procesar-reunion — turn meeting notes into action items
  - /norte — daily AI guide
  - /brain — quick task capture in Notion
  
  MCPs (configure in Claude Desktop):
  - Notion (notes + tasks)
  - Gmail (email automation)
  - Read.ai (meeting transcripts)
  
  Install: claude plugin install hola-amigo/starter-pack
  ```

**Tier 2 — Power User (6-10)**
- Label: `LEVEL-UP BUNDLE`
- Title: "5 skills that will move you to Operator"
- Body: orchestrator skills that chain what they already have. Recommend `/aios-find` to discover their gaps.

**Tier 3 — Operator (11-15)**
- Label: `ARCHITECT'S PLAYBOOK`
- Title: "Sub-agent recipes + hook templates"
- Body: 3 hook templates to copy into `~/.claude/settings.json` + 5 sub-agent patterns. Link to GitHub gist.

**Tier 4 — Architect (16-20)**
- Label: `INNER CIRCLE`
- Title: "AI Operators Network — invite-only"
- Body: Discord invite link (placeholder until v3.0). Top 3% of users sharing workflows + opportunities. Apply with link.

**Tier 5-6 — Founder Mode / Singularity (21-30)**
- Label: `FRONTIER CHALLENGE`
- Title: "Build the future. Self-monitoring autonomous loops."
- Body: Reference patterns for agents-spawning-agents. List 3 frontier projects to attempt:
  1. Self-cleaning skill registry (auto-archive dormant skills)
  2. Autonomous content motion (monitor → ideate → produce → publish)
  3. Multi-agent CRM (agents managing leads end-to-end)
  
  Prize: First 10 to ship a working autonomous loop get featured in the next /level-up release.

### CTA by Tier (small, secondary to deliverable)
- **Tier 1-2:** "Want a guided path? My 8-week program takes you from Tourist to Operator."
- **Tier 3:** "Ready for the jump to Architect? My advanced cohort is for you."
- **Tier 4:** "You're ahead of 97%. Join the AI Operators Network."
- **Tier 5-6:** "You're building the future. Let's talk about what's next."

### Percentile Mapping (illustrative — flagged as such in dashboard)
- 0-5 → top 100% | 6-10 → top 55% | 11-15 → top 25%
- 16-20 → top 10% | 21-25 → top 3% | 26-30 → top 1%

These are estimates based on observed Claude Code adoption patterns — NOT measured population data. Real percentiles require the leaderboard backend (v3.0). The dashboard disclaimer makes this explicit.

---

## Flag Modes

### `/level-up --history`
Read all JSON files in `~/.config/ai-flywheel/runs/` and render a timeline:
```
Run 1 — 2026-02-15 — Tier 3 Operator (14/30)
Run 2 — 2026-03-10 — Tier 4 Architect (18/30) [+4]
Run 3 — 2026-05-01 — Tier 5 Founder Mode (24/30) [+6]
```
Plus simple ASCII chart of total over time. No dashboard.

### `/level-up --diff`
Compare last 2 runs:
- Skills added/removed
- MCPs added/removed
- Score changes per dimension
- Active skills changes (some skills became dormant, others activated)

### `/level-up --cleanup`
Generate (don't execute) a cleanup script:
```bash
# ~/Desktop/ai-flywheel-cleanup.sh
mkdir -p ~/.claude/skills/_archive
mv ~/.claude/skills/dead-skill-1 ~/.claude/skills/_archive/
# ... etc
```
Plus list of memory files to review (don't auto-delete — let user choose).
Plus list of MCPs that haven't been called in 30+ days (suggest review, don't touch config).

User runs the script themselves after review.

---

**Version:** 2.1.0
**Created by:** Mateo Folador (https://linkedin.com/in/mateofh)
**Changelog:**
- v2.1: Vitality scoring, radar chart, history tracking, dead weight detection, tier deliverables, modernized autonomy criteria, honest percentile disclaimer
- v2.0: Initial release with 5 dimensions / 6 tiers / inventory-only scoring
