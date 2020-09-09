---
title: APIs
---

## Interfaces and types

### CeramicApi

Ceramic API interface exported by the [`@ceramicnetwork/ceramic-common` library](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/ceramic-common)

### DID

`DID` instance exported by the [`dids` library](https://github.com/ceramicnetwork/js-did)

### DIDProvider

DID Provider interface exported by the [`dids` library](https://github.com/ceramicnetwork/js-did)

### Doctype

Doctype interface exported by the [`@ceramicnetwork/ceramic-common` library](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/ceramic-common)

### Resolver

`Resolver` instance exported by the [`did-resolver` library](https://github.com/decentralized-identity/did-resolver)

### ResolverOptions

`ResolverOptions` interface exported by the [`dids` library](https://github.com/ceramicnetwork/js-did)

### DocID

The ID of a Ceramic document.

```ts
type DocID = string
```

### Definition

```ts
interface Definition<T extends Record<string, unknown> = Record<string, unknown>> {
  name: string
  schema: DocID
  description?: string
  url?: string
  config?: T
}
```

### DefinitionsAliases

```ts
type DefinitionsAliases = Record<string, DocID>
```

### Entry

```ts
interface Entry {
  tags: Array<string>
  ref: DocID
}
```

### DefinitionEntry

```ts
interface DefinitionEntry extends Entry {
  def: DocID
}
```

### ContentEntry

```ts
interface ContentEntry extends DefinitionEntry {
  content: unknown
}
```

### RootIndexContent

```ts
type RootIndexContent = Record<DocID, Entry>
```

### SchemaType

```ts
type SchemaType =
  | 'BasicProfile'
  | 'Definition'
  | 'DocIdDocIdMap'
  | 'DocIdMap'
  | 'Index'
  | 'StringMap'
```

### SchemasAliases

```ts
type SchemasAliases = Record<SchemaType, DocID>
```

### IDXOptions

```ts
interface IDXOptions {
  ceramic: CeramicApi
  definitions?: DefinitionsAliases
  resolver?: ResolverOptions
  schemas: SchemasAliases
}
```

### AuthenticateOptions

```ts
interface AuthenticateOptions {
  paths?: Array<string>
  provider?: DIDProvider
}
```

### ContentIteratorOptions

```ts
interface ContentIteratorOptions {
  did?: string
  tag?: string
}
```

## IDX class

### constructor

**Arguments**

1. `options: IDXOptions`

### .authenticate

**Arguments**

1. `options?: AuthenticateOptions`

**Returns** `Promise<void>`

### .authenticated

**Returns** `boolean`

### .ceramic

**Returns** `CeramicApi`

### .resolver

**Returns** `Resolver`

### .did

> Accessing this property will throw an error if the instance is not authenticated

**Returns** `DID`

### .id

> Accessing this property will throw an error if the instance is not authenticated

**Returns** `string`

### .has

Returns whether an entry with the `name` alias or definition `DocID` exists in the Root Index of the specified `did`

**Arguments**

1. `name: string | DocID`
1. `did?: string = this.id`

**Returns** `Promise<boolean>`

### .get

Returns the referenced content for the given `name` alias or definition `DocID` of the specified `did`

**Arguments**

1. `name: string | DocID`
1. `did?: string = this.id`

**Returns** `Promise<unknown>`

### .set

Sets the content for the given `name` alias or definition `DocID` in the Root Index of the authenticated DID

**Arguments**

1. `name: string | DocID`
1. `content: unknown`

**Returns** `Promise<DocID>` the `DocID` of the created content document

### .addTag

Adds a tag for the given `name` alias or definition `DocID` in the Root Index of the authenticated DID

**Arguments**

1. `name: string | DocID`
1. `tag: string`

**Returns** `Promise<Array<string>>` the updated set of tags

### .removeTag

Removes a tag for the given `name` alias or definition `DocID` in the Root Index of the authenticated DID

**Arguments**

1. `name: string | DocID`
1. `tag: string`

**Returns** `Promise<Array<string>>` the updated set of tags

### .remove

Removes the definition for the `name` alias or definition `DocID` in the Root Index of the authenticated DID

**Arguments**

1. `name: string | DocID`

**Returns** `Promise<void>`

### .getIDXDocID

**Arguments**

1. `did: string`

**Returns** `Promise<DocID | null>`

### .getIDXContent

**Arguments**

1. `did?: string = this.id`

**Returns** `Promise<RootIndexContent | null>`

### .createDefinition

**Arguments**

1. `definition: Definition`

**Returns** `Promise<DocID>`

### .getDefinition

**Arguments**

1. `id: DocID`

**Returns** `Promise<Definition>`

### .getEntryContent

**Arguments**

1. `definitionId: DocID`
1. `did?: string = this.id`

**Returns** `Promise<unknown | null>`

### .getEntryTags

**Arguments**

1. `definitionId: DocID`
1. `did?: string = this.id`

**Returns** `Promise<Array<string>>`

### .setEntryContent

Sets the content of the entry reference.

> The provided tags will only be set if the entry is getting created, if it already exists tags will not be changed

**Arguments**

1. `definitionId: DocID`
1. `content: unknown`
1. `tags?: Array<string> = []`

**Returns** `Promise<DocID>` the `DocID` of the created content document

### .addEntryTag

**Arguments**

1. `definitionid: DocID`
1. `tag: string`

**Returns** `Promise<Array<string>>` the updated set of tags

### .removeEntryTag

**Arguments**

1. `definitionId: DocID`
1. `tag: string`

**Returns** `Promise<Array<string>>` the updated set of tags

### .removeEntry

**Arguments**

1. `definitionId: DocID`

**Returns** `Promise<void>`

### .getEntries

**Arguments**

1. `did?: string = this.id`

**Returns** `Promise<Array<DefinitionEntry>>`

### .getTagEntries

Returns an array of `DefinitionEntry` having the provided `tag`.

**Arguments**

1. `tag: string`
1. `did?: string = this.id`

**Returns** `Promise<Array<DefinitionEntry>>`

### .contentIterator

**Arguments**

1. `options?: ContentIteratorOptions`

**Returns** `AsyncIterableIterator<ContentEntry>`