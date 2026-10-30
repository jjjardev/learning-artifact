# Learning Artifact

[![skills.sh](https://skills.sh/b/jjjardev/learning-artifact)](https://skills.sh/jjjardev/learning-artifact)

Learning Artifact is an Agent Skill that diagnoses what a learner actually knows, teaches only the gaps, reviews them on a forgetting schedule computed per gap, then tests understanding with applied problems and a Feynman essay scored on a five-axis rubric.

Built for opencode and pi first. Works in any host that implements the [Agent Skills specification](https://agentskills.io/specification).

## What it does

1. **Interview.** Adaptive multiple-choice rounds map goals, knowledge, constraints, and confirmed gaps. No expert defaults, so the diagnosis cannot be led.
2. **Teach.** Micro-lessons only for shaky and unknown sub-topics, each gated by free recall and a teach-back. Socratic probes are capped at two, never carry the answer, and never run where the process is measuring. Lessons may assign experiments: small scripts or builds the learner runs and shows back.
3. **Review.** Every session opens with review scheduled by a memory model. Each gap carries difficulty and stability; it comes due when its recall odds drop below 90 percent, and every review answer updates the schedule.
4. **Test.** Applied or academic problems plus a 400 to 800 word essay, scored 1 to 10 on accuracy, completeness, reasoning, application, and clarity. Pass requires 7 or higher on every axis.
5. **Report.** Final status, evidence quotes, remaining gaps, and next steps with a review forecast from the memory model. Honest by design: no grade inflation, no progress flattery.

In an AI harness with file access, the skill builds a `learning-space/` folder before Phase 1 and keeps it current:

```text
learning-space/
├── STATE.md      the LEARNER STATE block, rewritten every session
├── gap-map.md    sub-topic statuses plus per-gap memory state (D, S, last reviewed)
├── log.md        append-only session log: checks, scores, misconceptions, re-teaches
├── answers/      one file per session: questions plus the learner's ANSWERS: blocks
├── experiments/  code and activities the lessons assign, plus their results
└── reports/      one learning report per completed run
```

File access is optional: without it the same process runs in plain chat, the transcript is the store, and the printed `LEARNER STATE` block is the resume payload.

## What's new in v2

- Review is scheduled by a per-gap memory model (difficulty, stability, retrievability) instead of oldest-first.
- Session questions land in one answer file per session under `answers/`, filled in from memory and checked from the file. Failed checks produce a follow-up file, never an overwrite.
- Lessons can assign experiments that live in `experiments/`; a working experiment is evidence of mastery and can flip a shaky gap.
- Probing is scoped and capped: two probes maximum, then the direct re-teach, and never during review checks or the test.
- Transcript hygiene rules fold finished lessons into log lines so long runs stay compact without losing evidence.

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

In harness mode, each session's questions land in one file under `learning-space/answers/`: fill in the `ANSWERS:` blocks from memory, without scrolling back to the lesson text, and the skill checks the file. A failed check produces a follow-up file (`session-01b.md`), never an overwrite.

Resume by pasting a `LEARNER STATE` block, pointing at an existing `learning-space/` folder, or starting a fresh session in the same project. On resume, answer the days-elapsed question so the review schedule stays anchored.

## Plain-chat prompt

For a chat interface without skill support, paste [`prompts/learning-artifact.md`](prompts/learning-artifact.md) as the first message and add a `SEED:` line. File access is optional.

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
