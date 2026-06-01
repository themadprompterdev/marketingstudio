# Changelog

All notable changes to this skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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

[1.1.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.1.0
[1.0.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.0.0
