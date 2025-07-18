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

These new properties MAY be at the apex of the catalog zone, where they will affect all member zones, or under a member zone label, where they will affect just that member zone. Any property under a member zone label will override that same property at the apex.

# New Properties

Body text \[REPLACE\]

## Primaries

This property defines which server(s) the zone(s) will be fetched from. The resource record types on this property MUST be either A or AAAA. If there are multiple resource records, they will be used in random order.
Different primaries MAY be distinguished by an additional label, which will allow binding additional attributes to each server.

\[WRITE EXAMPLE\]

### TSIG Key Name

The primaries property, with or without the extra label, MAY also have a TXT resource record, which will contain the name of the TSIG key to use to protect zone transfers. The key(s) MUST be defined elsewhere, such as in the configuration file of the consumer. If the key cannot be found, the consumer MUST NOT attempt a zone transfer.

\[WRITE EXAMPLE\]

### TLSA

The primaries property, with or without the extra label, MAY also have one or more TLSA resource records

\[WRITE EXAMPLE\]

## Also Notify

The also-notify property MAY be used to list hosts that the consumer will send NOTIFY messages to when it loads a new version of the target zone(s).

\[WRITE EXAMPLE\]
 
## Allow Query

The allow-query property MAY be used to define an access list of hosts or networks that are allowed to send queries for the target zone(s).
The resource record type MUST be either APL or CNAME. The APL record (RFC3123) MAY be used to define the access-list directly, while the CNAME record MAY be used to refer to an access-list already defined elsewhere. The CNAME MUST point to a name that has an APL record.

\[WRITE EXAMPLE\]
QUESTION1: should we define a label convention for pre-defining access-lists in a CATZ?
QUESTION2: should we allow pre-defined access-lists in external zones?

## Allow Transfer

The allow-transfer property MAY be used to define an access list of hosts or networks that are allowed to transfer the target zone(s) from the consumer.
The resource record type MUST be either APL or CNAME. The APL record (RFC3123) MAY be used to define the access-list directly, while the CNAME record MAY be used to refer to an access-list already defined elsewhere. The CNAME MUST point to a name that has an APL record.

\[WRITE EXAMPLE\]

# Name Server Behavior

Body text \[REPLACE\]
Default notifies? Default transfers?

# Implementation and Operational Notes

Body text \[REPLACE\]

# IANA Considerations {#IANA}

IANA is requested to add the following entries to the "DNS Catalog Zones Properties" registry under the "Domain Name System (DNS) Parameters" page:

| Property Prefix | Description              | Status          | Reference         |
|-----------------|--------------------------|-----------------|-------------------|
| primaries       | Primary name servers     | Standards Track | \[this document\] |
| also-notify     | Also send NOTIFY to      | Standards track | \[this document\] |
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

