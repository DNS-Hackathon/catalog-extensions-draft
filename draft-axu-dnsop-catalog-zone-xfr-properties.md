---
title: "DNS Catalog Zone Properties for Zone Transfers"
abbrev: catalog-zone-xfr-properties
category: std


docname: draft-axu-dnsop-catalog-zone-xfr-properties-00
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: Operations and Management Area
workgroup: DNSOP Working Group
keyword:
 - DNS
 - authoritative
 - catalog zones
 - properties
venue:
  group: dnsop
  type: Working Group
  mail: dnsop@iets.org
  arch: https://mailarchive.ietf.org/arch/browse/dnsop/
  github: https://github.com/DNS-Hackathon/catalog-extensions-draft
  latest:

author:
 -
    fullname: Aleksi Suhonen
    ins: A. Suhonen
    org: TREX Regional Exchanges Oy
    abbrev: TREX
    street: Kuninkaankatu 30 A
    city: Tampere
    code: 33200
    country: FI
    email: i-d-2025@ssd.axu.tm
 -
    name: Willem Toorop
    ins: W. Toorop
    org: NLnet Labs
    street: Science Park 400
    code: 1098 XH
    city: Amsterdam
    country: NL
    email: willem@nlnetlabs.nl
 -
    name: Anand Buddhdev
    ins: A. Buddhdev
    org: RIPE NCC
    street: Stationsplein 11
    code: 1012 AB
    city: Amsterdam
    country: NL
    email: anandb@ripe.net
 -
    name: Karl Dyson
    ins: K. Dyson
    org: Nominet UK
    street: Minerva House
    street: Edmund Halley Road
    street: Oxford Science Park
    city: Oxford
    code: OX4 4DQ
    country: UK
    email: karl.dyson@nominet.uk
    uri: https://nominet.uk
  
normative:

informative:

--- abstract

This document specifies DNS Catalog Zones Properties that define the primary name servers from which specific or all member zones can transfer their associated zone, as well as properties for access control for those transfers.

--- middle

# Introduction

{{!RFC9432 (DNS Catalog Zones)}} described a method for automatic DNS zone provisioning among DNS name servers by the catalog of zones to be provisioned as one or more regular DNS zones.
Configuration associated with the member zones, such as from which primary name servers and with which {{!RFC8945 (TSIG keys)}} to transfer the zones, and from which IP addresses and with which TSIG keys {{!RFC1996 (DNS notifies)}} are allowed, were assumed to be preprovisioned at the catalog consumer.

This document specifies DNS Catalog Zones Properties to specify primary name servers and TSIG keys to use to transfer the member zones in a catalog, as well as properties to specify which IP addresses, using which TSIG keys, are allowed to notify {{!RFC1996}} the secondary name server serving the member zones, in order to remove the need to preprovision those at the catalog consumers.

## Requirements language

{::boilerplate bcp14-tagged}

# Description

Body text \[REPLACE\]
    
# Catalog Zone Structure

These new properties can be at the top of the catalog zone, where they will affect all member zones, or under a member zone label, where they will affect just that member zone.

# New Properties

Body text \[REPLACE\]

## Primaries

Body text \[REPLACE\]

### TSIG Key Name

Body text \[REPLACE\]

### TLSA

Body text \[REPLACE\]

## Allow Notify

Body text \[REPLACE\]
 
## Allow Transfer

Body text \[REPLACE\]

## Allow Query

Body text \[REPLACE\]
   
# Name Server Behavior

Body text \[REPLACE\]

# Implementation and Operational Notes

Body text \[REPLACE\]

# IANA Considerations {#IANA}

IANA is requested to add the following entries to the "DNS Catalog Zones Properties" registry under the "Domain Name System (DNS) Parameters" page:

| Property Prefix | Description              | Status          | Reference         |
|-----------------|--------------------------|-----------------|-------------------|
| primaries       | Primary name servers     | Standards Track | \[this document\] |
| allow-notify    | Allow NOTIFY from        | Standards track | \[this document\] |
| allow-transfer  | Allow zone transfer from | Standards track | \[this document\] |
| allow-query     | Allow queries from       | Standards track | \[this document\] |

# Implementation Status

**\[NOTE to the RFC Editor: Please remove this section before publication\]**

This section records the status of known implementations of the protocol defined by this specification at the time of posting of this Internet-Draft {{?RFC7942}}.

# Security and Privacy Considerations

Security and Privacy Considerations

--- back

# Example Catalog with One of Everything

Example Catalog with One of Everything

# Acknowledgements
{:numbered="false"}

Thanks everybody who helped making this work possible.

# Contributors
{:numbered="false"}

Thanks to all of the contributors.

