# Hyperdescribe

**Version**: 0.0.0

Hyperdescribe is a hypermedia message description format used to describe a hypermedia message. The purpose of this format is to provide a way to convert from one hypermedia format to another. In other words, once a message has been parsed into Hyperdescribe, it can then be used to build into other formats.

## Example

Starting with a document marked up with HTML+RDFa, we will then describe it with Hyperdescribe. Please note that this does example does not use all of the possible elements of Hyperdescribe.

```html
<!DOCTYPE html>
<html lang="en" prefix="schema: http://schema.org/">
  <head>
    <title>Top 3 NBA Players of All Time</title>
    <link rel="alternate" type="application/hal+json" href="/top-3-players">
  </head>
  <body>
    <h1>Top 3 Players of All Time</h1>

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
  </body>
</html>
```

This HTML document could then be represented like this.

```json
{
  "prefixes": [{
    "name": "schema",
    "href": "http://schema.org/"
  },
  {
    "name": "s",
    "href": "http://example.com/rels/"
  }],
  "meta": {
    "language": "en",
    "data": {
      "title": "Top 3 NBA Players of All Time"
    },
    "links": [{
      "rel": "alternate",
      "type": "application/hal+json",
      "href": "/top-3-players"
    },
    {
      "rel": "self",
      "href": "/top-3-players"
    }]
  },
  "resources": [{
    "typeof": "schema:Person",
    "properties": {
      "givenName": "Michael",
      "familyName": "Jordan"
    },
    links: [{
      "rel": "s:stats",
      "href": "/players/jordan/michael/stats"
    }]
  },
  {
    "typeof": "schema:Person",
    "properties": {
      "givenName": "Larry",
      "familyName": "Bird"
    },
    links: [{
      "rel": "s:stats",
      "href": "/players/bird/larry/stats"
    }]
  },
  {
    "typeof": "schema:Person",
    "properties": {
      "givenName": "Bill",
      "familyName": "Russell"
    },
    links: [{
      "rel": "s:stats",
      "href": "/players/russell/bill/stats"
    }]
  }]
}
```

From this standard format, we can then convert to other hypermedia formats.

## Elements

### Prefixes

For specifying prefixes, or curies, the `prefixes` property can be used, which MUST be an array (OPTIONAL). This property MUST exist in the root. Each item of the `prefixes` property MUST be an object with a `name` (REQUIRED), which is a string and `href` property, which is a string.

```json
{
  "prefixes": [{
    "name": "schema",
    "href": "http://schema.org/"
  }]
}
```

### Meta

For specifyig meta data about the message, the `meta` property can be used (OPTIONAL). This property MUST exist in the root. The `meta` property MUST be an object that includes a `language` property (OPTIONAL), which is a string, an `data` property (OPTIONAL), which is an object, and a `links` property (OPTIONAL), which is an array. The `data` property is an object containing names and values, which are both strings.

```json
{
  "meta": {
    "language": "en",
    "data": {
      "title": "Top 3 NBA Players of All Time"
    },
    "links": [{
      "rel": "alternate",
      "type": "application/hal+json",
      "href": "/top-3-players"
    },
    {
      "rel": "self",
      "href": "/top-3-players"
    }]
  }
}
```

### Resource

A resource is a thing that is being described in the message based on a particular vocabulary.

### Links

For specifying links, the `links` array MAY be used in the root, in `meta`, and in `resource` objects. The `links` property is OPTIONAL in all of those locations.

### Actions

For specifying actions that can be performed, the `actions` array may be used. The `actions` property MAY exist in the root or in `resources`.









