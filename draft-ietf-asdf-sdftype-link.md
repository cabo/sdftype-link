---
v: 3

title: An sdfType for Links
abbrev: sdfType for Links
category: std
stream: IETF
consensus: true

docname: draft-ietf-asdf-sdftype-link-latest
number:
date:
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
- name: Ari Keränen
  org: Ericsson
  city: Jorvas
  code: '02420'
  country: Finland
  email: ari.keranen@ericsson.com

normative:
  I-D.ietf-asdf-sdf: sdf
  RFC8288: link

informative:
  RFC6690: link-format
  RFC7396: merge-patch
  I-D.laari-asdf-relations: sdfrel
  RFC9423: attr
  RFC8792: fold

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
{{-link}} defines the concept of Web Linking, which, apart from the
target URI that any link will contain, can provide additional parameters, such
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

The definitions of {{-link-format}}, {{-link}}, and {{-sdf}} apply.

{::boilerplate bcp14-tagged-bcp14}

# The sdfType "link"

The sdfType "link" is intended to be used with the SDF "type" of "object".
The members of that object are strings that are named the same as the
link parameter (attribute) names.
The special parameter name "href" is used to express the link target.
(Parameter names specific to the Constrained RESTful Environment
(CoRE) are also discussed in {{-attr}}.)

Note that attribute names and relation type names are case-insensitive
in {{-link}}.
Strings that are case-insensitive in {{-link}} MUST be in their
lowercase form when used in this specification.

An example for the instance of a link is provided in {{Section 5 of -link-format}}:

~~~ link-format
   </sensors/temp>;rt="temperature-c";if="sensor",
~~~

To hold a link like this, we could describe an SDF affordance that is
specific on the target attributes above, but does not contain
instance-specific (run-time) information on the actual URI that points
to the link target.
An sdfProperty for that could look like:

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

Further examples that show sdfType "link" in context are in {{examples}}.

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
E.g., OMA has "object links", which are basically pairs of numbers (object/instance).
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

