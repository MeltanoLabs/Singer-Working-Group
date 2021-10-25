# SIP x - Composable state artifacts

## Status

|  |  |
| ------ | ------ |
| State | Draft |
| Issue Link | [#6](https://github.com/MeltanoLabs/Singer-Working-Group/issues/6) |
| Discussion Thread(s) | (optional link) |
| Created | 2021-10-25 |

-----------------------

## Proposal

### TL;DR Overview

Singer State artifacts can be split between its constituent streams and combined again.

### What specific change do you propose to make?

According to the [Singer specification](https://github.com/singer-io/getting-started/blob/master/docs/SPEC.md#state-message):

> The semantics of a STATE value are not part of the specification, and should be determined independently by each Tap.

This SIP proposes to standardize the structure of state message `value` objects in Singer taps, to allow splitting and combining state artifacts from multiple streams.

The proposed `value` structure looks like this:

```json
{
  "bookmarks": {
    "stream_1": {},
    "stream_2": {}
  }
}
```

That is, there MUST be one key for every stream in the source and the object value MUST contain all necessary state information to sync the stream in question. For taps with streams that need access to a _global_ state, the same duplicate value MUST be stored per-stream.

## Motivation

### What problem does it solve?

By making state (de)composable, some interesting use cases open for orchestrators. For example, syncing a Singer Tap with independent stream bookmarks, is [embarrassingly parallelizable](https://en.wikipedia.org/wiki/Embarrassingly_parallel). It should be possible to split the workload of such a tap among its constituent streams to fully utilize compute resources and reduce syncing time.

### Why is it needed?

By standardizing on the structure of state values, more performant and scalable use cases open for Singer taps and targets, without sacrificing inter-operability.

## Other Considerations

### Are there any downsides to this change?

- Every stream would need to track any global state independently.

### Is the change backwards compatible?

No. Changing the state schema of a tap would break incremental replication for current users.

### Which users are affected by the change?

Users of existing taps that use differently structured state artifacts. For example, [tap-stripe](https://github.com/singer-io/tap-stripe).

### How are users affected by the change? (e.g. DB upgrade required?)

Users of non-compliant taps would need to manually massage the schema into the right format or use a one-off script.

### Prototype Implementations

The SDK currently enforces this state schema.

### Future Plans

NA

### Excluded Alternatives

NA

### Acknowledgements 

- [@dmosorast](https://github.com/dmosorast)
- [@aaronsteers](https://github.com/aaronsteers)

## What defines this SIP as "done"?

The working group has agreed upon the standard for state values.
