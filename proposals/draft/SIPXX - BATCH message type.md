## Status

| header | header |
| ------ | ------ |
| State | Draft |
| Issue Link | https://github.com/MeltanoLabs/Singer-Working-Group/issues/2 |
| Discussion Thread(s) | https://gitlab.com/meltano/sdk/-/issues/9 |
| Created | 2021-10-26 |

-----------------------

## Proposal

### TL;DR Overview

To i) better support sources with bulk-export and reporting capabilities and ii) to improve performance for high volume tabular sources (databases with millions of records) we propose adding a new `BATCH` message type to the Singer specification.

As a completely new and optional message type, no changes are required to exisiting taps and targets.

### What specific change do you propose to make?

---

### BATCH Message

BATCH messages contain references to multiple `RECORD.record` payloads serialised to storage. They must have the following properties:

- `stream` **Required**. The string name of the stream.
- `time_extracted` **Optional**. The time these records were observed in the source. This should be an RFC3339 formatted date-time, like "2017-11-20T16:45:33.000Z".
- `size` **Optional**. The number of records per batch file.
- `format` **Optional**. A JSON map containing serialisation format details. A default json-lines (JSONL) format is expected in the absence of `format` information.
- `compression` **Optional**. A JSON map containing serialised file compression details.
- `manifest` **Required**. A list of string URLs indicating the files in this BATCH.

A single Tap may output BATCH messages with different stream
names.  A single BATCH may only contain records for a single
stream. The BATCH message may contain pre-signed URLs but it may not directly contain other credential information. Although not strictly required, BATCH message would generally be immediately followed by a STATE message.

Example:

*Note: Every message must be on its own line, but the examples here use multiple lines for readability.*

```json
{
  "type": "BATCH",
  "stream": "stream_name",
  "time_extracted": "2017-11-20T16:45:33.000Z",
  "size": 100000, // upper limit rather than file-exact (last file likely to contain fewer)
  "format": {
    "name": "csv",
    "options": {
      "delimiter": "\t",
      "quote_char": "\""
    }
  },
  "compression": {
    "name": null
    "options": {}
    "enabled": false
  },
  "manifest": [ // can be one or more files
    "s3://path/to/file001.csv",
    "s3://path/to/file002.csv",
    "s3://path/to/file003.csv"
  ]
}
```

## Motivation
> >

### Which layer(s) of the Singer ecosystem does this proposal directly touch?

Select all that apply:

- [ ] Singer Specification - required capabilities and behaviors
- [X] Singer Specification - optional capabilities and behaviors
- [X] Singer best practices and other guidance
- [ ] Singer Working Group - practices and procedures
- [X] Singer documentation (Other)

### What problem does it solve?

This proposal addresses 2 different use-cases:

1. Sources with large volumes of upstream records to be retrieved, for example the initial sync of a large (millions of records) RDBMS source.
1. Sources that support batching natively. This may be warehouses with UNLOAD capabilities or API's with bulk export or reporting endpoints for performantly extracting large volumes of records (vs. per-record or per-page).

This offeres several new avenues for improving performance:

- For taps and targets that _both_ support a common BATCH format (e.g. RDBMS to Warehouse) we have the opportunity to implement 'pass through', whereby records are _never deserialised_ between source to destination, greatly reducing overhead and improving throughput. This would drastically accelerate this common use-case in the singer ecosystem, inspired by the `fast_sync` feature of [Pipelinwise](https://transferwise.github.io/pipelinewise/concept/fastsync.html).

- BATCH messages can readily be converted to plain RECORD messages (or visa versa) for asymetric scenarious (where ony Tap or Target prefer/facilitate a BATCH format). Utilities like the Singer SDK can implement built-in methods for batching record-wise sources in Taps (for Targets that prefer BATCH messages, like Warehouses) and similarly for reading RECORDS back from their serialised BATCH format in Targets (from Taps with accelerated facilities for retrieveing batches), enabled via config.

- BATCH files can be processed/inserted using separate threads or processes in the Target.

### Why is it needed?

Without formal support for a BATCH message type, batches in the Tap must be converted to individual RECORD messages. Similarly in the Target, RECORD payloads must be 'bucketed' in memory and 'flushed' in batches for destinations that benefit from batch uploads. Adding a BATCH message addresses these needs directly.

## Other Considerations
> >
### Are there any downsides to this change?

There is some risk that introducing BATCH messages, with options for every combination of serilisation formats, compression and storage services, that we might errode the founding goal of interoperability in the Singer ecosystem. We propose mitigating this in the following ways:

- BATCH messages _never_ replace RECORD messages, they are a configurable addition. I.e. all Taps should support emitting RECORD messages as per the spec, but can optionally emit BATCH messages if supported and configured.
- A default BATCH format (JSONL), compression (None) and storage service (local disk) is included in the spec, and is the bare minimum to be supported by Taps and Targets implementing the BATCH message type.
- Using frameworks like the Singer SDK, we can make it such that _any_ Tap implemented with the SDK can also emit BATCH messages, by allowing the SDK to serialise individual records recieved (in stead of emitting RECORD messages). This allows us to a broad-based support relatively cheaply.
- Some thought has been given [here](https://gitlab.com/meltano/sdk/-/issues/9#note_660225849) to 'storage transforms' that expand user choice regarding intermediate storage locations/services without burdening Taps and Targets with extensive first-class support for every format:compression:location combination.

### Is the change backwards compatible?

Yes. Emitting BATCH messages are completely optional. Current Target implementations raise an error when they encounter message types they cannot handle.

### Which users are affected by the change?

Both Tap and Target developers are impacted by this change.

### How are users affected by the change? (e.g. DB upgrade required?)

New functionality to make use of, which means changes/additions to current Tap and Target implementations.

### Prototype Implementations

- [tailsdotcom/pipelinewise-target-snowflake](https://github.com/tailsdotcom/pipelinewise-target-snowflake/blob/master/target_snowflake/__init__.py#L249) fork. In this implementation, records were deserialised in the Target (for validation and a metadata injection), which slowed the overall throughput down compared to a true 'pass through' mode. However Memory preassure was drastically reduced when using large batch sizes (as records are streamed line-by-line from disk rather than bucketed in-memory).

### Future Plans

- Storage transforms [as per this discussion](https://gitlab.com/meltano/sdk/-/issues/9#note_660225849)
- BATCHing in the Tap, in conjunction with [composable STATE messages]() opens the door to greater in-stream parallalisation options within Targets (compared to in-memory bucketing as is the current common practice).

### Excluded Alternatives

...(if applicable)...

### Acknowledgements

- tails.com for their first trials and early implementation.
- Based heavily on the `fast_sync` feature of the wonderful Pipelinewise project, and in discussion with their team.
- Feedback from the engineering team at Meltano.

## What defines this SIP as "done"?

The new BATCH message type is included in the [Singer Specification](https://github.com/singer-io/getting-started/blob/master/docs/SPEC.md).