0.4.0 (beta)
------------

## Transition Types 

Transitions are now condensed to these areas:

1. Links
2. Queries
3. Actions
4. Templated Links
5. Templated Actions
6. Embedded Resources (Includes)

## Types

Types have been changed to be handled by the `typesOf` array. Before, there was `type`, `format`, and `typesOf`, which have been combined. This allow for types to be specified elsewhere. A type can also specify additional properties, which can be used with `extend`.

## Methods

Methods for transitions is now just a string known as `method`. Only one method is available now per transition.

A `method` property is now added to the resource templates.

## Rels added to templates

A `rels` property is now added to templates

## Meta

Now Verbose supports a meta property on the resource.

