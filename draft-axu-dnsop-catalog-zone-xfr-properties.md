---
title: "DNS Catalog Zone Extensions"
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
    fullname: Aleksi Suhone
    ins: A. Suhonen
    org: TREX Regional Exchanges Oy
    abbrev: TREX
    street: Kuninkaankatu 30 A
    city: Tampere
    code: 33720
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
  
normative:

informative:

--- abstract

The Catalog Zones RFC defines some basics for using a DNS zone to configure secondary DNS servers, but it does not define all the basics.
This document extends the definition to include all the basics.

--- middle

# Introduction

The Catalog Zones RFC {{!RFC9432}} defines the terminology and some basics for using a DNS zone to configure secondary DNS servers, but it does not define all the basics.
This document extends the definition to include all the basics.

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

## Allow Transfer

Body text \[REPLACE\]

## Allow Query

Body text \[REPLACE\]

## Allow Notify

Body text \[REPLACE\]
    
# Name Server Behavior

Body text \[REPLACE\]

# Implementation and Operational Notes

Body text \[REPLACE\]

# IANA Considerations {#IANA}

IANA has created the "DNS Catalog Zones Properties" registry under the "Domain Name System (DNS) Parameters" registry.
The following properties are added to the registry:

# Security Considerations

This document should not affect the security of the Internet. \[CHECK\]

--- back

# Example Catalog with One of Everything

This becomes an Appendix \[REPLACE\]

# Acknowledgements
{:numbered="false"}

Thanks everybody who helped making this work possible.

# Contributors
{:numbered="false"}

Thanks to all of the contributors. \[REPLACE\]

