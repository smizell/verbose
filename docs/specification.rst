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

.. _definitions:

Definitions
-----------

Since Verbose reuses a lot of properites, this section will first define some of these common properties. 

Identifying
###########

These properties define identifying information for items throughout a document.

``id``
  A unique identifier for an item. It MUST be unique for the entire document. It is a ``string``.

``name``
  A way to give a name to an item in the Verbose document. It is a ``string``.

``label``
  Human-readable label for a property

``title``
  A human-readable title for an item. It is a ``string``.

``description``
  A human-readable description for an item. It is a ``string``.

``typesOf``
  A way to specify what type of item an item is. Useful when specifying Schema.org values or linking to profiles.

Hypermedia
##########

These properties are used to define hypermedia information and hints related to links, queries, actions, and resources.

``embedAs``
  A way to tell the client how an item should be embedded. These are the possible values:

  1. image
  2. audio
  3. video
  4. text
  5. application

``method``
  For specifying an HTTP method

``rels``
  An ``array`` of link relations for an item.

``responseTypes``
  An ``array`` of available media types on the server.

Parameters
##########

These properites are used to define semantics for URI template parameters, query strings parameters, and body parameters.

``bodyParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for the body of a request  

``queryParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for a query

``uriParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for a URI template

URLs
####

These properties are used to define the URL or templated URL.

``href``
  URL of the item

``hreft``
  Templated URL of the item

Types
#####

These properties are used to define the type of field and semantics.

``type``
  The JSON type for this field value. This can be:

  1. string
  2. number
  3. boolean
  4. array
  5. object

``format``
  HTML input types. This is a ``string``.

Extensibility
#############

Each of these properties use :ref:`Verbose Path <verbose_path>` to reference items from the same document or other documents. Please see the documentation on this to understand how Verbose Path works.

``extends``
  A way to extend the item with other items. It is an ``array``.

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
            "forEach": [ "#", "#/includes[rel=item]" ]
          }
        ]
      }
    }

  In this example above, the ``forEach`` array contains a reference to ``#``, which references the root resource of the document, and ``#/includes@item``, which references any included items with a link relation of ``item``. Please see the Verbose Path section to see how it is used. 

``mapsTo``
  An ``array`` of Verbose Path strings (see  section for details on how this is used)

.. _prefixes:

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
      "version": "0.3",
      "prefixes": [
        {
          "prefix": "schema",
          "href": "http://schema.org"
        }
      ]
    }
  }

.. _namespace:

Namespace
---------

All Verbose documents MUST have a ``verbose`` namespace.

::

  {
    "verbose": {}
  }

.. _properties:

Properties
----------

The ``properties`` object is simply a JSON object. Its semantics are defined by the Semantic object.

.. _semantics:

Semantics
---------

The ``semantics`` array is an array of Semantic objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for semantic
2. ``name`` - Name of property being defined
3. ``type`` - Type of the property
4. ``format`` - HTML format of the property
5. ``typesOf`` - For pointing to another semantic or schema for the property
6. ``mapsTo`` - Property to which the semantic point

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
          "label": "Email",
          "mapsTo": "#/properties.email"
        }
      ],
      "properties": {
        "email": "john@doe.com"
      }
    }
  }

.. _field:

Field
-----

A Field object supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``type`` - Type of the field
2. ``format`` - Format of the field (HTML inputs)

A ``field`` object also provides the following properties:

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

.. _links:

Links
-----

The ``links`` property is an array of Link objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for link
2. ``name`` - Name of link
3. ``rels`` - Link relation of link
4. ``responseTypes`` - Types with which the server may respond
5. ``embedAs`` - Ways to inform the client how an item should be transcluded
6. ``href`` - URL for the link
7. ``mapsTo`` - An array of Verbose Paths to map a link to another property
8. ``typesOf`` - For pointing to another semantic or schema for the link

Example
#######

The link below provides a link to a customer resource.

* It shows ``name`` being used, which has a name of ``customer`` 
* It defines the link relations for this link using the ``rels`` property
* It uses ``responseTypes`` to hint at what representations are available from the server
* It uses ``href`` to provide the actual URL to the resource

