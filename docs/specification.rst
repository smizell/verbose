Specification
=============

Base Objects
------------

Properties
----------

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

Links
-----

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

Embedded Resources
------------------

Errors
------