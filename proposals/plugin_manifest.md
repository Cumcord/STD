# Plugin manifest standards proposal

Cumcord plugins are required to have a manifest containing essential information and metadata about the plugin.
This includes the plugin name, author, and a short description.

The manifest is required to be a plain json object adhering to the following structure:
```json
{
  "name": string,
  "description": string,
  "author": string,
  "file": string file path,
  "license": string,
  "media": optional string absolute URL
}
```

## Name
The name field must contain the name of the plugin to identify it, and to show in user interfaces.

## Description
The description field must exist, and contain a short description of what the plugin does, and any additional info the plugin developer wishes to specify.
It may be an empty string if the developer considers the plugin purpose simple enough, and the name self-explanatory enough.

## Author
The author field must contain the author of the plugin. Multiple authors must be specified, and while no format is required,
the recommended format for this is to comma-separate the authors, using "and" for the last two, with no comma before the and.

Discord tags may be optionally used, but there is no requirement nor special handling for this.

## File
The file field is non-negotiably required to point relatively to the index Javascript file on disk.
This is so that the development tool (sperm) can bundle the plugin.

This file must meet the following criteria:
 - either a Javascript (\*.js) or React JSX (\*.jsx) file
 - default export a function to be called on load - see the plugin code format specification

## License
The license field must contain an [SPDX identifer](https://en.wikipedia.org/wiki/Software_Package_Data_Exchange#License_syntax)
for the licence under which your plugin's content falls.

No limitations are placed upon accepatble licenses - even proprietary licenses are accepatble, however we do recommend [OSI-approved licenses](https://opensource.org/licenses).

## Media
The media field need not exist, but if it does exist, it should specify a banner image to use to represent your plugin.
This may be a descriptive screenshot, or a more marketing-style banner, or really anything you want.