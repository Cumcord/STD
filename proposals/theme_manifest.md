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

## General Implementation Details
 - use plugin manifest -like json manifests

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
      "description": "The absolute URL of an image to display with your plugin",
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
  "description": "A cool theme to demonstrate manifests"
  "author": "generic name",
  "license": "BSD-3",
  "media": [
    "https://cumcord.com/assets/screenshots/codcum.png",
    "https://cumcord.com/assets/screenshots/lmao.png"
  ]
}

```

---

Author: Yellowsink <yellowsink@protonmail.com>
