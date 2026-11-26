# Bobo Universe Global Banner — Control Center (V1)

## Status
Control manifest prepared. Product integration is NOT deployed. Slot 1 is the Bobo Universe banner with the native soft-blinking NEW badge. Slot 2 is a temporary BoboStop promotion using `assets/banners/bobostop-banner.png`. Replace Slot 2 by editing this manifest; no app rebuild is required.

## Live manifest
`https://bobouniverseapp.github.io/website/global-banner/config-v1.json`

All current and future Bobo Universe products should fetch the same HTTPS JSON. GitHub Pages is the source of banner content. No additional Firebase project is needed for this feature.

## Universal self-promotion exclusion (configuration revision 3)

**Mandatory client behavior:** Each app/game must supply its own stable `client_app_id` (for example `bobomusic`, `bobostop`, `bobopop`, `boboknock`, `bobosnakeandladder`). This identifier is not downloaded from the manifest, since the client must identify itself independently.

- Load the global manifest once from the shared URL; do **not** create per-product manifests.
- If `self_promotion_policy.enabled` is true and `slots[1].product_id === client_app_id`, resolve the **effective** Slot 2 as a copy of Slot 1 (Bobo Universe) without mutating the server manifest.
- The BoboStop campaign stays in the shared manifest for all other products; BoboStop itself only displays the Bobo Universe banner.
- When the two effective slots have the same image URL and target URL, show one stationary Bobo Universe banner. Do not run a redundant slide/fade or alternate identical images.
- If Slot 2 later promotes BoboMusic, BoboMusic itself displays Bobo Universe only, while other products promote BoboMusic, without a new binary release.
- Do not add an exception to hide the entire promotional area. Keep the global banner visible.
- Test identity matching for all five existing product IDs, plus unmatched or missing IDs. Missing/unrecognized IDs must not silently act as another product; default to normal configured Slot 2 unless an application-specific safe fallback is required.
- Applying this new rule requires clients to implement it. Updating the JSON alone does not modify existing app binaries.

## Rules
- Exactly two logical slots displayed in **one** visible banner area.
- Slot 1 stays `bobo_universe`. Slot 2 may be `bobo_universe` or a product promotion.
- If the two slots resolve to identical images and destinations, skip visible rotation.
- When different, transition at the interval in `rotation`. Do not start multiple overlapping timers.
- "NEW" is a permanent soft-blinking badge for the Bobo Universe banner; do not infer whether an individual product is newly released. Keep the base logo static.
- Click destinations must use HTTPS and the approved domain allowlist. Avoid accepting arbitrary deep links or script URLs.
- If the remote request fails, response is malformed, or an image cannot load, use a cached last-known-good banner or a built-in Bobo Universe fallback. Never leave a blank area.
- Cache the manifest locally and refresh while respecting `refresh_after_seconds`. Network delays and GitHub Pages CDN caching mean changes are not instantaneous.
- The manifest must never contain passwords, API secrets, private keys, or sensitive configuration.
- Commercial ad placements (AdMob) are independent and unaffected.
- Each previously released Android app/game requires a single updated build to add this client capability. Later artwork and target URL changes do **not** require a new AAB.
- Honor reduced-motion/accessibility preferences. Keep a nonanimated badge as a fallback.
- Do not require sign-in to load a banner or allow banner errors to block an app launch.

## Integration verification checklist

1. Verify that both approved banner image URLs and the JSON manifest resolve successfully from GitHub Pages.
2. Integrate the manifest parser, cache, fallback, rotation, NEW badge behavior, and universal self-promotion exclusion in the pilot client (BoboMusic).
3. Check BoboMusic renders Bobo Universe + BoboStop; BoboStop renders Bobo Universe only, with no duplicate rotation.
4. Test an alternate promoted product by changing Slot 2 in the global manifest, confirming self-exclusion follows the new promoted product, then restore the desired promotion.
5. Test offline and malformed-manifest fallback before release.
6. Reuse the same protocol in each Godot or React Native product, avoiding unrelated feature modifications.

## Changing promotions later
- Change Slot 2 `image_url`, `target_url`, `type`, and `alt_text` only.
- Commit the manifest change in the website repository, allow GitHub Pages to deploy, and let installed client apps refresh the manifest.
- No app rebuild is necessary if the client already supports the schema and the new art format.
- Use a new asset filename rather than overwriting a cached image to avoid users seeing old creative.

## Notes
`asset_ready: true` means the approved artwork URL is in place. The global manifest declares the self-promotion rule; each client must implement it before users see the behavior. Changes to client logic require an app update, while changes to Slot 2 image/URL/product_id after that do not.
