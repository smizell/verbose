Examples
========

Link Relation Example
---------------------

Link Relation
#############

::

  {
    "verbose": {
      "version": "1.0",
      "href": "http://example.com/rels/customers",
      
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
          "forEach": [ "#", "#/includes.customer" ],
          "mediaTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            { "name": "first_name", "type": "string", "label": "First Name" },
            { "name": "last_name", "type": "string", "label": "Last Name" }
          ]
        }
      ],

      "includes": [
        {
          "id": "customer",
          "classes": [ "customer" ],
          "rels": [ "item" ],
          "semantics": [
            {
              "title": "First Name",
              "description": "First name of customer",
              "name": "first_name",
              "type": "string"
            },
            {
              "title": "Last Name",
              "description": "Last name of customer",
              "name": "last_name",
              "type": "string"
            }
          ]
        }
      ]
    }
  }

Resource Representation
#######################

::

  {
    "verbose": {
      "version": "1.0",
      "classes": [ "customers" ],
      "rels": [ "http://example.com/rels/customers" ],
      "href": "/customers",

      "properties": {
        "total_customers": 45
      },
      
      "includes": [
        {
          "rels": [ "http://example.com/rels/customers#customer" ],
          "classes": [ "customer" ],
          "href": "/customers/1",
          "rels": [ "item" ],
          "properties": {
            "first_name": "John",
            "last_name": "Doe"
          }
        },
        {
          "rels": [ "http://example.com/rels/customers#customer" ],
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

Profile
-------

Profile
#######

::

  {
    "verbose": {
      "version": "1.0",

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

Resource Representation
#######################

::

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

Compared to Other Formats
-------------------------

HAL+JSON
########

This example is the example from the `HAL spec <http://stateless.co/hal_specification.html>`_. Because the HAL spec says you should not assume the embedded resources are full resources, I've put them in the `partials` array.

::

  {
    "verbose": {
      "version": "1.0",

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

Siren
#####

This is taken from the example in the `Siren spec <https://github.com/kevinswiber/siren#example>`_. I took a little bit of liberty with this one and considered one of the embedded entities to be a partial representation.


::

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
            { 
              "name": "orderNumber",
              "type": "number",
              "format": "hidden",
              "value": "42"
            },
            { 
              "name": "productCode",
              "type": "string",
              "format": "text"
            },
            {
              "name": "quantity",
              "type": "number",
              "format": "number"
            }
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

Collection+JSON
---------------

::

  {
    "verbose": {
      "version": "1.0",
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
          "forEach": [ "#", "#/includes.customer" ],
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
          "availableMethods": [ "GET", "PUT" ],
          "rels": [ "item" ],
          "properties": {
            "first_name": "John",
            "last_name": "Doe"
          }
        },
        {
          "classes": [ "customer" ],
          "href": "/customers/2",
          "availableMethods": [ "GET", "PUT" ],
          "rels": [ "item" ],
          "properties": {
            "first_name": "Jane",
            "last_name": "Doe"
          }
        }
      ]
    }
  }

JSON API
--------

This takes the example from the `JSON API page <http://jsonapi.org/>`_. There are several ways to do this in Verbose, so below are a couple of different examples.

This example lets the templated links map its parameters to specific properties in the document.

::

  {
    "verbose": {
      "version": "1.0",
      "properties": {
        "id": 1,
        "title": "Rails is Omakase",
        "author_id": "9",
        "comment_ids": [ "5", "12", "17", "20" ]
      },
      "templatedLinks": [
        {
          "classes": [ "author", "people" ],
          "hreft": "http://example.com/people/{author_id}",
          "uriParams": [
            {
              "name": "author_id",
              "mapsTo": [ "#/properties!author_id" ]
            }
          ]
        },
        {
          "classes": [ "comments" ],
          "hreft": "http://example.com/comments/{comment_id}",
          "uriParams": [
            {
              "name": "comment_id",
              "mapsTo": [ "#/properties!comment_ids" ]
            }
          ]
        }
      ]
    }
  }

