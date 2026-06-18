# GML Cheat Console

Build a full in-game dev console for GameMaker without writing the boilerplate. Type your cheats in plain lines, get one ready script. Paste it once, call three functions, done.

🔗 **[Open the tool »](https://ganjaviruss.github.io/GML-Cheat-Console/)**

A dev console is the little command line you open while testing to flip cheats, set values, spawn items and so on. This tool generates the whole thing from a few lines of input, so you don't hand-write the parser every time.

---

## How it works

You write cheats on the left. The **box** you put a line in decides what kind of command it becomes — no clicking, no dropdowns. Left of the `=` (or `:`) is the command you'll type in game, right of it is what it does. Empty boxes are skipped.

The tool builds one script with four functions:

- `console_create()` — sets up globals and cheat flags
- `console_step()` — reads the keyboard, runs commands, applies toggle effects every frame
- `console_draw()` — draws the input line and the last result
- `Cheats()` — the command parser itself

You paste that script into one script asset, then call the three `console_*` functions from any object.

---

## The five command types

**Toggle** — an on/off switch.
```
god mode = obj_player.hp = obj_player.hp_max
```
Makes a `god mode` command that flips a flag, and the effect runs **every frame while it's on**. Leave the right side empty (`show collisions =`) to get just a flag you check yourself — handy for dev-only drawing.

**Set** — type a value in game.
```
set cash = global.g_cash
```
You type `set cash 500` in game and it writes 500.

**Adjust** — bump a value up or down.
```
ammo 10 = obj_player.ammo 0 99
```
Makes `add ammo` and `minus ammo`, stepping by 10, clamped to 0–99. The two numbers (min/max) are optional.

**Action** — run code once.
```
refill = obj_player.hp = obj_player.hp_max; obj_player.ammo = obj_player.ammo_max
```
Runs when typed. Separate multiple statements with `;`.

**Give** — hand over an item by **ID or name**.
```
item = obj_player.item = _id
```
Makes a `give item` command that accepts both `give item 5` (by ID) and `give item sword` (by name). `_id` is the resolved item ID.

The right side is **just an example** — change it to however your game adds items: an inventory array, a script call, a struct, whatever. Point the **items array** and **count / end** fields (under Settings) at your own data. The name lookup compares against `your_array[_id].name`, so adjust that if your items store names differently.

---

## Setup

1. Make a new **Script** asset (e.g. `scr_console`) and paste the generated script into it.
2. Pick any object that's always in the room — a dedicated `obj_console` or your game controller, your call.
3. Add three events on that object, one call each:
   - **Create** → `console_create();`
   - **Step** → `console_step();`
   - **Draw GUI** → `console_draw();`
4. Run the game, press your open key (default F2), type a command, hit Enter.

The console only works while `DEV_MODE` is `true`. Flip it to `false` for release builds so players can't cheat.

---

## Options

- **Open key** — pick from a list (F-keys, tilde, letters, etc), the tool turns it into the right code
- **Separator** — use `=` or `:`, whichever you prefer
- **DEV_MODE guard** — wraps everything so cheats are dev-only
- **Debug log** — optional `show_debug_message` on each command
- **true/false feedback** — readable on/off text in the console
- **Auto help** — a `help` command listing everything, generated for you
- **Flag prefix** — controls the global flag naming (`global.g_cheat_` by default)
- **Font** — leave empty to use the current font, or name a font asset

Everything you type is saved in your browser, so it's still there when you come back. **Load example** fills in a full working set to start from.

---

## Notes

`obj_player` in the examples is just a placeholder — use your real object and variable names. The generated code is a starting point, not a locked black box. Open it, read it, change what you need.

Lines starting with `//` are ignored, so you can leave comments in the boxes.

---

## License

MIT. Free to use, modify and share.

Part of [GML Tools](https://github.com/GanjaViruss/GML-Tools).

*Built by [GanjaViruss](https://github.com/GanjaViruss) with Claude Opus 4.8 & Sonnet 4.6.*
