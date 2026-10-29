# GAMMA Character Selector

Version `d2026.10.29.2`

A character creator for G.A.M.M.A. On the New Game screen you choose a gender,
a portrait from that gender's faction pool and a generated name, and the
character keeps that look, voice, movement sounds, health-HUD silhouette,
portrait and dialogue wording for that life, across saves and loads; an Azazel
possession adopts the possessed NPC's identity. It offers **Male**, which uses
GAMMA's own files at their stock paths. Further genders come from gender packs,
separate mods that register them with the Selector, and you can define your own
(see Options).

## What it does

- **New Game screen.** Gender replaces the money display beneath the name.
  Select a gender, open the portrait picker, choose one of twelve visible
  portraits per page and press **Apply**; the larger preview keeps portrait
  animation, and **Cancel** or **Escape** keep the committed portrait. Each
  faction and gender remembers its committed portrait while you create a
  character. The small refresh arrow beside the name generates another name for
  the current gender and faction without touching the other choices. Random
  also picks a gender and a matching name. With no saved last choice, the screen
  opens on the first available gender in menu order (Male, unless a pack's
  gender comes first); otherwise it opens on your last choice (see Last
  settings). If Start is refused, a message box gives the reason.
- **Last settings.** New Game remembers every choice you made on it: name,
  faction, gender, portrait, starting location, both difficulties, loadout and
  game modes. Back, Escape and a successful Start save them; a refused Start
  does not. The screen opens with them the next time, also after a restart.
- **Presets.** 24 named preset slots. The **Presets** button lists saved
  presets; **New** saves the current setup under a name, and a saved preset can
  be applied, updated or deleted (updating and deleting ask for a second
  click). Cards show the saved portrait (animated when it is), faction icon,
  name, gender, both difficulties, the starting location and the enabled
  options.
- **Male** uses GAMMA's own bodies, hands, player voice, movement sounds, health
  HUD, text and NPC recordings at their stock paths. Its shared portrait list
  holds neutral **No Data**.
- **Gender packs** can bring their own bodies, hands, voice, sounds, health
  HUD, portraits and text, and choose the NPC recordings used for lines
  addressed to the player (see the Dialogue and sound item below). The
  Selector also ships three static original portraits that packs can use.
- **Custom genders** are data: a pack file can define a gender that reuses any
  registered body, voice, movement, name, dialogue and health profile, with its
  own or inherited portrait pools. No Lua is needed (see Options).
- **During play** the creation portrait and gender stay fixed for that life;
  outfit and disguise changes do not replace the portrait. The engine has one
  process-wide actor configuration, so the Selector writes only the keys it
  owns, records each key's stock value once per process before it first changes
  it, and restores those stock values on every load and every new game without
  a gender.
- **Inventory portraits** keep their real shape. GAMMA's inventory stretches
  every portrait over a box that is wider than the portrait (7 to 9% on a
  16:9 screen). The Selector trims the portrait's top and bottom instead, a
  third of it from the top, so it fills the box unstretched (about 8% of its
  height at 16:9); where the box is narrower than the portrait (4:3, 16:10),
  it trims the sides. It never trims more than a fifth: on ultrawide screens,
  where GAMMA stretches the whole inventory, part of the stretch stays. This
  applies in every game, including one without a chosen gender, to your
  portrait and the other side's (a trader, a companion, a looted body or a
  mechanic). The inventory's layout and artwork stay as they are.
- **Azazel.** Possession adopts the chosen NPC's mapped identity, name,
  portrait and faction while GAMMA's inventory, task and candidate rules stay as
  they are. NPCs without a mapping, and NPCs mapped to a gender that is not
  installed, are not candidates for a managed character. When the body, hands
  or cached actor sounds change, a same-map possession finishes its transfer,
  writes a `gcs_azazel_*` checkpoint save and reconnects to it, so expect one
  loading screen; a cross-map possession uses the normal level change. While a
  possession is still in progress, a further death is refused before Azazel's
  own death handling runs, so it counts no death and starts no second
  possession. A failed or rejected transfer does not block the next death.
- **Dialogue and sound.** Male's text profile uses GAMMA's own English and
  Russian wording. A pack's text is selected per character through namespaced
  text aliases: quest conditions and actions are untouched, and the language
  setting, HUD and dialogs are never reloaded. Close captions, dialogs that
  scripts build, item descriptions, PDA achievements and tasks given during
  play follow the selected profile too. Existing recordings are reused where
  their routing and wording can be established; an NPC line with no suitable
  verified recording is text only. No speech was generated.
