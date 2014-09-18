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

  If left blank, it is assumed to be a normal link.

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

.. _resource:

Resource
--------

A Verbose Resource is an ``object`` for defining everything dealing with a particular resource. It uses these properties from the definition list.

1. ``id`` - Unique identifier for resource
2. ``name`` - Name of resource
3. ``typesOf`` - For pointing to another semantic or schema for the resource

It also supports.

``id``
  Unique identifier for resource

``name``
  Name of resource

``typesOf``
  For pointing to another semantic or schema for the resource

``meta``
  A :ref:`Meta object <meta>`

``href``
  Link to the resource

``prefixes``
  An ``array`` of :ref:`Prefix objects <prefixes>`

``semantics``
  An ``array`` of :ref:`Semantic objects <semantics>`

``properties``
  A :ref:`Properties object <properties>`

``links``
  An ``array`` of :ref:`Link objects <links>`

``queries``
  An ``array`` of :ref:`Query objects <queries>`

``actions``
  An ``array`` of :ref:`Action objects <actions>`

``templatedLinks``
  An ``array`` of :ref:`Templated Link objects <templated_links>`

``templatedActions``
  An ``array`` of :ref:`Templated Action objects <templated_actions>`

``templates``
  An ``array`` of :ref:`Resource Template objects <resource_template>`

``includes``
  An ``array`` of full :ref:`Resource objects <resource>`

``errors``
  An :ref:`Error object <errors>`

See the :ref:`Examples <examples>` page for examples of a resource

.. _namespace:

Namespace
---------

All Verbose documents MUST have a ``verbose`` namespace.

::

  {
    "verbose": {}
  }

.. _prefixes:

Prefixes
--------

Prefixes can be used to shorten URLs. When used, they are available throughout the entire document.

``prefixes``
  This is an ``array`` of prefix objects.

  ``prefix``
    The short prefix name.

  ``href``
    The URL to be used as the prefix.

Example
#######

::

  {
    "verbose": {
      "version": "0.4",
      "prefixes": [
        {
          "prefix": "schema",
          "href": "http://schema.org/"
        }
      ]
    }
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
3. ``title`` - Title of semantic
4. ``description`` - Description of semantic
5. ``label`` - Human-readable label or prompt for semantic
6. ``typesOf`` - For pointing to another semantic or schema for the property
7. ``mapsTo`` - Property to which the semantic point

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
3. ``title`` - Title of field
4. ``description`` - Description of field
5. ``label`` - Human-readable label or prompt for field
6. ``typesOf`` - Types of the field
7. ``extend`` - Added details determined by the type

A ``field`` object also provides the following properties:

``defaultValue``
  The optional default value of the field. This is a ``string``.

``currentValue``
  The current value of the field. This is a ``string``.

``value``
  The value of the field which cannot be changed. The ``defaultValue`` and ``currentValue`` properties allow for the values to be changed or set, though the ``value`` property is unchangeable. It is a way for the API to provide unchangeable field data, equivalent to a hidden field in HTML.

``options``
  An ``array`` of option objects. Option objects have a ``name`` and ``value`` property for each option.

Transitions
-----------

A transition is an available progression from one state to another state. There are many characteristics of transitions, and several different categories that hypermedia formats use. Transitions have been broken down into links, queries, actions, templated links, templated actions, and embedded resources below.

.. _links:

Links
#####

The ``links`` property is an array of Link objects. It supports all of the properties of the :ref:`Resource object <resource>`, along with the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``label`` - Human-readable label or prompt for link
2. ``rels`` - Link relations of the link
3. ``responseTypes`` - Types with which the server may respond
4. ``embedAs`` - Ways to inform the client how an item should be transcluded
5. ``typesOf`` - For pointing to another semantic or schema for the link

A link is always considered safe. The HTTP method ``GET`` should be used for these requests.

Example
^^^^^^^

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

.. _queries:

Queries
#######

Queries are safe GET requests that provide a way for specifying query parameters.

The queries property is an array of Query objects. It supports all of the properties of the :ref:`Resource object <resource>`, along with the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``label`` - Human-readable label or prompt for link
2. ``rels`` - Link relations of the link
3. ``responseTypes`` - Types with which the server may respond
4. ``embedAs`` - Ways to inform the client how an item should be transcluded
5. ``typesOf`` - For pointing to another semantic or schema for the link
6. ``queryParams`` - An ``array`` of ``field`` objects to be used for query parameters

Example
^^^^^^^

This example shows how ``queryParams`` are used.

::

  {
    "verbose": {
      "queries": [
        {
          "name": "customer",
          "rels": [ "search"],
          "responseTypes": [
            "application/json",
            "application/hal"
          ],
          "href": "/customers",
          "queryParams": [
            {
              "name": "email",
              "label": "Email"
            }
          ]
        }
      ]
    }
  }

.. _actions:

Actions
#######

An action is a way to provide an unsafe action.

The ``actions`` property is an array of Action objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for item
2. ``name`` - Name of action
3. ``title`` - Title of action
4. ``description`` - Description of action
5. ``label`` - Human-readable label or prompt for action
6. ``rels`` - Link relation of the action
7. ``responseTypes`` - Types with which the server may respond
8. ``requestTypes`` - Types in which the server accepts
9. ``embedAs`` - Ways to inform the client how an item should be transcluded
10. ``href`` - URL for the action
11. ``typesOf`` - For pointing to another semantic or schema for the action
12. ``method`` - HTTP method for the action
13. ``bodyParams`` - An ``array`` of ``field`` objects that is used for specifying the parameters for the body of a request 

The HTTP method for the action MUST be ``POST``, ``PUT``, or ``DELETE``.

Example
^^^^^^^

This action can be used to create a customer.

* It uses the ``POST`` method
* It has two body parameters: ``first_name`` and ``last_name`` which are both strings

::

  {
    "verbose": {
      "actions": [
        {
          "title": "Add Customer",
          "rels": [ "append"],
          "href": "/customers",
          "method": "POST",
          "bodyParams": [
            {
              "name": "first_name",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "label": "Last Name"
            }
          ]
        }
      ]
    }
  }

.. _templated_links:

Templated Links
###############

The ``templatedLinks`` property is an array of Templated Link objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for item
2. ``name`` - Name of link
3. ``title`` - Title of link
4. ``description`` - Description of link
5. ``label`` - Human-readable label or prompt for link
6. ``rels`` - Link relations of the link
7. ``responseTypes`` - Types with which the server may respond
8. ``embedAs`` - Ways to inform the client how an item should be transcluded
9. ``hreft`` - URI template for the link
10. ``typesOf`` - For pointing to another semantic or schema for the link
11. ``uriParams`` - An ``array`` of ``field`` objects to be used for URI template parameters

Example
^^^^^^^

This shows a resource that has a templated link for a customer resource This is very similar to a regular link, but it provides a ``hreft`` property, which is a templated URL, along with URI parameters.

In this case, there is one URI parameters call ``id``, which is a number.

::

  {
    "verbose": {
      "templatedLinks": [
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
              "name": "id"
            }
          ],
        }
      ]
    }
  }

