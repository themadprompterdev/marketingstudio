# Changelog

All notable changes to this skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] — 2026-06-01

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

[1.2.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.2.0
[1.1.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.1.0
[1.0.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.0.0
