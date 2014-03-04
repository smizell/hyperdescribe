Describing an HTML+RDFa Document
================================

## Original HTML+RDFa

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
      <p>
        Check out Jordan's <a rel="s:stats" href="/players/jordan/michael/stats">stats</a>.
      </p>
    </div>
    <div typeof="schema:Player" rel="s:player" resource="/players/bird/larry">
      <h2>
        <span property="givenName">Larry</span>
        <span property="fullName">Bird</span>
      </h2>
      <p>
        Check out Bird's <a rel="s:stats" href="/players/bird/larry/stats">stats</a>.
      </p>
    </div>
    <div typeof="schema:Player" rel="s:player" resource="/players/russell/bill">
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

## Described with Hyperdescribe

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
      "context": {
        "typeof":[ "schema:Person" ]
      },
      "rel": "s:player",
      "url": "/players/jordan/michael",
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
      "context": {
        "vocab": "http://schema.org/",
        "typeof":[ "Person" ]
      },
      "rel": "s:player",
      "url": "/players/bird/larry",
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
      "context": {
        "typeof":[ "http://schema.org/Person" ]
      },
      "rel": "s:player",
      "url": "/players/russell/bill",
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

## YAML Format for Hyperdescribe

```yml
---
  prefixes: 
    - 
      name: "schema"
      href: "http://schema.org/"
    - 
      name: "s"
      href: "http://example.com/rels/"
  meta: 
    language: "en"
    data: 
      title: "Top NBA Players of All Time"
    links: 
      - 
        rel: "alternate"
        type: "application/hal+json"
        href: "/top-players"
      - 
        rel: "self"
        href: "/top-players"
  resources: 
    - 
      context: 
        typeof: 
          - "schema:Person"
      rel: "s:player"
      url: "/players/jordan/michael"
      properties: 
        givenName: "Michael"
        familyName: "Jordan"
      links: 
        - 
          rel: "s:stats"
          href: "/players/jordan/michael/stats"
    - 
      context: 
        vocab: "http://schema.org/"
        typeof: 
          - "Person"
      rel: "s:player"
      url: "/players/bird/larry"
      properties: 
        givenName: "Larry"
        familyName: "Bird"
      links: 
        - 
          rel: "s:stats"
          href: "/players/bird/larry/stats"
    - 
      context: 
        typeof: 
          - "http://schema.org/Person"
      rel: "s:player"
      url: "/players/russell/bill"
      properties: 
        givenName: "Bill"
        familyName: "Russell"
      links: 
        - 
          rel: "s:stats"
          href: "/players/russell/bill/stats"
  links: 
    - 
      rel: "s:nba_worst"
      href: "/nba-worst"
  actions: 
    - 
      name: "add-player"
      title: "Add Player"
      method: "POST"
      href: "/top-players"
      type: "application/x-www-form-urlencoded"
      fields: 
        - 
          name: "givenName"
          type: "text"
        - 
          name: "familyName"
          type: "text"

```