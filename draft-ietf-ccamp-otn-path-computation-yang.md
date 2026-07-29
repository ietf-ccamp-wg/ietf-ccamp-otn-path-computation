---
title: "A YANG Data Model for requesting Path Computation in an Optical Transport Network (OTN)"
abbrev: "YANG for OTN Path Computation"
category: std

docname: draft-ietf-ccamp-otn-path-computation-yang-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "CCAMP Working Group"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "ietf-ccamp-wg/ietf-ccamp-otn-path-computation"
  latest: "https://ietf-ccamp-wg.github.io/ietf-ccamp-otn-path-computation/draft-ietf-ccamp-otn-path-computation-yang.html"

author:
  -
    name: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    name: Aihua Guo
    org: Futurewei Technologies
    email: aihuaguo.ietf@gmail.com
  -
    name: Sergio Belotti
    org: Nokia
    email: sergio.belotti@nokia.com

contributor:
  -
    name: Daniel King
    org: Old Dog Consulting
    email: daniel@olddog.co.uk

--- abstract

This document provides a mechanism to request path computation in an Optical Transport Network (OTN) by augmenting the Remote Procedure Calls (RPCs) defined in RFC YYYY.

--- middle

# Introduction

{{!I-D.ietf-teas-yang-path-computation}} describes key use cases, where a client needs to request
underlying SDN controllers for path computation. In some of these use cases, the
underlying SDN controller can control an
Optical Transport Network (OTN).

This document defines a YANG data model, which augment the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, with OTN technology-specific augmentations required to request path computation to an underlying OTN SDN controller. These models allow
a client to delegate path computation tasks to the underlying SDN controller without having to obtain OTN detailed information from the controller and performing feasible path computation itself.

## Editorial Note (To be removed by RFC Editor)

> Note to the RFC Editor: This section is to be removed prior to publication.

This document contains placeholder values that need to be replaced
with finalized values at the time of publication.  This note
summarizes all of the substitutions that are needed.

Please apply the following replacements:

- XXXX --> the assigned RFC number for this I-D
- YYYY --> the assigned RFC number for {{!I-D.ietf-teas-yang-path-computation}}
- ZZZZ --> the assigned RFC number for {{!I-D.ietf-ccamp-layer1-types}}
- KKKK --> the assigned RFC number for {{!I-D.ietf-teas-yang-te}}
- 2026-05-19 --> the actual date of the publication of this document

## Terminology and Notations

  Refer to {{?I-D.ietf-ccamp-otn-topo-yang}} and {{?I-D.ietf-ccamp-layer1-types}}
  for the OTN specific terms used in this document.

  The following terms are defined in {{!RFC7950}} and are not
  redefined here:

  *  client

  *  server

  *  augment

  *  data model

  *  data node

  The following terms are defined in {{!RFC6241}} and are not redefined
  here:

  *  configuration data

  *  state data

  The terminology for describing YANG data models is found in
  {{!RFC7950}}.

## Tree Diagram

  A simplified graphical representation of the data model is used in
  {{otn-pc-tree}} of this document.  The meaning of the symbols in these
  diagrams is defined in {{!RFC8340}}.

## Prefix in Data Node Names

  In this document, names of data nodes and other data model objects
  are prefixed using the standard prefix associated with the
  corresponding YANG imported modules, as shown in
  {{tab-prefixes}}.

| Prefix       | YANG module                      | Reference
| l1-types     | ietf-layer1-types                | \[RFCZZZZ]
| te           | ietf-te                          | \[RFCKKKK]
| te-pc        | ietf-te-path-computation         | \[RFCYYYY]
| otn-pc       | ietf-otn-path-computation        | RFCXXXX
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

# YANG Data Model for OTN Path Computation

## YANG Model Overview

The YANG data model for requesting OTN path computation is defined as an augmentation of the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, as shown in {{fig-otn-pc}}.

~~~~ aasvg
                    +--------------------------+
       TE generic   | ietf-te-path-computation |
                    +--------------------------+
                                 ^
                                 |
                                 | Augments
                                 |
                   +-------------+-------------+
       OTN         | ietf-otn-path-computation |
                   +---------------------------+
