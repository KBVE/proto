# buf.build/kbve/proto

Canonical Protocol Buffer schemas for the KBVE ecosystem.

## Layers

| Package | Contents | May import |
| --- | --- | --- |
| `kbve.type.v1` | identifiers and primitive wrappers | well-known types only |
| `kbve.common.v1` | math, results, key-values, locales, shared enums | `kbve.type.v1` |
| `kbve.<domain>.v1` | domain schemas | `kbve.type.v1`, `kbve.common.v1` |

Imports run one way. A domain never imports another domain: shared messages
move down into `common`, or become a domain of their own.

## Conventions

- Every package carries a version suffix. Breaking changes ship as `v2`
  alongside `v1`.
- Identifiers use `kbve.type.v1.Ulid` / `Uuid`, never a bare `string id`.
- Timestamps use `google.protobuf.Timestamp`.
- Removed fields are `reserved`, both number and name.
- Content registries embed `kbve.common.v1.RegistryMeta` as field 1.
