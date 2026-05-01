---
description: AI Flywheel — measure how much work your AI can do without you
---

# AI Flywheel

Measure how much work your AI can do without you. Works for everyone — from ChatGPT in the browser to autonomous agent swarms.

**Usage:**
- `/level-up` - Full assessment: scan + score + dashboard + share copy
- `/level-up --assess` - Score only, no dashboard

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

## Phase 1: Environment Scan

Silently scan everything. Do NOT ask permission. Gather raw signals.

```
1. CONTEXT signals
   - CLAUDE.md: check CLAUDE.md, .claude/CLAUDE.md, CLAUDE.local.md, ~/.claude/CLAUDE.md
   - MEMORY.md: check ~/.claude/projects/*/memory/MEMORY.md — count lines
   - Auto-memory files: count files in ~/.claude/projects/*/memory/
   - Project memory: check for memory/ directory
   - Settings: check .claude/settings.json
   - Does it reference clients, patterns, examples, conventions?
   - Is there evidence of feedback loops (memory files being updated over time)?

2. DATA REACH signals
   - Local MCPs: .mcp.json and ~/.claude.json mcpServers
   - Cloud MCPs: count tools starting with mcp__claude_ai_ in your context
   - Categorize: data, comms, dev, browser, automation, scraping, design
   - Total MCP count (local + cloud)
   - Can it access a browser? (Playwright, Puppeteer)
   - Can it scrape the web? (Apify, browser automation)

3. WORKFLOW POWER signals
   - Count: .claude/commands/ + .claude/skills/
   - Complexity check (read 3-5 skills):
     Simple (<30 lines) | Medium (30-100) | Complex (100+)
   - Multi-phase skills? Approval gates? Orchestrators?
   - Skills that chain other skills?
   - Agent definitions: .claude/agents/ or ~/.claude/agents/
   - Identify SIGNATURE WORKFLOW (longest/most complex skill)

4. OUTPUT QUALITY signals
   - Deliverable-generating skills (HTML, PDF, PPTX, emails, images)
   - Output templates or format specs in skills
   - Reference files (examples/, templates/, patterns/)
   - Quality hooks in settings.json

5. AUTONOMY signals
   - Shell scripts with `claude -p` or `claude --print`
   - MCP automation (n8n, Make, Zapier)
   - Apify actors
   - Cron, launchd, pm2, systemd
   - tmux configs or multi-session scripts
   - Remote triggers, scheduled agents
   - Autonomous loop scripts
```

---

## Phase 2: Score Each Dimension (0-5)

### Context Depth
| Score | Criteria |
|-------|----------|
| 0 | No CLAUDE.md, no memory, no persistent context |
| 1 | CLAUDE.md OR MEMORY.md exists, basic info (<50 lines) |
| 2 | Structured memory: 5-15 files covering projects or preferences. CLAUDE.md references them |
| 3 | Deep memory: 15-30 files, cross-referenced, covers clients + patterns + conventions |
| 4 | Living system: 30+ files, auto-updating MEMORY.md index, feedback memories, versioned context |
| 5 | Institutional memory: complete business knowledge base. Every client, process, decision documented. AI knows the business as well as the founder |

### Data Reach
| Score | Criteria |
|-------|----------|
| 0 | No MCPs, no integrations |
| 1 | 1-2 MCPs or basic file access |
| 2 | 3-5 MCPs spanning 2 categories |
| 3 | 6-10 MCPs spanning 3+ categories |
| 4 | 10-15 MCPs including browser + scraping |
| 5 | 15+ MCPs covering every business tool, real-time access to CRM + email + docs + chat + analytics + web |

### Workflow Power
| Score | Criteria |
|-------|----------|
| 0 | No custom skills |
| 1 | 1-5 simple skills (<30 lines each) |
| 2 | 6-20 skills, some with multi-step logic |
| 3 | 20-50 skills, complex ones (100+ lines), approval gates, agent definitions |
| 4 | 50-100 skills with orchestrators, skill chaining, sub-agents, hooks |
| 5 | 100+ skills covering every business process. Orchestrators compose smaller skills. New workflows built from existing components |

### Output Quality
| Score | Criteria |
|-------|----------|
| 0 | No output templates or deliverable skills |
| 1 | Some skills with basic format instructions |
| 2 | Skills with structured output templates and voice/style matching |
| 3 | Skills generating professional deliverables (HTML dashboards, formatted emails, reports) |
| 4 | Client-ready output: PPTX, complete campaigns, interactive dashboards. Quality hooks enforce standards |
| 5 | Production-grade across all workflows. Consistent quality. Clients cannot tell AI was involved |

### Autonomy
| Score | Criteria |
|-------|----------|
| 0 | Every task requires manual prompting |
| 1 | Multi-step tasks execute within a session with checkpoints |
| 2 | Complete workflows with approval gates run end-to-end |
| 3 | Programmatic automation: MCP-based workflows (n8n, Make, Apify), headless scripts |
| 4 | Scheduled automation: cron, launchd, background agents running on timer |
| 5 | Autonomous agents: multi-agent orchestration, agents spawning agents, self-monitoring loops |

---

