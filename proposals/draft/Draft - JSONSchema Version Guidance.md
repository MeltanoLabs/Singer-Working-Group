## Status

| header | header |
| ------ | ------ |
| State | Draft  |
| Issue Link | https://github.com/MeltanoLabs/Singer-Working-Group/issues/17 |
| Discussion Thread(s) | (optional link) |
| Created | 2021-10-25 |

-----------------------

## Proposal

### TL;DR Overview

We should provide guidance for recommended and supported JSON Schema versions to tap and target developers which 

### What specific change do you propose to make?

We should document in the Singer Spec that Singer taps and targets expect **Draft 04** version of the JSON Schema spec.

## Motivation

As of the time of writing, the latest version of json schema is **Draft 2020-12**. While most changes between spec drafts are backwards compatible, we should nevertheless document for users and developer which version of the spec they should expect to use for best compatibility.

### Which layer(s) of the Singer ecosystem does this proposal directly touch?

Select all that apply:

- [x] Singer Specification - required capabilities and behaviors
- [ ] Singer Specification - optional capabilities and behaviors
- [ ] Singer best practices and other guidance
- [ ] Singer Working Group - practices and procedures
- [ ] Singer documentation (Other)

### What problem does it solve?

This prevents unnecessary trial and error and unnecessary uncertainty around which version(s) are supported or recommended.

### Why is it needed?

Without specific guidance, developers may waste development cycles unnecessarily (at best) or introduce incompatibilities (at worst).

## Other Considerations

### Are there any downsides to this change?

There are no downsides to this change.

### Is the change backwards compatible?

Not applicable.

### Which users are affected by the change?

Users are not affected.

### How are users affected by the change? (e.g. DB upgrade required?)

Not applicable.

### Prototype Implementations

Not applicable.

### Future Plans

Not applicable.

### Excluded Alternatives

Not applicable.

### Acknowledgements 

Not applicable.

## What defines this SIP as "done"?

This SIP is published and related documentation for Singer Spec is also updated with this guidance.
