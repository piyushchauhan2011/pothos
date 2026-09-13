# @pothos/plugin-prisma-next

## 0.1.1

### Patch Changes

- 8178e96: Narrow the parent shape of the cross-file field helpers (`prismaObjectField`,
  `prismaObjectFields`, `prismaInterfaceField`, `prismaInterfaceFields`) and of
  `prismaNode` to the dependencies that were actually declared.

  These helpers defaulted their parent `Shape` to the full `Row<Types, M>`, so a
  resolver added with the model-name string form was typed as if every column
  were loaded, when the plugin only loads what some `select` or `t.expose*`
  declared. Reading an unselected column compiled and then failed at query time
  with `Cannot return null for non-nullable field`. The string form now starts
  from the same brand-only `ObjectBaseShape` that `prismaObject` uses, and
  `prismaNode` computes its parent from its own `select` like `prismaObject`
  does. Field-level `select` still layers on additively, `t.expose*` and
  `t.relation` are unaffected, and the ref form is unchanged.

  `prismaNode`'s custom `id.resolve` additionally receives the columns named by
  `id.field` — single or compound — since those are selected for the ID field
  already, so it needs no redundant object-level `select`. `prismaNode`'s
  `select` keeps its existing key validation: a misspelled column or relation is
  still rejected at the call site.

  Runtime behaviour is unchanged — declaring `select` is still how a field
  adds a column dependency. To opt back into the old parent type, pass `Shape`
  explicitly: `builder.prismaObjectField<'User', Row<Types, 'User'>>('User', …)`.

- 15ae360: Infer the connection node shape from `prismaConnectionHelpers`' `resolveNode`

  `resolveNode` replaces every `edge.node`, but `wrap` still declared its result as
  `ConnectionPage<WrapRow>` — the row that was passed in. A helper configured with
  `resolveNode: (row) => ({ post: row, decoratedAt: new Date() })` typed `edges[0].node.id`
  as `string` while the runtime value was `undefined`, and strict `tsc` accepted it.

  `resolveNode`'s return type is now inferred as a `Node` generic and resolved at `wrap`.
  `PrismaConnectionHelpers` gains a defaulted fourth type parameter, so existing
  `PrismaConnectionHelpers<Types, M, Args>` references keep compiling. Helpers without a
  `resolveNode` are unaffected: `Node` stays at its `never` default and `wrap` keeps
  inferring the node from its own argument, including rows narrowed before materializing.

  Because `resolveNode` is supplied when the helper is built, its parameter can only be
  annotated with the model's full row. `wrap` now requires the full row whenever a
  `resolveNode` is configured, so a callback that mentions its parameter — `(row) => row`, or
  `(row) => ({ ...row, extra: 1 })` — can no longer launder that annotation into the node
  type and promise columns the caller never loaded. Passing narrowed rows to such a helper is
  now rejected at the `wrap` call, naming the missing columns.

  A `resolveNode` supplied conditionally (`enabled ? fn : undefined`) may never run, and `wrap`
  leaves the row untouched when it is absent. Such a helper's node is therefore typed as the
  callback's result _or_ the original row, so the caller has to narrow, rather than promising a
  transform that did not happen.

  Runtime behaviour is unchanged — the transform still runs after `buildConnectionPage`, so
  each edge's cursor is still encoded from the original row.

- a5eb463: Type an optional `prismaFieldWithInput` input as possibly `undefined`, matching the omitted-arg value GraphQL passes to the resolver
  Correct the cross-file field helper docs: extend a variant with its ref, not its variant-name string
- Updated dependencies [da938c5]
  - @pothos/core@4.15.0
  - @pothos/selection-mapper@0.1.0

## 0.1.0

### Minor Changes

- 05942a1: Initial 0.1.0 release of `@pothos/plugin-prisma-next` for Prisma ORM 8 (formerly
  Prisma Next), targeting exact `8.0.0-rc.9` framework and SQL-family packages.

  Derives GraphQL objects, interfaces, variants, relations and field types from
  an emitted contract. Resolvers return an unexecuted Collection; the plugin
  applies the selection plan and materializes it. Materialized rows are
  rejected. An optional context-aware Collection provider enables batched fallback
  loading for deferred selections and incompatible to-one relation consumers.

  Includes batched Relay node loading, root/related connections, compound cursors
  with explicit direction, null placement, and application scalar codecs, selection-aware counts,
  and contract-derived aggregate operations with custom GraphQL scalar support.
  Connections accept existing ordering that matches a prefix of the cursor's query
  order, including the reversed order used for backward pages.

  Applications own drivers and transactions. The same SQL-family plugin is tested
  against SQLite and PostgreSQL, including lossless included values and Temporal
  cursors. Mongo's separate family is unsupported. Without a fallback Collection
  provider, deferred selections load eagerly and incompatible to-one refinements
  are rejected.

  Use Node.js 24 or later. Prisma ORM 8 remains a release candidate; change its exact
  pin only with compatibility validation. This package is separate from
  `@pothos/plugin-prisma`, which targets the classic `@prisma/client`.

### Patch Changes

- Updated dependencies [0c7565a]
- Updated dependencies [1358f71]
  - @pothos/core@4.14.0
  - @pothos/selection-mapper@0.1.0

## 0.1.0 (unpublished)

Initial fork from `@pothos/plugin-drizzle` and conversion to prisma-next. See README.md for status.
