Schema
======

.. warning ::
  This schema is out of date. Do not use. Will be updated soon.

Provided here is a JSON schema for validating Verbose documents.::

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
          "properties": {
            "type": "object"
          },
          "affordances": {
            "type": "array",
            "items": { "$ref": "http://hyperschema.org/extensions/hyperextend/affordance#" }
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
          "errors": { "$ref": "#/definitions/resource" }
        }
      }
    }
  }