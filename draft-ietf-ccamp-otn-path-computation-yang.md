---
coding: utf-8

title: A YANG Data Model for requesting Path Computation in an Optical Transport Network (OTN)

abbrev: YANG for OTN Path Computation
docname: draft-ietf-ccamp-otn-path-computation-yang-01
submissiontype: IETF
workgroup: CCAMP Working Group
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

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

\[RFC EDITOR NOTE: Please replace RFC YYYY with the RFC number of
draft-ietf-teas-yang-path-computation once it has been published.

--- middle

# Introduction

{{!I-D.ietf-teas-yang-path-computation}} describes key use cases, where a client needs to request
underlying SDN controllers for path computation. In some of these use cases, the
underlying SDN controller can control an
Optical Transport Network (OTN).

This document defines a YANG data model, which augment the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, with OTN technology-specific augmentations required to request path computation to an underlying OTN SDN controller. These models allow
a client to delegate path computation tasks to the underlying SDN controller without having to obtain OTN detailed information from the controller and performing feasible path computation itself.

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

RFC Editor Note:
Please replace XXXX with the RFC number assigned to this document.
Please replace YYYY with the RFC number assigned to {{!I-D.ietf-teas-yang-path-computation}}.
Please replace ZZZZ with the RFC number assigned to {{!I-D.ietf-ccamp-layer1-types}}.
Please replace KKKK with the RFC number assigned to {{!I-D.ietf-teas-yang-te}}.
Please remove this note.

# YANG Data Model for OTN Path Computation 

## YANG Model Overview

The YANG data model for requesting OTN path computation is defined as an augmentation of the generic Path Computation RPC defined in {{!I-D.ietf-teas-yang-path-computation}}, as shown in {{fig-otn-pc}}.

~~~~ ascii-art
                    +--------------------------+    o: augment
       TE generic   | ietf-te-path-computation |
                    +--------------------------+
                                 o 
                                 |
                                 |
                                 |
                   +---------------------------+
       OTN         | ietf-otn-path-computation |
                   +---------------------------+
~~~~
{: #fig-otn-pc title="Relationship between OTN and TE path computation models"}

The entities and Traffic Engineering (TE) attributes, such as requested path and tunnel attributes, defined in {{!I-D.ietf-teas-yang-path-computation}}, are still applicable when requesting OTN path computation and the models defined in this document only specifies the additional OTN technology-specific attributes/information, using the attributes defined in {{!I-D.ietf-ccamp-layer1-types}}.

The YANG module ietf-otn-path-computation defined in this document conforms
to the Network Management Datastore Architecture (NMDA) defined in
{{!RFC8342}}.

{: #otn-te-bandwidh}

## Bandwidth Augmentation

The OTN path computation model augments all the occurrences of the te-bandwidth container
with the OTN technology-specific attributes using the otn-link-bandwidth and otn-path-bandwidth groupings defined in {{!I-D.ietf-ccamp-layer1-types}}.

{: #otn-te-label}

## Label Augmentations

The OTN path computation model augments all the occurrences of the label-restriction list
with OTN technology-specific attributes using the
otn-label-range-info grouping defined in {{!I-D.ietf-ccamp-layer1-types}}.

Moreover, the model augments all the occurrences of the te-label
container with the OTN technology-specific attributes using the
otn-label-start-end, otn-label-hop and otn-label-step groupings defined in {{!I-D.ietf-ccamp-layer1-types}}.

{: #otn-pc-tree}

# OTN Path Computation Tree Diagram

{{fig-otn-pc-tree}} below shows the tree diagram of the YANG data model defined in module ietf-otn-path-computation.yang.

~~~~ ascii-art
{::include ./ietf-otn-path-computation.tree}
~~~~
{: #fig-otn-pc-tree title="OTN path computation tree diagram"
artwork-name="ietf-otn-path-computation.tree"}

{: #otn-pc-yang}

# YANG Model for OTN Path Computation

~~~~ yang
{::include ./ietf-otn-path-computation.yang}
~~~~
{: #fig-otn-pc-yang title="OTN path computation YANG module"
sourcecode-markers="true" sourcecode-name="ietf-otn-path-computation@2022-07-10.yang"}

# Manageability Considerations

This document provides a method for requesting path computations for OTN tunnels. Consideration of mechanisms to gather and collate information required for the path computations will be necessary. Furthermore, storing path computation requests and responses and triggering actions will also need to be carefully managed and secured.

Future versions of this document will contain additional information.

# Security Considerations

The YANG module defined in this document will be accessed via the NETCONF protocol {{!RFC6241}} or RESTCONF protocol {{!RFC8040}}. The lowest NETCONF layer is the secure transport layer, and the mandatory-to-implement secure transport is Secure Shell (SSH) {{!RFC6242}}. The lowest RESTCONF layer is HTTPS and the mandatory-to-implement secure transport is TLS {{!RFC8446}}.

The Network Configuration Access Control Model (NACM) {{!RFC8341}} provides the means to restrict access to particular NETCONF or RESTCONF users to a pre-configured subset of all available NETCONF or RESTCONF protocol operations and content.

Some of the RPC operations defined in this YANG module may be
considered sensitive or vulnerable in some network environments. It is thus essential to control access to these operations.

Operations defined in this document, and their sensitivities and possible vulnerabilities, will be discussed further in future versions of this document.

# IANA Considerations

   This document registers the following URIs in the "ns" subregistry
   within the "IETF XML registry" {{!RFC3688}}.

~~~~
  URI: urn:ietf:params:xml:ns:yang:ietf-otn-path-computation
  Registrant Contact:  The IESG.
  XML: N/A, the requested URI is an XML namespace.
~~~~

   This document registers the following YANG module in the "YANG Module Names"
   registry {{!RFC7950}}.

~~~~
  name:      ietf-otn-path-computation
  namespace: urn:ietf:params:xml:ns:yang:ietf-otn-path-computation
  prefix:    otn-pc
  reference: this document
~~~~

--- back

# Change Log

The initial YANG data model requesting path computation in optical networks was draft-gbb-ccamp-optical-path-computation-yang-00. This document included path computation request capabilities for WSON, Flexi-Grid and OTN technologies. However, it was proposed at IETF 113 (March 25, 2022) to split the initial document into separate documents for WDM (WSON and Flexi-Grid) and OTN technologies, as each technology may be developed and implemented separately.

The WDM technology capabilities were kept in {{?I-D.draft-gbb-ccamp-optical-path-computation-yang}}, and the OTN capabilities were moved into this document.

Editors note, please remove this appendix before publication.

{: numbered="false"}

# Acknowledgments

The authors of this document would like to thank the authors of {{?I-D.ietf-teas-actn-poi-applicability}} for having identified the gap and requirements to trigger this work.

This document was prepared using kramdown.
