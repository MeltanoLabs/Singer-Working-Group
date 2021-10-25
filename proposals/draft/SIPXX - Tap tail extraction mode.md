## Status

| header | header |
| ------ | ------ |
| State | Draft |
| Issue Link | https://github.com/MeltanoLabs/Singer-Working-Group/issues/7 |
| Discussion Thread(s) | (optional link) |
| Created | 2021-10-25

-----------------------

## Proposal

### TL;DR Overview

To support streaming use cases, tap's may want to support continuous "tailing" of sources. 

There are no changes needed to the spec and it's possible to create and use a tap in an "always-on" fashion today. However, there's no explicit guidance or best practices defined for tap developers or end-users on leveraging this pattern.


### What specific change do you propose to make?

The underlying mechanic (polling micro-batches, reading a streaming endpoint, etc) rests with the tap developer and is still based on the feature set and data source implementation details. However, we could support a standardized a boolean setting like `tail` that would cause taps to alter their behavior and begin to extract data from the source continuously. A tap would be expected to continue extracting data until its stopped or encounters errors. 

That is, the `tail` setting might alter a taps behavior such that: 

- It may enter an infinite loop using its existing extraction implementation and enter a `extract -> sleep -> extract -> ...` pattern.
- It may adjust and begin reading/emitting data continuously by connecting to a dedicated streaming endpoint.

Additionally, we should detail a standardized set of supporting options and their expected behaviors for use with `tail` mode. 

1. To honor resumption after a hard interrupt, a `state_message_min_interval_ms` setting should be established, with the expectation that this forces a state message to flush at least every x interval, for instance, assuming 1 or more records have been sent since the last STATE message. Tap developers may also want to tie this closely to commit intervals with their source (i.e. commit Kafka offsets to match) and should still document how this relates to deliverability semantics and guarantees. 
2. For incremental streams, where polling is being used a `new_record_polling_interval_ms` setting should be established, with the expectation that this would be the interval used when polling for data. (i.e. sleeping for this interval in a polling loop) 
3. Optionally, to keep non-incremental streams whole while running a tap in `tail` mode, a `max_full_table_age_seconds`  setting should be established, which would be expected to drive new extracts of streams pulled with `FULL_TABLE` after the set amount of time has elapsed. This may not be desirable in all scenarios. 
4. `SIGTERM` signal use should be supported and expected to immediately trigger halting extraction and emitting a state message. After which the tap should exit cleanly. Again some users may also want to tie this to commit intervals with their data source - and tap developers should still provide guidance around data duplication and deliverability semantics.


To provide a consistent user experience for end-users, time unit based settings are suffixed with their expected unit of time (`_ms`). 

### Which layer(s) of the Singer ecosystem does this proposal directly touch?

Select all that apply:

- [ ] Singer Specification - required capabilities and behaviors
- [x] Singer Specification - optional capabilities and behaviors
- [x] Singer best practices and other guidance
- [ ] Singer Working Group - practices and procedures
- [ ] Singer documentation (Other)

## Motivation
> >

### What problem does it solve?

The ability to extract data continuously or more frequently is highly desirable. Data freshness is often the primary problem but in some situations, the data being extracted may also be short-lived at the source. 


### Why is it needed?

In streaming or near-realtime scenarios obtaining data in batches with larger intervals impacts data freshness. 

Obtaining data more frequently with micro-batches is possible, but when a complete client setup/tear down also occurs can be undesirable, as frequent client disconnects may cause additional overhead upstream with the data source.

Some data sources also have dedicated high performance or low overhead streaming endpoints that are preferred - for which batch patterns are sub-optimal or even unsupported.

## Other Considerations
> >
### Are there any downsides to this change?

There are no provisions for retry semantics and end users may need to make deployment changes to accommodate long running process should they wish to use this feature.

### Is the change backward compatible?

This is backward compatible in the sense there's no actual change to the spec, but existing taps would require updates to expose this functionality.

### Which users are affected by the change?

Tap developers are the only users directly affected since they would need to accommodate this mode of operation.

### How are users affected by the change? (e.g. DB upgrade required?)

While completely optional, end users must accommodate managing a long-running process to leverage this.  

### Prototype Implementations (optional)

N/A 

### Future Plans

Retries. With the increased run times, it's much more likely a tap may begin to encounter transient errors (i.e. endpoint disconnects, temporary throttling). In the future, we may want to consider providing guidance around retry handling and define some basic standardized named settings (i.e. `tail_retry`, `tail_max_retries`, `tail_full_table_on_failure`).

Guidance around capturing `HUP` signal to support restarting/reconnecting active connections.

Guidance on how to handle SIGSTOP/SIGCONT use by users. 

### Excluded Alternatives

...(if applicable)...

### Acknowledgements 

...(if applicable)...

## What defines this SIP as "done"?

...