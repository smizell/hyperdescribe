# Hyperdescribe

**Version**: 0.0.1

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

    <div typeof="schema:Player">
      <h2>
        <span property="givenName">Michael</span>
        <span property="fullName">Jordan</span>
      </h2>
      <p>
        Check out Jordan's <a rel="s:stats" href="/players/jordan/michael/stats">stats</a>.
      </p>
    </div>
    <div typeof="schema:Player">
      <h2>
        <span property="givenName">Larry</span>
        <span property="fullName">Bird</span>
      </h2>
      <p>
        Check out Bird's <a rel="s:stats" href="/players/bird/larry/stats">stats</a>.
      </p>
    </div>
    <div typeof="schema:Player">
      <h2>
        <span property="givenName">Bill</span>
        <span property="fullName">Russell</span>
      </h2>
      <p>
        Check out Russell's <a rel="s:stats" href="/players/russell/bill/stats">stats</a>.
      </p>
    </div>

    <p>
      Also check out <a rel="s:nba_worst" href="/nba-worst">NBA Worst Players</a>.
    </p>

    <form name="add-player" method="POST" action="/top-players">
      <input type="text" name="givenName" />
      <input type="text" name="familyName" />
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
        "rel":"alternate",
        "type":"application/hal+json",
        "href":"/top-players"
      },
      {
        "rel":"self",
        "href":"/top-players"
      }
    ]
  },
  "resources":[
    {
      "typeof":[ "schema:Person" ],
      "properties":{
        "givenName":"Michael",
        "familyName":"Jordan"
      },
      "links":[
        {
          "rel":"s:stats",
          "href":"/players/jordan/michael/stats"
        }
      ]
    },
    {
      "vocab": "http://schema.org/",
      "typeof":[ "Person" ],
      "properties":{
        "givenName":"Larry",
        "familyName":"Bird"
      },
      "links":[
        {
          "rel":"s:stats",
          "href":"/players/bird/larry/stats"
        }
      ]
    },
    {
      "typeof":[ "http://schema.org/Person" ],
      "properties":{
        "givenName":"Bill",
        "familyName":"Russell"
      },
      "links":[
        {
          "rel":"s:stats",
          "href":"/players/russell/bill/stats"
        }
      ]
    }
  ],
  "links":[
    {
      "rel":"s:nba_worst",
      "href":"/nba-worst"
    }
  ],
  "actions":[
    {
      "name":"add-player",
      "title":"Add Player",
      "method":"POST",
      "href":"/top-players",
      "type":"application/x-www-form-urlencoded",
      "fields":[
        {
          "name":"givenName",
          "type":"text"
        },
        {
          "name":"familyName",
          "type":"text"
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

For specifying meta data about the message, the `meta` property can be used. This property MUST exist in the root. It is optional.

```json
{
  "meta":{
    "language":"en",
    "data":{
      "title":"Top NBA Players of All Time"
    },
    "links":[
      {
        "rel":"alternate",
        "type":"application/hal+json",
        "href":"/top-players"
      },
      {
        "rel":"self",
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
      "typeof":[ "schema:Person" ],
      "properties":{
        "givenName":"Michael",
        "familyName":"Jordan"
      },
      "links":[
        {
          "rel":"s:stats",
          "href":"/players/jordan/michael/stats"
        }
      ]
    },
    {
      "vocab": "http://schema.org/",
      "typeof":[ "Person" ],
      "properties":{
        "givenName":"Larry",
        "familyName":"Bird"
      },
      "links":[
        {
          "rel":"s:stats",
          "href":"/players/bird/larry/stats"
        }
      ]
    }
  ]
}
```

#### `vocab`

The `vocab` may be used to specify the vocabulary used for a specific resource. The `vocab` is optional.

For example, if the `vocab` is set to `http://schema.org/` and the `typeof` (described below) is set to `Person`, the vocab for the specific resource will be `http://schema.org/Person`. 

#### `typeof`

The `typeof` property is used to tell what type of resource the resource object is. The `typeof` property MUST be an array, which allows for multiple types to be specified. The `typeof` property is optional.

Used along with the `vocab` property, it can be something like `Person`. Used with a prefix, it can be something like `schema:Person`, where `schema` is a URL prefix (note the example in this section). In this case, the `typeof` with the prefix would be `http://schema.org/Person`.

#### `properties`

The `properties` property is an object that contains names and values representing the property of the resource object. The values for each property can be of any type, such as another object or an array. This allows for flexibility.

#### `links`

The property `links` property is a `links` array, as outlined below.

#### `actions`

The property `actions` property is a `actions` array, as outlined below.

#### `resources`

For embedding resources, a `resources` array can be included in a resource object. 

### Links

For specifying links, the `links` property MAY be used in the root, in `meta`, and in a resource object. The `links` property is optional in all of those locations. The `links` property is an array of link objects.

### Link object

A link object is an object of name and values pairs specifying data about a specific link. The link object MUST exist in a `links` array. 

```json
{
  "links":[
    {
      "rel":"s:nba_worst",
      "href":"/nba-worst"
    }
  ]
}
```

### `rel`

The `rel` property specifies the link relation for the link. This can be a full URL (e.g. http://example.org/rels/stats) or a prefixed links (e.g. s:stats). The `rel` property is required.

### `href`

The `href` property specifies the URL for the resource being linked. The `href` property is required.

### `type`

The `type` property specifies the media type of the resource being linked. This is optional.

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
      "type":"application/x-www-form-urlencoded",
      "fields":[
        {
          "name":"givenName",
          "type":"text"
        },
        {
          "name":"familyName",
          "type":"text"
        }
      ]
    }
  ]
}
```

#### `name`

The `name` is a property to specify a unique name for an action. This is required.

#### `method`

The `method` property is used to specify the transport method used for completing the action. If HTTP is the protocol, this would include HTTP verbs, such as GET, POST, PUT, etc. This is required.

#### `href`

The `href` property is for specifying the URL for the action. This is required.

#### `fields`

The `fields` property is an array of field objects. This is required.

##### Field Object

###### `name`

The `name` property is for specifying the name of the field. This is required.

###### `type`

The `type` property specifies the type of the field, which can be any of the types available for HTML5 inputs. This is required.

###### `value`

The `value` property is used for specifying the default value of the field. This is optional.

#### `title`

The `title` attribute specifies the title of the action. This is optional.

#### `type`

The `type` property specifies the media type to be used for the action. This is optional.









