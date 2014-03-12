# Hyperdescribe

**Version**: 0.0.4

Hyperdescribe is a hypermedia message description format used to describe a hypermedia message. The purpose of this format is to provide a way to convert from one hypermedia format to another. In other words, once a message has been parsed into Hyperdescribe, it can then be used to build into other formats.

## Example

Starting with a document marked up with HTML+RDFa, we will then describe it with Hyperdescribe. Please note that this does not use all of the possible elements of Hyperdescribe.

```html
<!DOCTYPE html>
<html lang="en" prefix="schema: http://schema.org/ s: http://example.com/rels/">
  <head>
    <title>Top NBA Players of All Time</title>
    <link rel="alternate" type="application/hal+json" href="/top-3-players">
  </head>
  <body>
    <h1>Top NBA Players of All Time</h1>

    <div typeof="schema:Player" rel="s:player" resource="/players/jordan/michael">
      <h2>
        <span property="givenName">Michael</span>
        <span property="fullName">Jordan</span>
      </h2>
      <p><img src="/players/jordan/michael/game-photo.jpg" rel="s:game-photo" alt="Game Photo" /></p>
      <p>
        Check out Jordan's <a rel="s:stats" href="/players/jordan/michael/stats" title="Stats">stats</a>.
      </p>
    </div>
    <div typeof="schema:Player" rel="s:player" resource="/players/bird/larry">
      <h2>
        <span property="givenName">Larry</span>
        <span property="fullName">Bird</span>
      </h2>
      <p>
        Check out Bird's <a rel="s:stats" href="/players/bird/larry/stats" title="Stats">stats</a>.
      </p>
    </div>
    <div typeof="schema:Player" rel="s:player" resource="/players/russell/bill">
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

This HTML document could then be represented like this.

```json
{
  "prefixes":[
    {
      "name":"schema",
      "href":"http://schema.org/"
    },
    {
      "name":"s",
      "href":"http://example.com/rels/"
    }
  ],
  "meta":{
    "language":"en",
    "data":{
      "title":"Top NBA Players of All Time"
    },
    "links":[
      {
        "rel":["alternate"],
        "type":["application/hal+json"],
        "href":"/top-players"
      },
      {
        "rel":["self"],
        "href":"/top-players"
      }
    ]
  },
  "resources":[
    {
      "context": {
        "typeof":[ "schema:Person" ]
      },
      "rel": ["s:player"],
      "url": "/players/jordan/michael",
      "properties":[
        { "name": "givenName", "value": "Michael", "label": "First Name" },
        { "name": "familyName", "value": "Jordan", "label": "Last Name" }
      ],
      "links":[
        {
          "rel":["s:stats"],
          "href":"/players/jordan/michael/stats",
          "label": "Stats"
        },
        {
          "rel":["s:game-photo"],
          "href":"/players/jordan/michael/game-photo.jpg",
          "label": "Game Photo",
          "render": "image"
        }
      ]
    },
    {
      "context": {
        "vocab": "http://schema.org/",
        "typeof":[ "Person" ]
      },
      "rel":["s:player"],
      "url": "/players/bird/larry",
      "properties":[
        { "name": "givenName", "value": "Larry", "label": "First Name" },
        { "name": "familyName", "value": "Bird", "label": "Last Name" }
      ],
      "links":[
        {
          "rel":["s:stats"],
          "href":"/players/bird/larry/stats",
          "label": "Stats"
        }
      ]
    },
    {
      "context": {
        "typeof":[ "http://schema.org/Person" ]
      },
      "rel":["s:player"],
      "url": "/players/russell/bill",
      "properties":[
        { "name": "givenName", "value": "Bill", "label": "First Name" },
        { "name": "familyName", "value": "Russell", "label": "Last Name" }
      ],
      "links":[
        {
          "rel":["s:stats"],
          "href":"/players/russell/bill/stats",
          "label": "Stats"
        }
      ]
    }
  ],
  "links":[
    {
      "rel":["s:nba_worst"],
      "href":"/nba-worst",
      "label": "NBA Worst Players"
    }
  ],
  "actions":[
    {
      "name":"add-player",
      "title":"Add Player",
      "method":"POST",
      "url":"/top-players",
      "sendAs":["application/x-www-form-urlencoded"],
      "rel": ["s:add-player"],
      "fields":[
        {
          "name":"givenName",
          "type":"text",
          "label": "First Name"
        },
        {
          "name":"familyName",
          "type":"text",
          "label": "Last Name"
        }
      ]
    }
  ]
}
```

From this standard format, we can then convert to other hypermedia formats.

## Prefixes

For specifying prefixes, or curies, the `prefixes` property can be used, which MUST be an array. This property MUST exist in the root. Each item of the `prefixes` property MUST have a `name` and `href` property. The `prefixes` property is optional.

```json
{
  "prefixes":[
    {
      "name":"schema",
      "href":"http://schema.org/"
    },
    {
      "name":"s",
      "href":"http://example.com/rels/"
    }
  ]
}
```

### `name`

The property `name` is a string. This is required for `prefixes`.

### `href`

The property `href` is a string. This is required for `prefixes`.

## Meta

For specifying meta data about the message, the `meta` property can be used. This property MAY exist in the root or in a resource object. It is optional.

```json
{
  "meta":{
    "language":"en",
    "data":{
      "title":"Top NBA Players of All Time"
    },
    "links":[
      {
        "rel":["alternate"],
        "types":["application/hal+json"],
        "href":"/top-players"
      },
      {
        "rel":["self"],
        "href":"/top-players"
      }
    ]
  }
}
```

### `language`

The property `language` property is a string. This property is optional.

### `data`

The property  `data` property is an object of names and values for meta data. This property is optional.

### `links`

The property `links` property is a `links` array, as outlined below.

## Resources

For resources (or things), the `resources` property can be used, which is an array of resource objects. A resource is a thing that is being described in the message based on a particular vocabulary. It can exist in the root or in a resource object.

### Resource Object

A resource object is an array that MAY have a `vocab`, `typeof`, `properties`, `links`, `actions`, and `resources`. A resource object MUST be in the `resources` array, another resource object, or nested in the `properties` of a resource (see `properties` property below).

```json
{
  "prefixes":[
    {
      "name":"schema",
      "href":"http://schema.org/"
    },
    {
      "name":"s",
      "href":"http://example.com/rels/"
    }
  ],
  "resources":[
    {
      "context": {
        "typeof":[ "schema:Person" ]
      },
      "rel": ["s:player"],
      "url": "/players/jordan/michael",
      "properties":[
        { "name": "givenName", "value": "Michael", "label": "First Name" },
        { "name": "familyName", "value": "Jordan", "label": "Last Name" }
      ],
      "links":[
        {
          "rel":["s:stats"],
          "href":"/players/jordan/michael/stats",
          "label": "Stats"
        },
        {
          "rel":["s:game-photo"],
          "href":"/players/jordan/michael/game-photo.jpg",
          "label": "Game Photo",
          "render": "image"
        }
      ]
    },
    {
      "context": {
        "vocab": "http://schema.org/",
        "typeof":[ "Person" ]
      },
      "rel":["s:player"],
      "url": "/players/bird/larry",
      "properties":[
        { "name": "givenName", "value": "Larry", "label": "First Name" },
        { "name": "familyName", "value": "Bird", "label": "Last Name" }
      ],
      "links":[
        {
          "rel":["s:stats"],
          "href":"/players/bird/larry/stats",
          "label": "Stats"
        }
      ]
    }
  ]
}
```

#### `context`

The `context` property can be used to express the `vocab` and/or `typeof` of a a resource object. This is optional.

##### `vocab`

The `vocab` may be used to specify the vocabulary used for a specific resource. The `vocab` is optional.

For example, if the `vocab` is set to `http://schema.org/` and the `typeof` (described below) is set to `Person`, the vocab for the specific resource will be `http://schema.org/Person`. 

