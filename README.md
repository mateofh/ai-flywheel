# AI Flywheel

Measure how much work your AI can do without you.

Works for **everyone** — from ChatGPT in the browser to autonomous agent swarms. 5 dimensions, 30 points, 6 tiers.

## The 5 Dimensions

| Dimension | What It Measures |
|-----------|-----------------|
| **Context Depth** | How well does your AI know you? (0 = fresh chat every time, 5 = knows your entire business) |
| **Data Reach** | How many real data sources can it access? (0 = copy-paste, 5 = every tool connected) |
| **Workflow Power** | How much work is automated? (0 = type every prompt, 5 = every process is a command) |
| **Output Quality** | Is what it produces ready to use? (0 = needs rewrite, 5 = clients can't tell AI was involved) |
| **Autonomy** | Can it work without you? (0 = every step supervised, 5 = agents managing agents) |

## The 6 Tiers

| Score | Tier | Who This Is |
|-------|------|-------------|
| 0-5 | **Tourist** | Using AI like Google. Ask, get answer, move on. |
| 6-10 | **Power User** | Customized AI with GPTs or Projects. Better output, still manual. |
| 11-15 | **Operator** | AI executes workflows. Connected to your data. You direct, AI does. |
| 16-20 | **Architect** | AI is a team. Complex systems, sub-agents, quality gates. |
| 21-25 | **Founder Mode** | AI is infrastructure. Scheduled, autonomous, integrated everywhere. |
| 26-30 | **Singularity** | AI manages AI. You set vision, AI builds toward it. |

## What's New in v2.1

- **Vitality scoring** — measures actual usage (last 30 days), not just inventory. Hoarding skills no longer earns points.
- **Radar chart** — visual dashboard with your scores vs reference benchmark.
- **History tracking** — `/level-up --history` shows progression over time. Run it monthly to track your evolution.
- **Dead weight detection** — surfaces skills not used in 90+ days, stale memory, MCPs without recent calls.
- **Tier-specific deliverables** — Tourist gets a Starter Pack, Architect gets Pro Patterns, Founder Mode gets Frontier Challenges. Real value, not just CTA.
- **Modernized Autonomy** — counts hooks, scheduled remote agents, sub-agent orchestration (not just legacy cron).
- **Honest percentiles** — distribution stats are flagged as illustrative until v3.0 leaderboard ships.

## Install (Claude Code users)

```bash
mkdir -p ~/.claude/commands && curl -sL https://raw.githubusercontent.com/mateofh/ai-flywheel/main/.claude/commands/level-up.md -o ~/.claude/commands/level-up.md
```

Then run:

```
/level-up
```

### Modes

```
/level-up              Full assessment with dashboard
/level-up --assess     Score only, no dashboard
/level-up --history    Timeline of past runs
/level-up --diff       What changed since last run
/level-up --cleanup    Generate cleanup script for dead weight
```

## Don't have Claude Code?

Take the assessment on the web (coming soon in v3.0): **aiflywheel.com/assess**

6 questions. 2 minutes. Same score.

## What You Get

- **Score** across 5 dimensions (0-30) — vitality-weighted, not gameable
- **Radar chart** — your shape vs reference benchmark
- **Tier** with progression vs your last run
- **Signature Workflow** — data-driven from real invocations (last 30 days)
- **Top 3 Workhorses** — your most-invoked skills with monthly counts
- **Dead Weight** — skills/memory/MCPs to clean up
- **Tier Deliverable** — concrete next-step bundle, not generic upsell
- **Gap Analysis** — your weakest dimension + 3 specific steps
- **Dashboard** — self-contained HTML on your Desktop
- **Share Copy** — ready-to-paste with vitality stats

## Example

```
Tier: Architect (22/30)  +4 since last run

Context Depth:   ████░ 4/5 — 47/89 memory files touched in 30d
Data Reach:      █████ 5/5 — 12/14 MCPs live
Workflow Power:  █████ 5/5 — 38/103 skills active
Output Quality:  ████░ 4/5 — 8 deliverable skills, brand QA active
Autonomy:        ████░ 4/5 — 3 hook types + scheduled agents

Signature: /cold-email-weekly — 23x in last 30 days

What's your AI flywheel?
```

## License

MIT

## Author

[Mateo Folador](https://www.linkedin.com/in/mateofh/?locale=en)
