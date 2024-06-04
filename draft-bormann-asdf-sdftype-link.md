---
title: An sdfType for Links
abbrev: sdfType for Links
category: std
stream: IETF
consensus: true

docname: draft-bormann-asdf-sdftype-link-latest
number:
date:
v: 3
area: "Applications and Real-Time"
workgroup: ASDF WG
keyword:
 - IoT
 - Link
 - Web Linking
venue:
  group: "A Semantic Definition Format for Data and Interactions of Things"
  mail: "asdf@ietf.org"
  github: "cabo/sdftype-link"


author:
-
  name: Carsten Bormann
  org: Universität Bremen TZI
  street: Postfach 330440
  city: Bremen
  code: D-28359
  country: Germany
  phone: +49-421-218-63921
  email: cabo@tzi.org

normative:
  I-D.ietf-asdf-sdf: sdf
  RFC8288: link

informative:
  RFC6690: link-format
  I-D.laari-asdf-relations: sdfrel
  RFC9423: attr

--- abstract

This document defines and registers an sdfType "link" for the
Semantic Definition Format (SDF) for Data and Interactions of Things
(draft-ietf-asdf-sdf).

--- middle

# Introduction

The Semantic Definition Format for Data and Interactions of Things
(SDF, {{-sdf}}) is a format for domain experts to use in the creation
and maintenance of data and interaction models in the Internet of
Things.

A common data type that occurs in the modeling of IoT devices is the
*link*.
{{-link}} defines the concept of Web Linking, which complements the
target URI that any link will contain, with additional parameters, such
as the "link relation type" that explains the relationship expressed
by the link, as well as "target attributes" that provide additional
information about the target of the link (without a need to
"dereference", i.e., follow, the link).

This document defines and registers an sdfType "link" for the Semantic
Definition Format.
This type models an abstract "serialization" {{-link}} of a link, in a
way that is compatible with the way SDF maps information models to its
data modeling language.

## Conventions and Definitions

<!--
{: :boilerplate bcp14-tagged}
 -->

The definitions of {{-link-format}}, {{-link}}, and {{-sdf}} apply.

# The sdfType "link"

The sdfType "link" is intended to be used with the SDF "type" of "object".
The members of that object are strings that are named the same as the
link parameter (attribute) names.
The special parameter name "href" is used to express the link target.
(Parameter names specific to the Constrained RESTful Environment (CoRE) are also discussed in {{-attr}}.)

An example for the instance of a link is provided in {{Section 5 of -link-format}}:

~~~ link-format
   </sensors/temp>;rt="temperature-c";if="sensor",
~~~

An sdfProperty that is used to describe an SDF affordance that is intended to
hold a link like this (without getting specific on the actual link to
the link target) could look like:

~~~ sdf
{
 "sdfProperty": {
  "temp-c-link": {
   "type": "object",
   "sdfType": "link",
   "properties": {
     "href": { "type": "string"},
     "rt": { "type": "string", "const": "temperature-c"},
     "if": { "type": "string", "const": "sensor"}
   }
  }
 }
}
~~~

# Discussion

Links play an important role in SDF modeling both during definition
time (for adding information to a model, such as in `sdfRef`) and
during run time (for making links to instances into a subject of data
and interaction modeling).
The present document is an early attempt at addressing the run-time
usage of links, in particular links that fit the Web Linking {{-link}}
abstractions.
A related draft {{-sdfrel}} addresses definition-time links, but does
seem to touch modeling run-time use of links as well (e.g., by
discussing "writable" link relations).

Not all links used in ecosystems are based on URIs.
E.g., OMA has "object links", which are pairs of numbers (object/instance).
These ecosystem links may have some structure that should be modeled
in the SDF model (e.g., where the object id part of a link always has
to have a specific value).
This structure can be mapped into URI strings using some convention,
e.g., an OMA object link could be `oma-object:3303:0` (where
`oma-object` is placeholder for a URI scheme to be defined).
However, burying structural components of the ecosystem-specific link
in a string syntax makes it hard to access and control those
components from the model.

TODO: Examples are needed to show how the OCF collection pattern is
addressed by the current specification.

# Security Considerations

The security considerations of {{-link}} apply in a general way,
although modeling a link as a datatype does not incur all of the
security considerations that will apply to actually interchanging
these links.

(TODO)


# IANA Considerations

// RFC Ed.: please replace RFC XXXX with this RFC number and remove this note.

IANA is requested to register the sdfType "link" in the "sdfType Values" sub-registry in
the "SDF Parameters" registry, with the following completion for the
registration template:

| Name | Description       | type   | JSON Representation                | Reference |
|------|-------------------|--------|------------------------------------|-----------|
| link | A Web Link {{-link}} | object | object members for link attributes | RFCXXXX   |
{: #sdftype-r title="Registration for sdfType \"link\""}

--- back

# Acknowledgments
{:unnumbered}

Discussions in the OneDM liaison organization shaped this proposal.
