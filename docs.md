---
title: House Builder documentation
---

# House Builder

Documentation for version 1.0.2, the README that ships with the script. [Buy on Tebex](https://house-builder.tebex.store/package/7734521) · [Support on Discord](https://discord.gg/hTRXSmp238)

A Sims-style house builder for FiveM. Players build their own houses from a top-down camera on a 2 m grid: walls,
doors, windows, garages, floors, stairs, roofs, decks, fences, paint, kitchens, bathrooms, furniture and hundreds of
props. Houses are drawn for everyone, doors open, close and lock, and furniture can be sat and lain on.

It runs on its own: no framework, no database, no other resources. With QBCore, Qbox, ESX or ox_core running, plots
belong to characters instead of players.

**Requirements:**

- **FiveM (Legacy).** FiveM for GTAV Enhanced is not supported yet: Cfx.re's asset protection (escrow) does not work
  there yet, so the protected scripts cannot load on it. Support follows in an update once it does (the kit already
  carries the Enhanced pieces in `stream_enhanced/`; FiveM Legacy ignores that folder).
- **OneSync**, which nearly every server runs. It is set when the server starts (`+set onesync on`, or txAdmin's
  settings).
- **A recent game build**, for example `sv_enforceGameBuild 3889`: the Furnish props include pieces from recent game
  updates, and a game build without them refuses those pieces ("That piece is not in your game").

## Install

1. Put the `[housebuilder]` folder (it holds `housebuilder` and `housebuilder_kit`) in your `resources` folder.
2. In `server.cfg`, start the kit first:

   ```
   ensure housebuilder_kit
   ensure housebuilder
   ```

3. Give the houses' props room next to your other scripts. Put this line near the top of `server.cfg`:

   ```
   increase_pool_size "Object" 1000
   ```

   It is read when the server starts; each player's game restarts itself once on its first join after it changes.

4. Give your admins the builder's admin rights (`config.lua` explains the other ways):

   ```
   add_ace group.admin housebuilder.admin allow
   ```

5. Restart the server.

**Restarting:** start or restart these two resources only on an empty server, or as part of a full restart. Players
who were online when the kit started are asked to rejoin before they see houses.

## Getting started

As an admin:

1. Walk to an empty lot and type `/plot mark`. Click grid points to put down corner markers, close the shape and
   **Save plot**. The tool lines the grid up with the road (**Match road**) and sets the floor height for you.
2. Players stand on the plot and type `/plot claim` (or you give it: `/plot give <plot id> <player id>`).
3. The owner stands on the plot and types `/house` to build.

For a build server with no admin set-up, set `Config.Plots.playersCreate = true`: players type `/house new` to make a
24 x 24 m plot where they stand.

## Commands

| Command | Who | What it does |
| --- | --- | --- |
| `/house` | everyone | Opens the builder on your plot (stand on it or near it). Also on a key, see below. |
| `/house share <player id>` | plot owner | Gives a player the keys: they build on your plot and lock its doors. |
| `/house unshare <player id>` | plot owner | Takes the keys back. Without a player id it lists who has them. |
| `/house new` | admins (players with `playersCreate`) | Makes a new 24 x 24 m plot where you stand. |
| `/house <plot id>` | admins | Opens any plot. |
| `/house delete` | admins | Removes the nearest plot (not a land plot's: use `/plot`). |
| `/plot` | everyone | Says which plot you stand on and whose it is. |
| `/plot claim` | everyone | Claims the free plot you stand on (`Config.Plots.claim`). |
| `/plot mark` | admins | Opens the plot marking tool. |
| `/plot edit [id]` | admins | Edits a plot's outline, grid and floor height. No id: pick one from a list. |
| `/plot show` | admins | Shows every plot's outline and name nearby. |
| `/plot list` | admins | Lists the plots and their owners in chat. |
| `/plot tp <id>` | admins | Takes you to a plot. |
| `/plot give <id> <player id>` | admins | Gives a plot to a player. |
| `/plot take <id>` | admins | Takes a plot back. What is built stays. |
| `/plot reset <id>` | admins | Empties the house on a plot. |
| `/plot delete <id>` | admins | Removes a plot and the house on it. |
| `/plot decor <id> <n\|default>` | admins | How many furniture and decor props each house on that plot may hold. |

Command names can be changed in `config.lua`.

**The open key.** Players can bind a key to open the builder in **Settings > Key Bindings > FiveM** ("House builder:
open on your plot"). There is no default key, because emote menus and other scripts often use the F keys (dpemotes
and older rpemotes open on F3) and FiveM runs every script bound to a key. To give everyone a default, set
`Config.OpenKey` (for example `'F3'` if nothing else on your server uses it). The key only opens a plot you are at and
says nothing anywhere else, so sharing it with another menu does no harm beyond opening the builder at home.

## In the builder

The bottom panel has two modes: **Build** (walls, doors, windows, floors, stairs, steps, roof, outdoor, blueprints) and
**Furnish** (kitchens, bathrooms, living rooms and props for every room). The rail on the left has Paint, Delete and
Copy (picks up a piece's style). Ctrl+Z / Ctrl+Y undo and redo. The key help at the bottom of the screen lists the
rest: WASD moves, Q / E turn the camera, the wheel zooms, C changes the walls view, H hides the roofs.

Blueprints save a selection of the house. Players keep their own, can share them with everyone, and place the
built-in ready-made houses. Admins can save new ready-made ones.

## Settings (`config.lua`)

| Setting | Default | |
| --- | --- | --- |
| `Config.Commands` | `house`, `plot` | The two command names. |
| `Config.OpenKey` | `''` | A default key that opens the builder (`''`: none; players bind their own). |
| `Config.AdminAce` | `'housebuilder.admin'` | The ACE that makes a player an admin of the builder. |
| `Config.AdminFallbacks` | `true` | Also counts the `command` ACE and the framework's own admins. |
| `Config.Identity` | `'auto'` | `'auto'`: a framework's character when one runs, else the Rockstar licence. `'license'`: always the licence. |
| `Config.Plots.claim` | `true` | Players may claim free plots. |
| `Config.Plots.maxPerPlayer` | `1` | Plots one player may own. |
| `Config.Plots.playersCreate` | `false` | Players may make plots anywhere with `/house new`. |
| `Config.Plots.openRange` | `150.0` | How far from their plot a player may be to open it. |
| `Config.DrawBudget` | `560` | Building pieces each game draws at once, nearest first. |
| `Config.DecorLimit` | `100` | Props one house may hold (per plot: `/plot decor`). |
| `Config.Notify` | chat | Where messages go. Replace the function to use your own notifications. |

## Saved data

Everything is saved in `housebuilder/data/`:

- `plots.json`: every house.
- `landplots.json`: the plots admins marked.
- `blueprints.json`: players' blueprints and the admins' ready-made ones.

Back these up with your server. Keep the files when you update the resource.

## For developers

Server exports:

```lua
exports.housebuilder:GetLandPlots()            -- { { id, name, status = 'free'|'owned', ownerName, squares, x, y, z }, ... }
exports.housebuilder:GiveLandPlot(landId, src) -- gives a plot to an online player; true, or false and why
exports.housebuilder:FreeLandPlot(landId, wipe) -- frees a plot; wipe = true also empties its house
```

These are handy for a real-estate script: sell the plot your way, then call `GiveLandPlot`.

Client event, for hiding your own HUD while the builder or the plot tool is open:

```lua
AddEventHandler('housebuilder:client:builderOpen', function(open) --[[ hide your HUD when open is true ]] end)
```

Client exports for scripts that walk NPCs through houses: `IsDoorLeaf(entity)`, `PlotFrame(plotId)`,
`OpenDoorsNear(x, y, z, radius, ms)`.

## Performance

Each house is drawn from separate pieces. Every game keeps at most `Config.DrawBudget` pieces at once (all houses
together, the nearest first), and only the outside of houses past 60 m. Furniture and props are drawn within 75 m.
