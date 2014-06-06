Specification
=============

**Current Version**: |version|

**Current Release**: |release|

**Build Date**: |today|

**Proposed Registration**: ``application/vnd.verbose+json``

The document outlines the Verbose hypermedia format.

.. note ::
  This is currently in active development. There may be parts of this change before it is ready for use.

.. note ::
  The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
  NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
  "OPTIONAL" in this document are to be interpreted as described in
  RFC 2119.

Definitions
-----------

Verbose provides a set of common properties for every supported item along with providing properties that are shared between specific items. Because of this, these properties are defined here to allow for them to be used throughout this document.

Common Properties
#################

These common properties are available for any item in the document, from links to resources.

``id``
  A unique identifier for an item. It MUST be unique for the entire document. It is a ``string``.

``classes``
  An ``array`` of classifiers for an item. MAY be used to specify of what kind an item is or names for an item.

``title``
  A human-readable title for an item. It is a ``string``.

``description``
  A human-readable description for an item. It is a ``string``.

``extends``
  A way to extend the item with other items. It is an ``array``.

Shared
######

These properties are reused by several items but are not common to all. 

``rels``
  An ``array`` of link relations for an item.

``responseTypes``
  An ``array`` of available media types on the server.

``typeOf``
  A way to specify what type of item an item is. Useful when specifying Schema.org values or linking to profiles.

``embedAs``
  A way to tell the client how an item should be embedded. These are the possible values:

  1. image
  2. audio
  3. video
  4. text
  5. application

``method``
  For specifying an HTTP method

``uriParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for a URI template

``queryParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for a query

``bodyParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for the body of a request  

``href``
  URL of the item

``hreft``
  Templated URL of the item

``forEach``
  An ``array`` of Verbose Paths that specify for what item a template can be used. These templates can be for links, queries, actions, or resource templates.

  If the Verbose Path string specified references more than one item in the document, it means that template can be used for each of those items.

  ::

    {
      "verbose": {
        "href": "/customers",
        "availableMethods": [ "GET", "POST" ],
        "templates": [
          {
            "forEach": [ "#", "#/includes@item" ]
          }
        ]
      }
    }

  In this example above, the ``forEach`` array contains a reference to ``#``, which references the root resource of the document, and ``#/includes@item``, which references any included items with a link relation of ``item``. Please see the Verbose Path section to see how it is used. 

Prefixes
--------

Prefixes can be used to shorten URLs. When used, they are available throughout the entire document.

``prefixes``
  This is an ``array`` of prefix objects.

``prefix``
  This is an object with two properties: ``prefix`` and ``href``. 

Example
#######

::

  {
    "verbose": {
      "version": "1.0",
      "prefixes": [
        {
          "prefix": "schema",
          "href": "http://schema.org"
        }
      ]
    }
  }

Namespace
---------

All Verbose documents MUST have a ``verbose`` namespace.

::

  {
    "verbose": {}
  }

Properties
----------

Semantics
#########

``name``
  Name of property

``type``
  The JSON type for this field value

``format``
  HTML input types. This is a ``string``.

``label``
  Human-readable label for a property

``mapsTo``
  An ``array` of Verbose Path strings (see Verbose Path section for details on how this is used)

Properties
##########

The ``properties`` object is simply a JSON object. Its semantics are defined by the Semantic object

Example
#######

Below is an example showing a resource that has ``properties`` and ``semantics`` for those properties. In this example, the property is ``email``, which is a ``string`` and uses the HTML5 formatting for ``email``. The instance data for that property is ``john@doe.com``.

::

  {
    "verbose": {
      "semantics": [
        {
          "name": "email",
          "type": "string",
          "format": "email",
          "label": "Email"
        }
      ],
      "properties": {
        "email": "john@doe.com"
      }
    }
  }

Field
-----

A ``field`` object provides the following properties:

``name``
  The name of the field. This is a ``string``.

``defaultValue``
  The optional default value of the field. This is a ``string``.

``currentValue``
  The current value of the field. This is a ``string``.

``options``
  An ``array`` of option objects

``option``
  An object with a ``name`` and ``value`` property. This is an ``object``.

``type``
  The JSON type for this field value

``format``
  HTML input types. This is a ``string``.

``label``
  Human-readable label for the field

``mapsTo``
  An ``array` of Verbose Path strings (see Verbose Path section for details on how this is used)