##### `typeof`

The `typeof` property is used to tell what type of resource the resource object is. The `typeof` property MUST be an array, which allows for multiple types to be specified. The `typeof` property is optional.

Used along with the `vocab` property, it can be something like `Person`. Used with a prefix, it can be something like `schema:Person`, where `schema` is a URL prefix (note the example in this section). In this case, the `typeof` with the prefix would be `http://schema.org/Person`.

#### `id`

The `id` property is for uniquely identifying a resource in the document. This MUST be unique for both resources and actions. It is optional.

#### `name`

The `name` property is for identifying a resource in the document. This does not have to be unique. It is optional.

#### `class`

The `class` property is an array of strings. This describes the nature of the resource. It is optional.

#### `rel`

The `rel` property specifies the link relations for the resource object. This is an array and is optional.

#### `url`

The `url` property specifies the URL for the resource object. This is optional.

#### `properties`

The `properties` array contains Property objects.

##### Property Object

A Property Object contains information about each property for a resource.

###### `name`

The `name` is the name of the property. This is required. It can be a number, string, boolean, or null.

###### `value`

The `value` is the value of the property. This is required. It can be a number, string, boolean, or null.

###### `label`

The `label` is a human-readable name for the property. This is optional. It can be a number, string, boolean, or null.

#### `links`

