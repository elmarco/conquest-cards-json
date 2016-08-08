Conquest cards JSON data
=========

The goal of this repository is to store Conquest LCG card data in a format that can be easily updated by multiple people and their changes reviewed.

## Validating and formatting JSON

Using python >=2.6, type in command line:

```
./validate.py --verbose --fix_formatting
```

The above script requires python package `jsonschema` which can be installed using `pip` via `pip install -U jsonschema`.

You can also just try to follow the style existing files use when editing entries. They are all formatted and checked using the script above.

## Description of properties in schemas

Required properties are in **bold**.

#### Cycle schema

* **code** - identifier of the cycle. All lowercase and using dashes instead of spaces. Examples: `"core"`, `"warlord"`, `"death-world"`.
* **name** - properly formatted name of the cycle. Examples: `"Core Set"`, `"Warlord"`, `"Death World"`..
* **position** - number of the cycle, counting in chronological order. For packs released outside of normal constructed play cycles (such as draft packs), the special cycle with position `0` should be used. Examples: `1` for Core Set, `6` for Death World Cycle.
* **size** - number of packs in the cycle. Examples: `1` for big boxes, `6` for regular datapack cycles.

#### Pack schema

* **code** - identifier of the pack. All lowercase and using dashes instead of spaces. Examples: `"core"` for Core Set, `"ts"` for The Scourge.
* **cycle_code** - identifier of the cycle the pack belongs to. Must refer to one of the values from cycles' `"code"`. Examples: `"core"` for Core Set, `"warlord"` for Warlord Cycle.
* **name** - properly formatted name of the pack. Examples: `"Core Set"`, `"The Scourge"`.
* **position** - number of the pack within the cycle. Examples: `1` for Core Set, `2` for The Scourge
* **date_release** - date when the pack was officially released by FFG. When in doubt, look at the date of the pack release news on FFG's news page. Format of the date is YYYY-MM-DD. May be `null` - this value is used when the date is unknown. Examples: `"2014-10-03"` for Core Set, `null` for unreleased previewed packs.
* **size** - number of different cards in the pack. May be `null` - this value is used when the pack is just an organizational entity, not a physical pack.  Examples: `188` for Core Set, `20` for most datapacks, `null` for assorted draft cards.

#### Card schema

* **code** - 5 digit card identifier. Consists of two zero-padded numbers: first two digits are the cycle position, last three are position of the card within the cycle (printed on the card). Examples: `"01048"` for Suppressive Fire (48th card in cycle) from Core Set (1st cycle),
* cost - Play cost of the card. Relevant for all cards except warlords and planets.
* **pack_code** - Code of the pack this card is in.
* **faction_code** - Faction this cards belongs to. Possible values: `"space"`, `"astra"`, `"neutral"`, `"eldar"`, `"orks"`, `"chaos"`, `"tau"`, `"tyranid"`, `"necrons"`,
* flavor - Flavor text of the card. May be empty.
* illustrator - Illustrator's name.
* keywords - array of `"ambush"`, `"area-effect"`, `"armorbane"`, `"bloodied"`, `"brutal"`, `"flying"`, `"immune"`, `"limited"`, `"mobile"`, `"no-attachments"`, `"ranged"`
* position - Card number within that pack.
* **quantity** - How many copies of this card are in the pack.
* text - Text of the card.
* **title** - Name of the card.
* **type_code** - Type of the card. Possible values: `"planet"`, `"warlord"`, `"attachment"`, `"army"`, `"support"`, `"event"`
* unique - True if the card is unique (black diamond printed next to the title), false otherwise. Possible values: `true`, `false`.
* area_effect - value of Area Effect, if any
* atk - ATK value
* bloodied_atk - ATK value when bloodied
* bloodied_hp - HP value when bloodied
* card_bonus - card bonus (generally for planets)
* cmd - Number of Command symbols
* hp - Hit-Points
* loyal - whether the card is loyal
* signed - whether the card is signed
* planet_symbols - The list of symbols on the planet, possible values: `"material"`, `"tech"`, `"strongpoint"`
* resource_bonus - resource bonus (generally for planets)
* shields - shields value
* starting_cards - number of starting cards, for warlords.
* starting_resources - number of starting resources, for warlords.
* traits - array of traits (we follow card naming and casing here for now)

#### Card Translation schema

* **code**
* flavor
* text
* **title**


## JSON text editing tips

Full description of (very simple) JSON format can be found [here](http://www.json.org/), below there are a few tips most relevant to editing this repository.

#### Non-ASCII symbols

When symbols outside the regular [ASCII range](https://en.wikipedia.org/wiki/ASCII#ASCII_printable_code_chart) are needed, UTF-8 symbols come in play. These can be escaped using `\u<4 letter hexcode>`, such as `\u0101` (ā from *Pālanā Foods*), but there is no obligation. UTF-8 characters can be present in the values.

To get the 4-letter hexcode of a UTF-8 symbol (or look up what a particular hexcode represents), you can use a UTF-8 converter, such as [this online tool](http://www.ltg.ed.ac.uk/~richard/utf-8.cgi).

#### Quotes and breaking text into multiple lines

To have text spanning multiple lines, use `\n` to separate them. To have quotes as part of the text, use `\"`.  For example, `"flavor": "\"I'm overpowered.\"\n-Whizzard"` results in the following flavor text:

> *"I'm overpowered."*  
> *-Whizzard*

#### Conquest symbols

These can be used in a card's `text` section.

* `[ASTRA MILITARUM]`
* `[CHAOS]`
* `[DARK ELDAR]`
* `[ELDAR]`
* `[ORK]`
* `[RESOURCE]`
* `[SPACE MARINE]`
* `[TAU]`
