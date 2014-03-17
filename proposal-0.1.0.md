# Proposal for Hyperdescribe 0.1.0

**Status**: Incomplete

After working through quite a few samples and helping think through UBER, I've come up with some design changes for Hyperdescribe to get it to a usable state. These changes allow for handling media types with data at the root element (like HAL and Siren) and nested data (like Collection+JSON and UBER). 

This also provides a tool for media type designers and implementers to use in designing and using media types.

## Current Proposed Changes

* Add root `hyperdescribe` element, per some discussions I've been reading around UBER's root element.
* Change `resources` to `entities` to make more generic
* Make `entities` a recursive object while also make a root-level entity
* Move data-specific items under `content` property for entities.
* Add Code object for linking to code
* Remove `rels` and `url` from entity object. Move to link where `rel=self`
* Change `name` (in current spec) to Array `classes`.
* Add `options` to Field Objects. This allows the field to act like a `<select>` in HTML.
* Add `defaultValue` to Field Objects. This says what the default value of the field is, and can be changed. If the `value` field is set, though, the value cannot be changed. 

## Other Questions

1. Should `version` be in every Entity Object? Should it be considered optional meta data?
2. Should `actions` and `links` be combined? I go back and forth. Currently, I've combined them. **Should this combined item be called actions? Links? Transitions?**
3. Should all Arrays be plural names (e.g. rels, actions, links, etc.)? Reason being, I want this format to be easy for parsing, and I like `for link of links` language.
4. Should I nest properites, links, etc, under `content` like I'm doing? I think it helps make sense when translating to HTML+RDFa.
5. Should links/actions be viewed a "links/actions you can take on this particular entity" or "links/actions related to this particular entity?" So far, I have done the latter.
6. Should I use protocol-agnostic action types like UBER? This would allow this to describe messagse not in HTTP. I could add a object in `meta` for mapping from an action name to an HTTP method.
7. Should I seek to register a `vnd` media type? I have purposely stayed away from it, but I can see where it could be useful for a client that uses Hyperdescribe. It would be the equivalent to server rendering the HTML rather than the client doing it from some other media type.

## Hyperdescribe Document

```javascript
{
  hyperdescribe: Object
}
```

Now just an object with one root property, `hyperdescribe`. The `hyperdescribe` property takes an Entity Object. 

## Entity Object

An entity is anything that has identity. This can be a person, address, blog post, etc.

```javascript
{
  version: String,
  prefixes: Array,
  id: String,
  classes: Array,
  property: String,
  content: Object
}
```

### Available Properties

* `version` is a String of the Hyperdescribe version of this entity object
* `prefixes` is a Prefix Object. It is OPTIONAL.
* `id` is a String for documen-unique identifier. Either `id` or one `classes` MUST be set, but MAY NOT be both.
* `classes` is an Array of Strings of non-unique identifiers. Either `id` or one `classes` MUST be set, but MAY NOT be both.
* `property` is a String for the property name of the parent entity (only used on nested entities). It is OPTIONAL.
* `content` is a Content Object. It is OPTIONAL.

## Prefix Object

```javascript
{
  name: String,
  url: String
}
```

## Available Properties

