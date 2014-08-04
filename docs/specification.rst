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

``methods``
  For specifying an HTTP method. This is an array of strings.

``rels``
  An ``array`` of link relations for an item.

``responseTypes``
  An ``array`` of available media types on the server.

Types
#####

``typesOf``
  A way to specify what type of item an item is. Useful when specifying Schema.org values or linking to profiles. It is an array of strings. If multiple types are specified, the first type is of most priority and the last type is of least, in the event that there are conflicting restraints. Having several types is an option, but it may be better to create and define a new type if there are too many.

  By default, Verbose supports the JSON primitives:

  1. string
  2. number
  3. boolean
  4. array
  5. object

  You can of course create your own types and define certain constraints and validations on the object. See the 

Extensibility
#############

Each of these properties use :ref:`Verbose Path <verbose_path>` to reference items from the same document or other documents. Please see the documentation on this to understand how Verbose Path works.

``extend``
  A way to extend the item with other items. It is an object, the contents of which can be defined outside of this spec.

``forEach``
  An ``array`` of Verbose Paths that specify for what item a template can be used. These templates can be for links, queries, actions, or resource templates.

  If the Verbose Path string specified references more than one item in the document, it means that template can be used for each of those items.

  ::

    {
      "verbose": {
        "href": "/customers",
        "methods": [ "GET", "POST" ],
        "templates": [
          {
            "forEach": [ "#", "#/includes[rel=item]" ]
          }
        ]
      }
    }

  In this example above, the ``forEach`` array contains a reference to ``#``, which references the root resource of the document, and ``#/includes[rel=item]``, which references any included items with a link relation of ``item``. Please see the Verbose Path section to see how it is used. 

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

1. ``id`` - Unique identifier for field
2. ``name`` - Name of field
3. ``typesOf`` - Types of the field
4. ``extend`` - Added details determined by the type

A ``field`` object also provides the following properties:

``defaultValue``
  The optional default value of the field. This is a ``string``.

``currentValue``
  The current value of the field. This is a ``string``.

``value``
  The value of the field which cannot be changed. The ``defaultValue`` and ``currentValue`` properties allow for the values to be changed or set, though the ``value`` property is unchangeable. It is a way for the API to provide unchangeable field data, equivalent to a hidden field in HTML.

``options``
  An ``array`` of option objects. Option objects have a ``name`` and ``value`` property for each option.

.. _transitions:

Transitions
-----------

A transition is a way in which a client can interact with this resource or other related resources. It can be seen as a link to a resource or even an action that can be taken, such as updating a resource.

The ``transitions`` property is an array of Transition objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for item
2. ``name`` - Name of transition
3. ``rels`` - Link relation of the transition
4. ``responseTypes`` - Types with which the server may respond
5. ``embedAs`` - Ways to inform the client how an item should be transcluded
6. ``href`` - URL for the transition
7. ``hreft`` - URL template
8. ``mapsTo`` - An array of Verbose Paths to map a transition to another property
9. ``typesOf`` - For pointing to another semantic or schema for the transition
10. ``methods`` - For specifying available methods for the transition. If this is not set, GET is assumed.

An transition can have different types of parameters that can be used at different times.

``bodyParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for the body of a request  

``queryParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for a query

``uriParams``
  An ``array`` of ``field`` objects that is used for specifying the parameters for a URI template

The ``href`` and ``hreft`` properties MAY be used together, where the ``href`` property takes priority, though the ``hreft`` can be used to specify how to generate other links based on the pattern.

Link Example
############

The transition below provides a link to a customer resource.

* It shows ``name`` being used, which has a name of ``customer`` 
* It defines the link relations for this link using the ``rels`` property
* It uses ``responseTypes`` to hint at what representations are available from the server
* It uses ``href`` to provide the actual URL to the resource

::

  {
    "verbose": {
      "transitions": [
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

Action Example
##############

This action can be used to create a customer.

* It uses the ``POST`` method
* It has two body parameters: ``first_name`` and ``last_name`` which are both strings

::

  {
    "verbose": {
      "transitions": [
        {
          "name": "add-customer",
          "title": "Add Customer",
          "rels": [ "http://example.com/rels/customers"],
          "href": "/customers",
          "methods": [ "POST" ],
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

Query Example
#############

This query can be used for searching customers. It has two available query parameters.

* Company name: ``company_name``
* Email Address: ``email``

::

  {
    "verbose": {
      "transitions": [
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

Templated Link Example
######################

This shows a resource that has a templated link for a customer resource This is very similar to a regular link, but it provides a ``hreft`` property, which is a templated URL, along with URI parameters.

In this case, there is one URI parameters call ``id``, which is a number.

::

  {
    "verbose": {
      "transitions": [
        {
          "name": "customer",
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

Templated Action Example
########################

This templated action provides an action for editing any customer. This allows for including actions that can be used for multiple resources without including the action multiple times. 

In this example, there are both URI parameters and body parameters for building the request.

::

  {
    "verbose": {
      "transitions": [
        {
          "title": "Edit Customer",
          "rels": [ "http://example.com/rels/customer"],
          "hreft": "/customer/{id}",
          "methods": [ "PUT" ],
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

Templated Query Example
#######################

This is very similar to the templated action, where it provides a query that can be used for multiple resoures. The example below provides a URI template for creating a URL for an image search for each user.

In this example, there are both URI parameters and query parameters for building the request.

::

  {
    "verbose": {
      "transitions": [
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
      "methods": [ "GET", "POST" ],
      "templates": [
        {
          "forEach": [ "#", "#/includes[rel=item]" ],
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

A Verbose Resource is an ``object`` for defining everything dealing with a particular resource. It uses these properties from the definition list.

1. ``id`` - Unique identifier for resource
2. ``name`` - Name of resource

It also supports.

``href``
  Link to the resource

``methods``
  Defines the HTTP methods available for this resource

``semantics``
  An ``array`` of :ref:`Semantic objects <semantics>`

``properties``
  A :ref:`Properties object <properties>`

``transitions``
  An ``array`` of :ref:`Transition objects <transitions>`

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

ID
##

This shows the template can be used for the item where the ID is equal to ``person``.

::

  {
    "verbose": {
      "version": "0.3",
      "templates": [
        {
          "forEach": [ "#person" ],
          "fields": [
            { "name": "first_name" },
            { "name": "last_name" }
          ]
        }
      ],
      "includes": [
        {
          "id": "person",
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
          "mapsTo": "#/properties.customer"
        },
        {
          "name": "fullName",
          "type": "string",
          "mapsTo": "#/properties.customer.fullName"
        },
        {
          "name": "email",
          "type": "string",
          "mapsTo": "#/properties.customer.email"
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
          "mapsTo": "#/properties.customers[]"
        },
        {
          "name": "fullName",
          "type": "string",
          "mapsTo": "#/properties.customers[].fullName"
        },
        {
          "name": "email",
          "type": "string",
          "mapsTo": "#/properties.customers[].email"
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
          "name": "customer",
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