- **What it does not change:** it adds no NPC spawns and does not change any
  NPC's gender. Saves made before it was installed stay plain GAMMA saves.

## Requirements

- S.T.A.L.K.E.R. Anomaly 1.5.3.
- G.A.M.M.A., modpack definition 920 (built and checked against it; not tested
  in game yet, see Known issues), with its MCM, DLTX and DXML.
- The Modded Exes build that GAMMA installs. Stock Anomaly executables are not
  supported: the Selector needs the modded engine's DXML, in-memory settings,
  save-load and server-packet interfaces.

## Installation

Install each archive with MO2 and enable it. The order below runs from lower to
higher MO2 priority. Higher priority means lower in MO2's left pane, and when
two mods ship the same file, the one lower in the left pane wins:

1. G.A.M.M.A. and its normal dependencies.
2. **GAMMA Character Selector**. It overwrites one file,
   `gamedata/scripts/gamemode_azazel.script` (Anomaly's Azazel mode, changed
   at four integration points). No GAMMA mod ships that file, so its
   priority among GAMMA's mods does not matter. Give it higher priority than
   any other mod that ships its own `gamemode_azazel.script`, that is, place
   it lower in MO2's left pane than that mod.
3. Optional: gender packs, each as a separate MO2 mod. Follow each pack's own
   instructions for its MO2 priority. A pack whose files all have unique paths,
   such as one you make yourself (see Options), can go anywhere: the Selector
   reads every pack file from every mod, and only a file with the same path as
   another mod's file is hidden, by the one lower in the left pane.

A mod that overwrites GAMMA's own player files (bodies, hands or player
sounds), which Male and every pack use, is not supported. The Selector
recognises the known ones: while one is installed it offers and loads no
character, and the New Game screen names the file it found. Remove such a mod
before you play.

Do not enable a separate New Game Templates mod alongside the Selector: its
presets are built in. Existing New Game Templates presets are read without that
mod and are never changed; saving one stores a copy in the Selector's own
record.

### Updating from 1.0.0-rc15

Replace the old MO2 mod's contents with this archive, keeping the mod's name
and priority, and do not leave an old disabled copy. Also remove the mod that
rc15 required, which overwrote GAMMA's player files (see above).

**Do not copy back rc15's `10_genders.ltx`, `15_name_dialogue_profiles.ltx` or
`20_portrait_pools.ltx`** from `gamedata/configs/plugins/gcs/packs/`, and do not
keep a complete copy of them in another MO2 mod. In rc15 those files also
defined a second gender, which now comes from its own pack. With both copies
installed the Selector treats that gender (or its dialogue profile) as a
duplicate ID and disables both copies (for example "Duplicate gender: <id>"):
the gender is unavailable on the New Game screen, and every save that uses it
is refused. rc15's `10_genders.ltx` also makes Male require that gender's pack,
so without the pack Male is unavailable on the New Game screen and every Male
save is refused. If you had edited those files, re-apply only your own changes
to the new copies, and never re-add the sections rc15 used for that gender.

Saves made with rc15:

- **Male saves** load without any pack. Their stored package list drops rc15's
  old package requirement on load, and a saved body under rc15's retired
  private copies (the actor, and a former character's corpse after Azazel
  possession) is moved back to GAMMA's model at the same path before it loads.
- **Saves of rc15's other gender** keep their package requirement and need
  that gender's current pack, which keeps every ID they store. Without it they
  are refused with an explanation.

## Compatibility and conflicts

- **Mods that overwrite GAMMA's player files.** The known ones are detected and
  refused; remove them (see Installation).
- **Gender packs** register through the Selector's pack interface and require
  the Selector.
- **New Game Templates.** Superseded by the built-in presets; do not enable it
  alongside the Selector. Its stored presets stay readable and untouched.
- **Azazel mode.** The Selector replaces Anomaly's
  `gamedata/scripts/gamemode_azazel.script`. Candidate selection, warfare
  options, task failure, equipment creation, the grace period and the cross-map
  level change stay as in Anomaly, and the function names that GAMMA's magazine
  and perk scripts wrap stay available. Another mod that replaces the same file
  conflicts with it.
- **Inventory.** The portrait trim runs after the inventory's
  `UpdateCharacter` function, which every GAMMA and Anomaly copy of
  `ui_inventory.script` uses to set the portraits. A mod that replaces that
  script keeps the trim as long as it keeps that function.
