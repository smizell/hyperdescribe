# Hyperdescribe

**Version**: 0.1.0

Hyperdescribe is a hypermedia message description format used to describe the different components of a hypermedia message. The goal is to provide a standardized way to describe a message in order to:

1. Convert to other media types
2. Allow the server to respond with multiple media types
3. Allow the client to understand multiple media types

The point of this is to create servers and clients that describe and reference hypermedia components rather than work directly with the media types.

## Purpose

The purpose of this format is to make hypermedia easier and to help increaes adoption. It is also trying to standardize the way we already think about hypermedia messages. When building a client or server that works with a hypermedia type, there are usually four main components:

1. Entities - Anything that has identity (e.g. Person)
2. Properites - Properties about that entity (e.g. Name)
3. Links - Links to related entities
4. Actions - Actions related to this entity

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
  rels: Array,
  url: String,
  context: Object,
  property: String,
  content: Object
}
```

### Available Properties

* `version` is a String of the Hyperdescribe version of this entity object
* `prefixes` is a Prefix Object. It is OPTIONAL.
* `id` is a String for documen-unique identifier. Either `id` or one `classes` MUST be set, but MAY NOT be both.
* `classes` is an Array of Strings of non-unique identifiers. Either `id` or one `classes` MUST be set, but MAY NOT be both.
* `rels` is an Array of available link relations for this entity. It is OPTIONAL.
* `url` is a String for the `URL` of this entity. It SHOULD be consider a `self` link relation, along with any other link relations specified in the `rels` array. It is OPTIONAL.
* `context` is a Context Object. It is OPTIONAL.
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
  actions: Array,
  entities: Array
}
```

### Available Properties

* `meta` is a Meta Object. It is OPTIONAL.
* `context` is a Context Object. It is OPTIONAL.
* `properties` is an Array of Property Objects. It is OPTIONAL.
* `links` is an Array of Transition Objects, which MUST all be safe, idempotent links. It is OPTIONAL.
* `actions` is an Array of Transition Objects, which MUST all be non-safe, idempotent links. It is OPTIONAL.
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
* `code` is an Array of Transition Objects, which MUST all be safe, idempotent links. It is OPTIONAL.
* `links` is an Array of Transition Objects, which MUST all be safe, idempotent links. It is OPTIONAL.

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

## Transition Object

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

## Example

The example below shows an HTML+RDFa representation and the Hyperdescribe document for this message below it.

### HTML+RDFa

```html
<!DOCTYPE html>
<html lang="en" prefix="schema: http://schema.org/ s: http://example.com/rels/">
  <head>
    <title>Top NBA Players of All Time</title>
    <link rel="alternate" type="application/hal+json" href="/top-3-players">
  </head>
  <body>
    <h1>Top NBA Players of All Time</h1>

    <div class="player" typeof="schema:Player" rel="s:player" resource="/players/jordan/michael">
      <h2>
        <span property="givenName">Michael</span>
        <span property="fullName">Jordan</span>
      </h2>
      <p><img src="/players/jordan/michael/game-photo.jpg" rel="s:game-photo" alt="Game Photo" /></p>
      <p>
        Check out Jordan's <a rel="s:stats" href="/players/jordan/michael/stats" title="Stats">stats</a>.
      </p>
    </div>
    <div class="player" typeof="schema:Player" rel="s:player" resource="/players/bird/larry">
      <h2>
        <span property="givenName">Larry</span>
        <span property="fullName">Bird</span>
      </h2>
      <p>
        Check out Bird's <a rel="s:stats" href="/players/bird/larry/stats" title="Stats">stats</a>.
      </p>
    </div>
    <div class="player" typeof="schema:Player" rel="s:player" resource="/players/russell/bill">
      <h2>
        <span property="givenName">Bill</span>
        <span property="fullName">Russell</span>
      </h2>
      <p>
        Check out Russell's <a rel="s:stats" href="/players/russell/bill/stats" title="Stats">stats</a>.
      </p>
    </div>

    <p>
      Also check out <a rel="s:nba_worst" href="/nba-worst" title="NBA Worst Players">NBA Worst Players</a>.
    </p>

    <form name="add-player" method="POST" action="/top-players" rel="s:add_player">
      <input type="text" name="givenName" placeholder="First Name" />
      <input type="text" name="familyName" placeholder="Last Name" />
    </form>
  </body>
</html>
```

### Described in Hyperdescribe

```json
{
  "hyperdescribe": {
    "version": "0.1.0",
    "entities": [
      {
        "classes": [ "player" ],
        "rels": [ "s:player" ],
        "url": "/players/jordan/michael"
        "context": {
          "typesOf": [ "schema:Player" ]
        },
        "content": {
          "properties": [
            { "name": "givenName", "value": "Michael" },
            { "name": "familyName", "value": "Jordan" }
          ],
          "links": [
            {
              "rels": [ "s:stats" ],
              "url": "/players/jordan/michael/stats",
              "label": "Stats"
            },
            {
              "rels": [ "s:game-photo" ],
              "url": "/players/jordan/michael/game-photo.jpg",
              "label": "Game Photo",
              "render": "image"
            }
          ]
        }
      },
      {
        "classes": [ "player" ],
        "rels": [ "s:player" ],
        "url": "/players/bird/larry"
        "context": {
          "typesOf": [ "schema:Player" ]
        },
        "content": {
          "properties": [
            { "name": "givenName", "value": "Larry" },
            { "name": "familyName", "value": "Bird" }
          ],
          "links": [
            {
              "rels": [ "s:stats" ],
              "url": "/players/bird/larry/stats",
              "label": "Stats"
            }
          ]
        }
      },
      {
        "classes": [ "player" ],
        "rels": [ "s:player" ],
        "url": "/players/russell/bill"
        "context": {
          "typesOf": [ "schema:Player" ]
        },
        "content": {
          "properties": [
            { "name": "givenName", "value": "Bill" },
            { "name": "familyName", "value": "Russell" }
          ],
          "links": [
            {
              "rels": [ "s:stats" ],
              "url": "/players/russell/bill/stats",
              "label": "Stats"
            }
          ]
        }
      }
    ],
    "actions": [
      {
        "classes": [ "add-player" ],
        "label": "Add Player",
        "method": "POST",
        "url": "/top-players",
        "requestTypes": [
          "application/x-www-form-urlencoded"
        ],
        "rels": [ "s:add-player" ],
        "fields": [
          {
            "name": "givenName",
            "type": "text",
            "label": "First Name"
          },
          {
            "name": "familyName",
            "type": "text",
            "label": "Last Name"
          }
        ]
      }
    ]
  }
}
```