Links
-----

The ``links`` property is an array of ``link`` objects. A ``link`` object allows for the following properties specified in the glossary:

1. ``rels``
2. ``responseTypes``
3. ``embedAs``
4. ``href``

Example
#######

The link below provides a link to a customer resource.

* It shows ``classes`` being used, which has a class of ``customer`` 
* It defines the link relations for this link using the ``rels`` property
* It uses ``responseTypes`` to hint at what representations are available from the server
* It uses ``href`` to provide the actual URL to the resource

::

  {
    "verbose": {
      "links": [
        {
          "classes": [ "customer" ],
          "rels": [ "item", "http://example.com/rels/customer"],
          "responseTypes": [
            "application/json",
            "application/hal"
          ],
          "href": "/customer/4"
        }
      ]
    }
  }

Actions
-------

An action is a way to provide non-idempotent actions that can be taken on a resource. 

The ``actions`` property is an array of ``action`` objects. An ``action`` object allows for the following properties specified in the glossary:

1. ``rels``
2. ``responseTypes``
3. ``embedAs``
4. ``method``
5. ``bodyParams``
6. ``href``

In addition to these properties, it also supports:

``href``
  URL of the resource on which the action is being taken

Example
#######

This action can be used to create a customer.

* It uses the ``POST`` method
* It has two body parameters: ``first_name`` and ``last_name`` which are both strings

::

  {
    "verbose": {
      "actions": [
        {
          "title": "Create Customer",
          "rels": [ "http://example.com/rels/customers"],
          "href": "/customers",
          "method": "POST",
          "bodyParams": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name"
            }
          ]
        }
      ]
    }
  }

Queries
-------

Queries are safe GET requests that provide a way for specifying query parameters.

The ``queries`` property is an array of ``query`` objects. A ``query`` object allows for the following properties specified in the glossary:

1. ``rels``
2. ``responseTypes``
3. ``embedAs``
4. ``queryParams``
5. ``href``

Example
#######

This query can be used for searching customers. It has two available query parameters.

* Company name: ``company_name``
* Email Address: ``email``

::

  {
    "verbose": {
      "queries": [
        {
          "id": "search",
          "rels": [ "search" ],
          "href": "/customers",
          "description": "Customer search",
          "queryParams": [
            {
              "title": "Company Name",
              "name": "company_name"
            },
            {
              "title": "Email Address",
              "name": "email"
            }
          ]
        }
      ]
    }
  }

Templated Links
---------------

The ``templatedLinks`` property is an array of ``templateLink`` objects. A ``templatedLink`` object allows for the following properties specified in the glossary:

1. ``rels``
2. ``responseTypes``
3. ``embedAs``
4. ``uriParams``
5. ``hreft``

Example
#######

This shows a resource that has a templated link for a customer resource This is very similar to a regular link, but it provides a ``hreft`` property, which is a templated URL, along with URI parameters.

In this case, there is one URI parameters call ``id``, which is a number.

::

  {
    "verbose": {
      "templatedLinks": [
        {
          "classes": [ "customer" ],
          "rels": [ "item", "http://example.com/rels/customer"],
          "responseTypes": [
            "application/json",
            "application/hal"
          ],
          "hreft": "/customer/{id}",
          "uriParams": [
            {
              "name": "id",
              "type": "number"
            }
          ],
        }
      ]
    }
  }

Templated Actions
-----------------

The ``templatedActions`` property is an array of ``templateAction`` objects. A ``templatedAction`` object allows for the following properties specified in the glossary:

1. ``rels``
2. ``responseTypes``
3. ``embedAs``
4. ``method``
5. ``bodyParams``
6. ``uriParams``
7. ``hreft``

Example
#######

This templated action provides an action for editing any customer. This allows for including actions that can be used for multiple resources without including the action multiple times. 

In this example, there are both URI parameters and body parameters for building the request.

