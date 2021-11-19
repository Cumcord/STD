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