# Theme manifest standards proposal

## Description Of Issue
Theme manifests should be documented and standardised. While themes are not part of Cumcord itself,
they are an essential part of most users' client mod experience,
so when theme loader plugins inevitably appear, the manifest (and repo) formats should be standardised and cross-compatible.

## Solution
Standardise and document the manifest format (new-BD-style), what the fields must be, and what each is for.

## Advantages

 - prevent 3rd parties from adding their own fields to manifests for their own use, cluttering repos.
 - act as documentation for theme developers on what to include in their manifest

## Disadvantages
 - locks down possibly helpful info that can be included in manifests

## General Implementation Details
 - only new BD style manifests should be carried forward into CC themes - not `//META`-style manifests

I propose the following TypeScript declaration, not because it has any direct use,
but purely to concretely define the data structure required:
```ts
declare class ThemeManifest {
 // The name of the theme
 name:        string;
 // A more detail description for extra info about the theme
 description: string;
 // Who wrote the theme; not required to be just one person
 author:      string;
 // An SPDX identifier for the license of your theme content - https://spdx.org/licenses/
 license:     string;
 // The absolute URL of an image to go with your theme, screenshot or otherwise
 media?:      URL;
}
```

---

Author: Yellowsink <yellowsink@protonmail.com>