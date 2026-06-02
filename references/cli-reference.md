# CLI REFERENCE — HIGGSFIELD

Verified commands and flags for the Higgsfield CLI as used by the marketingstudio skill.

---

## Global flags (all commands)

- `--json` — print raw JSON output (parseable)
- `--no-color` — disable color output
- `-h, --help` — help for any command

---

## `higgsfield model`

List or inspect available models.

```bash
higgsfield model list                    # full live catalog
higgsfield model get <model>             # spec for one model (params, defaults, required fields)
higgsfield model get marketing_studio_video
higgsfield model get nano_banana_2
higgsfield model get gpt_image_2
higgsfield model get seedance_2_0
```

**Source of truth rule:** Never invent model names. If unsure, run `higgsfield model list`.

---

## `higgsfield generate`

Create image and video generation jobs.

### Subcommands

- `create <model>` — create a generation job
- `cost <model>` — estimate cost before generating
- `get <job_id>` — show one job
- `list` — list recent jobs
- `wait <job_id>` — poll until done

### Universal create flags

- `--prompt "..."` — required for most models
- `--aspect_ratio` — values vary by model. For Marketing Studio Video: `auto, 21:9, 16:9, 4:3, 1:1, 3:4, 9:16`
- `--resolution` — values vary by model. For Marketing Studio Video: `480p, 720p, 1080p`
- `--duration` — integer seconds (Marketing Studio default: 15)
- `--wait` — block until job finishes, print result inline
- `--wait-timeout` — max time to wait
- `--wait-interval` — polling interval

### Media input flags

- `--image <path or UUID>` — single reference image
- `--start-image <path or UUID>` — first frame for image-to-video
- `--end-image <path or UUID>` — last frame for image-to-video
- `--medias <path1,path2,path3>` — array of reference images (preferred for multi-image)
- `--video <path or UUID>` — video reference

Paths can be local files (auto-uploaded) OR UUIDs of previously-uploaded assets / completed jobs.

### Marketing Studio Video specific flags

For `higgsfield generate create marketing_studio_video`:

- `--mode` — one of: `ugc, ugc_how_to, ugc_unboxing, product_showcase, product_review, tv_spot, wild_card, ugc_virtual_try_on, virtual_try_on` (default: `ugc`)
- `--avatars` — array of avatar IDs
- `--product_ids` — array of product IDs
- `--web_product_ids` — array of web product IDs (from `webproducts fetch`)
- `--ad_reference_id` — reference an existing ad
- `--hook_id` — reference a saved hook
- `--setting_id` — reference a saved setting
- `--generate_audio` — boolean, default false (recommend false; VO in post)

---

## `higgsfield marketing-studio`

Manage Marketing Studio entities.

### Subcommands

- `products` — Marketing Studio products
- `webproducts` — products fetched from a URL
- `avatars` — Marketing Studio avatars
- `hooks` — saved hooks
- `settings` — saved settings
- `brand-kits` — brand kits
- `ad-formats` — DTC Ads Engine ad format presets
- `ad-references` — ad references
- `dtc-ads` — DTC Ads Engine (branded image generation)

### Products

```bash
higgsfield marketing-studio products fetch                     # list all products
higgsfield marketing-studio products fetch --json              # parseable list
higgsfield marketing-studio products create \
  --title "Brand Product Name" \
  --description "Short description" \
  --image <upload_id_or_local_path>
```

If `products create` returns "Method Not Allowed" on your build, fall back to:
1. Submit video gen without `--product_ids` (uses image references only)
2. User creates product via web UI at higgsfield.ai/marketing-studio and pastes ID back
3. Use an existing product from `products fetch` list

### Web Products

```bash
higgsfield marketing-studio webproducts fetch \
  --url https://example.com/product \
  --wait
```

Returns a web product ID consumable by `--web_product_ids` on video gen.

### Avatars

```bash
higgsfield marketing-studio avatars list                       # list available
```

Use the avatar ID in the `--avatars` array on `generate create marketing_studio_video`.

For custom avatars, use the separate `higgsfield-soul-id` skill to train one, then pass the returned `reference_id` as an avatar.

### Hooks / Settings / Brand-kits

```bash
higgsfield marketing-studio hooks list
higgsfield marketing-studio settings list
higgsfield marketing-studio brand-kits list
```

---

## Example: Full Phase 3 + Phase 5 command sequence

```bash
# Phase 3A — item sheet
higgsfield generate create nano_banana_2 \
  --prompt "Multi-angle product reference sheet for Viking Wear chainsaw boots..." \
  --aspect_ratio 1:1 \
  --wait
# → captures path: /tmp/job_abc123_item_sheet.png

# Phase 3B — scenery (run 3 times for 3 different scenes)
higgsfield generate create nano_banana_2 \
  --prompt "Cinematic environmental photograph of smoky BC wildfire dusk..." \
  --aspect_ratio 1:1 \
  --wait
# → /tmp/job_def456_scenery_1.png

higgsfield generate create nano_banana_2 \
  --prompt "Cinematic environmental photograph of wet industrial logging yard blue hour..." \
  --aspect_ratio 1:1 \
  --wait
# → /tmp/job_ghi789_scenery_2.png

# Phase 3C — storyboard with 3A + 3B as inputs
higgsfield generate create gpt_image_2 \
  --prompt "Multi-panel storyboard layout for a 15-second industrial workwear TV spot..." \
  --medias /tmp/job_abc123_item_sheet.png,/tmp/job_def456_scenery_1.png,/tmp/job_ghi789_scenery_2.png \
  --aspect_ratio 16:9 \
  --wait
# → /tmp/job_jkl012_storyboard.png

# Phase 5A — product setup
higgsfield marketing-studio products fetch --json
# (parse output, look for existing Viking Wear product)
# If none: 
higgsfield marketing-studio products create \
  --title "Viking Wear Chainsaw Boots" \
  --description "CSA-certified firefighter chainsaw boots..." \
  --image /tmp/job_abc123_item_sheet.png
# → product_id: prd_xyz789

# Phase 5C — final video
higgsfield generate create marketing_studio_video \
  --prompt "Reference assets attached for this generation:..." \
  --mode tv_spot \
  --aspect_ratio 16:9 \
  --resolution 1080p \
  --duration 15 \
  --medias /tmp/job_jkl012_storyboard.png,/tmp/job_abc123_item_sheet.png,/tmp/job_def456_scenery_1.png,/tmp/job_ghi789_scenery_2.png \
  --product_ids prd_xyz789 \
  --wait
# → final video URL
```

---

## Verified by

`higgsfield generate --help`, `higgsfield marketing-studio --help`, `higgsfield model get marketing_studio_video` (output dated 2026-06-02).