## Phase 3: Present the Assessment

### Step 3.1: The Score

```
## AI Flywheel

### Your Tier: [TIER_NAME] ([TOTAL]/30)

  Context Depth:   [bar] [SCORE]/5 — [one-line why]
  Data Reach:      [bar] [SCORE]/5 — [one-line why]
  Workflow Power:  [bar] [SCORE]/5 — [one-line why]
  Output Quality:  [bar] [SCORE]/5 — [one-line why]
  Autonomy:        [bar] [SCORE]/5 — [one-line why]

### Your Signature Workflow:
/[name] ([X] lines) — [What it does. Be specific about inputs → outputs.]

### What Your Setup Can Do Today:
[5-7 concrete capabilities — what you CAN DO, not what files you have]

### Where You Stand:
[Total]/30 puts you in the top [X]% of AI users.
- 45% score 0-5 (Tourist)
- 30% score 6-10 (Power User)
- 15% score 11-15 (Operator)
- 7% score 16-20 (Architect)
- 2% score 21-25 (Founder Mode)
- <1% score 26-30 (Singularity)
```

### Step 3.2: The Gap

```
### Your Biggest Unlock: [WEAKEST DIMENSION]

[Plain language: what's missing, why it matters, what it would unlock.
Reference THEIR specific skills and tools by name.]

### 3 Steps to Level Up:
1. [Step referencing their actual setup]
2. [Step referencing their actual setup]
3. [Step referencing their actual setup]
```

**Tone:** Knowledgeable friend. Reference their skills by name. Genuine when impressed. Insightful about gaps, not critical.

If `--assess` was used, stop here.

---

## Phase 4: Interview (after results)

3 questions to personalize:

1. **What industry?** (marketing / consulting / dev / operations / education / other)
2. **What's one thing your AI does that surprises people?**
3. **Submit to leaderboard?** (yes → ask: name, handle, LinkedIn, email)

---

## Phase 5: Generate Dashboard

ALWAYS generate and open an HTML dashboard.