~~~~
{: #fig-otn-pc title="Relationship between OTN and TE path computation models"}

The entities and Traffic Engineering (TE) attributes, such as requested path and tunnel attributes, defined in {{!I-D.ietf-teas-yang-path-computation}}, are still applicable when requesting OTN path computation and the models defined in this document only specifies the additional OTN technology-specific attributes/information, using the attributes defined in {{!I-D.ietf-ccamp-layer1-types}}.

## Bandwidth Augmentation {#otn-te-bandwidh}

The OTN path computation model augments all the occurrences of the te-bandwidth container
with the OTN technology-specific attributes using the otn-path-bandwidth grouping defined in {{!I-D.ietf-ccamp-layer1-types}}.

## Label Augmentations {#otn-te-label}

The OTN path computation model augments all the occurrences of the label-restriction list
with OTN technology-specific attributes using the
otn-label-range-info grouping defined in {{!I-D.ietf-ccamp-layer1-types}}.

Moreover, the model augments all the occurrences of the te-label
container with the OTN technology-specific attributes using the
otn-label-start-end, otn-label-hop and otn-label-step groupings defined in {{!I-D.ietf-ccamp-layer1-types}}.

# YANG Model for OTN Path Computation {#otn-pc-yang}

~~~~ yang
{::include yang/ietf-otn-path-computation.yang}
~~~~
{: #fig-otn-pc-yang title="OTN path computation YANG module"
sourcecode-markers="true" sourcecode-name="ietf-otn-path-computation@2026-05-19.yang"}

# Security Considerations

This section is modeled after the template described in {{Section 3.7 of ?RFC9907}}.

The "ietf-otn-path-computation" YANG module defines a data model that is
designed to be accessed via YANG-based management protocols, such as
NETCONF {{?RFC6241}} and RESTCONF {{?RFC8040}}. These YANG-based management
protocols (1) have to use a secure transport layer (e.g., SSH {{?RFC4252}}, TLS {{?RFC8446}},
and QUIC {{?RFC9000}}) and (2) have to use mutual authentication.

The Network Configuration Access Control Model (NACM) {{!RFC8341}}
provides the means to restrict access for particular NETCONF or
RESTCONF users to a preconfigured subset of all available NETCONF or
RESTCONF protocol operations and content.

There are no particularly sensitive RPC or action operations.

This YANG module uses groupings from other YANG modules that
define nodes that may be considered sensitive or vulnerable
in network environments.  Refer to the Security Considerations
of {{!I-D.ietf-ccamp-layer1-types}} for information as to which nodes may
be considered sensitive or vulnerable in network environments.

The YANG module defined in this document augments the "tunnels-path-compute" and the "tunnel-actions" RPCs, defined in {{!I-D.ietf-teas-yang-te}} and in {{!I-D.ietf-teas-yang-path-computation}}, with OTN technology-specific attributes. The security considerations provided in {{!I-D.ietf-teas-yang-te}} and in {{!I-D.ietf-teas-yang-path-computation}} are also applicable to the YANG module defined in this document.

# IANA Considerations

IANA is requested to register the following URI in the "ns"
registry within the "IETF XML Registry" group {{?RFC3688}}:

~~~~
   URI: urn:ietf:params:xml:ns:yang:ietf-otn-path-computation
   Registrant Contact: The IESG
   XML: N/A; the requested URI is an XML namespace.
~~~~

IANA is requested to register the following YANG module in the "YANG
Module Names" registry {{!RFC6020}} within the "YANG Parameters"
registry group.

~~~~
   Name:         ietf-otn-path-computation
   Maintained by IANA?  N
   Namespace:    urn:ietf:params:xml:ns:yang:ietf-otn-path-computation
   Prefix:       otn-pc
   Reference:    RFC XXXX
~~~~

--- back

# OTN Path Computation Tree Diagram {#otn-pc-tree}

{{fig-otn-pc-tree}} below shows the tree diagram of the YANG data model defined in module ietf-otn-path-computation.yang. See {{?RFC8340}} for an explanation of the symbols used. The data type of every leaf node is shown near the right end of the corresponding line.

~~~~ ascii-art
{::include-fold yang/trees/ietf-otn-path-computation.tree}
~~~~
{: #fig-otn-pc-tree title="OTN path computation tree diagram"
artwork-name="ietf-otn-path-computation.tree"}

# Change Log

The initial YANG data model requesting path computation in optical networks was draft-gbb-ccamp-optical-path-computation-yang-00. This document included path computation request capabilities for WSON, Flexi-Grid and OTN technologies. However, it was proposed at IETF 113 (March 25, 2022) to split the initial document into separate documents for WDM (WSON and Flexi-Grid) and OTN technologies, as each technology may be developed and implemented separately.

The WDM technology capabilities were kept in {{?I-D.draft-gbb-ccamp-optical-path-computation-yang}}, and the OTN capabilities were moved into this document.

Editors note, please remove this appendix before publication.

{: numbered="false"}

# Acknowledgments

The authors of this document would like to thank the authors of {{?I-D.ietf-teas-actn-poi-applicability}} for having identified the gap and requirements to trigger this work.

This document was prepared using kramdown.
