# Theme settings format proposal

## Description Of Issue

Theme developers may want to provide an interface or the user to customise their theme.

Common ways of doing this include css variables, telling the user to modify Sass vars and compile, or a specific api.

## Solution

This standardises a new format for providing settings for a theme.

## Advantages

- Robust user friendly solution to theme customisation
- Generalisable outside of cumcord
  - [note: be careful with choice of constants perhaps?]
- Works with existing parsers and processors
- Does not crutch on comments like the previous de-facto format (that was never finished) did - which makes parsing easier, and both mistakes and learning curve lesser.

## Disadvantages

- Won't work with existing implementations
- Other mods are unlikely to adopt this as a scene-wide standard

## Terminology

Let the following annotated code example describe terminology used in this standards proposal:

```css
/* stylesheet */

@xyz abc(def); /* <- at-rule / @rule */

/* this block: rule */
.my-elem {
    /* property */
    xyz: abc;

    /*       /--------/----< fields */
    xyz-cc: abc(def) ghi(jkl); /* setting prop */
}
```

## The format

### Properties

To add a setting for a property, add a corresponding `-cc` suffix version of it ("setting prop") to your rule, for example, to expose a setting for `margin`, provide a `margin-cc` CSS property.

CC suffix versions of props are composed of a list of CSS functions ("fields").

They may be in any order.

All properties should have all of the required fields, one of the type fields, and any mixture of the optional fields.

Fields should not be duplicated.

If the same setting prop is encountered twice in the same rule, then only the first is parsed, the rest are entirely ignored.

#### Required fields

| field  | arguments | description                     | example               |
| ------ | --------- | ------------------------------- | --------------------- |
| `name` | `string`  | The display name of the setting | `name("Big margins")` |

#### Type fields

| field      | arguments             | description                                                                              | example                        |
| ---------- | --------------------- | ---------------------------------------------------------------------------------------- | ------------------------------ |
| `dropdown` | `...any, int`         | A list of items to work as a dropdown choice. Last is (0-based) index of default option. | `dropdown(5rem, 3rem, 6px, 2)` |
| `color`    | `color`               | A colour picker. Enough said. Arg is default.                                            | `color(#fa9)`                  |
| `text`     | `string`              | A textbox. Enough said. Arg is default.                                                  | `text("I love cumcord!!")`     |
| `number`   | `num, num, num, bool` | A number spinner. Args are min, max, default, int-only.                                  | `number(1.5, 5, 3, false)`     |
| `slider`   | `num, num, num, num`  | A slider. Args are min, max, default, step.                                              | `slider(1.5, 5, 3, 0.25)`      |
| `toggle`   | `any, any, bool`      | Switches between two values. Args are off choice, on choice, default.                    | `toggle(none, block, true)`    |

#### Optional fields

The default common here is the value of the field to be used if it is not specified - they are optional and in some cases they must still have *some* value for the logic of the format.

| field      | arguments | description                                                                | default                    | example                        |
| ---------- | --------- | -------------------------------------------------------------------------- | -------------------------- | ------------------------------ |
| `template` | `string`  | Describes how to place the value into the relevant property.               | `"<>"`                     | `template("translateX(<>px)")` |
| `group`    | `string`  | When specified, places the setting into a group of the given display name. | N/A - setting is ungrouped | `group("Color Customisation")` |
| `depend`   | `string`  | If supplied, only editable when a given togglable rule is on.              | N/A - always on            | `depend("bigmargins")`         |

### Rules

Rules may be toggled on and off. This is done via the `cc-toggle` property, and a selector.

Its notable for the implementation that the classes that make this work should be applied to the `<html>` / `:root`.

All togglable rules have a selector beginning EITHER `.CUMCORD_TOGGLE_id` or `:root:not(.CUMCORD_TOGGLE_id)`. The ID you pick groups all rules with it under the same toggle.

One of the rules for each togglable group should have a `cc-toggle` property. If more than one do, only the first encountered one will be used.

The proposed behaviour of `:root:not` is slightly strange - but can be justified.

- The parser / implementation ignores the distinction and treats this as if its just a `.CUMCORD_TOGGLE_id`.

- Obviously, this will make the block *not* apply when the toggle is on.

- This is a good solution to make styles that shouldn't apply when a toggle is enabled, without needing to overwrite all the styles when enabled.

- This may go well with `inverse()`.

The `cc-toggle` property should be made up of the following fields:

| field     | arguments | description                                                                 | optional / default | example         |
| --------- | --------- | --------------------------------------------------------------------------- | ------------------ | --------------- |
| `name`    | See above | See above prop tables                                                       | required           | See above       |
| `group`   | See above | See above                                                                   | optional, N/A      | See above       |
| `default` | `bool`    | If this toggle should default to on - see notes below.                      | optional, `false`  | `default(true)` |
| `inverse` | (none)    | If provided, then the state of the switch displayed to the user is flipped. | optional           | `inverse()`     |

A word on `default()`: default may be useful, but may not be suitable in many cases as loading your theme on an environment without support for the format will *not* apply your desired OOTB case.

`inverse()` seems like a really random inclusion, but read about `:root:not` above and think about a case where you actually want a toggle that disables a lot of things that are otherwise on by default. `html:not` + `inverse` is the best way to do this.

### @rules / at-rules

There is currently one supported @rule in this format: `@import-cc`.

This works just like `@import` except it only applies when enabled by a toggle.

Fields are as follows:

| field     | arguments       | description           | optional / default | example                               |
| --------- | --------------- | --------------------- | ------------------ | ------------------------------------- |
| `name`    | See above       | See above prop tables | required           | See above                             |
| `group`   | See above       | See above             | optional, N/A      | See above                             |
| `default` | See above       | See above rule table  | optional, `false`  | See above                             |
| `url`     | `string \| URL` | The source URL        | required           | `url(https://cumcord.com/styles.css)` |

## Putting it all together- an example

```css
@import-cc name("Alternative Modals") group("Visual Tweaks") url(https://cumcord.com/my-theme/alt-modals.css);

.CUMCORD_TOGGLE_bigmargins :root {
    cc-toggle: name("Enable big margins") group("Visual Tweaks");

    --margin-bigness-cc: name("Margin bigness") slider(1, 10, 5, 1) depend("bigmargins") group("Visual Tweaks");
    --margin-bigness: 5;
}

.CUMCORD_TOGGLE_bigmargins div  { margin: calc(1.2rem * var(--margin-bigness))   }
.CUMCORD_TOGGLE_bigmargins span { margin: calc(1.2rem * var(--margin-bigness)) 0 }


.my-element:after {
    display: inline;
    content: "I love cumcord!!";
    content-cc: name("Sentiment") dropdown("love", "hate", 0) template("I <> cumcord!!");
}
```

This results in a settings UI structured as so:

```
|- Visual Tweaks
|   |- Alternative modals (*-)
|   |- Enable big margins (*-)
|   |- Margin bigness 1---*---10 (uneditable and unapplied if above off)
|
|- Sentiment >love< | hate
```

---

Format Authors: Creatable \<omitted>, Yellowsink <yellowsink@riseup.net>

Proposal Author: Yellowsink <yellowsink@riseup.net>
