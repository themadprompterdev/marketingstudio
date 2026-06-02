# FAILURE MODES & TACTICAL FIXES

Production-tested issues and how to handle them. Reference these proactively in Phase 4 "risk flags" callout.

---

## Logo / Brand Text Garbling

**Symptom:** Logo typography on cups, boxes, labels renders garbled in the final video.

**In-prompt mitigation:** Specify "render brand logo as crisp permanent brand mark on the final frame, accurate typography, no text artifacts."

**Post-production fix:** Comp clean logo over warped AI version in CapCut / After Effects / Premiere. 10–15 minutes per shot.

---

## Two-Left-Hands / Anatomy Errors

**Symptom:** UGC avatars render duplicate hands, extra fingers, or mirrored limbs.

**In-prompt mitigation:** Always include the anatomy block at the end of UGC-family prompts:
> Anatomy: anatomically correct human hands with exactly five fingers per hand. One [left/right] hand [doing X]. One [right/left] hand [doing Y]. Natural finger positioning. No duplicate limbs, no mirrored hands, no extra fingers.

**If it persists:** Reduce hand visibility — mount phone in stand, tighten framing, product enters from below frame.

---

## Mode Mismatch (TV Spot delivering UGC aesthetic)

**Symptom:** User requested `tv_spot`, output looks like UGC (handheld, selfie energy, casual).

**Root cause:** The mode flag was correctly passed, but the prompt language was too UGC-flavored. Marketing Studio reads both the mode AND the prompt to determine aesthetic.

**Mitigation:** Strengthen mode-language cues in the prompt body:
- For `tv_spot`: explicit "broadcast commercial, cinematic camera moves, no first-person camera, no creator dialogue, professional cinematography"
- For `product_showcase`: explicit "polished studio quality, no handheld energy, no UGC aesthetic"

**Escalation rule:** If Generation 1 returns wrong mode, STOP. Do NOT iterate by regenerating with same setup. Report to user and ask for direction.

---

## Reference Images Not Being Used by Model

**Symptom:** Storyboard attached but final video doesn't follow shot sequence. Item sheet attached but product looks different in video.

**Diagnostic:** Run a control test — submit the same prompt with and without `--medias`. If output looks identical, model isn't reading the references.

**Mitigation:** 
1. Always put the storyboard FIRST in the `--medias` array (Seedance reads order-of-priority)
2. Reference the attached images explicitly in the prompt body with `@reference:` labels (see Phase 4 in SKILL.md)
3. If still not used, the model may be on a version that doesn't support multi-image reference for video — fall back to single most-important reference (the storyboard)

---

## CLI Syntax Errors

**Symptom:** "Too many positional args" or "Unknown flag" errors.

**Diagnostic:** Run `higgsfield <command> --help` to verify the exact flag syntax.

**Common mistakes:**
- Passing image paths as positional args instead of via `--image` or `--medias` flag
- Inventing flags like `--input_image`, `--ratio`, `--quality` (correct: `--image`, `--aspect_ratio`, `--resolution`)
- Using model names that don't exist (`nano_banana_pro` — correct: `nano_banana_2`)
- Using mode names with spaces or capitals ("TV Spot" — correct: `tv_spot`)

**Rule:** If a command throws a syntax error, STOP and run `--help`. Do not improvise alternative syntax.

---

## Product Creation "Method Not Allowed"

**Symptom:** `higgsfield marketing-studio products create` returns HTTP error.

**Diagnostic:** May be a build-specific endpoint issue, deprecated path, or auth scope limitation.

**Fallback path (documented in SKILL.md Phase 5A):**
1. Submit video gen WITHOUT `--product_ids` (use image references only)
2. User creates product manually via higgsfield.ai/marketing-studio web UI, pastes product ID back
3. Use existing product from `products fetch` list if there's a close match

---

## Sun-Blown Highlights on Golden-Hour

