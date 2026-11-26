# Global Banner V1 — Asset Upload / Activation Handoff

**Status: READY TO UPLOAD; NOT LIVE.** HQ has approved the two source images supplied on November 25, 2026. Both originals are transparent PNG 2048 × 682 (about 3:1), not the earlier incorrectly reported 661 × 396 artwork. Do **not** redesign either banner.

## Exact files to place in this website repository

Copy from the prepared **BoboUniverse_GlobalBanner_V2.zip** package into `assets/banners/`, preserving these names:

| Path | Size | Contents |
| --- | --- | --- |
| `assets/banners/bobo-universe-global-v2.png` | 1536 × 512 | Bobo Universe base artwork, **without** NEW |
| `assets/banners/bobostop-banner-v2.png` | 1536 × 512 | Approved BoboStop promotional artwork |
| `assets/banners/bobo-new-badge-v1.png` | 960 × 320 | Transparent NEW badge, standalone |

Both banner images were proportionally scaled from the supplied 2048 × 682 artwork to an exact 3:1 canvas; no design elements were redrawn. The badge is animated **only in the product clients**, by changing badge opacity.

## Activation instructions for website maintainer / Cursor

1. Add all three PNG files to the website repository, and commit + push them to `main`.
2. Verify each URL is actually reachable via HTTPS and responds with PNG content. These are the **new versioned** paths, not the current old banner paths.
3. Open `global-banner/config-v2.pending.json`, which contains the approved updated asset URLs, badge overlay configuration and existing self-promotion policy.
4. Once new image URLs work, **update** the active `global-banner/config-v1.json` to match the draft's production fields. Keep `control_url` pointing at `config-v1.json`, set `stage` appropriately for integration, and keep `schema_version: 1`.
5. Preserve Slot 1 = Bobo Universe and Slot 2 = BoboStop, both 8-second rotation / 450 ms transition. Do not alter `self_promotion_policy` or the commercial-ad separation.
6. Make sure `badge.image_url` refers only to the transparent overlay, rendered above the **badge-free** Bobo Universe image; placement is 15% of displayed banner width, x=1.5%, y=2% measured inside the 3:1 image viewport (scale with the banner).
7. Respect `badge.period_ms=1800`. NEW is permanent on the Bobo Universe banner, but fades gently rather than making the base banner blink.
8. Verify live JSON is valid and every referenced image is reachable after GitHub Pages deploy.
9. Tell BoboMusic Chad to switch to the final overlay asset contract and check a real Android screen. Existing automated checks are not a substitute for visual device testing.

**Do not modify existing app repos, home page, product pages, or AdMob. Do not run repository-wide investigations.**

## Safe rollout notes

- Current live `config-v1.json` must remain intact until the images have been uploaded and served.
- GitHub Pages/CDN and 3600-second client caching can delay changes; use versioned filenames to prevent mixing old/new art.
- The `config-v2.pending.json` file is **not** a client endpoint.
- Do not export APK/AAB in this website task.
