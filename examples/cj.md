Describing an Collection+JSON Document
======================================

## Original Collection+JSON Document

This is pulled from the [Collection+JSON documentation](http://amundsen.com/media-types/collection/examples/).

```json
{ "collection" :
  {
    "version" : "1.0",
    "href" : "http://example.org/friends/",
    
    "links" : [
      {"rel" : "feed", "href" : "http://example.org/friends/rss"}
    ],
    
    "items" : [
      {
        "href" : "http://example.org/friends/jdoe",
        "data" : [
          {"name" : "full-name", "value" : "J. Doe", "prompt" : "Full Name"},
          {"name" : "email", "value" : "jdoe@example.org", "prompt" : "Email"}
        ],
        "links" : [
          {"rel" : "blog", "href" : "http://examples.org/blogs/jdoe", "prompt" : "Blog"},
          {"rel" : "avatar", "href" : "http://examples.org/images/jdoe", "prompt" : "Avatar", "render" : "image"}
        ]
      },
      
      {
        "href" : "http://example.org/friends/msmith",
        "data" : [
          {"name" : "full-name", "value" : "M. Smith", "prompt" : "Full Name"},
          {"name" : "email", "value" : "msmith@example.org", "prompt" : "Email"}
        ],
        "links" : [
          {"rel" : "blog", "href" : "http://examples.org/blogs/msmith", "prompt" : "Blog"},
          {"rel" : "avatar", "href" : "http://examples.org/images/msmith", "prompt" : "Avatar", "render" : "image"}
        ]
      },
      
      {
        "href" : "http://example.org/friends/rwilliams",
        "data" : [
          {"name" : "full-name", "value" : "R. Williams", "prompt" : "Full Name"},
          {"name" : "email", "value" : "rwilliams@example.org", "prompt" : "Email"}
        ],
        "links" : [
          {"rel" : "blog", "href" : "http://examples.org/blogs/rwilliams", "prompt" : "Blog"},
          {"rel" : "avatar", "href" : "http://examples.org/images/rwilliams", "prompt" : "Avatar", "render" : "image"}
        ]
      }      
    ],
    
    "queries" : [
      {"rel" : "search", "href" : "http://example.org/friends/search", "prompt" : "Search",
        "data" : [
          {"name" : "search", "value" : ""}
        ]
      }
    ],
    
    "template" : {
      "data" : [
        {"name" : "full-name", "value" : "", "prompt" : "Full Name"},
        {"name" : "email", "value" : "", "prompt" : "Email"},
        {"name" : "blog", "value" : "", "prompt" : "Blog"},
        {"name" : "avatar", "value" : "", "prompt" : "Avatar"}
        
      ]
    }
  } 
}
```

## Described with Hyperdescribe

```json
{
  "meta":{
    "links":[
      {
        "rel":["self"],
        "href":"http://example.org/friends/"
      },
      {
        "rel":["feed"],
        "href":"http://example.org/friends/rss"
      }
    ]
  },
  "resources": [
    {
      "url": "http://example.org/friends/jdoe",
      "rel": ["item"],
      "properties": [
        { "name": "full-name", "value": "J. Doe", "label": "Full Name" },
        { "name": "email", "value": "jdoe@example.org", "label": "Email" }
      ],
      "links" : [
        {"rel" : ["blog"], "href" : "http://examples.org/blogs/jdoe", "label" : "Blog"},
        {"rel" : ["avatar"], "href" : "http://examples.org/images/jdoe", "label" : "Avatar", "render": "image"}
      ]
    },
    {
      "url": "http://example.org/friends/msmith",
      "rel": ["item"],
      "labels": {
        "full-name": "Full Name",
        "email": "Email"
      },
      "properties": [
        { "name": "full-name", "value": "M. Smith", "label": "Full Name" },
        { "name": "email", "value": "msmith@example.org", "label": "Email" }
      ],
      "links" : [
        {"rel" : ["blog"], "href" : "http://examples.org/blogs/msmith", "label" : "Blog"},
        {"rel" : ["avatar"], "href" : "http://examples.org/images/msmith", "label" : "Avatar", "render": "image"}
      ]
    },
    {
      "url": "http://example.org/friends/rwilliams",
      "rel": ["item"],
      "labels": {
        "full-name": "Full Name",
        "email": "Email"
      },
      "properties": [
        { "name": "full-name", "value": "R. Williams", "label": "Full Name" },
        { "name": "email", "value": "rwilliams@example.org", "label": "Email" }
      ],
      "links" : [
        {"rel" : ["blog"], "href" : "http://examples.org/blogs/rwilliams", "label" : "Blog"},
        {"rel" : ["avatar"], "href" : "http://examples.org/images/rwilliams", "label" : "Avatar", "render": "image"}
      ]
    }
  ],
  "actions": [
    {
      "name":"search",
      "title":"Search",
      "method":"GET",
      "href":"http://example.org/friends/search",
      "fields":[
        {
          "name":"search",
          "value": ""
        }
      ]
    },
    {
      "name":"add-item",
      "title":"Add Item",
      "method":"POST",
      "href":"http://example.org/friends/",
      "fields":[
        {
          "name":"full-name",
          "type":"text",
          "label": "Full Name"
        },
        {
          "name":"email",
          "type":"text",
          "label": "Email"
        },
        {
          "name":"blog",
          "type":"text",
          "label": "Blog"
        },
        {
          "name":"avatar",
          "type":"text",
          "label": "Avatar"
        }
      ]
    }
  ]
}
```