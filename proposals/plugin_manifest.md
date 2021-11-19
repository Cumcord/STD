# Plugin manifest standards proposal

## Description Of Issue
Plugin manifests should be documented and standardised to prevent addition of unnecessary items by potentially multiple 3rd parties.

## Solution
Standardise and document that the manifest must be a json, what the fields must be, and what each is for.

## Advantages

 - prevent 3rd parties from adding their own fields to manifests for their own use, cluttering repos.
 - act as documentation for plugin developers on what to include in their manifest

## Disadvantages

 - locks down possibly helpful info that can be included in manifests
 - will likely be very verbose

## General Implementation Details
 - Include the `media` field - though it is not required for cc, since the Discord bot already uses it and I have seen it used in the wild,
   I think it's sensible to standardise.
 - Require the `license` field to be a valid [SPDX identifer](https://en.wikipedia.org/wiki/Software_Package_Data_Exchange#License_syntax)

I propose the following schema:
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "N/A",
  "title": "Cumcord Plugin Manifest",
  "description": "The manifest format for Cumcord plugins",
  "type": "object",
  "properties": {
    "name": {
      "description": "The name of the plugin",
      "type": "string"
    },
    "description": {
      "description": "A more detail description for extra info about what the plugin does",
      "type": "string"
    },
    "author": {
      "description": "Who wrote the plugin; not required to be just one person",
      "type": "string"
    },
    "file": {
      "description": "The relative location on disk of the index JS file",
      "type": "string"
    },
    "license": {
      "description": "An SPDX identifier for the license of your plugin content",
      "type": "string"
    },
    "media": {
      "description": "The absolute URL of an image to display with your plugin",
      "type": "string"
    }
  },
  "required": [ "name", "description", "author", "file" ]
}
```

The following are two examples of acceptable manifests:
```json
{
  "name": "My awesome plugin",
  "description": "A plugin that does a thing to your Discord, thus being awesome",
  "author": "generic name",
  "file": "index.js",
  "license": "Unlicense"
}
```
```json
{
  "name": "My awesome plugin",
  "description": "A plugin that has a cool screenshot",
  "author": "generic name",
  "file": "index.js",
  "media": "https://cumcord.com/assets/screenshots/lmao.png"
}
```

---

Author: Yellowsink <yellowsink@protonmail.com>
