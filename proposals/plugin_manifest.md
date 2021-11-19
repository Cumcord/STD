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