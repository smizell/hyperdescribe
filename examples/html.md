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
  "hyperdescribe": {
    "version": "0.1.0",
    "entities": [
      {
        "classes": [ "player" ],
        "rels": [ "s:player" ],
        "url": "/players/jordan/michael",
        "context": {
          "typesOf": [ "schema:Player" ]
        },
        "content": {
          "properties": [
            { "name": "givenName", "value": "Michael" },
            { "name": "familyName", "value": "Jordan" }
          ],
          "transitions": [
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
        "url": "/players/bird/larry",
        "context": {
          "typesOf": [ "schema:Player" ]
        },
        "content": {
          "properties": [
            { "name": "givenName", "value": "Larry" },
            { "name": "familyName", "value": "Bird" }
          ],
          "transitions": [
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
        "url": "/players/russell/bill",
        "context": {
          "typesOf": [ "schema:Player" ]
        },
        "content": {
          "properties": [
            { "name": "givenName", "value": "Bill" },
            { "name": "familyName", "value": "Russell" }
          ],
          "transitions": [
            {
              "rels": [ "s:stats" ],
              "url": "/players/russell/bill/stats",
              "label": "Stats"
            }
          ]
        }
      }
    ],
    "transitions": [
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