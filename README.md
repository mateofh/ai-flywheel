# AI Leverage Assessment

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

45% of users are Tourists. Less than 1% reach Singularity.

## Install (Claude Code users)

```bash
mkdir -p ~/.claude/commands && curl -sL https://raw.githubusercontent.com/mateovalenzuela/ai-assessment/main/.claude/commands/level-up.md -o ~/.claude/commands/level-up.md
```

Then run:

```
/level-up
```

## Don't have Claude Code?

Take the assessment on the web (coming soon): **ai-leverage.dev/assess**

6 questions. 2 minutes. Same score.

## What You Get

- **Score** across 5 dimensions (0-30)
- **Tier** with percentile ranking
- **Signature Workflow** — your most impressive automation highlighted
- **Gap Analysis** — your weakest dimension + 3 steps to level up
- **Dashboard** — HTML with visual breakdown
- **Share Copy** — ready-to-paste LinkedIn/Twitter post

## Example

```
Tier: Architect (22/30)

Context Depth:   ████░ 4/5
Data Reach:      █████ 5/5
Workflow Power:  █████ 5/5
Output Quality:  ████░ 4/5
Autonomy:        ████░ 4/5

147 skills | 19 MCPs | 33 agents

What's your AI leverage?
```

## License

MIT

## Author

[Mateo Valenzuela](https://linkedin.com/in/mateovalenzuela)
