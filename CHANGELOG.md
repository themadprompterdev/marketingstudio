# Changelog

All notable changes to this skill will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.0.0] — 2026-06-02 — MAJOR REBUILD

### BREAKING CHANGES
Full rewrite informed by verified Higgsfield CLI output and real-world production post-mortem of v1.x. Not backward-compatible with v1.x; uninstall the old version before installing v2.0.

### Added
- **Exact CLI syntax embedded throughout SKILL.md.** Every generation step now includes the verbatim `higgsfield ...` command Alice should copy, not paraphrase or guess.
- **Verified model identifiers.** `nano_banana_2` (item + scenery), `gpt_image_2` (storyboards), `marketing_studio_video` (final video). Replaced incorrect references like `nano_banana_pro` and `chatgpt_image_2`.
- **Correct Marketing Studio Video mode enum.** All 9 modes documented with their CLI values (`tv_spot`, `ugc`, `ugc_how_to`, `ugc_unboxing`, `product_showcase`, `product_review`, `wild_card`, `ugc_virtual_try_on`, `virtual_try_on`).
- **`--medias` array flag for multi-image references.** Replaces the broken repeated `--input_image` pattern from v1.x.
- **`references/` subfolder** with five reference files loaded on-demand: `cli-reference.md`, `mode-selection.md`, `mode-prompts.md`, `brand-archetypes.md`, `failure-modes.md`.
- **Pre-flight test recommendation.** Optional 3-second test generation before committing to full duration to avoid credit waste from mode mismatch.
- **Audio reality-check.** Skill now always informs user that Marketing Studio Video does not generate audio. VO added in post.
- **Mode mismatch escalation rule.** If Generation 1 returns wrong aesthetic, STOP and report — do not iterate.
- **Product creation fallback path.** Handles "Method Not Allowed" errors from `products create` with three documented alternatives.
- **Reference attachment verification.** Pre-submission check that storyboard + item sheet + scenery are all in the `--medias` array.

### Changed
- **Hyper Motion handling.** Removed from Marketing Studio mode list (confirmed not in `marketing_studio_video` enum). Skill now offers alternatives when requested: `product_showcase` mode or separate Seedance gen.
- **Phase 3 structure.** Now explicitly a 3-step pipeline (3A item sheet → 3B scenery → 3C storyboard composite using 3A and 3B as input images). The storyboard takes the actual product and scenery references via `--medias` for visual consistency.
- **Phase 5 structure.** Three substeps: 5A product setup, 5B avatar (mode-dependent), 5C video gen. Each with verified CLI commands.
- **Main SKILL.md under 300 lines.** Content reorganized: decision-making in SKILL.md, content (commands, prompts, archetypes, failure modes) in `references/` subfolder. Loads faster, focuses agent attention.
- **YAML frontmatter** updated to canonical Higgsfield skill format (`name`, `description`, `version`, `author`, `license`).

### Why v2.0 over v1.3.1 patch
v1.x was built against unverified assumptions about the Higgsfield CLI. When real production runs failed (Viking Wear chainsaw boots TV Spot, June 2026), the post-mortem showed our skill had taught Alice incorrect model names, incorrect mode names, and incorrect flag syntax. Patching wouldn't fix the foundation. v2.0 is a clean rebuild against verified CLI output from `higgsfield generate --help`, `higgsfield marketing-studio --help`, and `higgsfield model get marketing_studio_video`.

## [1.3.0] — 2026-06-01

### Added
- Phase 3 rebuilt as 3-step sequential asset pipeline (item sheet → scenery → storyboard).
- Strict per-phase model locks documented in a table.
- Phase 5C labeled `@reference:` block in final prompt.
- Phase 3 → Phase 4 confirmation gate.

## [1.2.0] — 2026-06-01

### Added
- Critical Execution Contract header at the top of SKILL.md.
- Sequential conversational intake (one question at a time, not wall of text).
- Forbidden behaviors list for Phase 1.
- Power-user escape hatch for users who dump a complete brief upfront.

## [1.1.0] — 2026-06-01

### Added
- Higgsfield-exclusive routing language.
- YAML frontmatter with trigger phrases.
- skill.json manifest.

## [1.0.0] — 2026-05-27

### Added
- Initial release with 6-phase workflow.
- 9 Marketing Studio mode prompt structures (incorrect mode names — fixed in v2.0).
- 15 brand archetype anchors.
- 15 tactical failure mode callouts.

[2.0.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v2.0.0
[1.3.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.3.0
[1.2.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.2.0
[1.1.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.1.0
[1.0.0]: https://github.com/themadprompterdev/marketingstudio/releases/tag/v1.0.0
