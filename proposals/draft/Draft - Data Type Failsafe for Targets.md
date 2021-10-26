## Status

| header | header |
| ------ | ------ |
| State | Draft  |
| Issue Link | https://github.com/MeltanoLabs/Singer-Working-Group/issues/20 |
| Discussion Thread(s) | (optional link) |
| Created | 2021-10-25 |

-----------------------

## Proposal

### TL;DR Overview

Targets should make every attempt to successfully serialize data received from a tap. This includes having build-in failsafe behavior to handle JSON Schema parsing errors and unexpected JSON Schema types.

### What specific change do you propose to make?

We will add guidance to Singer documentation resources which strongly recommend specific practices to handle JSON Schema parsing errors and unexpected types.

## Motivation

While JSON Schema is a broadly adopted standard, there are countless ways to describe data types in JSON Schema. Today most targets are build with simple case statements, and with no `else` treatment. Types that are unexpected explicitly, or types which cannot be parsed will often cause failures and a poor experience for the community.

### Which layer(s) of the Singer ecosystem does this proposal directly touch?

Select all that apply:

- [ ] Singer Specification - required capabilities and behaviors
- [x] Singer Specification - optional capabilities and behaviors
- [ ] Singer best practices and other guidance
- [ ] Singer Working Group - practices and procedures
- [ ] Singer documentation (Other)

### What problem does it solve?

This solves the perception that "not all taps and targets are compatible" - when in reality the problem is simply a lack of robustness in the target's handling of JSON Schema types.

### Why is it needed?

To improve the experience for Singer users and to give tap developers more flexibility in their use of advanced data types.

## Other Considerations

### Are there any downsides to this change?

None.

### Is the change backwards compatible?

Yes.

### Which users are affected by the change?

Users who rely on taps and targets to "just work" should have a better experience if targets subscribe to this behavior.

### How are users affected by the change? (e.g. DB upgrade required?)

Users should experience broader compatibility amongst taps and targets overall.

### Prototype Implementations

None known. (TBD.)

### Future Plans

The `datatype-failsafe` capability could be advertised in the `--about` proposed feature.

### Excluded Alternatives

Not applicable.

### Acknowledgements 

None noted yet.

## What defines this SIP as "done"?

The behavior is clearly documented. The behavior has at least one reference implementation which can be referenced for example. Ideally one or more Python library (perhaps `singer-sdk` by Meltano, or one of the `singer-python` forks) would have a callable implementation to help new developers.
