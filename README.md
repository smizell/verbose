# Verbose

**Status**: Draft  
**Version**: 0.1.0  
**Proposed Registration**: application/vnd.verbose+json

## Description

Verbose is a general-purpose, multi-use hypermedia format that lives up to its name. It can be used for profiles, link relations, resource representations, and api documentation if you so dare. It is built from all of the main [Hyperextend](https://github.com/smizell/hyperextend) components. Because of this, it should cover most or all of the [Hyperschema.org Core Definitions](http://hyperschema.org/core/).

## Schema

 It's schema is heavily dependent on Hyperextend's schema.

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
        "partials": {
          "type": "array",
          "items": { "$ref": "#/definitions/resource" }
        },
        "includes": {
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

The entire Verbose object is namespaced within a `verbose` property at root. The `verbose` property is a verbose object.

### Verbose Object

#### Resource Properties

Verbose provides a place for resource properties along with semantics about those properties.

##### `properties`

The `properties` property is simply a JSON object that contains key/value pairs.

##### `semantics`

The `semantics` property allows for defining semantics about each property. See the [Hyperextend Semantic](https://github.com/smizell/hyperextend#semantic) component for details.

#### Links

The `links` property is an array of [Hyperdescribe Link](https://github.com/smizell/hyperextend#link) components. A link should be considered safe and idempotent and by default uses the `GET` HTTP method.

The `href` property is required and at least one item should be included in `rels` for link objects.

#### Actions

The `actions` property is an array of [Hyperdescribe Action](https://github.com/smizell/hyperextend#action) components. An action can be safe or unsafe, and idempotent or non-idempotent depending on the method.

The `href` and `method` properties are required.

#### Queries

The `queries` property is an array of [Hyperdescribe Query](https://github.com/smizell/hyperextend#query) components. A query should be considered safe and idempotent and by default uses the `GET` HTTP method.

The `href` property is required and at least one item should be included in `rels` for link objects.

#### Templated Links

The `templatedLinks` property is an array of [Hyperdescribe TemplatedLink](https://github.com/smizell/hyperextend#templatedlink) components. A link should be considered safe and idempotent and by default uses the `GET` HTTP method. This type of link provides a way to have both URI templates and a normal link in the same object.

The `href` property is required and at least one item should be included in `rels` for link objects.

#### Templated Actions

The `templatedActions` property is an array of [Hyperdescribe templatedAction](https://github.com/smizell/hyperextend#templatedaction) components. An action can be safe or unsafe, and idempotent or non-idempotent depending on the method. This type of action provides a way to have both URI templates and body params in the same object.

The `href` and `method` properties are required.

#### Templated Queries

The `templatedQueries` property is an array of [Hyperdescribe TemplatedQuery](https://github.com/smizell/hyperextend#templatedquery) components. A templated query should be considered safe and idempotent and by default uses the `GET` HTTP method. This type of query provides a way to have both URI templates and a normal query string in the same object.

The `href` property is required and at least one item should be included in `rels` for link objects.

#### Resource Template

The `templates` property is an array of [Hyperdescribe Template](https://github.com/smizell/hyperextend#resource-template) components. A templated query should be considered safe and idempotent and by default uses the `GET` HTTP method.

#### Partials

The `partials` property is an array of Verbose objects. Partials are verbose objects, but can be used to include objects that are not full representations.

#### Includes

The `includes` property is an array of Verbose objects. It is a way to nest and embed verbose objects.

### Fields

Within `queries`, `actions`, `templatedLinks`, `templatedQueries`, and `templatedActions`, there are arrays that include [Hyperextend Field](https://github.com/smizell/hyperextend#field) objects. Verbose has some special rules for them.

* `defaultValue` is that value that should be sent if no value is picked
* `currentValue` is the current value of the field, which can be changed
* `options` if options is present, they provide the only values for that field
* `value` is a way to say that this is the value of the field and MUST NOT be changed

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
    
    "includes": [
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
    
    "includes": [
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

### Profile Example

Verbose can also provide a profile with all of its wordiness.

#### Profile

```json
{
  "verbose": {
    "version": "0.1",

    "title": "Collection of Customers",
    "description": "A collection of customers",

    "id": "customers",
    "rels": [ "collection" ],
    
    "queries": [
      {
        "id": "search",
        "rels": [ "search" ],
        "description": "Customer search",
        "queryParams": [
          {
            "title": "Company Name",
            "description": "Company name search field",
            "name": "companyName"
          },
          {
            "title": "Email Address",
            "description": "Email address search field",
            "name": "email"
          }
        ],
        "returns": "#customers"
      }
    ],
    
    "includes": [
      {
        "id": "customer",
        "rels": [ "item" ],
        "description": "Customer resource",
        "semantics": [
          { "name": "companyName" },
          { "name": "firstName" },
          { "name": "lastName" },
          { "name": "email" },
          { "name": "phone" }
        ]
      }
    ]
  }
}
```

#### Resource Representation

This is what an actual collection of customers could look like.

```json
{
  "verbose": {
    "version": "0.1",

    "id": "customers",
    "rels": [ "collection" ],
    "typeOf": "http://example.com/customers#customers",

    "links": [
      {
        "rels": [ "profile" ],
        "href": "http://example.com/customers"
      }
    ],
    
    "queries": [
      {
        "id": "search",
        "rels": [ "search" ],
        "typeOf": "http://example.com/customers#search",
        "description": "Customer search",
        "queryParams": [
          {
            "title": "Company Name",
            "name": "companyName"
          },
          {
            "title": "Email Address",
            "name": "email"
          }
        ]
      }
    ],
    
    "includes": [
      {
        "classes": [ "customer" ],
        "href": "/customer/1",
        "rels": [ "item" ],
        "typeOf": "http://example.com/customers#customer", 
        "properties": {
          "companyName": "ACME, Inc.",
          "firstName": "John",
          "lastName": "Doe",
          "email": "john@acme.com"
        }
      },
      {
        "classes": [ "customer" ],
        "href": "/customer/2",
        "rels": [ "item" ],
        "typeOf": "http://example.com/customers#customer", 
        "properties": {
          "companyName": "ACME, Inc.",
          "firstName": "Jane",
          "lastName": "Doe",
          "email": "jane@acme.com"
        }
      }
    ]
  }
}
```

## Compared to Others

Since Verbose can includes many things from other media types, it should be able to look them if you want.

### HAL

This example is the example from the [HAL spec](http://stateless.co/hal_specification.html). Because the HAL spec says you should not assume the embedded resources are full resources, I've put them in the `partials` array.

```json
{
  "verbose": {
    "version": "0.1",

    "prefixes": [
      {
        "prefix": "ea",
        "href": "http://example.com/docs/rels/"
      }
    ],

    "properties": {
      "currentlyProcessing": 14,
      "shippedToday": 20
    },

    "links": [
      {
        "rels": [ "self" ],
        "href": "/orders"
      },
      {
        "rels": [ "next" ],
        "href": "/orders?page=2"
      },
      {
        "rels": [ "ea:admin" ],
        "href": "/admins/2",
        "label": "Fred"
      },
      {
        "rels": [ "ea:admin" ],
        "href": "/admins/5",
        "label": "Kate"
      }
    ],

    "templatedLinks": [
      {
        "rels": [ "ea:find" ],
        "hreft": "/orders{?id}",
        "uriParams": [
          {
            "name": "id",
            "mapsTo": "id"
          }
        ]
      }
    ],

    "partials": [
      {
        "rels": [ "ea:order" ],
        "properties": {
          "total": 30.00,
          "currency": "USD",
          "status": "shipped"
        },
        "links": [
          {
            "rels": [ "self" ],
            "href": "/orders/123"
          },
          {
            "rels": [ "ea:basket" ],
            "href": "/baskets/98712"
          },
          {
            "rels": [ "ea:customer" ],
            "href": "/customers/7809"
          }
        ]
      },
      {
        "rels": [ "ea:order" ],
        "properties": {
          "total": 20.00,
          "currency": "USD",
          "status": "processing"
        },
        "links": [
          {
            "rels": [ "self" ],
            "href": "/orders/124"
          },
          {
            "rels": [ "ea:basket" ],
            "href": "/baskets/98713"
          },
          {
            "rels": [ "ea:customer" ],
            "href": "/customers/12369"
          }
        ]
      }
    ]
  }
}
```

### Siren

This is taken from the example in the [Siren spec](https://github.com/kevinswiber/siren#example). I took a little bit of liberty with this one and considered one of the embedded entities to be a partial representation.

```json
{
  "verbose": {
    "classes": [ "order" ],
    "properties": { 
        "orderNumber": 42, 
        "itemCount": 3,
        "status": "pending"
    },
    "actions": [
      {
        "classes": [ "add-item" ],
        "title": "Add Item",
        "method": "POST",
        "href": "http://api.x.io/orders/42/items",
        "requestTypes": [ "application/x-www-form-urlencoded" ],
        "bodyParams": [
          { "name": "orderNumber", "type": "number", "format": "hidden", "value": "42" },
          { "name": "productCode", "type": "string", "format": "text" },
          { "name": "quantity", "type": "number", "format": "number" }
        ]
      }
    ],
    "links": [
      {
        "rels": [ "self" ],
        "href": "http://api.x.io/orders/42"
      },
      {
        "rels": [ "previous" ],
        "href": "http://api.x.io/orders/41"
      },
      {
        "rels": [ "next" ],
        "href": "http://api.x.io/orders/43"
      }
    ],
    "partials": [
      { 
        "classes": [ "items", "collection" ], 
        "rels": [ "http://x.io/rels/order-items" ], 
        "href": "http://api.x.io/orders/42/items"
      }
    ],
    "includes": [
      {
        "classes": [ "info", "customer" ],
        "rels": [ "http://x.io/rels/customer" ], 
        "properties": { 
          "customerId": "pj123",
          "name": "Peter Joseph"
        },
        "links": [
          { 
            "rels": [ "self" ],
            "href": "http://api.x.io/customers/pj123"
          }
        ]
      }
    ]
  }
}
```

## Thanks

Verbose stands on the shoulders of giants. It comes from including everything it can from other media types. A big thanks to all those who work tirelessly on building great hypermedia stuff. Here are just a few pieces to which I give a tip of my hat.

* Collection+JSON for its collection/item model, CRUD influences, and queries
* HAL for its curies and minimalism
* Siren for its domain-drive model (and the `id` and `class` properties, which I use here)
* UBER for doing everything with minimalism (where Verbose tries to do everything and more without minimalism)

These are just small things I got from each of these formats, which are all awesome. I could go on a for a long time about them. A big thanks to their creators.