- **Duplicate IDs.** Two packs that define the same gender or profile ID
  disable both copies; a gender that depends on them becomes unavailable.
- **Other mods' text.** The Selector never edits another mod's translations or
  dialogue files; it adds its own namespaced strings.

## Options

No MCM page. The settings are three editable files in the Selector's
`gamedata/configs/plugins/gcs/packs/` folder; each explains its keys in its
comments. Restart the game completely after any change.

- `gamedata/configs/plugins/gcs/packs/10_genders.ltx`: Male's label, menu
  order, availability and its six profile references (`body`, `voice`,
  `movement`, `health`, `names`, `dialogue`).
- `gamedata/configs/plugins/gcs/packs/15_name_dialogue_profiles.ltx`: name and
  dialogue profiles; the name profiles draw on GAMMA's own name sets.
- `gamedata/configs/plugins/gcs/packs/20_portrait_pools.ltx`: Male's faction
  pools and shared portrait list.
- `gamedata/configs/plugins/gcs/packs/00_builtin.ltx` is the generated catalog;
  do not edit it.

Edit an existing definition only in its own file; a second definition with the
same ID in another file disables both. Updating the Selector replaces these
files, so keep a copy of your edits outside the `packs` folder.

The game removes every space from an LTX value; keys keep theirs. So the file
and folder names a pack refers to, `probe` and `conflicts` paths and
`conflict_signatures` needles must not contain spaces, and a label of more
than one word needs a string ID (see below).

To add a gender, create a uniquely named file in the same folder (or in your
own MO2 mod at `gamedata/configs/plugins/gcs/packs/`), for example
`80_my_gender.ltx`:

```ini
[my_pack_package]
kind = package
id = my_pack
revision = 1
requires = gcs

[my_pack_gender]
kind = gender
id = my_gender
requires = my_pack
label = Custom
order = 30
enabled = true
inherits = male
```

Five inactive, commented examples ship in the folder
`gamedata/configs/plugins/gcs/examples` (an inherited gender, a mixed gender,
a portrait addition, an NPC mapping and a name profile). The Selector never
loads that folder: copy one into the `packs` folder under a unique name and
remove the `.example` suffix to use it.

`inherits = male` reuses Male's six profiles and portrait pools; `inherits`
can name any other registered gender instead, whose package then belongs in
your package's `requires`. Any of the six profile keys can then name another
registered profile. Without `inherits`, a gender must name all six profiles
and a `label`, and needs at least one portrait for each of the 13 factions:
from its own `shared` list, which covers every faction, or from pool sections
(a pool with `faction = shared` also covers every faction). A
one-word `label` is shown as written; for a longer label use a string ID you
define in your own `text\eng` and `text\rus` XML files (Windows-1251). Keep
`id` values unchanged once a save uses them; `enabled = false` hides a gender
from new characters while its saves still load.

## Adding or removing mid-save

- **Adding the Selector to an existing game:** older saves load unchanged as
  plain GAMMA characters, with no gender. Start a new game to create a managed
  character.
- **Adding a gender pack:** its genders appear on the New Game screen;
  existing characters keep the gender they were created with.
- **Removing a gender pack:** saves that use its gender are refused, with an
  explanation, from the Load menu, Continue and quickload until it is
  restored. Male saves need no pack. If your last New Game choice used its
  gender, the New Game screen shows it as "Unavailable: <id>" until you pick
  another.
- **Removing the Selector:** turning characters created with it into plain
  GAMMA characters is not supported; keep the Selector (and their packs)
  installed for those saves. Saves made without it are unaffected.

## Uninstall

1. Exit Anomaly.
2. Disable or remove the mod in MO2.

Left behind: your presets and last creation choices in the
`[gcs_character_templates_v1]` section of `axr_options.ltx` (harmless without
the Selector), and any `gcs_azazel_*` checkpoint saves, which you can delete
from the Load menu. Continuing a character created with the Selector without
it is not supported.

## Known issues

- Apart from its build number, this build changes only its README and
  meta.ini; nothing in this build has been tested in game yet. In the previous
  build, your own inventory portrait was checked in one save at 1920x1080. The
  build before it, which in game lacks only the inventory portrait fix, was
  played for a short while in a new game with a gender from a gender pack.
  Male, which the Selector offers on its own, has not been played in game
  since it moved to GAMMA's own files. Everything else, including the other
  side's inventory portrait and other screen shapes, is covered by component
  tests only. Report problems with the newest `xray_*.log` from Anomaly's
  `appdata\logs` folder.