**Symptom:** Golden-hour shots blow out highlights, crushing product color.

**Mitigation:** Add to style block — "balanced exposure, product colors remain vibrant and saturated, not sun-blown, dynamic range preserved."

---

## Engine / Mechanical Audio Generic (Automotive)

**Symptom:** Generated score sounds like generic action-movie stock.

**Mitigation:** Note that audio is silent from Marketing Studio. For post: pull real engine audio from stock library (PremiumBeat, Epidemic Sound, Splice).

---

## Foreign-Language Signage

**Symptom:** Background signage renders as pseudo-glyph gibberish.

**Mitigation:** Fine for atmospheric background only. Never for focal storefront signage. Composite real signage in post if focal.

---

## Reference Image Hygiene

**Symptom:** Mixed-quality references degrade output (sketches alongside photos, watermarked stock).

**Mitigation:** 4-5 consistent photo-realistic references only. Remove technical drawings, line art, watermarked stock before attaching.

---

## UGC Dialogue Length Overflow

**Symptom:** VO script too long for the spot length, delivery feels rushed in post.

**Mitigation:** ~40 spoken words across 15s = 150 wpm. Count words. Read aloud against stopwatch. See `mode-prompts.md` word count table.

---

## Avatar Consistency Across Batches

**Symptom:** Avatars drift between gens when running variants.

**Mitigation:** Pin and rename the avatar in the Higgsfield library before running multi-variant tests. Reference the same avatar ID via `--avatars` across all variants.

---

## Product Variant Inaccuracy

**Symptom:** Generic descriptors produce generic visuals.

**Mitigation:** Name the specific variant in the prompt (e.g. "Strawberry Matcha Latte" not "boba drink", "yellow CSA-certified chainsaw boots" not "work boots").

---

## Holographic / Futuristic UI Text Drift

**Symptom:** Seedance adds UI text/labels to futuristic product reveals.

**Mitigation:** Explicit "no text, no labels, no UI elements, no typography" in style block.

---

## Worker / Talent Glamour-Drift

**Symptom:** Seedance defaults industrial talent to model-looking faces.

**Mitigation:** Specify "weathered real workers cast for authenticity not models, worn-in clothing, unselfconscious movement, real-world wear on all gear."

---

## Seedance Storyboard-As-Reference Behavior (USE THIS)

**Mechanic:** Seedance reads multi-panel composite reference images as TEMPORAL SEQUENCES — shot 1 → shot 2 → shot 3 — not as compositional layouts to copy.

**Implication:** When you generate a storyboard composite and pass it as the FIRST item in `--medias`, Seedance treats each panel as a shot in order. This is exactly what we want for TV Spot mode.

**Verification:** If the final video isn't following the storyboard sequence, double-check that the storyboard is FIRST in the `--medias` array. Order matters.

---

## Pre-Flight Test (recommended for high-stakes spots)

**Rationale:** Avoid burning 100+ credits on a 15s video that comes back wrong.

**Method:** Before submitting the full video gen, run a single 3-second test:

```bash
higgsfield generate create marketing_studio_video \
  --prompt "[full Phase 4 prompt]" \
  --mode tv_spot \
  --aspect_ratio 16:9 \
  --duration 3 \
  --medias <storyboard>,<item_sheet>,<scenery_1> \
  --wait
```

Review the 3-second output. If aesthetic matches expectations, commit to the full duration. If aesthetic is wrong (UGC instead of TV Spot, etc.), escalate per the mode-mismatch rule above.

---

## Audio Reality Check

**Rule:** Marketing Studio Video does NOT generate audio. Always inform the user before delivering the prompt:

> Heads up — Marketing Studio Video renders silent. Your VO script and any sound design will be added in post (CapCut, Premiere, Descript). The visual sequence will follow the storyboard.

Never imply audio will be in the output. Never set `--generate_audio true` unless the user has explicitly tested and confirmed it works on their build.