::

  {
    "verbose": {
      "templatedActions": [
        {
          "title": "Edit Customer",
          "rels": [ "http://example.com/rels/customer"],
          "hreft": "/customer/{id}",
          "method": "PUT",
          "uriParams": [
            {
              "name": "id",
              "type": "number"
            }
          ],
          "bodyParams": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name"
            }
          ]
        }
      ]
    }
  }

Templated Queries
-----------------

The ``templatedQueries`` property is an array of ``templatedQuery`` objects. A ``templatedQuery`` object allows for the following properties specified in the glossary:

1. ``rels``
2. ``responseTypes``
3. ``embedAs``
4. ``queryParams``
5. ``uriParams``
6. ``hreft``

Example
#######

This is very similar to the templated action, where it provides a query that can be used for multiple resoures. The example below provides a URI template for creating a URL for an image search for each user.

In this example, there are both URI parameters and query parameters for building the request.

::

  {
    "verbose": {
      "templatedQueries": [
        {
          "title": "User Image Search",
          "rels": [ "search" ],
          "hreft": "/users/{id}/images",
          "uriParams": [
            {
              "name": "id",
              "type": "number"
            }
          ],
          "queryParams": [
            {
              "name": "image_name",
              "type": "string",
              "label": "Image Name"
            }
          ]
        }
      ]
    }
  }


Resource Template
-----------------


``mediaTypes``
  Defines the media types for the request. Can be an array of media types.

``fields``
  An ``array`` of field objects.

Example
#######

This is an example of a resource that provides templates for working with this particular resource and/or embedded resources. It shows this template can be used for the root resource and for any included resource with ``item`` as a rel.

::

  {
    "verbose": {
      "href": "/customers",
      "availableMethods": [ "GET", "POST" ],
      "templates": [
        {
          "forEach": [ "#", "#/includes@item" ],
          "mediaTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name"
            }
          ]
        }
      ],
      "includes": [
        {
          "rels": [ "item" ],
          "href": "/customers/1",
          "properties": {
            "first_name": "John",
            "last_name": "Doe"
          }
        },
        {
          "rels": [ "item" ],
          "href": "/customers/2",
          "properties": {
            "first_name": "Jane",
            "last_name": "Doe"
          }
        }
      ]
    }
  }

Templates can also use JSON Schema to define how a request should be formed.

::

  {
    "verbose": {
      "forEach": [ "#", "#/includes@item" ],
      "templates": [
        {
          "mediaTypes": [ "application/json" ],
          jsonSchema: {
            properties: {
              "first_name": { "type": "string" },
              "last_name": { "type": "string" }
            }
          }
        }
      ],
      "includes": [
        {
          "rels": [ "item" ],
          "href": "/customers/1",
          "properties": {
            "first_name": "John",
            "last_name": "Doe"
          }
        },
        {
          "rels": [ "item" ],
          "href": "/customers/2",
          "properties": {
            "first_name": "Jane",
            "last_name": "Doe"
          }
        }
      ]
    }
  }

Embedded Resources
------------------

Partials
########

Partial resources are considered to be a partial representation of the embedded resource. If the entire resource for the partial is desired, the semantics of the API can specificy how this is done.

Includes
########

Included resources are considered to be full representations.

Resource
--------

A Verbose Resource is an ``object`` for defining everything dealing with a particular resource.

``href``
  Link to the resource

``semantics``
  An ``array`` of Semantic objects

``properties``
  A Properties object

``links``
  An ``array`` of Link objects

``actions``
  An ``array`` of Action objects

``queries``
  An ``array`` of Query objects

``templatedLinks``
  An ``array`` of Templated Link objects

``templatedActions``
  An ``array`` of Templated Action objects

``templatedQueries``
  An ``array`` of Templated Query objects

``templates``
  An ``array`` of Template objects

``partials``
  An ``array`` of partial Resource objects

``includes``
  An ``array`` of full Resource objects

``errors``
  An Error object

See the Examples page for examples of a resource

Errors
------

The ``errors`` property is a Verbose object that can be used specifically for errors. The properties and links for the error are left up to the designer.

