# KBVE Proto

Canonical Protocol Buffer schemas for the KBVE ecosystem.

This repository is the single source of truth. Schemas are published to the
Buf Schema Registry as `buf.build/kbve/proto`; consumers depend on the
generated SDKs rather than vendoring `.proto` files or compiling them from a
sibling checkout.

## Layout

```
kbve/type/v1/       identifiers and primitive wrappers   (imports: WKT only)
kbve/common/v1/     math, results, key-values, locales   (imports: type)
kbve/<domain>/v1/   domain schemas                       (imports: type, common)
```

Imports run in one direction only. A domain never imports another domain; if
two domains need the same message, it belongs in `common` or in a domain of
its own.

## Conventions

- Every package carries a version suffix (`kbve.item.v1`). A breaking change
  ships as `v2` alongside `v1` rather than mutating `v1` in place.
- Identifiers use the wrapper types in `kbve/type/v1/id.proto`. A bare
  `string id` field is not acceptable in new schemas.
- Timestamps use `google.protobuf.Timestamp`. Do not define your own.
- Removed fields are always `reserved`, both the number and the name.

## Local development

```sh
buf build           # compile all modules
buf lint            # style and layout rules
buf format -w       # apply canonical formatting
buf breaking --against '.git#branch=main'
```

## Status

Migration in progress. Domains are moved from `KBVE/kbve`
(`packages/data/proto/`) one at a time. That tree is frozen: it receives
bugfixes only, and new schema work happens here.
