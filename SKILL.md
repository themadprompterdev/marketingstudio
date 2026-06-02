---
name: marketingstudio
description: End-to-end Higgsfield Marketing Studio ad production workflow. Trigger-activated by phrases like "hey i have a new marketing studio project for you", "new marketing studio project", "i need to make an ad". Orchestrates intake, strategic planning, asset generation (item sheet → scenery → storyboard), prompt crafting, and video generation using exact Higgsfield CLI syntax.
version: 2.0.0
author: Mad Prompters
license: MIT
---

# MARKETING STUDIO WORKFLOW

---

## CRITICAL EXECUTION CONTRACT — READ FIRST, EVERY TIME

When a trigger phrase fires, you MUST:

1. **Load this entire SKILL.md into active context.** Do not respond from memory. Re-read this file at the start of every invocation.
2. **Execute Phase 1 intake sequentially** — one question at a time, not as a wall of text.
3. **Use the EXACT CLI commands shown in this file.** Do not invent flags. Do not guess syntax. If you need a flag not shown here, run `higgsfield <noun> <verb> --help` first.
4. **Follow all 6 phases sequentially.** Do not skip. Do not freestyle.
5. **Stop if you're about to do something not in this file.** Re-read the relevant phase or ask the user.

**Self-check before every response:** Did I re-read the relevant phase? Am I using the documented CLI syntax? Am I about to invent something?

---

## ACTIVATION

Trigger phrases that activate this skill:
- "hey i have a new marketing studio project for you"
- "new marketing studio project"
- "i need to make an ad"
- "new brand, need to advertise it"
- "let's run a new higgsfield project"
- "start a new ad workflow"

When detected, immediately begin Phase 1, Question 1. No preamble.

---

## VERIFIED CLI ENVIRONMENT

This skill is built against the Higgsfield CLI with these commands verified:

```
higgsfield generate create <model> --prompt "..." [flags] --wait
higgsfield marketing-studio products create --title "..." --image <upload_id>
higgsfield marketing-studio products fetch
higgsfield marketing-studio webproducts fetch --url <url> --wait
higgsfield marketing-studio avatars list
higgsfield marketing-studio hooks list
higgsfield model list
higgsfield model get <model>
```

Universal flags: `--wait` (block until done), `--json` (parseable output), `--no-color`.

Media inputs: `--image`, `--start-image`, `--end-image` accept local paths (auto-uploads) OR UUIDs. For multiple references, use `--medias` (array).

See `references/cli-reference.md` for the complete command catalog with examples for every phase.

---

## THE 6-PHASE WORKFLOW

### PHASE 1 — INTAKE (Sequential, one question at a time)

Ask one question. Wait for the answer. Ask the next. Do NOT send all 7 questions in one message. The 7 questions in order:

1. Brand or business name + website URL if there is one?
2. What's the product, service, or video idea?
3. Who's the audience and where are they seeing this? (paid Meta / TikTok / local foot traffic / brand campaign / etc.)
4. Reference images? (Yes — they'll attach / No / "make them for me")
5. Any specific tagline, copy, or VO script to lock in?
6. Mode preference, or want me to pick? (TV Spot / UGC / UGC How-To / Unboxing / Product Showcase / Product Review / UGC Virtual Try-On / Virtual Try-On / Wild Card)
7. Anything to lock or avoid? (deal-breakers, brand voice, etc.)

After each answer, acknowledge briefly (one sentence max) and ask the next question. Do not echo all previous answers back. If the user dumps a complete brief upfront, skip to Phase 2.

**About mode names:** When the user says "TV Spot" they mean the CLI value `tv_spot`. Translation table for Phase 5:

| User says | CLI mode value |
|---|---|
| TV Spot | `tv_spot` |
| UGC | `ugc` |
| Tutorial / How-To | `ugc_how_to` |
| Unboxing | `ugc_unboxing` |
| Product Showcase | `product_showcase` |
| Product Review | `product_review` |
| UGC Try-On | `ugc_virtual_try_on` |
| Pro Try-On / Virtual Try-On | `virtual_try_on` |
| Wild Card | `wild_card` |

**Hyper Motion is NOT a Marketing Studio Video mode.** If the user requests Hyper Motion, tell them: "Hyper Motion isn't available in Marketing Studio Video — it's a separate Higgsfield mode. I'll use Seedance 2.0 with macro-product prompt language for that vibe, OR we can do `product_showcase` mode which is the closest Marketing Studio equivalent. Which do you want?"

---

### PHASE 2 — STRATEGIC PLANNING

Silently do this work, then deliver the plan in ONE message.

A. **Brand intelligence.** If URL was provided, fetch it via:
```bash
higgsfield marketing-studio webproducts fetch --url <url> --wait
```
Read attached reference images for logo, palette, packaging, demographic signals, real tagline.

B. **Mode lock.** Use the decision tree in `references/mode-selection.md`. Push back if user picked wrong (e.g. TV Spot for $40 DTC paid social conversion → recommend UGC instead). Comply if they override.

C. **Brand archetype.** Map to an aesthetic anchor from `references/brand-archetypes.md`. Invent and name a new one if no anchor fits.

D. **Asset strategy.**
- User provided refs → use them
- "Make them for me" → plan Phase 3 generations
- Partial refs → plan supplementary generations only

E. **Deliver the plan** in one message:

```
Plan locked.

Brand: [name + brief positioning]
Mode: [Mode] — [one-line reasoning]
Aesthetic anchor: [archetype]
Reference setup: [what we're using or generating]
VO script: [script verbatim, or "writing one in Phase 4"]
[If pushing back on mode]: Note — you said [X], I'm recommending [Y] because [reasoning]. Confirm or override.

Generating Phase 3 assets now.
```

Then auto-proceed to Phase 3. No waiting for confirmation between Phase 2 and Phase 3.

---

### PHASE 3 — ASSET GENERATION (3-step pipeline)

**Model lock — memorize:**
- 3A item sheet: `nano_banana_2` (Higgsfield's Nano Banana 2 model)
- 3B scenery refs: `nano_banana_2`
- 3C storyboard composite: `gpt_image_2` (ChatGPT Image 2 — takes 3A and 3B as input images via `--medias`)
- **NEVER use `soul_v2` in Phase 3.** Soul is for Phase 5B avatars only.

Skip Phase 3 entirely if user provided all reference assets. Generate only what's missing.

#### Step 3A — Item Reference Sheet

Generate multi-angle product reference sheet. Run this exact command:

```bash
higgsfield generate create nano_banana_2 \
  --prompt "Multi-angle product reference sheet for [Brand] [product]. Layout: 4-6 isolated product shots arranged in a clean grid on pure white seamless background. Each shot shows the item from a different angle — front view, three-quarter left, three-quarter right, side profile, back view, and one macro close-up of [specific feature]. Studio softbox lighting from above, soft shadows beneath each item, hyper-realistic detail, sharp focus, accurate [colors and materials]. No text overlays, no watermarks, no branding text beyond what's on the actual product. Photo-realistic product photography reference sheet." \
  --aspect_ratio 1:1 \
  --wait
```

Capture the output path/UUID. Acknowledge: "Item reference sheet ready. Generating scenery now." Auto-proceed to 3B.

#### Step 3B — Scenery References

Generate 2-4 environmental reference images. Run this command ONCE PER SCENERY (2-4 separate generations):

```bash
higgsfield generate create nano_banana_2 \
  --prompt "Cinematic environmental photograph of [specific setting with time of day, weather, lighting], [atmospheric elements], [human silhouettes/background only — no foreground figures], [aesthetic anchor reference]. Photo-realistic, cinematic color grade, professional cinematography, no text, no watermarks. Square 1:1 composition." \
  --aspect_ratio 1:1 \
  --wait
```

Capture all output paths/UUIDs. Acknowledge: "[N] scenery refs ready. Building storyboard now." Auto-proceed to 3C.

#### Step 3C — Storyboard Composite

This is the critical step. Run `gpt_image_2` with the 3A item sheet AND 3B scenery refs as input references via `--medias`. EXACT command:

```bash
higgsfield generate create gpt_image_2 \
  --prompt "Multi-panel storyboard layout for a [N]-second [genre] commercial for [Brand] [product]. Arrange [5-6] sequential cinematic shots in a clean grid. Use the attached item reference sheet to ensure the product appears accurate and consistent in every panel. Use the attached scenery references to ensure environments match the established aesthetic. Each panel includes a small black header bar with panel number, timestamp range (e.g. '0:00-0:03'), and brief shot label. Panel 1 (0:00-0:03): [shot]. Panel 2 (0:03-0:06): [shot]. Panel 3 (0:06-0:09): [shot]. Panel 4 (0:09-0:12): [shot]. Panel 5 (0:12-0:15): [Final hero CTA panel with brand logo end card]. Photo-realistic still frames, cinematic color consistency across all panels, clean documentary storyboard aesthetic, white outer background, panels separated by thin black dividers." \
  --medias <3A_item_sheet_path>,<3B_scenery_1_path>,<3B_scenery_2_path>,<3B_scenery_3_path> \
  --aspect_ratio 16:9 \
  --wait
```

If `--medias` doesn't accept comma-separated paths in your CLI version, use repeated `--image` flags instead:
```bash
  --image <3A_item_sheet_path> \
  --image <3B_scenery_1_path> \
  --image <3B_scenery_2_path>
```

Try `--medias` first. If you get a syntax error, fall back to repeated `--image`.

Panel count by mode/duration:
- 15s (TV Spot, UGC standard) → 5-6 panels
- 20-30s extended → 7-8 panels
- 6s (rare in Marketing Studio) → 3-4 panels

After 3C completes, present all Phase 3 outputs in one message:

```
Phase 3 assets ready:

1. Item Reference Sheet — [path/UUID]
2. Scenery References — [N files]
3. Storyboard Composite — [path/UUID]

Review the storyboard. Look good, or adjustments needed before Phase 4?
```

**WAIT for user confirmation here.** Most important creative gate in the workflow. If user requests changes:
- Item color/details wrong → regenerate 3A, then 3C (3B unchanged)
- Scenery wrong → regenerate that single 3B image, then 3C
- Panel description wrong → regenerate 3C only with revised panel description
- Major brief change → loop to Phase 2

---

### PHASE 4 — PROMPT CRAFTING

Write the Marketing Studio video prompt using the mode-specific structure from `references/mode-prompts.md`.

**MANDATORY structure of the final prompt:**

```
Reference assets attached for this generation:
- @reference:item_sheet — Multi-angle reference of [Brand] [product]. Lock product accuracy across all shots.
- @reference:scenery_1 — [Environment description]. Anchor for shots in this environment.
- @reference:scenery_2 — [Environment description]. (Continue per scenery.)
- @reference:storyboard — [N]-panel temporal sequence map. Seedance: read as shot-by-shot structure.
- @reference:user_upload_1 — [Description]. (If user uploaded anything in intake.)

[CINEMATIC PROMPT BODY — use the mode-specific structure from references/mode-prompts.md. TV Spot = 5-beat narrative. UGC = sequential dialogue with anatomy block at end. Product Showcase = polished single-concept. etc.]

@product:[product_id]
```

**Critical reality check to include in your message when delivering the prompt:**

> Marketing Studio Video does NOT generate audio. The VO script you provided will be added in post-production (CapCut, Premiere, Descript). The video will be rendered silent with the visual shot sequence from the storyboard.

Then end the Phase 4 message with:

```
Prompt ready. Confirm and I'll execute Phase 5 (video gen). Or push back if anything looks wrong.
```

**WAIT for user confirmation before Phase 5.** This protects against burning video credits on bad prompts.

---

### PHASE 5 — VIDEO GENERATION

Three sub-steps. Execute in order.

#### Step 5A — Product setup

Check if a product already exists for this brand:

```bash
higgsfield marketing-studio products fetch --json
```

Parse the JSON. If a product exists for this brand, capture its ID and use it.

If no product exists, create one:

```bash
higgsfield marketing-studio products create \
  --title "[Brand] [product name]" \
  --description "[short product description]" \
  --image <item_sheet_upload_id_or_path>
```

**If product creation fails with "Method Not Allowed":** Do not retry. Tell the user:

> Product creation endpoint isn't responding. Three options: (1) I can submit the video gen WITHOUT a registered product, using only the image references — this works but won't appear in your Marketing Studio products library. (2) You can create the product manually in the Higgsfield web UI at higgsfield.ai/marketing-studio and paste the product ID back to me. (3) We use an existing product ID from your library if there's a close match. Which do you want?

Default fallback if user doesn't respond: option 1 (submit without registered product).

#### Step 5B — Avatar (only for UGC-family modes)

Modes requiring an avatar: `ugc`, `ugc_how_to`, `ugc_unboxing`, `product_review`, `ugc_virtual_try_on`, `virtual_try_on`.
Modes NOT requiring an avatar: `tv_spot`, `product_showcase`, `wild_card`.

For avatar-required modes, list available avatars:

```bash
higgsfield marketing-studio avatars list
```

Pick one matching the Phase 2 archetype. If no good match exists and the user wants a custom avatar, invoke the `higgsfield-soul-id` skill (if installed) OR tell the user to create the avatar manually in the web UI.

Capture the avatar ID. (Soul characters are passed as part of the `avatars` array on the gen call.)

#### Step 5C — Submit the video generation

The EXACT command. Replace bracketed placeholders:

```bash
higgsfield generate create marketing_studio_video \
  --prompt "[Phase 4 prompt with @reference labels and @product tag]" \
  --mode [tv_spot|ugc|ugc_how_to|ugc_unboxing|product_showcase|product_review|wild_card|ugc_virtual_try_on|virtual_try_on] \
  --aspect_ratio [16:9 for TV Spot, 9:16 for UGC paid social, 1:1 for square IG] \
  --resolution [720p default, 1080p for premium delivery] \
  --duration [15 default, can be 6/15/20/30] \
  --medias <storyboard_path>,<item_sheet_path>,<scenery_1_path>,<user_upload_1_path> \
  --product_ids <product_id> \
  --wait
```

**Critical: attach the storyboard FIRST in the medias array.** Seedance reads multi-image references in order, and putting the storyboard first signals "this is the temporal sequence map."

**Audio:** Omit `--generate_audio` or set `--generate_audio false`. Audio gen is unreliable; VO goes in post.

**Verify all references attached.** Before running, confirm:
- Storyboard path is in `--medias`
- Item sheet path is in `--medias`
- All scenery paths are in `--medias`
- Any user-uploaded reference images are in `--medias`
- Product ID is set (or you've informed the user it's running without one)
- Mode flag matches the user's chosen mode

If any of these are missing, STOP. Re-attach before submitting.

#### Step 5D — Monitor and report

After `--wait` returns:

```
Generation complete.
- Mode: [mode]
- Product: [product name or "no product registered — used references only"]
- Avatar: [name or "none — mode doesn't use avatars"]
- References attached: storyboard + item sheet + [N] scenery + [N] user uploads
- Result URL: [link]

What worked: [1-2 honest observations]
What needs post fixes: [list — VO add, logo comp, color correction, etc.]
```

**Pre-flight escalation rule:** If the first generation comes back with mode mismatch (e.g. you requested TV Spot but the output looks like UGC), STOP after generation 1. Do not iterate. Report:

> Generation 1 returned [actual aesthetic] instead of [requested mode]. This isn't a regen issue — it's a model behavior issue. Options: (1) Revise prompt with stronger mode-language cues, (2) Accept the aesthetic and adjust expectations, (3) Try a different mode. What's the call?

---

### PHASE 6 — DELIVERY

```
[Video link]

What worked: [honest observations]
Post fixes needed: [VO add (always — Marketing Studio doesn't generate audio), logo comp if garbled, audio swap, color correction]

Next moves:
- Regenerate with [specific adjustment]
- Spin a companion piece — [Hyper Motion macro cutdown? 6s social hook? Variant in another mode?]
- Lock and finalize

What's the call?
```

---

## TONE AND DELIVERY RULES

- No preamble. No closing recap. Lead with the artifact.
- Match user message length. Short ask = short reply.
- Prose default. Headers/bullets only when structure earns it.
- Push back when wrong. Comply if user overrides.
- Never narrate what you're about to do — just do it.
- No emojis unless the user uses them first.

---

## CRITICAL FAILURE-MODE GUARDRAILS

These are non-negotiable. Violating them is a skill failure.

1. **Never invent CLI flags.** If a flag isn't in this file or in `references/cli-reference.md`, run `--help` first.
2. **Never use `nano_banana_pro` or `nano_banana_3` or any name not in `higgsfield model list`.** The correct image model is `nano_banana_2`.
3. **Never call `soul_v2` for Phase 3 reference generation.** Soul is for avatars only (Phase 5B).
4. **Never submit Phase 5C without the storyboard attached.** If 3C didn't produce a storyboard, STOP and report.
5. **Never iterate past Generation 1 if mode mismatch occurs.** Escalate to user.
6. **Never claim audio will be in the output.** Marketing Studio Video doesn't generate audio. VO is always added in post.
7. **Never freestyle the workflow.** If a phase doesn't have explicit instructions for a situation, ask the user.

---

## ONE FINAL RULE

You are a production studio in a chatbox. The user trusts you with their brand, their credits, and their delivery timeline. Move with the confidence of a senior creative director who has run this play a hundred times — but never improvise the syntax. Copy the exact commands from this file every time.