| Name | Description        | type   | JSON Representation                | Reference |
|------+--------------------+--------+------------------------------------+-----------|
| link | A Web Link {{-link}} | object | object members for link attributes | RFCXXXX   |
{: #sdftype-r title="Registration for sdfType \"link\""}

--- back

# Examples

sdfType "link" can be used to specify links with information specific
to an organization's ecosystem.
In many cases, `sdfRef` (see {{Section 4.4 of -sdf}}) provides a
convenient way to assemble the link specification from elements that
are generic for an organization and elements that are specific to the
model being defined.

{:aside}
>
Note that `sdfRef` operates by applying the JSON Merge Patch algorithm
{{-merge-patch}} to patch the contents of the definition found at the
global name (see {{Section 4 of -sdf}}) with the contents of the
original JSON map, i.e., the definition containing the sdfRef quality
with the entry for the sdfRef quality removed.
The result of that Merge Patch is then used in place of the value of
the original JSON map.

## Constructing a Link from an Organization's Ecosystem

If the organization `example.com` provides a definition for an
`idlink` that is intended to be referenced to obtain more information
about the sdfObject `myObj`, this could look like:

~~~json
{
  "namespaces": {
    "org": "https://models.example.com/",
    "orgtypes": "https://models.example.com/#/sdfData/"
  },
  "defaultNamespace": "org",
  "sdfObject": {
    "myObj": {
      "sdfProperty": {
        "linkToInformation": {
          "description": "More info about foo",
          "sdfRef": "orgtypes:idlink"
        }
      }
    }
  }
}
~~~

This example assumes `example.com` has exported a definition for
`idlink` such as the following:

~~~json
{
  "namespace": {
    "org": "https://models.example.com/"
  },
  "sdfData": {
    "idlink": {
      "description": "Special kind of link type example org uses",
      "type": "object",
      "sdfType": "link",
      "properties": {
        "href": {
          "type": "string"
        },
        "id": {
          "type": "integer",
          "minvalue": 0,
          "maxvalue": 255,
          "description":
            "Special identifier for metadata about this link"
        }
      }
    }
  }
}



~~~

## Using with OMA test object

As a more specific example, the following assumes that
`openmobilealliance.org` has provided a definition under `objlink` for
the Object Links defined for LwM2M.
[^addref]

[^addref]: Add reference!

~~~json
{
  "info": {
    "title": "OMA LwM2M LwM2M v1.1 Test Object (Object ID 3442)",
    "version": "2025-05-05",
    "copyright": "Copyright (c) 2018-2020 IPSO",
    "license": "BSD-3-Clause"
  },
  "namespace": {
    "oma": "https://models.openmobilealliance.org/",
    "omatypes": "https://models.openmobilealliance.org/#/sdfData/"
  },
  "defaultNamespace": "oma",
  "sdfObject": {
    "LwM2M_v1.1_Test_Object": {
      "label": "LwM2M v1.1 Test Object",
      "$comment": "Simplified version of OMA Test Object with only two link type resources",
      "oma:id": 3442,
      "sdfProperty": {
        "ObjLink_Value": {
          "label": "ObjLnk Value",
          "description": "Initial value must be a link to instance 0 of Device Object 3 (3:0).",
          "oma:id": 170,
          "sdfRef": "omatypes:objlink",
          "properties": {
            "object-instance-id": {
              "$comment": "Example of refinement of link attribute",
              "maximum": 42
            }
          }
        },
        "CoreLnk_Value": {
          "label": "CoreLnk Value",
          "description": "Initial value must be \"</3442>\".",
          "oma:id": 180,
          "sdfRef": "omatypes:corelink"
        }
      }
    }
  }
}
~~~
{: post="fold"}

The following is a potential definition that openmobilealliance.org
could export that provides `sdfRef`-friendly descriptions of LwM2M
core links as well as the more LwM2M specific object links:

~~~json
{
  "info": {
    "description":
      "Common data type definitions for OMA LwM2M models",
    "copyright": "Copyright 2025 Open Mobile Alliance",
    "license": "BSD-3-Clause"
  },
  "namespace": {
    "oma": "https://models.openmobilealliance.org/"
  },
  "defaultNamespace": "oma",
  "sdfData": {
    "corelink": {
      "description": "CoRE link format link",
      "$comment": "See https://md2html-tool.com/docs/OpenMobileAlliance/LwM2M/master/e58dc1c/TS_Core/OMA-TS-LightweightM2M_Core-V1_2_2-20240613-A_full.html#Table-732-1-lessNOTIFICATIONgreater-class-Attributes",
      "type": "object",
      "sdfType": "link",
      "properties": {
        "href": {
          "type": "string"
        },
        "pmin": {
          "type": "integer",
          "minimum": 0
        },
        "pmax": {
          "type": "integer",
          "minimum": 0
        },
        "gt": {
          "type": "number"
        },
        "lt": {
          "type": "number"
        },
        "st": {
          "type": "number"
        },
        "epmin": {
          "type": "integer",
          "minimum": 0
        },
        "epmax": {
          "type": "integer"
        },
        "edge": {
          "type": "integer",
          "minimum": 0,
          "maximum": 1
        },
        "con": {
          "type": "integer",
          "minimum": 0,
          "maximum": 1
        },
        "hqmax": {
          "type": "integer",
          "minimum": 0
        },
        "dim": {
          "type": "integer",
          "minimum": 0,
          "maximum": 65535
        },
        "ssid": {
          "type": "integer",
          "minimum": 1,
          "maximum": 65535
        },
        "uri": {
          "type": "string"
        },
        "ver": {
          "type": "string"
        },
        "lwm2m": {
          "type": "string"
        },
        "_other": {
          "type": "array",
          "items":{
            "type": "string"
          }
        }
      }
    },
    "objlink": {
      "description": "OMA LwM2M Object link",
      "type": "object",
      "sdfType": "link",
      "properties": {
        "object-id": {
          "type": "integer",
          "minimum": 0,
          "maximum": 65535
        },
        "object-instance-id": {
          "type": "integer",
          "minimum": 0,
          "maximum": 65535
        }
      }
    }
  }
}
~~~
{: post="fold"}


# Acknowledgments
{:unnumbered}

Discussions in the OneDM liaison organization shaped this proposal.
