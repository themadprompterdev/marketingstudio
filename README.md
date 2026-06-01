# Marketing Studio Workflow — Hermes Agent Skill

> End-to-end Higgsfield Marketing Studio ad production in one Hermes Agent skill. From "I have a new brand to advertise" to "here's the finished video" — without leaving your terminal.

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Hermes Agent](https://img.shields.io/badge/Hermes-v0.10+-purple.svg)](https://hermes-agent.nousresearch.com)
[![Higgsfield MCP](https://img.shields.io/badge/Higgsfield-MCP-orange.svg)](https://higgsfield.ai/mcp)

---

## What it does

Trigger this skill in any Hermes Agent conversation with **"hey I have a new Marketing Studio project for you"** (or any natural variant) and it orchestrates the full production pipeline:

1. **Intake** — Asks 7 bundled questions about the brand, product, audience, and goals
2. **Strategic planning** — Picks the right Marketing Studio mode (1 of 9), brand archetype, and reference strategy
3. **Asset generation** — Calls Higgsfield image tools (Soul 2.0, Nano Banana, etc.) to create storyboards and reference images
4. **Prompt crafting** — Writes the Marketing Studio video prompt with mode-specific structure
5. **Video generation** — Creates the product, picks the avatar, submits the Marketing Studio gen
6. **Delivery** — Returns the finished video with honest assessment and next-step options

Brand-agnostic by design. Works for any category — beverages, industrial workwear, kawaii decor, automotive, fashion, beauty, SaaS, whatever.

---

## Why use it

- **Production execution, not just prompts.** Most Higgsfield skills generate text. This one actually runs the generations end-to-end via the Higgsfield MCP server.
- **All 9 Marketing Studio modes covered.** TV Spot, UGC, Tutorial, Product Review, Unboxing, UGC Try-On, Pro Try-On, Hyper Motion, Wild Card — each with its own prompt structure, dialogue word-count rules, and tactical failure-mode handling.
- **Real Power User craft baked in.** The skill includes the anatomy-block fix for the two-left-hands bug, the Seedance storyboard-as-reference behavior, the 40-word UGC dialogue cap, the dolly-zoom-into-product-reveal pattern, and the 14 brand archetype anchors that ship as starting points.
- **Push-back logic.** The skill will tell you when you're picking the wrong mode for your conversion goal. TV Spot for a $40 paid social product? It pushes you to UGC with reasoning.

---

## Prerequisites

1. **[Hermes Agent](https://hermes-agent.nousresearch.com)** v0.10.0 or later installed (macOS, Linux, or WSL2)
2. **An LLM provider** configured in Hermes (Claude, GPT-4-class, or any tool-capable model recommended)
3. **[Higgsfield MCP](https://higgsfield.ai/mcp)** connected to your Hermes Agent
4. **A Higgsfield account** with credits available (a full workflow run is typically 50–150 credits)

---

## Install

### Option A: One-line install (recommended)

```bash
hermes skills install marketingstudio --from https://github.com/themadprompterdev/marketingstudio
```

Hermes pulls the skill, drops it into `~/.hermes/skills/marketingstudio/`, and registers the trigger phrases automatically.

### Option B: Manual install

```bash
# Clone into the Hermes skills directory
cd ~/.hermes/skills
git clone https://github.com/themadprompterdev/marketingstudio.git

# Reload Hermes
hermes skills reload
```

### Verify the install

```bash
hermes skills list
```

You should see `marketingstudio` in the list. If not:

```bash
hermes skills audit marketingstudio
```

Surfaces any loading errors.

---

## Connect Higgsfield MCP (if you haven't already)

This skill requires the Higgsfield MCP server to execute generations. If you haven't connected it yet:

```bash
hermes mcp add higgsfield --url https://mcp.higgsfield.ai/mcp
```

Then authenticate via your Higgsfield account when prompted.

Verify the connection:

```bash
hermes mcp test higgsfield
```

You should see Higgsfield's tools available — Soul image generation, Nano Banana image generation, Marketing Studio video generation, product management, avatar management, etc.

---

## Usage

Start any Hermes conversation and trigger the workflow:

```
hey I have a new Marketing Studio project for you
```

The skill responds with the 7-question intake. From there, the workflow runs phase by phase, asking for confirmation at the key decision points (prompt approval, generation submission).

### Example session

```
You: hey I have a new Marketing Studio project for you

Skill: [delivers 7-question intake]

You: Brand is Lumi Skincare, lumiskincare.com. New vitamin C serum. 
Audience mid-twenties women on Meta/TikTok. Need creator generated. 
Tagline: "Wake up your skin." UGC.

Skill: [plans, generates creator reference via Soul 2.0]
[delivers Marketing Studio prompt with anatomy block]

You: confirm, run it.

Skill: [creates product in Marketing Studio with your uploaded shots]
[submits UGC generation with the prompt + product + creator avatar]
[returns finished video with assessment + next-step options]
```

### Shortcuts

The skill responds to velocity modes if you mention them during intake:

- **"Fastest path possible"** — skips storyboard generation, uses library avatars, defaults to UGC
- **"Highest quality possible"** — generates storyboards, custom avatars, multi-angle product refs, recommends companion cutdowns
- **"Tight budget"** — defaults to lower-credit modes, single variant, no Hyper Motion unless primary

### Push-back

If you pick a mode that won't serve your goal, the skill pushes back with reasoning. You can override by saying "no, run TV Spot anyway" — it'll comply but flag the concern.

---

## The 9 Marketing Studio modes

| Mode | Best for | Avatar |
|------|----------|--------|
| TV Spot | Brand awareness, hero campaigns | No |
| UGC | Paid social conversion (DTC under $200) | Yes |
| Tutorial | Beauty, kitchen tools, tech setup | Yes |
| Product Review | Tech, gadgets, considered purchases | Yes |
| Unboxing | Kawaii, collectibles, gifts | Yes |
| UGC Virtual Try On | Apparel, accessories, fashion DTC | Yes |
| Pro Virtual Try On | Premium fashion, streetwear, drops | Yes |
| Hyper Motion | Kinetic product reveals, 6s loops | No |
| Wild Card | Viral / brand experiments | Varies |

Full structures, runtimes, dialogue caps, and tactical failure modes for each are in [`reference.md`](reference.md).

---

## Customization

The skill is built brand-agnostic, but you can specialize it for your team's house style:

- **Override the mode decision tree** — edit `SKILL.md` Phase 2 to lock specific defaults for your category
- **Add a Phase 0** — auto-load your house style preferences at the top of `SKILL.md`
- **Extend brand archetypes** — add entries to the brand archetypes section in `reference.md`
- **Tune the failure modes** — add or remove tactical risk callouts in `reference.md` based on what your team actually hits

Do NOT remove:
- The trigger phrase YAML frontmatter (breaks activation)
- The 6-phase workflow structure (load-bearing)
- The push-back logic (it's a feature, not a bug)
- The anatomy block instructions for UGC modes (prevents the two-left-hands bug)

---

## Troubleshooting

**Skill not activating on the trigger phrase.**
Run `hermes skills list` and confirm it's loaded. If not, verify the folder structure and that `SKILL.md` has YAML frontmatter intact.

**Skill activates but doesn't call Higgsfield tools.**
Run `hermes mcp list` and `hermes mcp test higgsfield`. Re-authenticate if the token expired.

**Generated images don't match the brand.**
Check that your Higgsfield brand kit (colors, fonts, tone) is set up. The MCP integration pulls from it automatically. Also verify reference image hygiene — 4-5 consistent photo-realistic refs, no technical drawings or watermarked stock.

**UGC ads coming back with two left hands.**
The anatomy block in the skill should prevent this. If it persists, regenerate with the same prompt — Seedance has run-to-run variance. If it persists across multiple regens, the hand assignment in your specific prompt needs tightening.

**Want to use a non-Higgsfield image generator.**
This skill is Higgsfield-exclusive by design. The full ecosystem (brand kits, product library, avatar consistency, credits) lives in Higgsfield. If you need a different image gen provider, fork the skill and adapt Phase 3 accordingly.

---

## Contributing

PRs welcome. Especially looking for:

- New brand archetype entries (with proven aesthetic anchors)
- New failure-mode discoveries (and their fixes)
- Mode-specific prompt improvements that ship higher-quality output
- Translations of the intake questions to other languages

Open an issue first if it's a bigger change so we can align on direction.

---

## Versioning

**v1.1.0** — Higgsfield-exclusive routing, Hermes YAML frontmatter, 9-mode catalog

See [CHANGELOG.md](CHANGELOG.md) for the full history.

---

## Related resources

- [Hermes Agent docs](https://hermes-agent.nousresearch.com/docs/)
- [Hermes Skills Hub](https://agentskills.io)
- [Higgsfield MCP setup](https://higgsfield.ai/mcp)
- [Higgsfield Marketing Studio](https://higgsfield.ai/marketing-studio-intro)
- [Seedance 2.0](https://higgsfield.ai/seedance/2.0) (the video model powering Marketing Studio)
- [Soul 2.0](https://higgsfield.ai/soul-intro) (the avatar and image model powering Phase 3)

---

## License

MIT. See [LICENSE](LICENSE).

Use it. Fork it. Ship it.