::

  {
    "verbose": {
      "links": [
        {
          "name": "customer",
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

.. _actions:

Actions
-------

An action is a way to provide non-idempotent actions that can be taken on a resource. 

The ``actions`` property is an array of Action objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for action
2. ``name`` - Name of action
3. ``rels`` - Link relation of link
4. ``responseTypes`` - Types with which the server may respond
5. ``embedAs`` - Ways to inform the client how an returned resource should be transcluded
6. ``method`` - HTTP method for the action
7. ``bodyParams`` - An array of available body parameters
8. ``href`` - URL for the action
9. ``mapsTo`` - An array of Verbose Paths to map a action to another property
10. ``typesOf`` - For pointing to another semantic or schema for the action

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

.. _queries:

Queries
-------

Queries are safe GET requests that provide a way for specifying query parameters.

The ``queries`` property is an array of Query objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for query
2. ``name`` - Name of action
3. ``rels`` - Link relation of query
4. ``responseTypes`` - Types with which the server may respond
5. ``embedAs`` - Ways to inform the client how an returned resource should be transcluded
6. ``queryParams`` - An array of available query parameters
7. ``href`` - URL for the query
8. ``mapsTo`` - An array of Verbose Paths to map a query to another property
9. ``typesOf`` - For pointing to another semantic or schema for the query

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

.. _templated_links:

Templated Links
---------------

The ``templatedLinks`` property is an array of Templated Link objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for query
2. ``name`` - Name of action
3. ``rels`` - Link relation of query
4. ``responseTypes`` - Types with which the server may respond
5. ``embedAs`` - Ways to inform the client how an returned resource should be transcluded
6. ``uriParams`` - An array of available parameters for the URI template
7. ``hreft`` - URL template
8. ``mapsTo`` - An array of Verbose Paths to map a link to another property
9. ``typesOf`` - For pointing to another semantic or schema for the link

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

.. _templated_actions:

Templated Actions
-----------------

The ``templatedActions`` property is an array of Templated Action objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for action
2. ``name`` - Name of action
3. ``rels`` - Link relation of link
4. ``responseTypes`` - Types with which the server may respond
5. ``embedAs`` - Ways to inform the client how an returned resource should be transcluded
6. ``method`` - HTTP method for the action
7. ``bodyParams`` - An array of available body parameters
8. ``uriParams`` - An array of available parameters for the URI template
9. ``hreft`` - URL template
10. ``mapsTo`` - An array of Verbose Paths to map a action to another property
11. ``typesOf`` - For pointing to another semantic or schema for the action

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

.. _templated_queries:

Templated Queries
-----------------

The ``templatedQueries`` property is an array of Templated Query objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for query
2. ``name`` - Name of action
3. ``rels`` - Link relation of query
4. ``responseTypes`` - Types with which the server may respond
5. ``embedAs`` - Ways to inform the client how an returned resource should be transcluded
6. ``queryParams`` - An array of available query parameters
7. ``uriParams`` - An array of available parameters for the URI template
8. ``hreft`` - URL template
9. ``mapsTo`` - An array of Verbose Paths to map a query to another property
10. ``typesOf`` - For pointing to another semantic or schema for the query

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

.. _resource_template:

Resource Template
-----------------

This item uses the ``forEach`` from the :ref:`Definitions <definitions>` list. It also supports:

``mediaTypes``
  Defines the media types for the request. Can be an array of media types.

``semantics``
  An ``array`` of Verbose Semantic objects. This is useful to define semantic properties for a template.

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

.. _embedded_resources:

Embedded Resources
------------------

Partials
########

Partial resources are considered to be a partial representation of the embedded resource. If the entire resource for the partial is desired, the semantics of the API can specificy how this is done.

Includes
########

Included resources are just to be considered as included resources and MAY be full representations. The reason for this and the ``partials`` property is that it allows for explicitly telling the client that the resource needs to be requested if the full resource is desired.

.. _resource:

Resource
--------

A Verbose Resource is an ``object`` for defining everything dealing with a particular resource.

``href``
  Link to the resource

``availableMethods``
  Defines the HTTP methods available for this resource

``semantics``
  An ``array`` of :ref:`Semantic objects <semantics>`

``properties``
  A :ref:`Properties object <properties>`

``links``
  An ``array`` of :ref:`Link objects <links>`

``actions``
  An ``array`` of :ref:`Action objects <actions>`

``queries``
  An ``array`` of :ref:`Query objects <queries>`

``templatedLinks``
  An ``array`` of :ref:`Templated Link objects <templated_links>`

``templatedActions``
  An ``array`` of :ref:`Templated Action objects <templated_actions>`

``templatedQueries``
  An ``array`` of :ref:`Templated Query objects <templated_queries>`

``templates``
  An ``array`` of :ref:`Resource Template objects <resource_template>`

``partials``
  An ``array`` of partial :ref:`Resource objects <resource>`

``includes``
  An ``array`` of full :ref:`Resource objects <resource>`

``errors``
  An :ref:`Error object <errors>`

See the :ref:`Examples <examples>` page for examples of a resource

.. _errors:

Errors
------

The ``errors`` property is a Verbose object that can be used specifically for errors. The properties and links for the error are left up to the designer.

::

  {
    "versbose": {
      "version": "0.3",
      "errors": {
        "properties": {
          "message": "There was an error when creating this resource"
        }
      }
    }
  }

.. _verbose_path:

Verbose Path
------------

Verbose Path is a way to reference objects throughout a Verbose document or in other Verbose documents. It is very simple and tries to only provide what is needed to reference items throughout a document.

Root Resource
#############

The ``#`` alone SHOULD be considered the path to the root resource of a Verbose document. The example below shows a template that can be used for the root resource.

::

  {
    "verbose": {
      "version": "0.3",
      "templates": [
        {
          "forEach": [ "#" ],
          "fields": [
            { "name": "first_name" },
            { "name": "last_name" }
          ]
        }
      ]
    }
  }

Nested Properties
#################

Properties of an object can be specified with a dot. Shown below, the semantics ``fullName`` and ``email`` are mapped to properties of the ``customer`` object.

::

  {
    "verbose": {
      "version": "0.3",
      "semantics": [
        {
          "name": "customer",
          "type": "object",
          "mapsTo": "#/customer"
        },
        {
          "name": "fullName",
          "type": "string",
          "mapsTo": "#/customer.fullName"
        },
        {
          "name": "email",
          "type": "string",
          "mapsTo": "#/customer.email"
        }
      ],
      "properties": {
        "customer": {
          "fullName": "John Doe",
          "email": "johndoe@example.com"
        }
      }
    }
  }

Arrays
######

Arrays can also be referenced.

::

  {
    "verbose": {
      "version": "0.3",
      "semantics": [
        {
          "name": "customers",
          "type": "array",
          "mapsTo": "#/customer"
        },
        {
          "name": "fullName",
          "type": "string",
          "mapsTo": "#/customers[].fullName"
        },
        {
          "name": "email",
          "type": "string",
          "mapsTo": "#/customers[].email"
        }
      ],
      "properties": {
        "customers": [
          {
            "fullName": "John Doe",
            "email": "johndoe@example.com"
          },
          {
            "fullName": "Jane Doe",
            "email": "janedoe@example.com"
          }
        ]
      }
    }
  }

Filtering Arrays
################

The square brackets can be used to filter arrays. The example below shows the template is usable for all included resources with the name equal to customer.

::

  {
    "verbose": {
      "version": "0.3",
      "templates": [
        {
          "forEach": [ "#/includes[name=customer]" ],
          "fields": [
            { "name": "first_name" },
            { "name": "last_name" }
          ]
        }
      ],
      "includes": [
        {
          "name": [ "customer" ],
          "properties": {
            "customer": {
              "fullName": "John Doe",
              "email": "johndoe@example.com"
            }
          }
        }
      ]
    }
  }
