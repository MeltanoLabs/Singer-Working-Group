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

The proposed `value` structure if a **flat** JSON object with the following schema:

```json
{
  "$schema": "http://json-schema.org/draft-06/schema#",
  "title": "Singer State",
  "description": "State payload format for Singer taps",
  "type": "object",
  "properties": {
    "bookmarks": {
      "title": "Stream Bookmarks",
      "type": "object",
      "additionalProperties": false,
      "patternProperties": {
        "^\\w+(\\/\\w+\\=[A-Za-z0-9_-]+)*$": {
          "type": "object",
          "additionalProperties": true,
          "properties": {
            "replication_key": {
              "type": "string"
            },
            "replication_key_value": {
              "oneOf": [
                {
                  "type": "string"
                },
                {
                  "type": "integer"
                }
              ]
            }
          }
        }
      }
    },
    "additionalProperties": false
  }
}
```

That is, there MUST be one key for every stream or stream partition in the source, and the object value MUST contain all necessary state information to sync the stream or partition in question. For taps with streams that need access to a _global_ state, the same duplicate value MUST be stored per-stream AND per-partition.

Notes:

- The keys for partitioned stream bookmarks need to indicate the partition keys (e.g. `catalogs/shop_id=143`). Thus, order of the keys must be deterministic.
- A flat object like this is the most straightforward to merge.

In the following examples, the stream hierarchy `shops` > `catalogs` > `products` is synced from an API. The bookmark value of a catalog might change from one shop to another, so it's necessary to store one `catalogs` bookmark for each `shop_id`. Similarly, `products` require a `shop_id` and `catalog_id`.

#### Example valid state objects

```json
{
  "bookmarks": {
    "shops": {
      "replication_key": "updated_at",
      "replication_key_value": "2021-02-01T00:00:00Z",
      "some_global_state": 123
    },
    "catalogs/shop_id=143": {
      "replication_key": "updated_at",
      "replication_key_value": "2021-02-01T00:00:00Z"
    },
    "products/shop_id=143/catalog_id=2022-01": {
      "replication_key": "updated_at",
      "replication_key_value": "2021-02-01T00:00:00Z"
    }
  }
}
```

#### Example invalid state objects

Global state defined at the top level:

```json
{
  "bookmarks": {
    "shops": {
      "replication_key": "updated_at",
      "replication_key_value": "2021-02-01T00:00:00Z"
    },
    "catalogs/shop_id=143": {
      "replication_key": "updated_at",
      "replication_key_value": "2021-02-01T00:00:00Z"
    },
    "products/shop_id=143/catalog_id=2022-01": {
      "replication_key": "updated_at",
      "replication_key_value": "2021-02-01T00:00:00Z"
    }
  },
  "global_state_can_not_live_here": 123
}
```

The SDK implementation becomes invalid under the current proposal:

```json
{
  "bookmarks": {
    "shops": {
      "replication_key": "updated_at",
      "replication_key_value": "2021-02-01T00:00:00Z"
    },
    "catalog": {
      "partitions": [
        {
          "context": {
            "shop_id": 143
          },
          "replication_key": "updated_at",
          "replication_key_value": "2021-02-01T00:00:00Z"
        }
      ]
    },
    "products": {
      "partitions": [
        {
          "context": {
            "shop_id": 143,
            "catalog_id": "2022-01"
          },
          "replication_key": "updated_at",
          "replication_key_value": "2021-02-01T00:00:00Z"
        }
      ]
    }
  }
}
````

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

NA

### Future Plans

NA

### Excluded Alternatives

#### Meltano SDK (up to 2022-02)

The SDK currently uses a state schema where partitioned streams contain an array of different _context_ objects:

```json
{
  "bookmarks": {
    "products": {
      "partitions": [
        {
          "context": {
            "shop_id": 143
          },
          "replication_key": "updated_at",
          "replication_key_value": "2022-01-22T06:49:41.005000Z"
        },
        {
          "context": {
            "shop_id": 158
          },
          "replication_key": "updated_at",
          "replication_key_value": "2022-01-24T12:53:08.000000Z"
        }
      ]
    }
  }
}
```

#### Other alternatives

Some taps use a structure like:

```json
{
  "bookmarks": {
    "products": {
      "updated_at(shod_id:143)": "2022-01-22T06:49:41.005000Z",
      "updated_at(shod_id:158)": "2022-01-24T12:53:08.000000Z"
    }
  }
}
```

This is close to the current proposal, but adds an unnecessary level of _nested-ness_.

### Acknowledgements 

- [@dmosorast](https://github.com/dmosorast)
- [@aaronsteers](https://github.com/aaronsteers)

## What defines this SIP as "done"?

The working group has agreed upon the standard for state values.
