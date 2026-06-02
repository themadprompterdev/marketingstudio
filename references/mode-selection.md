# MODE SELECTION

Decision tree for picking the right Marketing Studio Video mode based on goal, product, and distribution.

---

## The 9 verified modes

From `higgsfield model get marketing_studio_video`:

| CLI value | User-facing name | Best for | Avatar? |
|---|---|---|---|
| `ugc` | UGC | DTC paid social conversion, $40–$200 AOV | Yes |
| `ugc_how_to` | Tutorial | Beauty applicators, kitchen tools, app onboarding | Yes |
| `ugc_unboxing` | Unboxing | Kawaii, collectibles, gifts, subscription boxes | Yes |
| `product_showcase` | Product Showcase | Polished single-product highlight, hero shot vibe | Optional |
| `product_review` | Product Review | Tech, gadgets, considered purchases, hands-on demo | Yes |
| `tv_spot` | TV Spot | Brand awareness, hero campaigns, automotive, hospitality, luxury, industrial | No |
| `wild_card` | Wild Card | Viral concept, model picks the vibe, portfolio pieces | Varies |
| `ugc_virtual_try_on` | UGC Try-On | Apparel home try-on, casual fit demos | Yes |
| `virtual_try_on` | Pro Try-On | Premium fashion editorial, polished model | Yes |

**Hyper Motion is NOT in this enum.** If user asks for it, see `cli-reference.md` for alternatives.

---

## Decision tree

**What's the primary distribution goal?**

→ **Paid social conversion (Meta, TikTok), AOV $40–$200, DTC**
- Default: `ugc`
- Beauty/supplements/skincare: `ugc` or `ugc_how_to`
- Fashion: `ugc_virtual_try_on`
- Unboxing-natural products: `ugc_unboxing`
- Tech/gadgets: `product_review`

→ **Brand awareness / hero campaign / TV / website hero**
- Default: `tv_spot`
- Premium product reveal: `product_showcase`

→ **Local foot traffic (restaurant, retail)**
- Default: `ugc` with geo-anchored creator archetype

→ **Editorial fashion drop / streetwear**
- Default: `virtual_try_on`

→ **Viral experiment / portfolio piece**
- Default: `wild_card`

→ **B2B / industrial / workwear**
- Default: `tv_spot` (cinematic brand film)
- Companion piece for paid: `product_showcase` or `product_review`

---

## Push-back rules

The skill should push back (not refuse, push back with reasoning) when the user picks a mode that won't serve their stated goal.

**Examples of when to push back:**

- User says "TV Spot" for a $40 DTC product on Meta/TikTok → recommend `ugc` instead because TV Spot doesn't convert as well at that price point
- User says "UGC" for a luxury hospitality brand awareness campaign → recommend `tv_spot` instead for cinematic register
- User says "Product Review" for a beverage with no tech feature to review → recommend `ugc` (lifestyle) or `tv_spot` (brand)
- User says "Virtual Try-On" for accessories that aren't worn (e.g., backpack, watch) → recommend `product_showcase` instead

**Push-back format:**

```
You said [user's choice], but I'd recommend [my choice] because [one-line reasoning].
[User's choice] works when [scenario], but for [their actual goal] [my choice] tends to convert better because [reason].

Want to go with my recommendation, or override and run [user's choice] anyway?
```

If they override, comply. Don't lecture.

---

## Multi-mode campaign stacks

For full-funnel campaigns, combine modes:

**Full DTC Funnel (premium budget):**
- `tv_spot` (top of funnel, brand film)
- `product_showcase` (mid-funnel, polished hero)
- 3-5 `ugc` variants with different creator archetypes (bottom-funnel conversion)

**Local Business Stack:**
- 3 `ugc` variants with local-creator energy (primary)
- 1 `tv_spot` for milestone/grand opening moments

**Fashion Drop Stack:**
- 1 `virtual_try_on` editorial (brand film)
- 3-5 `ugc_virtual_try_on` variants (fit confidence, multiple body types)
- 1 `product_showcase` (fabric detail / hardware macro)

**Industrial / B2B Stack:**
- 1 `tv_spot` (cinematic brand film for sales deck and hero)
- 1 `product_showcase` (spec reveal)
- 2-3 `product_review` (real worker on-site testimonials)
