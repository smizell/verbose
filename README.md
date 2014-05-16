# Verbose

**Status**: Draft  
**Version**: 0.1.0

Verbose is a hypermedia format to cover a wide array of uses, including profiles, link relations, instance messages, and JSON-based documentation. As you will see, Verbose lives up to its name.

## Schema

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
        "availableMethods": {},
        "semantics": {},
        "properites": {},
        "links": {},
        "queries": {},
        "actions": {},
        "templatedLinks": {},
        "templatedQueries": {},
        "templatedActions": {}
      }
    }
  }
  
}
```