The property `links` property is a `links` array, as outlined below.

#### `actions`

The property `actions` property is a `actions` array, as outlined below.

#### `resources`

For embedding resources, a `resources` array can be included in a resource object. 

#### `property`

If a resource is a property for another resource, `property` can be used to specify this relationship. This MAY only exist on a nested resource. This is optional.

### Links

For specifying links, the `links` property MAY be used in the root, in `meta`, and in a resource object. The `links` property is optional in all of those locations. The `links` property is an array of link objects.

### Link object

A link object is an object of name and values pairs specifying data about a specific link. The link object MUST exist in a `links` array. 

```json
{
  "links":[
    {
      "rel": ["s:nba_worst"],
      "href":"/nba-worst",
      "label": "NBA Worst Players"
    }
  ]
}
```

### `rel`

The `rel` property specifies the link relation for the link. This can be a full URL (e.g. http://example.org/rels/stats) or a prefixed links (e.g. s:stats). The `rel` property is an array and is required.

### `href`

The `href` property specifies the URL for the resource being linked. The `href` property is required.

### `types`

The `types` property specifies available the media types of the resource being linked. This is an array and is optional.

### `label`

The `label` property is used to give a human-readable label to the field. This is optional.

### `transclude`

The `transclude` property is a boolean field. If it is not included, it is false, unless `render` is set (see below). If it is true, it is saying that the link should be included as part of the current document, much like an `<img>` tag in HTML is saying to include an image in the current representation.

### `render`

The `render` property conveys how the link should be transcluded. If render is defined, it is assumed that the link should be transcluded no matter if `transclude` is set or not.

### `template`

The `template` property can be used to show a link is a templated link.

### `property`

The `property` property allows for specifying a property name in relation to the resource the link is within. This field is optional.

## Actions

For specifying actions that can be performed, the `actions` MAY be used. The `actions` property MAY exist in the root or in a resource object. The `actions` property MUST contain an array or action objects.

### Action Object

An action object is an object of name and values pairs specifying data about a specific action. The action object MUST exist in an `actions` array.

The action object MUST contain the `name`, `method`, `href`, and `fields` properties.

The action object MAY contain the `title` and `type` properties.

```json
{
  "actions":[
    {
      "name":"add-player",
      "title":"Add Player",
      "method":"POST",
      "href":"/top-players",
      "sendAs":["application/x-www-form-urlencoded"],
      "rel": ["s:add-player"],
      "fields":[
        {
          "name":"givenName",
          "type":"text",
          "label": "First Name"
        },
        {
          "name":"familyName",
          "type":"text",
          "label": "Last Name"
        }
      ]
    }
  ]
}
```

#### `id`

The `id` property is for uniquely identifying a resource in the document. This MUST be unique for both resources and actions. It is optional.

#### `name`

The `name` is a property to specify a name for an action. This is required.

#### `class`

The `class` property is an array of strings. This describes the nature of the action. It is optional.

#### `name`

The `name` is a property to specify a unique name for an action. This is required.

#### `method`

The `method` property is used to specify the transport method used for completing the action. If HTTP is the protocol, this would include HTTP verbs, such as GET, POST, PUT, etc. This is required.

#### `url`

The `url` property is for specifying the URL for the action. This is required.

#### `sendAs`

The `sendAs` property specifies the media types the server accepts for this action. This is optional, and if it is not present, `application/x-www-form-urlencoded` is assumed.

#### `rel`

The `rel` property specifies one or more link relations for the action. This is an array and is optional.

#### `fields`

The `fields` property is an array of field objects. This is required.

##### Field Object

###### `name`

The `name` property is for specifying the name of the field. This is required.

###### `type`

The `type` property specifies the type of the field, which can be any of the types available for HTML5 inputs. This is required.

###### `value`

The `value` property is used for specifying the default value of the field. This is optional.

###### `label`

The `label` property is used to give a human-readable label to the field. This is optional.

###### `mapsTo`

The `mapsTo` property is used to to tell what property a particular field maps to, if any. For example, if there is a property called `email` and a action field called `e` that can take an email address from the resource, `mapsTo` can specify this relationship. This is optional.

#### `title`

The `title` attribute specifies the title of the action. This is optional.









