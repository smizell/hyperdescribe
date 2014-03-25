Describing a Siren Document
============================

## Original Siren Document

This is pulled from the [Siren spec](https://github.com/kevinswiber/siren).

```json
{
  "class": [ "order" ],
  "properties": { 
      "orderNumber": 42, 
      "itemCount": 3,
      "status": "pending"
  },
  "entities": [
    { 
      "class": [ "items", "collection" ], 
      "rel": [ "http://x.io/rels/order-items" ], 
      "href": "http://api.x.io/orders/42/items"
    },
    {
      "class": [ "info", "customer" ],
      "rel": [ "http://x.io/rels/customer" ], 
      "properties": {
        "customerId": "pj123",
        "name": "Peter Joseph"
      },
      "links": [
        { "rel": [ "self" ], "href": "http://api.x.io/customers/pj123" }
      ]
    }
  ],
  "actions": [
    {
      "name": "add-item",
      "title": "Add Item",
      "method": "POST",
      "href": "http://api.x.io/orders/42/items",
      "type": "application/x-www-form-urlencoded",
      "fields": [
        { "name": "orderNumber", "type": "hidden", "value": "42" },
        { "name": "productCode", "type": "text" },
        { "name": "quantity", "type": "number" }
      ]
    }
  ],
  "links": [
    { "rel": [ "self" ], "href": "http://api.x.io/orders/42" },
    { "rel": [ "previous" ], "href": "http://api.x.io/orders/41" },
    { "rel": [ "next" ], "href": "http://api.x.io/orders/43" }
  ]
}
```

## Described with Hyperdescribe

```json
{
  "hyperdescribe": {
    "version": "0.1.0",
    "classes": [ "order" ],
    "url": "http://api.x.io/orders/42",
    "content": {
      "properties": [
        { "name": "orderNumber", "type": "integer", "value": "42" },
        { "name": "itemCount", "type": "integer", "value": "3" },
        { "name": "status", "value": "pending" }
      ],
      "entities": [
        {
          "classes": [ "items", "collection" ], 
          "rels": [ "http://x.io/rels/order-items" ], 
          "url": "http://api.x.io/orders/42/items"
        },
        {
          "classes": [ "info", "customer" ],
          "rels": [ "http://x.io/rels/customer" ], 
          "url": "http://api.x.io/customers/pj123",
          "content": {
            "properties": [
              { "name": "customerId", "value": "pj123" },
              { "name": "name", "value": "Peter Joseph" }
            ]
          }
        }
      ],
      "transitions": [
        { "rels": [ "previous" ], "url": "http://api.x.io/orders/41" },
        { "rels": [ "next" ], "url": "http://api.x.io/orders/43" },
        {
          "classes": [ "add-item" ],
          "label": "Add Item",
          "method": "POST",
          "url": "http://api.x.io/orders/42/items",
          "requestType": ["application/x-www-form-urlencoded"],
          "fields": [
            { "name": "orderNumber", "type": "hidden", "value": "42" },
            { "name": "productCode", "type": "text" },
            { "name": "quantity", "type": "number" }
          ]
        }
      ]
    }
  }
}
```