* `name` is a String of the shortened prefix value (e.g. schema, foaf). It is REQUIRED.
* `url` is a String of the URL that is shortened (e.g. http://schema.org). It is REQUIRED.

## Default Prefixes

By default, Hyperdescribe will honor all of the prefixes in the [core initial contexts of RDFa](http://www.w3.org/2011/rdfa-context/rdfa-1.1) without having to specify them.

## Content Object

```javascript
{
  meta: Object,
  context: Object,
  properties: Array,
  links: Array,
  entities: Array
}
```

### Available Properties

* `meta` is a Meta Object. It is OPTIONAL.
* `context` is a Context Object. It is OPTIONAL.
* `properties` is an Array of Property Objects. It is OPTIONAL.
* `links` is an Array of Link Objects. It is OPTIONAL.
* `entities` is an Array of Entity Objects. It is OPTIONAL.

### Example: Translated to HTML+RDFa

This translates well to HTML+RDFa:

```html
<div id="" class="" rel="" resource="" vobab="" typeof="" prefix="">
  <div class="meta"></div>
  <div class="properties"></div>
  <div class="entities"></div>
  <div class="links"></div>
</div>
```

## Meta Object

```javascript
{
  data: Object,
  code: Array,
  links: Array
}
```

### Available Properties

* `data` is an Object of name/value pairs. It is OPTIONAL.
* `code` is an Array of Link Objects. It is OPTIONAL.
* `links` is an Array of Link Objects. It is OPTIONAL.

## Context Object

```javascript
{
  vocab: String,
  typeOf: Array
}
```

Context defines the vocab and typeOf to be used for all of the properties in the Entity Object in which it is found. If a Content Object is used, only `typeOf` is REQUIRED. This is based on the http://schema.org vocab and how RDFa defines vocabularies and types.

### Available Properties

* `vocab` is a String for the URL to the vocab being used for this entity. It is OPTIONAL.
* `typeOf` is an Array for the type of entity being described here. It is REQUIRED.

## Property Object

```javascript
{
  type: String,
  name: String,
  value: String,
  label: String,
  context: Object
}
```

### Available Properties

* `type` is a String for the type of property here. If not present, `text` is default
  - boolean
  - date
  - dateTime
  - time
  - float
  - int
  - text
* `name` is a String for the name of the property. It is REQUIRED.
* `value` is a String for the value of the property. It is OPTIONAL.
* `label` is a String for an OPTIONAL, human-readable name of the property
* `context` is a Context Object. It is OPTIONAL.

## Link Object

```javascript
{
  id: String,
  classes: Array,
  property: String,
  rels: Array,
  url: String,
  method: String,
  isTemplated: Boolean,
  responseTypes: Array,
  requestTypes: Array,
  fields: Array,
  label: String,
  renderAs: String
}
```

### Available Properties

* `id` is a String for documen-unique identifier. Either `id` or one `classes` must be set, but cannot be both.
* `classes` is an Array of Strings of non-unique identifiers. Either `id` or one `classes` must be set, but cannot be both.
* `rels` is an Array of available link relations for this link. It is OPTIONAL.
* `url` is a String for the `URL` of this link. It is REQUIRED.
* `method` is a String for the protocol method to be used when sending a request. By default, it is `GET`. It is OPTIONAL.
* `isTemplated` is a Boolean stating whether the URL is templated or not. By default, this is `false`. It is OPTIONAL.
* `responseTypes` is an Array of media types available to be sent as the response. It is OPTIONAL.
* `requestTypes` is an Array of media types the server will accept in the request. It is OPTIONAL.
* `fields` is an Array of Field Objects. It is OPTIONAL.
* `label` is a String for an OPTIONAL, human-readable name of the property. It is OPTIONAL.
* `renderAs` is a String conveying what the link response should be rendered as. It is OPTIONAL.

## Field Object

```javascript
{
  type: String,
  name: String,
  valueOptions: Array,
  defaultValue: String,
  value: String,
  label: String,
  mapsTo: String,
  isRequired: Boolean
}
```

### Available Properties

* `type` is a String for the type of the field. This MAY be any value that is allowable in the HTML5 `<input>` tag `@type` property. It is REQUIRED.
* `name` is a String for the name of the field. It is REQUIRED.
* `options` is an Array of Option Objects. It is OPTIONAL.
* `defaultValue` is a String for the default value of this field. It is conveying the default value of the field, but may be able to change if there is no value specified. It is OPTIONAL.
* `value` is a String for the value of the field. If set, the field MUST be this value and cannot change. It is OPTIONAL. 
* `label` is a String for an OPTIONAL, human-readable name of the property. It is OPTIONAL.
* `mapsTo` is a String that says what this field maps to in either this entity or somewhere else in Hyperdescribe tree. It is OPTIONAL.
* `isRequired` is a Boolean to convey a field is required. By default, it is `false`. It is OPTIONAL.

## Field Option Object

```javascript
{
  value: String,
  label: String
}
```

### Available Properties

* `value` is a String for an option for the value of the field it's within. It is REQUIRED.
* `label` is a String for an OPTIONAL, human-readable name of the property. It is OPTIONAL.
