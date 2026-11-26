# Bobo Universe Global Banner — Control Center (V1)

## Status
Control manifest prepared. Product integration is NOT deployed. Slot 1 is the Bobo Universe banner with the native soft-blinking NEW badge. Slot 2 is a temporary BoboStop promotion using `assets/banners/bobostop-banner.png`. Replace Slot 2 by editing this manifest; no app rebuild is required.

## Live manifest
`https://bobouniverseapp.github.io/website/global-banner/config-v1.json`

All current and future Bobo Universe products should fetch the same HTTPS JSON. GitHub Pages is the source of banner content. No additional Firebase project is needed for this feature.

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

## Required work before product integration
1. Upload the approved high-resolution Bobo Universe banner PNG to `assets/banners/bobo-universe-global.png`.
2. Change both `slots[].image_url` to the new published asset URL, change `asset_ready` to `true`, and remove the temporary-asset note. Keep the two slots identical by default.
3. Verify image and manifest URLs over HTTPS in an ordinary browser after GitHub Pages deployment.
4. Test the remote update by temporarily setting Slot 2 to a real product banner hosted on the website (such as `assets/banners/bobomusic-banner.png`), confirm the JSON change, then restore Slot 2 to Bobo Universe.
5. Only then issue implementation contracts to the Expo and Godot product rooms.

## Changing promotions later
- Change Slot 2 `image_url`, `target_url`, `type`, and `alt_text` only.
- Commit the manifest change in the website repository, allow GitHub Pages to deploy, and let installed client apps refresh the manifest.
- No app rebuild is necessary if the client already supports the schema and the new art format.
- Use a new asset filename rather than overwriting a cached image to avoid users seeing old creative.

## Notes
`asset_ready: true` means the approved artwork URL is in place. Product integration is still a later step, so no app should treat this manifest as a finished client feature yet.
