# Limited Job Data

Canonical data for FFXIV's limited jobs, for [Blue Academy](https://github.com/RoarkGit/Blue-Academy) and [Strago](https://github.com/RoarkGit/Strago).

Covers Blue Mage spells and Beastmaster beasts. Each job lives in its own top-level directory, so adding another limited job means adding a directory rather than reshaping what is already here.

## Contents

### `spell/spell.yaml`

Structured data for every Blue Mage spell, including:

| Field | Description |
|---|---|
| `id` | Kebab-case identifier |
| `name` | Display name |
| `number` | Spell number in the Blue Mage spellbook |
| `actionType` | `Spell` or `Ability` |
| `spellType` | `Magic` or `Physical` |
| `spellAspect` | Element (Earth, Fire, Ice, Lightning, Water, Wind) |
| `rank` | Rarity rank |
| `range` | Targeting range in yalms |
| `radius` | AoE radius in yalms (if applicable) |
| `cast` | Cast time in seconds |
| `recast` | Recast time in seconds |
| `mp` | MP cost (optional) |
| `hp` | HP cost as a percentage (optional) |
| `face` | `true` if the spell requires facing targets (optional) |
| `location` | Where the spell can be learned |
| `description` | In-game tooltip description |
| `lore` | Lore entry from the in-game Blue Magic Spellbook |
| `target` | Array of valid targets (`enemy`, `self`, `ally`, etc.) |

### `spell/images/`

PNG spell icons for every spell, named by spell `id` (e.g. `water_cannon.png`).

### `beast/beast.yaml`

Structured data for every Beastmaster beast in the Master's Bestiary:

| Field | Description |
|---|---|
| `id` | Kebab-case identifier |
| `name` | Display name |
| `number` | Number in the Master's Bestiary |
| `classification` | Beastkin, Vilekin, Cloudkin, Seedkin, Wavekin, Scalekin, Soulkin, or Ashkin |
| `satiety` | How many feedings the beast can take |
| `habitat` | Where the beast can be captured |
| `lore` | Lore entry from the in-game Master's Bestiary |
| `autoAttack` | Element and range of the beast's auto-attack |
| `trick` | The beast's Trick action (Lv. 8) |
| `temperedRelease` | The beast's Tempered Release action (Lv. 18) |
| `stats` | Strength, Intelligence, Phys. Resistance, Mag. Resistance, and Constitution ratings |

Extracted from the game's own Excel sheets: `XBMPet` joined to `Pet`, `Action`, `ActionTransient`, `PlaceName` and `ContentFinderCondition`, read via [xivapi/ffxiv-datamining](https://github.com/xivapi/ffxiv-datamining), with icons from [xivapi](https://v2.xivapi.com/). Field names and the eight classifications come from the Master's Bestiary UI strings, `Addon` rows 17712 to 17748.

Tooltips have the boilerplate every beast repeats verbatim stripped out: TP gauge cost, instinctual affinity, the combo note, Lingering Vantage, and the One with Nature requirement. The trick's affinity is kept as its own field, since its icon in `action-icons/` encodes it.

`trick` and `temperedRelease` each carry `name`, `element`, `range`, `radius` and `description`, where `description` is the full in-game tooltip including potency. `element` is omitted for actions that deal no damage, and `trick` also carries `affinity` (its Instinctual Affinity). Tooltips have the boilerplate every beast repeats verbatim stripped out: TP gauge cost, instinctual affinity, the combo note, Lingering Vantage, and the One with Nature requirement.

### `beast/images/`

PNG beast icons for every beast, named by beast `id` (e.g. `cu_sith.png`).

### `beast/action-icons/`

The game's own action icons, for uploading as Discord emoji. The four Trick icons are colored paw prints, one per Instinctual Affinity (`rampant.png` red, `durant.png` blue, `eldritch.png` gold, `volant.png` green); every Tempered Release shares `tempered_release.png`. Five files rather than one per action, because the icons are keyed on affinity rather than on the individual action.

### `beast/element-icons/`

The game's inline element and damage-type glyphs, for uploading as Discord emoji: `fire`, `ice`, `wind`, `earth`, `lightning`, `water`, `blunt`, `piercing`, `slashing`, `magic`. Unaspected damage has no glyph of its own; the game shows `magic` for it.

These are not `ui/icon` entries and cannot be fetched from xivapi. They live in the font icon data (`common/font/gfdata.gfd` indexing into `common/font/fonticon_*.tex`), which is why they render inline next to text in the game. Extracted once from a local game install at GFD ids 56-61 (elements, in `Action.Aspect` order) and 185-188 (Blunt, Piercing, Slashing, Magic, matching `XBMElement` rows 7-9). Re-extract from a game install if they ever change.

### `weeklyTargets.yaml`

Weekly rotation data for the Masked Carnivale and duty roulettes, organized by expansion and difficulty tier.

## Usage

```bash
npm install
```

This package is intended to be consumed as a submodule or dependency. Consumer repositories are automatically notified via GitHub Actions dispatch on every push to `main`.

## Consumers

- **[Blue Academy](https://github.com/RoarkGit/Blue-Academy)**: Community knowledge site for FFXIV Blue Mage players, covering guides, spell loadouts, and other resources.
- **[Strago](https://github.com/RoarkGit/Strago)**: Discord bot for the Blue Academy server with commands for spell lookup, character registration, and other Blue Mage utilities.
