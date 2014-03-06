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
  "prefixes": [
    {
      "name":"ea",
      "href":"http://example.com/docs/rels/"
    }
  ],
  "links":[
    {
      "rel":"next",
      "href":"/orders"
    },
    {
      "rel":"ea:admin",
      "href":"/admins/2",
      "label": "Fred"
    },
    {
      "rel":"ea:admin",
      "href":"/admins/5",
      "label": "Kate"
    }
  ],
  "resources": [
    {
      "url": "/orders",
      "properties": {
        "currentlyProcessing": 14,
        "shippedToday": 20
      },
      "resources": [
        {
          "rel": "ea:order",
          "url": "/orders/123",
          "properties": {
            "total": 30.00,
            "currency": "USD",
            "status": "shipped"
          },
          "links": [
            {
              "rel":"ea:basket",
              "href":"/baskets/98712"
            },
            {
              "rel":"ea:customer",
              "href":"/customers/7809"
            }
          ]
        },
        {
          "rel": "ea:order",
          "url": "/orders/124",
          "properties": {
            "total": 20.00,
            "currency": "USD",
            "status": "processing"
          },
          "links": [
            {
              "rel":"ea:basket",
              "href":"/baskets/97213"
            },
            {
              "rel":"ea:customer",
              "href":"/customers/12369"
            }
          ]
        }
      ]
    }
  ],
  "actions": [
    {
      "name":"find",
      "method":"GET",
      "href":"/orders",
      "fields":[
        {
          "name":"id",
          "value": ""
        }
      ]
    }
  ]
}
```