# Theme manifest standards proposal

## Description Of Issue
Theme manifests should be documented and standardised. While themes are not part of Cumcord itself,
they are an essential part of most users' client mod experience,
so when theme loader plugins inevitably appear, the manifest (and repo) formats should be standardised and cross-compatible.

## Solution
Standardise and document the manifest format, what the fields must be, and what each is for.

## Advantages

 - prevent 3rd parties from adding their own fields to manifests for their own use, cluttering repos.
 - act as documentation for theme developers on what to include in their manifest

## Disadvantages
 - locks down possibly helpful info that can be included in manifests

## Manifest Format & Schema

**!! NOTE !! The media field is NOT for branding images.**

I propose the following schema:
```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "N/A",
  "title": "Cumcord Theme Manifest",
  "description": "The manifest format for Cumcord themes",
  "type": "object",
  "properties": {
    "name": {
      "description": "The name of the theme",
      "type": "string"
    },
    "description": {
      "description": "A more detail description for extra info about the theme",
      "type": "string"
    },
    "author": {
      "description": "Who wrote the plugin; not required to be just one person",
      "type": "string"
    },
    "license": {
      "description": "An SPDX identifier for the license of your plugin content",
      "type": "string"
    },
    "media": {
      "description": "The absolute URL of some images to demonstrate your theme. No branding.",
      "type": [ "array", "string" ],
      "items": {
        "type": "string"
      }
    }
  },
  "required": [ "name", "description", "author" ]
}
```

The following is an acceptable manifest format, but anything that matches the manifest is allowed.
For example the media field may be just a single string, and some fields may be left out.
```json
{
  "name": "My awesome theme",
  "description": "A cool theme to demonstrate manifests",
  "author": "generic name",
  "license": "BSD-3",
  "media": [
    "https://cumcord.com/assets/screenshots/codcum.png",
    "https://cumcord.com/assets/screenshots/lmao.png"
  ]
}

```

## Import URL

The import URL of a theme should be the URL to the theme css file.
The manifest should be available on the same route as this file as `cumcord_manifest.json`.

In the following example, the import url is `example.com/my-cool-theme/epic_theme.css`,
and the manifest must be available on `example.com/my-cool-theme/cumcord_manifest.json`.
```
|- example.com
  |- example.com/my-cool-theme
    |- example.com/my-cool-theme/epic_theme.css
    |- example.com/my-cool-theme/cumcord_manifest.json
```

The import URL should always be absolute.
This may be handled differently when referenced in theme repos (see separate spec),
but standalone import URLs must always be absolute and to the theme file.

---

Author: Yellowsink <yellowsink@protonmail.com>
