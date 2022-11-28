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
workgroup: "A Semantic Definition Format for Data and Interactions of Things"
keyword:
 - IoT
 - Link
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
  role: editor

normative:
  I-D.ietf-asdf-sdf: sdf
  RFC8288: link

informative:
  RFC6690: link-format

--- abstract

This document defines and registers an sdfType "link" for the
Semantic Definition Format (SDF) for Data and Interactions of Things
(draft-ietf-asdf-sdf-12.txt).

--- middle

# Introduction

The Semantic Definition Format for Data and Interactions of Things
(SDF, {{-sdf}}) is a format for domain experts to use in the creation
and maintenance of data and interaction models in the Internet of
Things.

A common data type that occurs in the modeling of IoT devices is the
*link*.
{{-link}} defines the concept of Web Linking, which complements the
target URI that any link will contain, with additional attributes, such
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

{::boilerplate bcp14-tagged}

The definitions of {{-link-format}}, {{-link}}, and {{-sdf}} apply.

# The sdfType "link"

The sdfType "link" can be used with the SDF "type" of "object".
The members of that object are strings that are named as the attribute
names.
The special attribute name "href" is used to express the link target.

An example for the instance of a link is provided in {{Section 5 of -link-format}}:

~~~ link-format
   </sensors/temp>;rt="temperature-c";if="sensor",
~~~

An sdfProperty that is used to describe a property that is intended to
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

# Security Considerations

The security considerations of {{-link}} apply in a general way,
although modeling a link as a datatype does not incur all of the
security considerations that will apply to actually interchanging
these links.

(TODO)


# IANA Considerations

TODO: This document registers the sdfType "link" in the SDF sdfType registry
(which is to be defined in the SDF specification).


--- back

# Acknowledgments
{:numbered="false"}

Discussions in the OneDM liaison organization shaped this proposal.