.. _templated_actions:

Templated Actions
#################

The ``templatedActions`` property is an array of Templated Action objects. It supports the following properites listed in the :ref:`Definitions <definitions>` list:

1. ``id`` - Unique identifier for item
2. ``name`` - Name of action
3. ``title`` - Title of action
4. ``description`` - Description of action
5. ``label`` - Human-readable label or prompt for action
6. ``rels`` - Link relation of the action
7. ``responseTypes`` - Types with which the server may respond
8. ``requestTypes`` - Types in which the server accepts
9. ``embedAs`` - Ways to inform the client how an item should be transcluded
10. ``hreft`` - URI template for the action
11. ``typesOf`` - For pointing to another semantic or schema for the action
12. ``method`` - HTTP method for the action
13. ``bodyParams`` - An ``array`` of ``field`` objects that is used for specifying the parameters for the body of a request 
14. ``uriParams`` - An ``array`` of ``field`` objects that is used for specifying the parameters for the URI template 

Example
^^^^^^^

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
              "name": "id"
            }
          ],
          "bodyParams": [
            {
              "name": "first_name",
              "label": "First Name"
            },
            {
              "name": "last_name",
              "label": "Last Name"
            }
          ]
        }
      ]
    }
  }

.. _embedded_resources:

Embedded Resources
##################

Included resources are just to be considered as included resources and MAY be full representations. The reason for this and the ``partials`` property is that it allows for explicitly telling the client that the resource needs to be requested if the full resource is desired.

.. _resource_template:

Resource Template
-----------------

This item uses the ``forEach`` from the :ref:`Definitions <definitions>` list. It also supports all of the properties that a transition supports except for the ``href`` and ``hreft`` properties.

Example
#######

This is an example of a resource that provides templates for working with embedded resources. It shows this template can be used for any included resource with ``item`` as a rel, and uses ``forEach`` to specify this.

::

  {
    "verbose": {
      "href": "/customers",
      "templates": [
        {
          "rels": [ "update" ],
          "forEach": [ "#", "#/includes[rel=item]" ],
          "requestTypes": [ "application/x-www-form-urlencoded" ],
          "method": "PUT"
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

.. _meta:

Meta
----

The ``meta`` property is a resource object that can be used to specify meta data, such as meta links or properties. The properties and links for the error are left up to the designer. 

::

  {
    "versbose": {
      "version": "0.4",
      "meta": {
        "transitions": [
          {
            "rels": [ "self" ],
            "href": "/customers"
          }
        ]
      }
    }
  }

.. _errors:

Errors
------

The ``errors`` property is a resource object that can be used specifically for errors. The properties and links for the error are left up to the designer.

::

  {
    "versbose": {
      "version": "0.4",
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
      "version": "0.4",
      "templates": [
        {
          "forEach": [ "#" ],
          "method": "POST",
          "rels": [ "create" ],
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
      "version": "0.4",
      "templates": [
        {
          "forEach": [ "#person" ],
          "method": "POST",
          "rels": [ "edit" ],
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
      "version": "0.4",
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
      "version": "0.4",
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
      "version": "0.4",
      "templates": [
        {
          "forEach": [ "#/includes[name=customer]" ],
          "method": "PUT",
          "rels": [ "update" ],
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
