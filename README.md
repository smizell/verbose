# Verbose

**Status**: Draft  
**Version**: 0.1.0

Verbose is a hypermedia format to cover a wide array of uses, including profiles, link relations, instance messages, and JSON-based documentation. As you will see, Verbose lives up to its name.

## Schema

Verbose is built from all of the main [Hyperextend](https://github.com/smizell/hyperextend) components. Because of this, it should cover most or all of the [Hyperschema.org Core Definitions](http://hyperschema.org/core/).

```json
{
  "properties":{
    "verbose": { "$ref": "#/definitions/verbose" }
  }
  "definitions": {
    "verbose: {
      "allOf": [
        { "$ref": "http://hyperschema.org/extensions/hyperextend/link#" },
        { "$ref": "#/definitions/resource" }
      ]
    },
    "resource": {
      "properties": {
        "availableMethods": {
          "type": "array",
          "items": { "$ref": "http://hyperschema.org/protocols/http#/definitions/methods" }
        },
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
        "resources": {
          "type": "array",
          "items": { "$ref": "#/definitions/resource" }
        },
      }
    }
  }
  
}
```

## Verbose Namespace

The entire Verbose object is namespaced within a `verbose` property at root.

## Resource Properties

Verbose provides a place for resource properties along with semantics about those properties.

### `properties`

The `properties` property is simply a JSON object that contains key/value pairs.

### `semantics`

The `semantics` property allows for defining semantics about each property. See the [Hyperextend Semantic](https://github.com/smizell/hyperextend#semantic) component for details.

## Links

The `links` property is an array of [Hyperdescribe Link](https://github.com/smizell/hyperextend#link) components. A link should be considered safe and idempotent and by default uses the `GET` HTTP method.

The `href` property is required and at least one item should be included in `rels` for link objects.

## Actions

The `actions` property is an array of [Hyperdescribe Action](https://github.com/smizell/hyperextend#action) components. An action can be safe or unsafe, and idempotent or non-idempotent depending on the method.

The `href` and `method` properties are required.

## Queries

The `queries` property is an array of [Hyperdescribe Query](https://github.com/smizell/hyperextend#query) components. A query should be considered safe and idempotent and by default uses the `GET` HTTP method.

The `href` property is required and at least one item should be included in `rels` for link objects.

## Templated Links

The `templatedLinks` property is an array of [Hyperdescribe TemplatedLink](https://github.com/smizell/hyperextend#templatedlink) components. A link should be considered safe and idempotent and by default uses the `GET` HTTP method. This type of link provides a way to have both URI templates and a normal link in the same object.

The `href` property is required and at least one item should be included in `rels` for link objects.

## Templated Actions

The `templatedActions` property is an array of [Hyperdescribe templatedAction](https://github.com/smizell/hyperextend#templatedaction) components. An action can be safe or unsafe, and idempotent or non-idempotent depending on the method. This type of action provides a way to have both URI templates and body params in the same object.

The `href` and `method` properties are required.

## Templated Queries

The `templatedQueries` property is an array of [Hyperdescribe TemplatedQuery](https://github.com/smizell/hyperextend#templatedquery) components. A templated query should be considered safe and idempotent and by default uses the `GET` HTTP method. This type of query provides a way to have both URI templates and a normal query string in the same object.

The `href` property is required and at least one item should be included in `rels` for link objects.

## Embedded Resource

The `resources` property is an array of Verbose resources. It is a way to nest and embed resources into one document.






