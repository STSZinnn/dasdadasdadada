# Sovereign SMP v1.7.1 — Witherborn + Tidelord

Paper **1.21.11**, Java **21**, Gradle project. This update retains the existing Sovereign Mace (special weapon) and Echo Scythe (Artifact I / VII). **Witherborn (II / VII)** and **Tidelord (III / VII)** are independent, unique artifacts. Only three of seven slots are filled.

## Install / upgrade

1. **Back up** `plugins/SovereignSMP/artifacts.yml` and `config.yml` on your server. Keep the same `artifacts.yml` to preserve previous Mace and Scythe UUIDs.
2. Upload source to GitHub and run the included `.github/workflows/build.yml` (Actions → Build Sovereign SMP Plugin → Run workflow), or use Gradle with Java 21 to run `gradle clean build`.
3. Remove the old SovereignSMP plugin JAR; install the new build and restart Paper 1.21.11.
4. New `wither:` and `tide:` config entries merge automatically into existing `config.yml` via Bukkit defaults.
5. Operators can create each item exactly once using `/sovereign mint witherborn <player>` and `/sovereign mint tidelord <player>`.
6. Install **`SovereignSMP_Complete_ResourcePack_v1.7.1.zip`** in Minecraft's Resource Packs screen. This combines the supplied v1.6 pack's original Mace/Scythe sprites, tooltip styles and particle models with the new Witherborn and Tidelord tooltips.
7. No `/sovereign tooltip` command is needed or available. The plugin *always* writes `setTooltipStyle` on all four unique weapons. RGB-gradient lore remains visible without the optional client resource pack.

## Witherborn (Netherite Axe) — Artifact II

- Passive **Withering Touch**: every third successful melee hit inflicts Wither I for 3 seconds, and damage to withered enemies builds up to 5 Soul Energy.
- **Right-click / Wither Barrage** (22s): fires 3 miniature Wither skulls in succession. Impacts deal 3 HP in a 2.3-block radius and inflict Wither. Skulls have 0 explosion yield; an additional tagged explosion event guard prevents grief.
- **Shift+Right-click / Soul Devour** (32s): damages nearby already-withered foes. Gains bounded Absorption when it drains at least one enemy; Soul Energy enhances damage and absorption.
- **F / Wither Ascension** (60s): shows 3 orbiting Wither skull visuals for 7 seconds and periodically targets nearby enemies with real projectiles, then a 5-block soul-fire burst.
- Soul-blue ↔ charcoal gradient lore and always-assigned `sovereignsmp:wither` background.

## Tidelord (Trident) — Artifact III

- Passive **Guardian's Blessing**: Water Breathing while held, Dolphin's Grace while swimming, and an 18% chance to reflect up to 2 HP against melee attackers (2-second reflection lockout).
- **Right-click / Guardian Beam** (24s): 2-second cyan laser channel. Aiming at the same visible target continuously empowers each 10-tick hit.
- **Shift+Right-click / Prismarine Spikes** (30s): 10-block sequential line of temporary prismarine BlockDisplays; 4 HP and upward knockback on first contact. Never edits real world blocks.
- **F / Elder's Judgment** (60s): summons a stationary temporary Elder Guardian with AI disabled, for 5 seconds; repeatedly lasers up to 3 nearby foes for 3 HP and Mining Fatigue.
- Prismarine cyan ↔ ocean-gold gradient lore and always-assigned `sovereignsmp:guardian` background.

## Other fixes and compatibility

- Fixes static initializer crash caused by 13 config pages but only 12 slots in the original v1.6.2 source (now allows all 20 settings sections).
- GUI includes separate Witherborn and Tidelord pages for passive/three ability settings, and HUD/action bar supports their cooldowns.
- Original `mace.*`, `echo_scythe.*` token keys, abilities, tooltip style IDs `sovereignsmp:royal` / `sovereignsmp:sculk` remain unchanged.
- Pickup claiming for Mace and Scythe now verifies the artifact actually reached the player's inventory the next tick instead of locking ownership during a possibly unsuccessful pickup event.
- Owner death makes each held artifact claimable again; works with `keepInventory`. Admin `/sovereign recover ...` invalidates previous copies by rotating the artifact's UUID. Only use recover if the artifact was genuinely lost.
- Disable/restart removes ephemeral new display entities and ability skulls when possible. All new ability effects are server-side; custom 16x16 sprite pack support from v1.6.2 remains optional.

## Commands

- `/sovereign status` — lists three artifacts and Mace owner.
- `/sovereign claim <mace|scythe|witherborn|tidelord>` — claim an unowned authentic item in your inventory.
- `/sovereign cooldowns` — show all current ability cooldowns.
- `/sovereign restyle` — refresh authentic gradient tooltips in your inventory.
- `/sovereign config` — operator-only configuration GUI.
- All legendary tooltip styles are permanently assigned by the plugin; players need the included client pack for the backgrounds.
- `/sovereign mint <mace|scythe|witherborn|tidelord> <player>` — OP only, one-time creation.
- `/sovereign recover <mace|scythe|witherborn|tidelord> <player>` — OP only, emergency recovery.

**Testing status:** Source updates and asset merge are complete. Full Paper/Gradle build and a live-server run were not available in this environment (Paper dependencies unavailable). Compile via included GitHub workflow before deploying. Check server console and test ability hitboxes, inventory transfer, and resource pack layering on a test server.

## GitHub Actions hidden `.github` folder workaround

The exact workflow path is `.github/workflows/build.yml` (a directory named `.github`, inside that a directory named `workflows`, then a file named `build.yml`). From GitHub repository web page, choose **Add file → Create new file**, enter `.github/workflows/build.yml` in the filename field and paste the YAML from the ZIP. **Commit changes.** Using *Upload files* with a hidden dot-folder on Windows can be troublesome; creating the file on GitHub avoids it. Once committed, open **Actions** and run the workflow; download the compiled artifact.
