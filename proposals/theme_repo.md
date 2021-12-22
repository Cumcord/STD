# Theme repo standard proposal

## Description Of Issue

Theme repositories are not standardised, outside of any de facto standards from theme loaders

## Solution

Standardise how repos specify the theme list, and how they expose their own metadata

## Advantages

- repos can provide metadata such as their name, maintainer, etc
- all repos work the same way

## Disadvantages

- other theme loaders may wish to add additional features, which this would lock down

## General Implementation Details

### Manifest

I propose the following JSON Schema:

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "N/A",
  "title": "Cumcord Theme Repo Manifest",
  "description": "The manifest format for Cumcord theme repos",
  "type": "object",
  "properties": {
    "meta": {
      "description": "Repo metadata",
      "type": "object",
      "properties": {
        "name": {
          "description": "The name of the repo",
          "type": "string"
        },
        "description": {
          "description": "A more detailed description of the repo",
          "type": "string"
        },
        "maintainer": {
          "description": "Who maintains the repo",
          "type": "string"
        }
      },
      "required": [
        "name",
        "description",
        "maintainer"
      ]
    },
    "themes": {
      "description": "The list of themes in the repo, either relative or absolute URLs",
      "type": "array",
      "items": {
        "type": "string"
      }
    }
  },
  "required": [
    "meta",
    "themes"
  ]
}
```

which validates correctly against the following example repo, which demonstrates metadata, relative urls, and absolute themes:

```json
{
  "meta": {
    "name": "My awesome repo",
    "maintainer": "generic author",
    "description": "A place for awesome themes to go"
  },
  "themes": [
    "https://raw.githubusercontent.com/generic_author/generic_theme/master/generic_theme.theme.css",
    "test_1/test_theme.css",
  ]
}
```

### URL handling behaviour

The import URL of theme repos should be the parent of the manifest, and as such the manifest should be available at the `/repo.json` route RELATIVE to the import URL.

For example: with the import url `https://example.com/discord-stuff/themes`, there must be a manifest at `https://example.com/discord-stuff/themes/repo.json`

URLs in the manifest can be either absolute or relative. If absolute, then simply fetch the theme. If relative, then attach relative to the import URL.

For example: with the import url `https://example.com/` and theme url `test/theme.css`, then the theme will reside at `https://example.com/test/theme.css`.

Theme importing and manifest handling should be handled as of [the theme manifest standard proposal](https://github.com/Cumcord/STD/blob/master/proposals/theme_manifest.md), so import url of themes is the css file, and the theme manifest is at `<import URL>/../cumcord_manifest.json`.