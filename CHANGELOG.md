0.4.0 (beta)
------------

## Transitions 

All links, actions, queries, and templates have been comibined into one property called `transitions`. The reason for this was to make it easy for clients to deal with finding specific links. This information can still be derived from the affordance.

* If there is no `methods`, or if the `methods` has GET, it can be a link
* If it has `queryParams`, it can be a query
* If it has `hreft`, it can be a templated link, action, or query
* If it has PUT, POST, or DELETE, it can be an action

## Types

Types have been changed to be handled by the `typesOf` array. Before, there was `type`, `format`, and `typesOf`, which have been combined. This allow for types to be specified elsewhere. A type can also specify additional properties, which can be used with `extend`.