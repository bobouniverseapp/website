# BOBO UNIVERSE — GLOBAL BANNER ARTWORK STANDARD V1
**HQ decision: November 25, 2026**
**Status: APPROVED DESIGN STANDARD; ARTWORK UPDATE PENDING**

## 1. One canonical artwork format

- Every remotely served **main promotional banner** (Bobo Universe and promoted products) uses a **3:1 landscape composition**.
- Preferred authored canvas: **1536 × 512 px** (exact 3:1), RGB/RGBA PNG for current assets. Export optimized images suitable for mobile download. Higher-resolution source masters can be retained privately.
- Preserve the approved typography, composition, logo and colors. Do not stretch, squash, crop important elements, or redesign the artwork.
- Scale proportionally to the target banner area. A product may have a different screen resolution; do **not** hardcode 1024 × 341 pixels or force the UI to match that exact number. Prefer a responsive 3:1 content area; contain with acceptable background only if unavoidable.
- For BoboMusic, the existing 1024 × 341 area is effectively 3:1. A proper 3:1 asset fits without the current large side gaps.
- If an existing product banner is not 3:1, have HQ produce and publish a standardized version before using it as a campaign asset. Do not stretch it on-device.

## 2. NEW badge must NOT be embedded in the base artwork

- **Bobo Universe base banner:** no visible NEW badge baked into pixels.
- **NEW badge:** a separate transparent PNG overlay asset, consistently drawn above the base banner by each client.
- The badge overlay is visible *only* when the effective banner is the Bobo Universe global banner, including the self-promotion-exclusion case.
- HQ owns badge appearance and placement. Clients must not independently redesign or add a second badge.
- Anchor the NEW overlay to the **upper-left corner of the orange paw tile itself**, overlapping the paw artwork within the fitted 3:1 image (NOT the far top-left corner of the banner container). Manifest revision 5 specifies `badge.placement = paw_top_left_overlay`, `badge.position_reference = fitted_banner_image`, `badge.width_fraction = 0.12`, `badge.offset_x_fraction = 0.135`, and `badge.offset_y_fraction = 0.055`. These values are fractions of the final rendered image rectangle and must scale with it. Ensure the badge is within visible bounds and no parent clips it.
- Use manifest animation settings: currently `soft_blink`, period **1800 ms**. Animate only overlay opacity, e.g. subtle alpha pulse rather than flashing the entire banner. Keep the base banner stationary; obey reduced-motion preferences.
- If the badge overlay fails to load, show the static base banner without duplicates. Never block the app.

## 3. Publication and compatibility

**Do not overwrite the existing production-facing image URL in place while clients may have it cached.**

1. Create a new versioned, badge-free, 3:1 global base asset, e.g. `assets/banners/bobo-universe-global-v2.png`.
2. Create the approved separate transparent badge asset, e.g. `assets/banners/bobo-new-badge-v1.png`.
3. Reformat the initial promoted BoboStop banner into a versioned 3:1 asset too (e.g. `assets/banners/bobostop-banner-v2.png`) if its present dimensions are not 3:1.
4. Verify the new asset URLs over HTTPS and visual quality on a mobile-sized 3:1 canvas.
5. Only then update the shared `global-banner/config-v1.json` to new versioned image URLs and add an explicit `badge.image_url` plus approved layout data; increment `configuration_revision`.
6. Keep the existing remote schema fields backwards compatible. Older clients that render a native NEW overlay must be adjusted to honor the new remote badge asset (and not draw an extra badge).
7. Validate BoboMusic with the new assets before rolling out to other products. **Automated PASS does not replace physical visual testing.**
8. Do not publish a new visual manifest until the replacement assets actually exist and clients can support the new overlay contract.

## 4. Unchanged shared system rules

- One visible banner area with two rotating logical slots.
- Slot 1 = Bobo Universe. Slot 2 = HQ-selected promotion (initially BoboStop).
- A product never promotes itself: when its `client_app_id` matches Slot 2's `product_id`, effective Slot 2 becomes Bobo Universe; skip redundant rotation.
- Rotation 8 seconds, transition 450 ms, manifest refresh 3600 seconds, pending future remote changes.
- AdMob stays independent; do not change core app features, purchases, networking or gameplay.
- Cross-platform implementation must be minimal and focused, not a broad code investigation.

## 5. Immediate assignments

**HQ / website artwork maintainer:** derive/export the versioned, approved 3:1 images and transparent badge asset; verify, then update manifest in one coordinated publication.

**BoboMusic client:** wait for the new assets and manifest. Do not commit/push additional banner rework from this artwork feedback until the updated contract has been supplied. Existing automated PASS is provisional; physical testing pending.

**Other product clients:** adopt this standard for new integrations. Do not implement double-NEW solutions.

## Known limitation

The BoboMusic report identifies the current global art as 661 × 396 (~1.67:1) against its 1024 × 341 (~3:1) placement. HQ has not independently decoded the published image pixels in this session. Treat those exact source dimensions as reported by BoboMusic and verify during artwork export.

## 6. Godot client migration (Snake & Ladder issue found Nov 27)

The existing `scripts/home/global_banner_badge.gd` still tries to split a red/painted badge out of the old banner image. The current global artwork is **badge-free**, so image extraction returns no NEW layer. The new `badge.image_url` must be parsed, downloaded/cached and displayed as a **distinct transparent overlay**, not extracted from the base image. Place it using the live fractional coordinates relative to the *fitted artwork rectangle*. Use the same overlay positioning for current and transition/previous layers. The default 3:1 image-fit rectangle should not be confused with the entire screen or parent canvas. Never modify multiplayer logic to fix this purely visual issue.
