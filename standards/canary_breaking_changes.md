# Canary breaking changes standard

## Description Of Issue

Changes made to Canary have the chance to break Cumcord and have potential to be merged into stable.

## Solution

Hold back updates on the Cumcord stable branch and release updates on master, then merge master into stable when Canary is merged into stable.

## Advantages

- Cumcord works properly on stable
- Cumcord will be fixed immediately upon Canary being merged into stable

## Disadvantages

- Some things don't make it past stable, and our fixes may not be necessary
- Updates are held back

## Alternatives

### Maintaining a Canary and Stable branch

This would add additional things to maintain and the wasted hours maintaining both branches is unnecessary with other alternatives.

### Supporting Canary over Stable

This requires less effort than the other proposed solutions, but most Discord users use Stable and Canary is ever-changing. It would not give us time to prepare for breaking changes in Discord.

## Conclusion

The proposed solution seems to work the best, and the alternatives have various downsides that seem worse than my proposal.
