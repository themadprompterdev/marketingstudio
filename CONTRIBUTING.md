# Contributing

Thanks for considering a contribution. This skill is open-source under MIT and improvements are welcome.

## What we're looking for

### Brand archetypes
New entries in `reference.md` under the Brand Archetypes section. A good archetype contribution includes:
- A clear category name
- Visual codes (palette, lighting, surfaces, camera language)
- Audio direction (score style, sound design)
- Copy register (tone of dialogue, hook patterns)
- A real-world reference brand or film (so the AI has an aesthetic anchor)
- An "avoid" list (what NOT to do for this category)

### Failure mode discoveries
New entries in `reference.md` under Tactical Failure Modes. A good failure mode contribution includes:
- The specific problem and when it happens
- An in-prompt fix (if one exists)
- A post-production fallback fix
- When to proactively flag this in tactical notes

### Mode-specific prompt improvements
PRs that improve the prompt structures in `reference.md` should:
- Be tested across 5+ real generations before submission
- Include the comparison output (before/after) in the PR description
- Not break backward compatibility — keep the existing skeleton, add to it

### Translations
The intake questions and key user-facing strings can be translated. Open an issue first to coordinate which language.

## What we're NOT looking for

- Fallbacks to non-Higgsfield image or video providers (this skill is Higgsfield-exclusive by design — fork it if you need a different provider)
- Removing the push-back logic (it's a feature, not friction)
- Removing the anatomy block for UGC modes (it prevents a known bug)
- Adding tool calls outside the Higgsfield ecosystem in core phases

## PR process

1. Open an issue first describing the proposed change. Get a sanity check before investing time.
2. Fork, branch, make the change.
3. Update `CHANGELOG.md` under an "Unreleased" section with a clear description of what changed.
4. Submit the PR with:
   - A clear description of the problem and the solution
   - Screenshots or generation outputs if relevant
   - Updated tests if you touched any behavior worth verifying

## Code of conduct

Be useful, be honest, be brief. Treat other contributors like working professionals. No dunking, no LARPing.

## Questions

Open an issue with the `question` label.
