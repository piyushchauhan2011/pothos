# @pothos/plugin-add-graphql

## 4.3.2

### Patch Changes

- ddef6d4: Use the operation roles declared by an imported schema instead of inferring them from type names: roots with custom names (`schema { query: Root }`) are imported as operation roots, types that the imported schema does not use as a root are no longer promoted to one because they are named `Query`, `Mutation` or `Subscription`, an imported root is no longer used for a second operation as well (`schema { query: Mutation }` no longer produces a schema that uses `Mutation` as both its query and mutation root), and an operation root the builder already defines is never re-declared. `builder.addGraphQLObject` accepts a new `rootKind` option to select the operation root a type is imported as, or `null` to import a type named `Query`, `Mutation` or `Subscription` as a normal object type; types imported without a `rootKind` (`add: { types }` and standalone `addGraphQLObject` calls) still infer roots from their names, and an imported schema that declares no query root still falls back to a type named `Query`. Also preserve `deprecationReason` on imported input object fields

## 4.3.1

### Patch Changes

- 76e06e7: Rebuild with TypeScript 7. Source files now use explicit `.js` import extensions (enforced by
  lint) instead of adding them during the build, and declaration files are emitted by TypeScript
  7's compiler. Published output is functionally unchanged.

## 4.3.0

### Minor Changes

- 5b29d51: Add support for adding astNodes to types, fields, and enum values

## 4.2.4

### Patch Changes

- 1622740: update dependencies

## 4.2.3

### Patch Changes

- cd7f309: Update dependencies

## 4.2.2

### Patch Changes

- 4af88f1: correctly pass through isOneOf setting from native GraphQL input objects

## 4.2.1

### Patch Changes

- 7a4fa4c: Fix added subscription fields with custom subscribe method

## 4.2.0

### Minor Changes

- e6ca3fa: Support nested lists

## 4.1.0

### Minor Changes

- 27af377: replace eslint and prettier with biome

## 4.0.2

### Patch Changes

- Updated dependencies [777f6de]
  - @pothos/core@4.0.2

## 4.0.1

### Patch Changes

- 9bd203e: Fix graphql peer dependency version to match documented minumum version
- Updated dependencies [9bd203e]
  - @pothos/core@4.0.1

## 4.0.0

### Major Changes

- 29841a8: Release Pothos v4 🎉 see https://pothos-graphql.dev/docs/migrations/v4 for more details

### Patch Changes

- c1e6dcb: update readmes
- Updated dependencies [c1e6dcb]
- Updated dependencies [29841a8]
  - @pothos/core@4.0.0

## 4.0.0-next.1

### Patch Changes

- update readmes
- Updated dependencies
  - @pothos/core@4.0.0-next.1

## 4.0.0-next.0

### Major Changes

- 29841a8: Release Pothos v4 🎉 see https://pothos-graphql.dev/docs/migrations/v4 for more details

### Patch Changes

- Updated dependencies [29841a8]
  - @pothos/core@4.0.0-next.0

## 3.2.1

### Patch Changes

- 1ecea46: revert accidental pinning of graphql peer dependency

## 3.2.0

### Minor Changes

- 41fe7d4: Make options optional when registering existing scalars/types

## 3.1.1

### Patch Changes

- 425435af: Improve typing of inputRefs and fix incorrectly normalized function properties of
  inputRef types

## 3.1.0

### Minor Changes

- 27b0638d: Update plugin imports so that no named imports are imported from files with side-effects

## 3.0.2

### Patch Changes

- 4c6bc638: Add provinance to npm releases

## 3.0.1

### Patch Changes

- b5620c75: fix issue with extending root types

## 3.0.0

### Major Changes

- f9b0e2eb: Initial release