- The console commands `load`, `load_last_save` and `start server(...)` bypass
  the Load menu's checks, so a save the Selector would refuse is loaded anyway
  (a body model that is no longer installed is replaced by GAMMA's no-outfit
  body). Load saves from the Load menu, Continue or quickload instead.
- A checkpoint failure after an Azazel possession performs no second
  possession; a message in the game's language says what failed, and the
  `xray_*.log` names the reason on a `[GCS Azazel]` line.
  Load an earlier save to recover.
- NPC lines addressed to the player that have no recording matching the
  character's gender are text only: the NPC stays silent for them.

## Changelog

### d2026.10.29.2

- The README describes the Selector on its own. No change in the game.

### d2026.10.29.1

- Portraits in the inventory are no longer stretched sideways. GAMMA's
  inventory draws every portrait into a box 7 to 9% wider than the portrait on
  a 16:9 screen; the Selector now trims the top and bottom of your portrait
  and the other side's so it fills the box at its real shape, and trims at
  most a fifth on other screen shapes. The inventory's layout and artwork are
  unchanged.

### d2026.10.28.1

- When a registered gender is missing from the New Game screen, the
  `xray_*.log` now says why on a `Gender unavailable` line.
- Replaced copies of two of GAMMA's sound scripts, `footstep_sounds.script` and
  `eft_jump_sounds.script`, installed by a mod that overwrites GAMMA's player
  files, are now recognised in game; GAMMA's own copies are not affected. The
  check looked for text with a space in it, which the game removes from config
  values.
- Options now state that the game removes every space from a pack's values.

### d2026.10.27.1

- Documentation only; the Selector's behaviour is unchanged.

### d2026.10.26.1

- First dated build; it follows 1.0.0-rc15.
- A Male character created on the New Game screen keeps GAMMA's own no-outfit
  body, the same one GAMMA alone and an unmanaged game show.
- A save that is refused at load (for example after a console `load`) no
  longer plays a pack's footsteps, voices or NPC addressing on a stock body:
  the world behaves like an unmanaged game until a valid life begins.
- Works on plain GAMMA: Male uses GAMMA's own files at their stock paths, and
  rc15's private copies of GAMMA's male files and NPC recordings are gone.
- Genders other than Male now come from separate packs. Known mods that
  overwrite GAMMA's player files are detected and refused.
- rc15 saves migrate on load (see Updating from 1.0.0-rc15).
- The first-person bare hands follow the chosen gender.
- A game with a pack's gender followed by a level change or quickload no longer
  leaves that gender's legs, hands or sounds in a later Male or unmanaged game.
- Every player sound a pack changes follows the gender; a game with no gender
  keeps GAMMA's player voice.
- Close captions, script-built dialogs, item descriptions, PDA achievements and
  tasks given during play follow the selected text profile.
- Azazel: a death during a possession is refused cleanly, and a failed or
  rejected transfer no longer blocks the next death.
- A refused save whose body model is no longer installed gets GAMMA's
  no-outfit body instead of crashing.

### 1.0.0-rc15

- Previous release candidate; it needed a separate mod that overwrote GAMMA's
  player files, for every gender.

## Credits

- GSC Game World; the Anomaly team; G.A.M.M.A. by Grokitach and contributors.
- DXML, DLTX and MCM authors; the Modded Exes contributors.
- New Game Templates authors: the built-in presets read its preset format.
- Assets this archive ships: the three static original portraits are the
  Selector's own artwork. Its NPC audio banks (`gcs_female` and `text_only`)
  are 8,458 unchanged copies of existing recordings: 6,434 by the Ladies and
  Sister Stalkers 2.1 team (Paint_Thinner, KILIMANAJARO, Alex_Marheaves,
  Blackbomb, MiK_, Isthar), 1,805 from Anomaly and 219 from GAMMA. It ships no
  models, textures or player sounds from other mods.
- Existing art, models, animations, translations and recordings remain the work
  of their original authors.
- GAMMA Character Selector design, code and build: Q4ln.

## Licence

The Selector's own code and artwork are by Q4ln under CC BY-NC-SA 4.0; see
`GAMMA Character Selector - LICENSE.txt`. Third-party recordings and other
assets it uses or ships remain under their authors' terms.