::

  {
    "versbose": {
      "version": "1.0",
      "errors": {
        "properties": {
          "message": "There was an error when creating this resource"
        }
      }
    }
  }

Verbose Path
------------

Verbose Path is a way to reference objects throughout a Verbose document or in other Verbose documents. It is meant to be used strictly with Verbose. It allows for the symbols below to be used in the path string.

* The ``#`` alone specifies the root-level resource
* The ``#`` MAY be used with an ID to specify a particular item
* The ``.`` specifies a class name
* The ``@`` specifies a link relation
* The ``/`` can be used for nesting
* The ``!`` can be used for getting the property of an object

Root
####

Using a ``#`` alone specifies the root resource.

::
  
  {
    "verbose": {
      "version": "1.0",
      "href": "/customers",
      "availableMethods": [ "GET", "POST" ],
      "templates": [
        {
          "forEach": [ "#" ],
          "mediaTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name"
            }
          ]
        }
      ]
    }
  }

ID
##

This example uses a path to point to an ID in the document. IDs MUST be unique for a document.

::
  
  {
    "verbose": {
      "version": "1.0",
      "href": "/customers",
      "availableMethods": [ "GET", "POST" ],
      "templates": [
        {
          "forEach": [ "#customer" ],
          "mediaTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name"
            }
          ]
        }
      ],
      "includes": [
        {
          "id": "customer",
          "properties": {
            "first_name": "John",
            "last_name": "Doe"
          }
        }
      ]
    }
  }

Class
#####

This example in the ``forEach`` section specifies the template can be used for each include that has a class of ``customer``.

::

  {
    "verbose": {
      "version": "1.0",
      "href": "/customers",
      "availableMethods": [ "GET", "POST" ],
      "templates": [
        {
          "forEach": [ "#/includes.customer" ],
          "mediaTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name"
            }
          ]
        }
      ],
      "includes": [
        {
          "classes": [ "customer" ],
          "properties": {
            "first_name": "John",
            "last_name": "Doe"
          }
        }
      ]
    }
  }

Link Relation
#############

This example says the template can be used for each include that has ``item`` for a link relation.

::

  {
    "verbose": {
      "version": "1.0",
      "href": "/customers",
      "availableMethods": [ "GET", "POST" ],
      "templates": [
        {
          "forEach": [ "#/includes@item" ],
          "mediaTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name"
            }
          ]
        }
      ],
      "includes": [
        {
          "rels": [ "item" ],
          "properties": {
            "first_name": "John",
            "last_name": "Doe"
          }
        }
      ]
    }
  }

Nested Items
############

Using the slash, the path can specify nested items. The path below in the ``forEach`` property says:

1. Look in the ``includes`` in the root resource for items with ``customer`` as class
2. In those items, look in the ``includes`` for items with the link relation ``item``

::

  {
    "verbose": {
      "version": "1.0",
      "href": "/",
      "templates": [
        {
          "forEach": [ "#/includes.customers/includes@item" ],
          "mediaTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name"
            }
          ]
        }
      ],
      "includes": [
        {
          "classes": [ "customers" ],
          "rels": [ "collection" ],
          "includes": [
            {
              "rels": [ "item" ],
              "properties": {
                "first_name": "John",
                "last_name": "Doe"
              }
            }
          ]
        }
      ]
    }
  }

Properties
##########

The ``!`` can be used to specify properties of an item. In the example below, ``mapsTo`` points to the corresponding properties.

::
  
  {
    "verbose": {
      "version": "1.0",
      "href": "/",
      "properties": {
        "first_name": "John",
        "last_name": "Doe"
      },
      "templates": [
        {
          "forEach": [ "#" ],
          "mediaTypes": [ "application/x-www-form-urlencoded" ],
          "fields": [
            {
              "name": "first_name",
              "type": "string",
              "label": "First Name",
              "mapsTo": [ "#/properties!first_name" ]
            },
            {
              "name": "last_name",
              "type": "string",
              "label": "Last Name",
              "mapsTo": [ "#/properties!last_name" ]
            }
          ]
        }
      ]
    }
  }

