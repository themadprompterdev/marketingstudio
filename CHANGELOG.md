# Changelog

All notable changes to this skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.3.0] — 2026-06-01

### Added
- **Phase 3 rebuilt as a 3-step sequential asset pipeline:**
  - **Step 3A — Item Reference Sheet (Nano Banana Pro):** Multi-angle product reference sheet (4-6 angles) so the product is visually locked across every downstream asset.
  - **Step 3B — Scenery References (Nano Banana Pro):** 2-4 environmental reference images matching the Phase 2 aesthetic anchor.
  - **Step 3C — Storyboard Composite (ChatGPT Image 2):** Multi-panel storyboard (5-6+ panels for 15s, more for longer spots) that takes the 3A item sheet and 3B scenery refs as INPUT images and composes the full temporal shot sequence.
- **Strict per-phase model locks** in a quick-reference table:
  - Nano Banana Pro = item and scenery references only
  - ChatGPT Image 2 = storyboard composites only (takes 3A/3B as inputs)
  - Soul 2.0 = avatars only (Phase 5B) — NEVER in Phase 3
  - Seedance 2.0 = video engine (accessed via Marketing Studio in Phase 5C)
- **Phase 5C labeled reference block** — when submitting the final Marketing Studio video generation, all Phase 3 outputs MUST be attached as input references AND labeled in the prompt itself (e.g., `@reference:item_sheet`, `@reference:scenery_1`, `@reference:storyboard`) so Seedance knows what each image is for.
- **Phase 3 → Phase 4 confirmation gate** — after all Phase 3 assets generate, present them to the user for review and wait for confirmation before moving to prompt crafting. This is the most important creative review gate in the workflow.
- **CLI syntax reminder** in the Tool Orchestration section to prevent invented flags.

### Why
v1.2.0 fixed the "skill loaded but not followed" problem. Real production runs after that revealed Phase 3 was too vague — agents were spraying generations across multiple wrong models (Soul 2.0 for product shots, generic text2image, etc.) and not building a coherent reference asset set. v1.3.0 makes Phase 3 a real production pipeline: lock the product visually first (3A), then lock the environments (3B), then compose the narrative storyboard that uses both as inputs (3C). All assets then get attached and labeled in the final video gen so Seedance has full visual context for what each reference is for.



### Added
- **Critical Execution Contract** header at the top of SKILL.md with explicit "MUST read first, every time" instructions to prevent agents from improvising the workflow from vague memory
- Self-check questions agents should run before every response in a Marketing Studio workflow
- **Sequential intake** in Phase 1 — questions are now asked one at a time, conversationally, instead of as a single wall of 7 questions. Feels like a creative director conducting an intake, not a customer service form.
- Power-user escape hatch — if user dumps a complete brief upfront, skip the sequential intake entirely and move to Phase 2
- Forbidden behaviors list for Phase 1 to prevent the most common improvisation drift (asking "what's the vibe" instead of the structured questions, skipping the VO script question, skipping the reference image question, etc.)
- Updated calibration example at the bottom of SKILL.md to demonstrate the sequential intake pattern

### Why
Real-world deployment revealed two issues:
1. Agents with the skill installed would acknowledge the skill exists but then improvise their own workflow from vague memory of previous Marketing Studio sessions. The skill was being "read as reference" instead of "executed as procedure." The Critical Execution Contract fixes this.
2. The original all-at-once intake (7 questions in one message) felt like a form rather than a conversation. On Discord and mobile, it was a wall of text that was easy to lose track of. Sequential intake feels native and lets the agent adapt based on each answer.

## [1.1.0] — 2026-06-01

### Added
- Higgsfield-exclusive routing language across Phase 3 and Tool Orchestration sections
- Explicit prohibition on falling back to non-Higgsfield image providers (DALL-E, Midjourney, etc.)
- YAML frontmatter with proper Hermes skill discovery metadata
- Trigger phrase registration for natural-language activation
- `required_mcp_servers` declaration for Higgsfield MCP dependency check
- `skill.json` manifest for Hermes Skills Hub compatibility
- Public-facing GitHub README with badges, install instructions, and contribution guide

### Changed
- Tool Orchestration section renamed to "HIGGSFIELD-EXCLUSIVE" with strengthened language
- Phase 3 intro line specifies "Higgsfield's image generation tools (via the Higgsfield MCP server)"

## [1.0.0] — 2026-05-27

### Added
- Initial release
- 6-phase workflow: Intake → Strategic Planning → Asset Generation → Prompt Crafting → Video Generation → Delivery
- All 9 Marketing Studio mode prompt structures (TV Spot, UGC, Tutorial, Product Review, Unboxing, UGC Virtual Try On, Pro Virtual Try On, Hyper Motion, Wild Card)
- 15 brand archetype anchors (American muscle, luxury hospitality, performance wellness, boba, kawaii decor, mom-creator, travel foodie, streetwear, premium skincare, tech launch, holiday gift, local business, app/SaaS, industrial workwear, two-half hybrid)
- 15 tactical failure mode callouts with in-prompt and post-production fixes
- Image generation prompt templates for product hero, environmental, creator archetype, and storyboard composite
- Decision shortcuts for fastest path / highest quality / tight budget velocity modes
- Push-back logic for incorrect mode selection
- Anatomy block for UGC modes (prevents the two-left-hands bug)
- Word count discipline (~40 word dialogue cap for UGC)
- Seedance storyboard-as-reference behavior documentation

[1.3.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.3.0
[1.2.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.2.0
[1.1.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.1.0
[1.0.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.0.0
