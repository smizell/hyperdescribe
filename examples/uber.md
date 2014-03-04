Describing an Uber Document
================================

## Original Uber Document

This is pulled from the [Uber documentation](https://rawgithub.com/mamund/media-types/master/uber-hypermedia.html#_json_example).

```json
{
  "uber" :
  {
    "version" : " 1.0",
    "data" :
    [
      {"rel" : "self", "url" : "http://example.org/"},
      {"rel" : "profile", "url" : "http://example.org/profiles/people-and-places"},

      {
        "data" :
        [
          {
            "id" : "people",
            "rel" : "collection",
            "url" : "http://example.org/people/",
            "data" :
            [
              {
                "name" : "create",
                "rel" : "http://example.org/rels/create",
                "url" : "http://example.org/people/",
                "model" : "g={givenName}&f={familyName}&e={email}",
                "action" : "append"
              },
              {
                "name" : "search",
                "rel" : "search",
                "url" : "http://example.org/people/search",
                "model" : "?g={givenName}&f={familyName}&e={email}"
              },
              {
                "name" : "person",
                "rel" : "item",
                "url" : "http://example.org/people/1",
                "data" :
                [
                  {"name" : "givenName", "value" : "Mike"},
                  {"name" : "familyName", "value" : "Amundsen"},
                  {"name" : "email", "value" : "mike@example.org"},
                  {"name" : "avatarUrl", "transclude" : "true", "value" : "http://example.org/avatars/1"}
                ]
              },
              {
                "name" : "person",
                "rel" : "item",
                "url" : "http://example.org/people/2",
                "data" :
                [
                  {"name" : "givenName", "value" : "Mildred"},
                  {"name" : "familyName", "value" : "Amundsen"},
                  {"name" : "email", "value" : "mildred@example.org"},
                  {"name" : "avatarUrl", "transclude" : "true", "value" : "http://example.org/avatars/2"}
                ]
              }
            ]
          },

          {
            "id" : "places",
            "rel" : "collection",
            "url" : "http://example.org/places/",
            "data" :
            [
              {
                "name" : "search",
                "rel" : "search",
                "url" : "http://example.org/places/search",
                "model" : "?r={addressRegion}&l={addressLocality}&p={postalCode}"
              },
              {
                "name" : "place",
                "rel" : "item",
                "url" : "http://example.org/places/a",
                "data" :
                [
                  {
                    "name" : "name",
                    "value" : "Home",
                    "data" :
                    [
                      {"name" : "streetAddress", "value" : "123 Main Street"},
                      {"name" : "addressLocalitly", "value" : "Byteville"},
                      {"name" : "addressRegion", "value" : "MD"},
                      {"name" : "postalCode", "value" : "12345"}
                    ]
                  }
                ]
              },
              {
                "name" : "place",
                "rel" : "item",
                "url" : "http://example.org/places/b",
                "data" :
                [
                  {
                    "name" : "name",
                    "value" : "Work",
                    "data" :
                    [
                      {"name" : "streetAddress", "value" : "1456 Grand Ave."},
                      {"name" : "addressLocalitly", "value" : "Byteville"},
                      {"name" : "addressRegion", "value" : "MD"},
                      {"name" : "postalCode", "value" : "12345"}
                    ]
                  }
                ]
              }
            ]
          }
        ]
      }
    ]
  }
}
```

## Described with Hyperdescribe

There are a couple of things missing here so far. 

1. For an action, there is only a name. Maybe Hyperdescribe should include a `rel` here as well.
2. For resources, Hyperdescribe does not have an `id`. Maybe that would be beneficial.

```json
{
  "meta":{
    "links":[
      {
        "rel":"self",
        "url":"http://example.org/"
      },
      {
        "rel":"profile",
        "url":"http://example.org/profiles/people-and-places"
      }
    ]
  },
  "resources":[
    {
      "rel":"collection",
      "url":"http://example.org/people/",
      "resources":[
        {
          "rel":"item",
          "url":"http://example.org/people/1",
          "context":{
            "typeof":["http://schema.org/Person"]
          },
          "properties":{
            "givenName":"John",
            "familyName":"Doe",
            "email":"johndoe@example.org",
            "image":"http://example.org/avatars/1"
          }
        },
        {
          "rel":"item",
          "url":"http://example.org/people/2",
          "context":{
            "typeof":["http://schema.org/Person"]
          },
          "properties":{
            "givenName":"Jane",
            "familyName":"Doe",
            "email":"janedoe@example.org",
            "image":"http://example.org/avatars/2"
          }
        }
      ],
      "actions":[
        {
          "name":"http://example.org/rels/create",
          "url":"http://example.org/people/",
          "method":"POST",
          "fields":[
            {
              "label":"givenName",
              "name":"g",
              "type":"text"
            },
            {
              "label":"familyName",
              "name":"f",
              "type":"text"
            },
            {
              "label":"emailName",
              "name":"e",
              "type":"text"
            }
          ]
        },
        {
          "name":"search",
          "url":"http://example.org/people/search",
          "method":"GET",
          "fields":[
            {
              "label":"givenName",
              "name":"g",
              "type":"text"
            },
            {
              "label":"familyName",
              "name":"f",
              "type":"text"
            },
            {
              "label":"emailName",
              "name":"e",
              "type":"text"
            }
          ]
        }
      ]
    },
    {
      "rel":"collection",
      "url":"http://example.org/places/",
      "resources":[
        {
          "meta":{
            "data":{
              "name":"Home"
            }
          },
          "rel":"item",
          "url":"http://example.org/places/a",
          "context":{
            "typeof":["http://schema.org/Place"]
          },
          "properties":{
            "streetAddress":"123 Main Street",
            "addressLocalitly":"Byteville",
            "addressRegion":"MD",
            "postalCode":"12345"
          }
        },
        {
          "meta":{
            "data":{
              "name":"Work"
            }
          },
          "rel":"item",
          "url":"http://example.org/places/b",
          "context":{
            "typeof":["http://schema.org/Place"]
          },
          "properties":{
            "streetAddress":"1456 Grand Ave.",
            "addressLocalitly":"Byteville",
            "addressRegion":"MD",
            "postalCode":"12345"
          }
        }
      ],
      "actions":[
        {
          "name":"search",
          "url":"http://example.org/places/search",
          "method":"GET",
          "fields":[
            {
              "label":"addressRegion",
              "name":"r",
              "type":"text"
            },
            {
              "label":"addressLocality",
              "name":"l",
              "type":"text"
            },
            {
              "label":"postalCode",
              "name":"p",
              "type":"text"
            }
          ]
        }
      ]
    }
  ]
}
```