### Instructions
1. Build self-contained HTML, replace all `{{TOKENS}}`
2. Write to `~/Desktop/ai-flywheel-results.html`
3. Open in browser (`open` on macOS)

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
  .summary { flex: 1; border: 1px solid #1a1a1a; padding: 24px; color: #bbb; font-size: 14px; line-height: 1.7; }
  .stats { display: flex; gap: 24px; justify-content: center; margin-bottom: 48px; padding: 20px; border: 1px solid #1a1a1a; background: #111; }
  .stat { text-align: center; }
  .stat-num { font-size: 28px; color: #f59e0b; font-weight: 700; }
  .stat-label { font-size: 10px; color: #555; text-transform: uppercase; letter-spacing: 1px; margin-top: 4px; }
  .section-title { font-size: 18px; letter-spacing: 3px; text-transform: uppercase; margin-bottom: 20px; padding-bottom: 10px; border-bottom: 1px solid #222; font-weight: 700; }
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
  .dim-score { width: 40px; text-align: right; font-size: 14px; color: #888; font-weight: 700; }
  .signature { margin-bottom: 48px; padding: 24px; border: 1px solid #222; background: #111; }
  .sig-label { font-size: 11px; color: #555; text-transform: uppercase; letter-spacing: 2px; margin-bottom: 8px; }
  .sig-name { font-size: 18px; color: #f59e0b; font-weight: 700; margin-bottom: 8px; }
  .sig-desc { font-size: 14px; color: #bbb; line-height: 1.6; }
  .capabilities { margin-bottom: 48px; }
  .cap-list { list-style: none; }
  .cap-list li { padding: 8px 0; border-bottom: 1px solid #111; font-size: 13px; color: #bbb; }
  .cap-list li::before { content: '>'; color: #4ade80; margin-right: 8px; }
  .percentile { text-align: center; padding: 24px; border: 1px solid #1a1a1a; margin-bottom: 48px; }
  .pct-main { font-size: 16px; color: #fff; margin-bottom: 12px; }
  .pct-main span { color: #f59e0b; font-weight: 700; }
  .pct-dist { font-size: 12px; color: #555; line-height: 1.8; }
  .gap { margin-bottom: 48px; }
  .gap-title { font-size: 16px; color: #f59e0b; margin-bottom: 12px; font-weight: 700; }
  .gap-text { font-size: 14px; color: #bbb; line-height: 1.7; margin-bottom: 20px; }
  .steps { display: flex; flex-direction: column; gap: 12px; }
  .step { display: flex; gap: 16px; padding: 16px; border: 1px solid #1a1a1a; background: #111; }
  .step-num { font-size: 24px; color: #f59e0b; font-weight: 700; flex-shrink: 0; width: 32px; }
  .step-title { font-size: 14px; color: #fff; font-weight: 600; margin-bottom: 4px; }
  .step-desc { font-size: 13px; color: #999; line-height: 1.5; }
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
</style>
</head>
<body>
<div class="container">
  <div class="header">
    <h1><span class="accent">AI</span> Leverage Assessment</h1>
    <div class="version">v2.0</div>
  </div>
  <div class="hero">
    <div class="score-display">
      <div class="score-label">Your Score</div>
      <div class="score-number">{{TOTAL_SCORE}}</div>
      <div class="tier-name">{{TIER_NAME}}</div>
      <div class="score-sub">out of 30</div>
    </div>
    <div class="summary">{{SUMMARY_TEXT}}</div>
  </div>
  <div class="stats">
    <div class="stat"><div class="stat-num">{{SKILL_COUNT}}</div><div class="stat-label">Skills</div></div>
    <div class="stat"><div class="stat-num">{{MCP_COUNT}}</div><div class="stat-label">MCPs</div></div>
    <div class="stat"><div class="stat-num">{{AGENT_COUNT}}</div><div class="stat-label">Agents</div></div>
    <div class="stat"><div class="stat-num">{{MEMORY_COUNT}}</div><div class="stat-label">Memory</div></div>
  </div>
  <div class="dimensions">
    <div class="section-title">Your 5 Dimensions</div>
    {{DIMENSION_ROWS}}
  </div>
  <div class="signature">
    <div class="sig-label">Signature Workflow</div>
    <div class="sig-name">{{SIGNATURE_NAME}}</div>
    <div class="sig-desc">{{SIGNATURE_DESC}}</div>
  </div>
  <div class="capabilities">
    <div class="section-title">What Your Setup Can Do</div>
    <ul class="cap-list">{{CAPABILITY_ITEMS}}</ul>
  </div>
  <div class="percentile">
    <div class="pct-main">{{TOTAL_SCORE}} out of 30 puts you in the <span>top {{PERCENTILE}}%</span> of AI users.</div>
    <div class="pct-dist">45% score 0-5 (Tourist)<br>30% score 6-10 (Power User)<br>15% score 11-15 (Operator)<br>7% score 16-20 (Architect)<br>2% score 21-25 (Founder Mode)<br>&lt;1% score 26-30 (Singularity)</div>
  </div>
  <div class="gap">
    <div class="section-title">Your Biggest Unlock</div>
    <div class="gap-title">{{GAP_DIMENSION}}: {{GAP_CURRENT}}/5 &rarr; {{GAP_TARGET}}/5</div>
    <div class="gap-text">{{GAP_EXPLANATION}}</div>
    <div class="steps">{{ROADMAP_STEPS}}</div>
  </div>
  <div class="share">
    <div class="section-title">Share Your Results</div>
    <div class="share-copy"><button class="copy-btn" onclick="copyShare(this)">Copy</button>{{SHARE_COPY}}</div>
  </div>
  <div class="cta">
    <div class="cta-title">{{CTA_TITLE}}</div>
    <div class="cta-text">{{CTA_TEXT}}</div>
  </div>
  <div class="footer">AI Flywheel v2.0<br><a href="https://github.com/mateofh/ai-flywheel">github.com/mateofh/ai-flywheel</a></div>
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

### Dimension Row Format
```html
<div class="dim-row">
  <div class="dim-name">Context Depth</div>
  <div class="dim-bar"><div class="dim-fill s4"></div></div>
  <div class="dim-score">4/5</div>
</div>
```
Use class `s0` through `s5` matching the score. Color gradient: s1=red, s2=orange, s3=amber, s4=lime, s5=green.

### Share Copy Format
```
Just ran the AI Flywheel.

Tier: {{TIER_NAME}} ({{TOTAL_SCORE}}/30)

Context Depth:   {{BARS}} {{SCORE}}/5
Data Reach:      {{BARS}} {{SCORE}}/5
Workflow Power:  {{BARS}} {{SCORE}}/5
Output Quality:  {{BARS}} {{SCORE}}/5
Autonomy:        {{BARS}} {{SCORE}}/5

{{SKILL_COUNT}} skills | {{MCP_COUNT}} MCPs | {{AGENT_COUNT}} agents

Signature: {{SIG_NAME}} — {{SIG_SHORT}}

What's your AI flywheel? → github.com/mateofh/ai-flywheel

#AIFlywheel
```

Bar characters: `█████` for 5/5, `████░` for 4/5, `███░░` for 3/5, `██░░░` for 2/5, `█░░░░` for 1/5, `░░░░░` for 0/5.

### CTA by Tier
- **Tier 1-2 (Tourist/Power User):** "Want to 5x your AI output?" / "I teach professionals to go from prompting to operating. My program takes you from Tourist to Operator in 8 weeks."
- **Tier 3 (Operator):** "Ready for the jump to Architect?" / "You're ahead of 85% of AI users. My advanced cohort is for professionals at your level."
- **Tier 4 (Architect):** "Join the AI Operators Network" / "Top 3%. Private network of AI operators sharing workflows and opportunities. Invite-only."
- **Tier 5-6 (Founder Mode/Singularity):** "You're building the future" / "Less than 2% of AI users reach this level. Let's talk about what's next."

### Percentile Mapping
- 0-5 → top 100% | 6-10 → top 55% | 11-15 → top 25%
- 16-20 → top 10% | 21-25 → top 3% | 26-30 → top 1%

---

**Version:** 2.0.0
**Created by:** Mateo Folador (https://linkedin.com/in/mateofh)
