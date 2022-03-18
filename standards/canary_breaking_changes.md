# Canary & build override breaking changes standard

## Description Of Issue

Changes made to Canary have the chance to break Cumcord and have potential to be merged into stable, as do build overrides.

## Solution

Hold back updates on the Cumcord stable branch and release updates on master, then merge master into stable when Canary is merged into stable,
or when the build override is applied.

As with inconsequential Canary changes, small inconsequential build overrides can be ignored.

## Advantages

- Cumcord works properly on stable
- Cumcord will be fixed immediately upon the change being merged into stable

## Disadvantages

- Some things don't make it past stable, and our fixes may not be necessary
- Updates are held back

## Alternatives

### Maintaining a Canary and Stable branch (and a branch for each breaking override)

This would add additional things to maintain and the wasted hours maintaining all branches is unnecessary with other alternatives.

### Supporting Canary over Stable

This requires less effort than the other proposed solutions, but most Discord users use Stable and Canary is ever-changing. It would not give us time to prepare for breaking changes in Discord.

Also highly infeasible for build overrides.

## Conclusion

The proposed solution seems to work the best, and the alternatives have various downsides that seem worse than my proposal.
