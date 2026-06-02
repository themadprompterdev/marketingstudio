# Marketing Studio Workflow — Hermes / Claude Code Skill

> End-to-end Higgsfield Marketing Studio ad production. Trigger-activated 6-phase workflow that drives the Higgsfield CLI with the exact verified syntax — no guessing, no improvised flags.

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-2.0.0-green.svg)](VERSION)
[![Higgsfield CLI](https://img.shields.io/badge/higgsfield-cli-orange.svg)](https://github.com/higgsfield-ai/cli)

---

## What it does

Say **"hey I have a new Marketing Studio project for you"** and the skill takes over a complete ad production pipeline:

1. **Intake** — Sequential 7-question conversational intake (not a wall of text)
2. **Strategic Planning** — Mode selection, brand archetype, push-back logic if the wrong mode is chosen
3. **Asset Generation** — 3-step pipeline:
   - **3A** Item reference sheet via `nano_banana_2`
   - **3B** Scenery references via `nano_banana_2`
   - **3C** Storyboard composite via `gpt_image_2` (takes 3A + 3B as inputs)
4. **Prompt Crafting** — Mode-specific prompt with labeled `@reference:` block and audio reality-check
5. **Video Generation** — `marketing_studio_video` model with all assets attached and `--mode` flag set
6. **Delivery** — Honest assessment, post-production fixes flagged, next-move options

---

## Built against verified CLI

v2.0 was built directly from the output of `higgsfield generate --help`, `higgsfield marketing-studio --help`, and `higgsfield model get marketing_studio_video`. Every command in the skill matches the actual CLI spec. No invented flags. No guessed syntax.

**Marketing Studio Video modes (9, verified):** `ugc, ugc_how_to, ugc_unboxing, product_showcase, product_review, tv_spot, wild_card, ugc_virtual_try_on, virtual_try_on`

**Image models used:** `nano_banana_2` (item + scenery), `gpt_image_2` (storyboards)

**Video model:** `marketing_studio_video` with mode flag, drives `seedance_2_0` underneath

---

## Prerequisites

1. **Higgsfield CLI** installed and authenticated:
   ```bash
   curl -fsSL https://raw.githubusercontent.com/higgsfield-ai/cli/main/install.sh | sh
   higgsfield auth login
   ```

2. **An AI agent that loads markdown skills** — Claude Code, Cursor, Codex, OpenClaw, or a Hermes Agent build that supports `SKILL.md` discovery

3. **A Higgsfield account** with credits available

---

## Install

### Claude Code / Cursor / Codex (canonical install)

```bash
# Clone into your agent's skills directory
cd ~/.claude/skills    # or ~/.cursor/skills, ~/.codex/skills
git clone https://github.com/themadprompterdev/marketingstudio.git
```

Reload your agent. The skill registers automatically via the YAML frontmatter.

### Hermes (managed deployments)

If you're on a Hermes Agent build where skills live elsewhere (e.g. `/opt/data/skills/`), drop the cloned folder into your build's skills directory:

```bash
git clone https://github.com/themadprompterdev/marketingstudio.git /opt/data/skills/marketingstudio
```

Then tell your agent to reload skills (varies by build — try `hermes skills reload` or `/reload-skills`).

### Verify

In a conversation:
```
hey i have a new marketing studio project for you
```

Expected response: **the first intake question only** — "Brand or business name + website URL if there is one?" — no preamble, no wall of 7 questions.

If you get anything else, the skill is loaded but not invoking. See Troubleshooting below.

---

## Usage

```
You: hey i have a new marketing studio project for you

Skill: Brand or business name + website URL if there is one?

You: Viking Wear, vikingwearonline.ca

Skill: Got it. What's the product or video idea?

[...sequential intake continues, one question at a time...]

Skill: [Phase 2 plan delivered in one message]
       Generating Phase 3 assets now.
       
Skill: [Runs 3A → 3B → 3C with exact CLI commands]
       Phase 3 assets ready: item sheet, 3 scenery refs, storyboard.
       Review before I move to Phase 4?

You: looks good

Skill: [Delivers Phase 4 prompt with @reference labels and audio reality-check]
       Confirm and I'll execute Phase 5.

You: confirm, run it

Skill: [Submits marketing_studio_video gen with --mode tv_spot, all assets attached]
       Generation complete. [Result URL]
       Post fixes needed: add VO from script, comp logo on end card.
```

---

## What's new in v2.0

This is a major rebuild informed by real-world production failures with v1.x.

**Architectural fixes:**
- ✅ Exact CLI syntax baked into every step — no more guessing
- ✅ Correct model identifiers (`nano_banana_2` not `nano_banana_pro`, `gpt_image_2` not `chatgpt_image_2`, `seedance_2_0` not `seedance 2.0`)
- ✅ Correct mode names matching CLI enum (`tv_spot` not "TV Spot", `ugc_how_to` not "Tutorial")
- ✅ Verified `marketing_studio_video` model with `--mode` flag
- ✅ Proper `--medias` array for multi-image references (not repeated positional args)
- ✅ Reference catalog in `references/` subfolder (CLI reference, mode selection, mode prompts, brand archetypes, failure modes)
- ✅ Pre-flight test recommendation to prevent failed video gen credit waste
- ✅ Audio reality-check (Marketing Studio Video doesn't generate audio — VO in post)
- ✅ Escalation rules for mode mismatch (don't iterate, ask user)
- ✅ Product creation fallback path when CLI returns "Method Not Allowed"

**Hyper Motion correction:** Not a Marketing Studio Video mode. The skill now handles requests for Hyper Motion by offering alternatives (`product_showcase` or separate Seedance gen) instead of pretending it works.

See [CHANGELOG.md](CHANGELOG.md) for full version history.

---

## Repository structure

```
marketingstudio/
├── SKILL.md                          # main workflow (under 300 lines, loaded on every invocation)
├── references/
│   ├── cli-reference.md              # verified Higgsfield CLI commands and flags
│   ├── mode-selection.md             # decision tree for choosing the right mode
│   ├── mode-prompts.md               # prompt structures for all 9 modes
│   ├── brand-archetypes.md           # aesthetic anchors for Phase 2
│   └── failure-modes.md              # production-tested issues + fixes
├── README.md                         # this file
├── CHANGELOG.md                      # version history
├── LICENSE                           # MIT
└── .gitignore
```

The references files are loaded on-demand by the agent when the relevant phase calls for them. Keeps the main SKILL.md focused on decision-making, not content dumps.

---

## Troubleshooting

**Trigger phrase doesn't activate the skill.**
- Run your agent's skill list command (`hermes skills list`, `/skills list`, etc.) and confirm `marketingstudio` is registered
- Check that the YAML frontmatter in SKILL.md is intact (between the `---` markers at the top)

**Agent loads the skill but improvises instead of following the workflow.**
- The Critical Execution Contract at the top of SKILL.md addresses this. If it still happens, the agent's runtime may not be respecting the contract — try priming the agent with: "Load and follow the marketingstudio skill. Re-read SKILL.md before responding to my next message."

**`higgsfield model list` returns empty or errors.**
- Run `higgsfield auth login` to refresh authentication

**Generations come back UGC when TV Spot was requested.**
- Check that `--mode tv_spot` was actually passed in the CLI call
- Strengthen TV Spot language in the prompt body (see `references/failure-modes.md` → "Mode Mismatch")

**Storyboard isn't being followed by the video model.**
- Verify the storyboard is FIRST in the `--medias` array (Seedance reads order-of-priority)
- Reference the storyboard explicitly in the prompt body with `@reference:storyboard — read as shot-by-shot structure`

---

## Contributing

PRs welcome. See [CONTRIBUTING.md](CONTRIBUTING.md). Especially looking for:
- New brand archetypes with proven aesthetic anchors
- New failure-mode discoveries with documented fixes
- Mode-specific prompt improvements tested across multiple generations

Do NOT add:
- Fallbacks to non-Higgsfield image/video providers
- Invented CLI flags or model names
- Soul model usage for Phase 3 reference generation (Soul is for avatars only)

---

## License

MIT. See [LICENSE](LICENSE). Use it, fork it, improve it.

---

## Related resources

- [Higgsfield AI](https://higgsfield.ai) — the platform
- [Higgsfield CLI](https://github.com/higgsfield-ai/cli) — the binary this skill drives
- [Higgsfield official skills](https://github.com/higgsfield-ai/skills) — the canonical reference for skill format
- [Higgsfield Marketing Studio](https://higgsfield.ai/marketing-studio) — the web UI

---

## Acknowledgments

Built and battle-tested in production by Mad Prompters. v2.0 informed by detailed post-mortem of v1.x failures including the Viking Wear chainsaw boots TV Spot run on 2026-06-02 — losing credits taught more than any spec doc.
