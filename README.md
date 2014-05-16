# Verbose

**Status**: Draft  
**Version**: 0.1.0

## Description

Verbose is a hypermedia type aimed at doing as much as possible by being overly verbose. Because Verbose provides so many ways to describe hypermedia, it has a number of uses.

* Profiles
* Link Relations
* Resource Representations
* API Documenation

Since it can be both the profile/link relation and message, as little or as much information can be included in the message depending

## Schema

Verbose is built from all of the main [Hyperextend](https://github.com/smizell/hyperextend) components. Because of this, it should cover most or all of the [Hyperschema.org Core Definitions](http://hyperschema.org/core/). It's schema is heavily dependent on Hyperextend's schema.

```json
{
  "properties":{
    "verbose": { "$ref": "#/definitions/verbose" }
  },
  "definitions": {
    "verbose": {
      "allOf": [
        { "$ref": "http://hyperschema.org/extensions/hyperextend/link#" },
        { "$ref": "#/definitions/resource" }
      ]
    },
    "resource": {
      "properties": {
        "availableMethods": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/protocols/http#/definitions/methods" }
        },
        "semantics": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/semantic#" }
        },
        "properites": {
          "type": "object"
        },
        "links": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/link#" }
        },
        "queries": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/query#" }
        },
        "actions": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/action#" }
        },
        "templatedLinks": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/templatedLink#" }
        },
        "templatedQueries": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/templatedQuery#" }
        },
        "templatedActions": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/templatedAction#" }
        },
        "templates": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/resourcetemplate#" }
        },
        "resources": {
          "type": "array",
          "items": { "$ref": "#/definitions/resource" }
        },
      }
    }
  }
  
}
```

## Verbose Documents

### Verbose Namespace

The entire Verbose object is namespaced within a `verbose` property at root.

### Resource Properties

Verbose provides a place for resource properties along with semantics about those properties.

#### `properties`

The `properties` property is simply a JSON object that contains key/value pairs.

#### `semantics`

The `semantics` property allows for defining semantics about each property. See the [Hyperextend Semantic](https://github.com/smizell/hyperextend#semantic) component for details.

### Links

The `links` property is an array of [Hyperdescribe Link](https://github.com/smizell/hyperextend#link) components. A link should be considered safe and idempotent and by default uses the `GET` HTTP method.

The `href` property is required and at least one item should be included in `rels` for link objects.

### Actions

The `actions` property is an array of [Hyperdescribe Action](https://github.com/smizell/hyperextend#action) components. An action can be safe or unsafe, and idempotent or non-idempotent depending on the method.

The `href` and `method` properties are required.

### Queries

The `queries` property is an array of [Hyperdescribe Query](https://github.com/smizell/hyperextend#query) components. A query should be considered safe and idempotent and by default uses the `GET` HTTP method.

The `href` property is required and at least one item should be included in `rels` for link objects.

### Templated Links

The `templatedLinks` property is an array of [Hyperdescribe TemplatedLink](https://github.com/smizell/hyperextend#templatedlink) components. A link should be considered safe and idempotent and by default uses the `GET` HTTP method. This type of link provides a way to have both URI templates and a normal link in the same object.

The `href` property is required and at least one item should be included in `rels` for link objects.

### Templated Actions

The `templatedActions` property is an array of [Hyperdescribe templatedAction](https://github.com/smizell/hyperextend#templatedaction) components. An action can be safe or unsafe, and idempotent or non-idempotent depending on the method. This type of action provides a way to have both URI templates and body params in the same object.

The `href` and `method` properties are required.

### Templated Queries

The `templatedQueries` property is an array of [Hyperdescribe TemplatedQuery](https://github.com/smizell/hyperextend#templatedquery) components. A templated query should be considered safe and idempotent and by default uses the `GET` HTTP method. This type of query provides a way to have both URI templates and a normal query string in the same object.

The `href` property is required and at least one item should be included in `rels` for link objects.

### Resource Template

The `templates` property is an array of [Hyperdescribe Template](https://github.com/smizell/hyperextend#resource-template) components. A templated query should be considered safe and idempotent and by default uses the `GET` HTTP method. This type of qu

### Embedded Resource

The `resources` property is an array of Verbose resources. It is a way to nest and embed resources into one document.

## Uses

### CRUD APIs

As you can see, this is very similar to what a [Collection+JSON](http://amundsen.com/media-types/collection/examples/) document would look like.

This would be a resource represenation where it says "Here is a resource and here are the ways you can interact with this specific resource." It provides a template for adding via POST request.

```json
{
  "verbose": {
    "version": "0.1",
    "classes": [ "customers" ],
    "rels": [ "collection" ],
    "href": "/customers",
    
    "availableMethods": [ "GET", "POST" ],
    
    "properties": {
      "total_customers": 45
    },
    
    "queries": [
      {
        "rels": [ "search" ],
        "href": "/customers",
        "queryParams": [
          { "name": "first_name", "type": "string", "label": "First Name" },
          { "name": "last_name", "type": "string", "label": "Last Name" }
        ]
      }
    ],
    
    "templates": [
      {
        "mediaTypes": [ "application/x-www-form-urlencoded" ],
        "fields": [
          { "name": "first_name", "type": "string", "label": "First Name" },
          { "name": "last_name", "type": "string", "label": "Last Name" }
        ]
      }
    ], 
    
    "resources": [
      {
        "classes": [ "customer" ],
        "href": "/customers/1",
        "rels": [ "item" ],
        "properties": {
          "first_name": "John",
          "last_name": "Doe"
        }
      },
      {
        "classes": [ "customer" ],
        "href": "/customers/2",
        "rels": [ "item" ],
        "properties": {
          "first_name": "Jane",
          "last_name": "Doe"
        }
      }
    ]
  }
}
```

### Minimal Message plus Link Relations

This will be very similar to how HAL is designed, where it is very minimal and relies on profiles and link relations to define how to interact with resources. For this approach, I'm going to show a link relation document and a resource representation.

#### Link Relation

As you can see, this link relation contains a lot of the same data as the CRUD example above, but with the resource-specific data stripped out, with more readable information added. It also includes semantics.

```json
{
  "verbose": {
    "version": "0.1",
    
    "title": "Customer Collection",
    "description": "This is a collection of customers",
    
    "classes": [ "customers" ],

    "availableMethods": [ "GET", "POST" ],
    
    "semantics": [
      { 
        "title": "Total Customers",
        "description": "The total number of customers in this collection.",
        "name": "total_customers",
        "type": "number"
      }
    ],
    
    "templates": [
      {
        "title": "Customer Template",
        "description": "Template for appending new customers to this collection",
        "mediaTypes": [ "application/x-www-form-urlencoded" ],
        "fields": [
          { "name": "first_name", "type": "string", "label": "First Name" },
          { "name": "last_name", "type": "string", "label": "Last Name" }
        ]
      }
    ]
  }
}
```

#### Resource Representation

Consider this a representation that is described by the link relation above. It has less information than the CRUD example.

```json
{
  "verbose": {
    "version": "0.1",
    "classes": [ "customers" ],
    "href": "/customers",

    "properties": {
      "total_customers": 45
    },
    
    "queries": [
      {
        "rels": [ "search" ],
        "href": "/customers",
        "queryParams": [
          { "name": "first_name", "type": "string", "label": "First Name" },
          { "name": "last_name", "type": "string", "label": "Last Name" }
        ]
      }
    ],
    
    "resources": [
      {
        "classes": [ "customer" ],
        "href": "/customers/1",
        "rels": [ "item" ],
        "properties": {
          "first_name": "John",
          "last_name": "Doe"
        }
      },
      {
        "classes": [ "customer" ],
        "href": "/customers/2",
        "rels": [ "item" ],
        "properties": {
          "first_name": "Jane",
          "last_name": "Doe"
        }
      }
    ]
  }
}
```

### Profile

Verbose can also provide a profile with all of its wordiness. In this example, I'll show a document that resembles an [ALPS document](http://alps.io/spec/index.html#rfc.section.1.3) (which is used for this example).

#### Profile

```json
{
  "verbose": {
    "version": "0.1",
    "title": "List of Contacts",
    "description": "A list of contacts",
    
    "queries": [
      {
        "id": "collection",
        "description": "Simple link/form for getting a list of contacts",
        "queryParams": [
          {
            "title": "Name Search",
            "description": "Input for search form",
            "name": "nameSearch"
          }
        ],
        "returns": "#contact"
      }
    ],
    
    "resources": [
      {
        "id": "contact",
        "description": "Individual contact",
        "semantics": [
          { "name": "fullName" },
          { "name": "email" },
          { "name": "phone" }
        ]
      }
    ]
  }
}
```

#### Resource Representation

## Thanks

Verbose stands on the shoulders of giants. It comes from including everything it can from other media types. A big thanks to all those who work tirelessly on building great hypermedia stuff. Here are just a few pieces to which I give a tip of my hat.

* Collection+JSON for its collection/item model, CRUD influences, and queries
* HAL for its curies and minimalism
* Siren for its domain-drive model (and the `id` and `class` properties, which I use here)
* UBER for doing everything with minimalism (where Verbose tries to do everything and more without minimalism)

These are just small things I got from each of these formats, which are all awesome. I could go on a for a long time about them. A big thanks to their creators.






