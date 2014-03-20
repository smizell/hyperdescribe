Describing an HAL+JSON Document
===============================

## Original HAL+JSON Document

This is pulled from the [HAL+JSON spec](http://stateless.co/hal_specification.html).

```json
{
    "_links": {
        "self": { "href": "/orders" },
        "curies": [{ "name": "ea", "href": "http://example.com/docs/rels/{rel}", "templated": true }],
        "next": { "href": "/orders?page=2" },
        "ea:find": {
            "href": "/orders{?id}",
            "templated": true
        },
        "ea:admin": [{
            "href": "/admins/2",
            "title": "Fred"
        }, {
            "href": "/admins/5",
            "title": "Kate"
        }]
    },
    "currentlyProcessing": 14,
    "shippedToday": 20,
    "_embedded": {
        "ea:order": [{
            "_links": {
                "self": { "href": "/orders/123" },
                "ea:basket": { "href": "/baskets/98712" },
                "ea:customer": { "href": "/customers/7809" }
            },
            "total": 30.00,
            "currency": "USD",
            "status": "shipped"
        }, {
            "_links": {
                "self": { "href": "/orders/124" },
                "ea:basket": { "href": "/baskets/97213" },
                "ea:customer": { "href": "/customers/12369" }
            },
            "total": 20.00,
            "currency": "USD",
            "status": "processing"
        }]
    }
}
```

## Described with Hyperdescribe

```json
{
  "hyperdescribe": {
    "version": "0.1.0",
    "prefixes": [
      {
        "name":"ea",
        "href":"http://example.com/docs/rels/"
      }
    ],
    "content": {
      "links":[
        {
          "rel": ["self"],
          "url": "/orders",
        },
        {
          "rels":["next"],
          "url":"/orders?page=2"
        },
        {
          "rels":["ea:admin"],
          "url":"/admins/2",
          "label": "Fred"
        },
        {
          "rels":["ea:admin"],
          "url":"/admins/5",
          "label": "Kate"
        },
        {
          "rels": ["ea:find"],
          "url": "/orders{?id}",
          "templated": true
        }
      ],
      "properties": [
        { "name": "currentlyProcessing", "type": "integer", "value": "14" },
        { "name": "shippedToday", "type": "integer", "value": "20" }
      ],
      entities: [
        {
          "rels": ["ea:order"],
          "url": "/orders/123",
          "content": {
            "properties": [
              { "name": "total", "type": "float", "value": 30.00 },
              { "name": "currency", "value": "USD" },
              { "name": "status", "value": "shipped" }
            ],
            "links": [
              {
                "rels":["ea:basket"],
                "href":"/baskets/98712"
              },
              {
                "rels":["ea:customer"],
                "href":"/customers/7809"
              }
            ]
          }
        },
        {
          "rels": ["ea:order"],
          "url": "/orders/124",
          "content":
            "properties": [
              { "name": "total", "type": "float", "value": "20.00" },
              { "name": "currency", "value": "USD" },
              { "name": "status", "value": "processing" }
            ],
            "links": [
              {
                "rels":["ea:basket"],
                "href":"/baskets/97213"
              },
              {
                "rels":["ea:customer"],
                "href":"/customers/12369"
              }
            ]
          }
        }
      ]
    }
  }
}
```