# Learning Artifact

[![skills.sh](https://skills.sh/b/jjjardev/learning-artifact)](https://skills.sh/jjjardev/learning-artifact)

Learning Artifact is an Agent Skill that diagnoses what a learner actually knows, teaches only the gaps, then tests understanding with applied problems and a Feynman essay scored on a five-axis rubric.

Built for opencode and pi first. Works in any host that implements the [Agent Skills specification](https://agentskills.io/specification).

## What it does

1. **Interview.** Adaptive multiple-choice rounds map goals, knowledge, constraints, and confirmed gaps. No expert defaults, so the diagnosis cannot be led.
2. **Teach.** Micro-lessons only for shaky and unknown sub-topics, each gated by free recall and a teach-back.
3. **Test.** Applied or academic problems plus a 400 to 800 word essay, scored 1 to 10 on accuracy, completeness, reasoning, application, and clarity. Pass requires 7 or higher on every axis.
4. **Report.** Final status, evidence quotes, remaining gaps, and next steps. Honest by design: no grade inflation, no progress flattery.

State lives in `learning-space/` when file access is present, or in the printed `LEARNER STATE` block when it is not.

## Install

The skills CLI requires Node.js 22.20 or later.

Install the skill with the skills CLI:

```bash
npx skills add jjjardev/learning-artifact
```

Install it globally:

```bash
npx skills add jjjardev/learning-artifact -g
```

Manual install for opencode or pi:

```bash
mkdir -p .agents/skills
git clone https://github.com/jjjardev/learning-artifact /tmp/learning-artifact
cp -R /tmp/learning-artifact/.agents/skills/learning-artifact .agents/skills/
```

Both opencode and pi discover project skills under `.agents/skills/`.

## Use

Give the skill a learning goal:

```text
Teach me Bayesian inference for product analytics.
```

Or invoke it directly:

```text
/skill:learning-artifact
```

The optional `SEED:` line works in plain chat:

```text
SEED: linear algebra for graphics programming
```

Resume by pasting a `LEARNER STATE` block, pointing at an existing `learning-space/` folder, or starting a fresh session in the same project.

## Plain-chat prompt

For a chat interface without skill support, paste [`prompts/learning-artifact.md`](prompts/learning-artifact.md) as the first message and add a `SEED:` line.

## Repository layout

```text
.agents/skills/learning-artifact/SKILL.md   the Agent Skill
prompts/learning-artifact.md                the portable plain-chat prompt
```

## Compatibility

- opencode: loads the skill from `.agents/skills/` or through the `skill` tool.
- pi: loads the skill from `.agents/skills/` or by `/skill:learning-artifact`.
- Other Agent Skills hosts: install under their standard skills directory.

## License

MIT
