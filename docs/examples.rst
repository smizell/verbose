.. _examples:

Examples
========

Compared to Other Formats
-------------------------

Because of :ref:`Verbose's Design <design>`, it can handle just about anything other formats can handle.

HAL+JSON
########

This example is the example from the `HAL spec <http://stateless.co/hal_specification.html>`_. Because the HAL spec says you should not assume the embedded resources are full resources, I've put them in the `partials` array.

::

  {
    "verbose": {
      "version": "0.4",
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
      "includes": [
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
      "typesOf": [ "order" ],
      "properties": { 
          "orderNumber": 42, 
          "itemCount": 3,
          "status": "pending"
      },
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
      "actions": [
        {
          "name": "add-item",
          "title": "Add Item",
          "method": "POST",
          "href": "http://api.x.io/orders/42/items",
          "requestTypes": [ "application/x-www-form-urlencoded" ],
          "bodyParams": [
            { 
              "name": "orderNumber",
              "typesOf": [ "number" ],
              "value": "42"
            },
            { 
              "name": "productCode",
              "typesOf": [ "string" ],
            },
            {
              "name": "quantity",
              "typesOf": [ "number" ]
            }
          ]
        },
      ],
      "includes": [
        { 
          "typesOf": [ "items", "collection" ], 
          "rels": [ "http://x.io/rels/order-items" ], 
          "href": "http://api.x.io/orders/42/items"
        },
        {
          "typesOf": [ "info", "customer" ],
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
###############

::

  {
    "verbose" : {
      "version" : "0.4",
      "rels": [ "collection" ],
      "href" : "http://example.org/friends/",
      "links" : [
        {
          "rels" : [ "feed" ],
          "href" : "http://example.org/friends/rss"
        }
      ],
      "queries": [
        {
          "rels" : [ "search" ],
          "href" : "http://example.org/friends/search",
          "label" : "Search",
          "queryParams" : [
            {
              "name" : "search",
              "defaultValue" : ""
            }
          ]
        }
      ],
      "includes" : [
        {
          "rels": [ "item" ],
          "href" : "http://example.org/friends/jdoe",
          "semantics" : [
            {"name" : "full-name", "label" : "Full Name"},
            {"name" : "email", "label" : "Email"}
          ],
          "properties": {
            "full-name": "J. Doe",
            "email": "jdoe@example.org"
          },
          "transitions" : [
            {
              "rels" : [ "blog" ],
              "href" : "http://examples.org/blogs/jdoe",
              "label" : "Blog"
            },
            {
              "rels" : [ "avatar" ],
              "href" : "http://examples.org/images/jdoe",
              "label" : "Avatar",
              "embedAs" : "image"
            }
          ]
        },
        {
          "rels": [ "item" ],
          "href" : "http://example.org/friends/msmith",
          "semantics" : [
            {"name" : "full-name", "label" : "Full Name"},
            {"name" : "email", "label" : "Email"}
          ],
          "properties": {
            "full-name": "M. Smith",
            "email": "msmith@example.org"
          },
          "transitions" : [
            {
              "rels" : [ "blog" ],
              "href" : "http://examples.org/blogs/msmith",
              "label" : "Blog"
            },
            {
              "rels" : [ "avatar" ],
              "href" : "http://examples.org/images/msmith",
              "label" : "Avatar",
              "embedAs" : "image"
            }
          ]
        },
        {
          "rels": [ "item" ],
          "href" : "http://example.org/friends/rwilliams",
          "semantics" : [
            {"name" : "full-name", "label" : "Full Name"},
            {"name" : "email", "label" : "Email"}
          ],
          "properties": {
            "full-name": "R. Williams",
            "email": "rwilliams@example.org"
          },
          "transitions" : [
            {
              "rels" : [ "blog" ],
              "href" : "http://examples.org/blogs/rwilliams",
              "label" : "Blog"
            },
            {
              "rels" : [ "avatar" ],
              "href" : "http://examples.org/images/rwilliams",
              "label" : "Avatar",
              "embedAs" : "image"
            }
          ]
        }
      ],
      "templates" : [
        {
          "forEach": [ "#" ],
          "rels": [ "append" ],
          "method": "POST",
          "fields" : [
            { "name" : "full-name", "defaultValue" : "", "label" : "Full Name" },
            { "name" : "email", "defaultValue" : "", "label" : "Email" },
            { "name" : "blog", "defaultValue" : "", "label" : "Blog" },
            { "name" : "avatar", "defaultValue" : "", "label" : "Avatar" }
          ]
        },
        {
          "forEach": [ "#/includes[rel=item]" ],
          "rels": [ "update" ],
          "method": "PUT",
          "fields" : [
            { "name" : "full-name", "defaultValue" : "", "label" : "Full Name" },
            { "name" : "email", "defaultValue" : "", "label" : "Email" },
            { "name" : "blog", "defaultValue" : "", "label" : "Blog" },
            { "name" : "avatar", "defaultValue" : "", "label" : "Avatar" }
          ]
        }
      ]
    }
  }

JSON API
########

This takes the example from the `JSON API page <http://jsonapi.org/>`_. There are several ways to do this in Verbose, so below are a couple of different examples.

This example lets the templated links map its parameters to specific properties in the document.

::

  {
    "verbose": {
      "version": "0.4",
      "properties": {
        "id": 1,
        "title": "Rails is Omakase",
        "author_id": "9",
        "comment_ids": [ "5", "12", "17", "20" ]
      },
      "templatedLinks": [
        {
          "typesOf": [ "author", "people" ],
          "hreft": "http://example.com/people/{author_id}",
          "uriParams": [
            {
              "name": "author_id",
              "mapsTo": [ "#/properties.author_id" ]
            }
          ]
        },
        {
          "typesOf": [ "comments" ],
          "hreft": "http://example.com/comments/{comment_id}",
          "uriParams": [
            {
              "name": "comment_id",
              "mapsTo": [ "#/properties.comment_ids[]" ]
            }
          ]
        }
      ]
    }
  }



Link Relation Example
---------------------

Link Relation
#############

::

  {
    "verbose": {
      "version": "0.4",
      "href": "http://example.com/rels/customers",
      "title": "Customer Collection",
      "description": "This is a collection of customers",
      "typesOf": [ "customers" ],
      "semantics": [
        { 
          "title": "Total Customers",
          "description": "The total number of customers in this collection.",
          "name": "total_customers",
          "typesOf": [ "number" ]
        }
      ],  
      "actions": [
        {
          "title": "Append Customer",
          "description": "Action for adding a new customer",
          "method": "POST",
          "requestTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            { "name": "first_name", "typesOf": [ "string" ], "label": "First Name" },
            { "name": "last_name", "typesOf": [ "string" ], "label": "Last Name" }
          ]
        }
      ],
      "includes": [
        {
          "id": "customer",
          "typesOf": [ "customer" ],
          "rels": [ "item" ],
          "semantics": [
            {
              "title": "First Name",
              "description": "First name of customer",
              "name": "first_name",
              "typesOf": [ "string" ]
            },
            {
              "title": "Last Name",
              "description": "Last name of customer",
              "name": "last_name",
              "types": [ "string" ]
            }
          ],
          "actions": [
            {
              "title": "Update Customer",
              "description": "Action for adding a new",
              "method": "PUT",
              "requestTypes": [ "application/x-www-form-urlencoded" ],
              "fields": [
                { "name": "first_name", "typesOf": [ "string" ], "label": "First Name" },
                { "name": "last_name", "typesOf": [ "string" ], "label": "Last Name" }
              ]
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
      "version": "0.4",
      "typesOf": [ "customers" ],
      "rels": [ "http://example.com/rels/customers" ],
      "href": "/customers",
      "properties": {
        "total_customers": 45
      },
      "includes": [
        {
          "rels": [ "http://example.com/rels/customers#customer" ],
          "typesOf": [ "customer" ],
          "href": "/customers/1",
          "rels": [ "item" ],
          "properties": {
            "first_name": "John",
            "last_name": "Doe"
          }
        },
        {
          "rels": [ "http://example.com/rels/customers#customer" ],
          "typesOf": [ "customer" ],
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
      "version": "0.4",
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
          ]
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
      "version": "0.4",
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
          "typesOf": [ "customer" ],
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
          "typesOf": [ "customer" ],
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
