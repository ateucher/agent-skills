# The install template: `nmfs-opensci/litellm-gateway-template`

Notes for the template repo, kept here because **the template itself must not
carry a `claude/` directory**: "Use this template" copies every file, so each
install repo would inherit the maintainer's notes, and a session hook that
loads `claude/handoff.md` made an agent answer "hello" with the maintainer's
status instead of helping the installer (Eli, 2026-09-29). The template's
working copy on this hub is `~/litellm-gateway-template`.

## What it is

The repo an installer copies to run one LiteLLM gateway to Bedrock. It is
"task B" of the gateway follow-ups in `nmfs-opensci/agent-coders-clinics`
(`claude/notes/gateway-skill-tasks.md` there has the plan, which called it
`litellm-bedrock-gateway`). Task C, the colleague's instructions for an
install in an org AWS account, comes next.

## Eli's direction (2026-09-28/29)

- **The template uses the skill; it does not reimplement it.** The
  `litellm-bedrock-gateway` skill is the source of truth. The template says
  how to get the skill and what to type; everything else is the skill's.
- **One prompt in the README**: "Help me set up LiteLLM". No list of example
  prompts there; `AGENTS.md` has the agent read the repo state and offer the
  prompts that fit, then follow the skill.
- **An installer sets up one gateway, then adds workshops.** Workshops are
  named, each with its own organizer (e.g. "Set up a workshop named orca with
  organizer jane-blow"). The skill's step 1 still mixes gateway and workshop
  questions: agent-skills#29.
- **Claude Code greets on startup** through a project `SessionStart` hook
  (`.claude/settings.json` → `.claude/hooks/greeting.sh`, a `systemMessage`
  that depends on whether `gateway.env` exists), because agents say nothing
  until spoken to. Other agents rely on `AGENTS.md`, which also answers a bare
  "hello" with suggested prompts.
- License CC0; the reuse section asks for attribution to NMFS Open Science,
  and install repos inherit it. **No NOAA disclaimer.** GitHub template flag on.

## Deliberate, may look wrong

- **No `.gitignore` in the template.** `init_deployment.sh` only adds the
  skill's `.gitignore` when none exists, so shipping one would stop
  `secrets/` and `build/` from being ignored.
- **No scripts in the template.** The skill copies them in at setup so a
  running gateway does not change when the skill does.
- `AGENTS.md` recognises the repo state by files: no `gateway.env` = fresh;
  `secrets/gateway-url` = installer's deployed copy; plus
  `secrets/organizer-key` = an organizer's copy (no AWS). The master key is in
  Parameter Store, never local.

## Tested

- `init_deployment.sh .` on a scratch clone: adds `scripts/`, `assets/`,
  `gateway.env`, `models.yaml`, `.gitignore`; leaves template files alone.
- In a clean copy: `claude -p` showed the greeting hook firing; `claude -p
  "hello"` reported the state, noticed the skill was missing, gave install
  steps, ended with a suggested prompt.
- **Not tested:** the greeting in an interactive terminal; a real install
  driven by `AGENTS.md`.

## Open

- An install repo keeps the template README; whether the agent should rewrite
  it for that install is undecided.
