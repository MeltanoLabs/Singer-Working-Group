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

{
  "type": "BATCH",
  "stream": "stream_name",
  "time_extracted": "2017-11-20T16:45:33.000Z",
  "size": 100000, // upper limit rather than file-exact (last file likely to contain fewer)
  "format": {
    "name": "CSV",
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


## Motivation
> >
...

### Which layer(s) of the Singer ecosystem does this proposal directly touch?

Select all that apply:

- [ ] Singer Specification - required capabilities and behaviors
- [ ] Singer Specification - optional capabilities and behaviors
- [ ] Singer best practices and other guidance
- [ ] Singer Working Group - practices and procedures
- [ ] Singer documentation (Other)

### What problem does it solve?

...

### Why is it needed?

...

## Other Considerations
> >
### Are there any downsides to this change?

...

### Is the change backwards compatible?

...

### Which users are affected by the change?

...

### How are users affected by the change? (e.g. DB upgrade required?)

...

### Prototype Implementations

...(if applicable)...

### Future Plans

...(if applicable)...

### Excluded Alternatives

...(if applicable)...

### Acknowledgements

...(if applicable)...

## What defines this SIP as "done"?

...