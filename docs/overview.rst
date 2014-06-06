Overview
========

Description
-----------

Verbose is a general-purpose, multi-use hypermedia format that can be used to build robust and highly-descriptive APIs. Because it aims to cover as many scenarios as possible, it can be used in many situations, such as:

* Resource representation
* Link Relation
* Profile
* Documentation
* Object Model

It can be as minimal or verbose as necessary to fit the situation. Because of it's design, it should be able to cover almost anything that other hypermedia formats can cover.

Design
------

Verbose comes from a need to have an object that can handle aspects of many of the popular hypermedia formats. To do this, these other hypermedia formats were taken down to their bare essentials. These essentials make up the `Hypermedia Core Definitions <http://hyperschema.org/core/>`_.

.. image :: _static/verbose.jpg
  :width: 600px

From these core definitions, the `Hyperextend library <http://hyperschema.org/extensions/hyperextend/>`_ was created. Hyperextend is a library of components that can be used to extend any other format to add in hypermedia elements (e.g. adding form-like semantics to HAL). 

Verbose was then created by combining all of these components into one format. This essentially makes it a format that is derived from all of the media types found on the `Hyperschema.org site <http://hyperschema.org/>`_.