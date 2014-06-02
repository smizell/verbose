Schema
======

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