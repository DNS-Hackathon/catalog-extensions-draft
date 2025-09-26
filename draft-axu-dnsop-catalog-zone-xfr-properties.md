---
title: "DNS Catalog Zone Properties for Zone Transfers"
abbrev: catalog-zone-xfr-properties
category: std


docname: draft-axu-dnsop-catalog-zone-xfr-properties-01
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
updates: 9432

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
 -
    name: Aram Sargsyan
    ins: A. Sargsyan
    org: Internet Systems Consortium
    email: aram@isc.org
  
normative:

informative:

--- abstract

This document specifies DNS Catalog Zones Properties that define the primary name servers from which specific or all member zones can transfer their associated zone, as well as properties related to zone transfers such as access control.

Besides the additional properties, this document updates RFC9432 by explicitly allowing CNAMEs and DNAMEs.

--- middle

# Introduction

{{!RFC9432 (DNS Catalog Zones)}} described a method for automatic DNS zone provisioning among DNS name servers by the catalog of zones to be provisioned as one or more regular DNS zones.
Configuration associated with the member zones, such as from which primary name servers and with which {{!RFC8945 (TSIG keys)}} to transfer the zones, and from which IP addresses and with which TSIG keys {{!RFC1996 (DNS notifies)}} are allowed, were assumed to be preprovisioned at the catalog consumer.

This document specifies DNS Catalog Zones Properties to specify primary name servers from which to transfer the member zones, as well as properties to specify which IP addresses, using which cryptographic keys, are allowed to notify the secondary name server serving the member zones, in order to:

  - remove the necessity to preprovision those at the catalog consumers,
  - to fascilitate ad-hoc changes, and
  - to fascilitate exceptions for individual member zones.

## Requirements language

{::boilerplate bcp14-tagged}

# Catalog Zone Structure

The new properties, specified in {{new-properties}}, MAY be at the apex of the catalog zone, where they will affect all member zones, or under a member zone label, where they will affect just that member zone. Any property under a member zone label will override that same property at the apex.

## Binding additional attributes

It is possible to distinguish groups of values with all the properties from {{new-properties}}, by adding an additional label before the property.
This allows binding additional attributes within the group, for example binding TSIG keys to certain IP addresses.

## CNAMEs and DNAMEs

This document updates {{!RFC9432}} by explicitly allowing CNAMEs {{!RFC1035}} and DNAMEs {{!RFC6672}} anywhere in the catalog.
The CNAME and DNAME RRs in an catalog zone MUST refer to names within the same (catalog) zone.
When a CNAME and DNAME RRs refer to owner names outside of the (catalog) zone, they MUST be considered invalid and MUST be ignored.

For example, using some of the properties from {{new-properties}}:

~~~ ascii-art
ZONELABEL1.zones.$CATZ                0 IN PTR   example.com.
ZONELABEL1.zones.$CATZ                0 IN DNAME customer1.config.$CATZ

primaries.customer1.config.$CATZ      0 IN A 192.0.2.53
primaries.customer1.config.$CATZ      0 IN TXT "TSIG key"
allow-transfer.customer1.config.$CATZ 0 IN CNAME internal.acls.config.$CATZ

internal.acls.config.$CATZ            0 IN APL 1:10.0.0.0/8 2:fd00:0:0:0:0:0:0:0/8
~~~

# New Properties

## Primaries

This property defines which server(s) the member zone(s) will be fetched from. The RR types on this property MUST be either A or AAAA. If there are multiple RRs, the order in which they are used or tried is undefined.
The order may be used following the default selection process in use by the catalog consumer name server software.

Different primaries MAY be distinguished by an additional label, which will allow binding additional attributes to each server.

~~~ ascii-art
primaries.$CATZ   0                  IN A 192.0.2.53

ZONELABEL1.zones.$CATZ  0            IN PTR example.com.
primaries.ZONELABEL1.zones.$CATZ  0  IN AAAA 2001:db8:35::53
~~~

If there are any RRs attached to the `primaries` that are not covered by this document, they SHOULD be ignored.

### TSIG Key Name

The `primaries` property, with or without the extra label, MAY also have a TXT resource record set (RRset), which MUST consist of a single TXT RR, which will contain the name of the TSIG key to use to protect zone transfers. The key(s) MUST be defined elsewhere, such as in the configuration file of the consumer.
If the key cannot be found, the consumer MUST NOT attempt a zone transfer from the name server addresses for which the TXT RRset was an additional attribute.
A TXT RRset for a `primaries` property containing more than a single TXT RR indicates a broken catalog zone that MUST NOT be processed (see {{Section 5.1 of !RFC9432}}).

~~~ ascii-art
ZONELABEL2.zones.$CATZ  0                IN PTR example.net.
ns1.primaries.ZONELABEL2.zones.$CATZ  0  IN AAAA 2001:db8:35::53
ns1.primaries.ZONELABEL2.zones.$CATZ  0  IN TXT "keyname-for-ns1"
ns2.primaries.ZONELABEL2.zones.$CATZ  0  IN AAAA 2001:db8:35::54
ns2.primaries.ZONELABEL2.zones.$CATZ  0  IN TXT "keyname-for-ns2"
~~~

### Signalling encrypted transports

The `primaries` property, *with* the extra label, MAY also have a TLSA RRset with one or more TLSA RRs {{!RFC6698}}.
The presence of a TLSA RRset signals support of DNS over TLS (DoT) {{!RFC7858}} or DNS over QUIC (DoQ) {{!RFC9250}} by the primary and the means by which the catalog consumer can successfully authenticate the primary.
TLSA RRsets MUST be prepended by two labels (below the `property` label *with* the extra label) indicating the decimal representation of the port number (see {{Section 3 of !RFC6698}}) and the protocol name of the transport (see {{Section 4 of ?I-D.draft-ietf-dnsop-svcb-dane-05}}).
Catalog consumers MUST transfer member zone and incremental updates over either DoT or DoQ in the presence of a TLSA RRset.

An authentication domain name (see {{Section 2 of !RFC8310}}) MAY be provided by a PTR RRset, which MUST consist of a single PTR RR, at the same name as the TLSA RRset.
When an authentication domain name is provided, catalog consumer MUST send the TLS SNI extension containing that name.

For the same reasons as given in {{Section 3.1.3 of !RFC7672}}, the TLSA RRs with certificate usage PKIX-TA(0) or PKIX-EE(1) SHOULD NOT be included.
Usage of such RRs by catalog consumers is undefined.
Catalog consumers MAY treat such RRs as "unusable".

~~~ ascii-art
ZONELABEL2.zones.$CATZ  0                IN PTR example.net.
ns1.primaries.ZONELABEL2.zones.$CATZ  0  IN AAAA 2001:db8:35::53
_853._quic.ns1.primaries.ZONELABEL2.zones.$CATZ  0  IN PTR  ns1.example.net.
_853._quic.ns1.primaries.ZONELABEL2.zones.$CATZ  0  IN TLSA 3 1 1 <SHA-256 pin>
~~~

## Notify

This property MAY be used to define the DNS NOTIFY {{!RFC1996}} message sending behavior of the consumer for the target zone(s).
A and AAAA RRsets list hosts that the consumer will send DNS NOTIFY messages to when it loads a new version of the target zone(s).

An additional label below the property name MAY be used to distinguish different groups of addresses, which will allow binding additional attributes to each group.

### TSIG Key Name

The `notify` property, with or without the extra label, MAY also have a TXT RRset, which MUST consist of a single TXT RR, which will contain the name of the TSIG key to use to sign the NOTIFY message.
The key(s) MUST be defined elsewhere, such as in the configuration file of the consumer.
If the key cannot be found, the consumer MUST NOT notify the name server addresses for which the key was an additional attribute.
A TXT RRset for a `notify` property containing more than a single TXT RR indicates a broken catalog zone that MUST NOT be processed (see {{Section 5.1 of !RFC9432}}).

~~~ ascii-art
notify.$CATZ                      0 IN A 192.0.2.49
notify.$CATZ                      0 IN TXT "name-of-default-key"

ZONELABEL3.zones.$CATZ            0 IN PTR example.org.
notify.ZONELABEL3.zones.$CATZ     0 IN AAAA 2001:db8:35::53
notify.ZONELABEL3.zones.$CATZ     0 IN TXT "keyname-for-ns4"

ns5.notify.ZONELABEL4.zones.$CATZ 0 IN AAAA 2001:db8:35::54
ns5.notify.ZONELABEL4.zones.$CATZ 0 IN TXT "keyname-for-ns5"
~~~

If there are any RRs attached to the `notify` property that are not covered by this document, they SHOULD be ignored.
 
## Allow Query

The `allow-query` property MAY be used to define an access list of hosts or networks that are allowed to send queries for the target zone(s).
The property MAY contain a RRset of type {{!RFC3123 (APL)}}, which MUST consist of only a single APL RR.
The prefixes listed in the APL RR are processed in order:
  - An IP address is allowed to query the zone when it matches a prefix.
  - An IP address is denied to query the zone when it matches a negated prefix.

The absence of an `allow-query` property at both apex of the catalog as well as at a member zone, means that the default policy applies, which may be that the member zone is allowed to be queried from any IP address without TSIG key.

Additional attributes (such as TSIG keys) can be bound to specific APL RRs by an additional label below the property label.
The prefixes (with or without attributes) will be processed in {{Section 6 of !RFC4034 (canonical order)}}, which means that the RRsets at the `allow-query` property label will be processed first, followed by the RRsets with the additional label in canonical order.
When a catalog consumer encounters an APL RRset containing more that a single APL RR, MUST be interpreted as an APL RRset containing a single APL RR denying all IP addresses, i.e.: `APL !1:0.0.0.0/0 !2:0:0:0:0:0:0:0:0/0`.

### TSIG Key Name

The `allow-query` property MAY also have a TXT RRset, which will (further) restrict the zone to be queryable only with the TSIG keys indicated in any of the TXT RRs in the set.
The key(s) MUST be defined elsewhere, such as in the configuration file of the consumer.

If a TXT RRset is present together with an APL RR, then first the policies in the APL are applied, and if that results in queries being allowed for the IP address, then in addition a TSIG key MUST match any of the TXT RRs in the TXT RRset.
If a TXT RRset is present without an APL RRset, then only a TSIG key MUST match in any of the TXT RRs in the TXT RRset, regardless of the querying IP address.

If an `allow-query` property is present *and* contains APL RRsets and/or TXT RRsets (either directly below the property label or below the additional label), *and* none of the ACLs and/or TSIG keys matched or could be found, then the consumer MUST NOT allow queries for the member zone to which the property applies.

~~~ ascii-art
ZONELABEL5.zones.$CATZ  0                IN PTR example.local.
00-internal.allow-query.ZONELABEL5.zones.$CATZ 0 IN APL 1:10.0.0.0/8 2:fd00:0:0:0:0:0:0:0/8
50-external.allow-query.ZONELABEL5.zones.$CATZ 0 IN TXT "keyname"
~~~

## Allow Transfer

The `allow-transfer` property MAY be used to define an access list of hosts or networks that are allowed to transfer the target zone(s) from the consumer.
The property MAY contain a RRset of type {{!RFC3123 (APL)}}, which MUST consist of only a single APL RR.
The prefixes listed in the APL are processed in order:
  - An IP address is allowed to query the zone when it matches a prefix.
  - An IP address is denied to query the zone when it matches a negated prefix.

The absence of an `allow-transfer` property at both apex of the catalog as well as at a member zone, signifies that transfers of the zone from the consumer MUST NOT be allowed.
Additional attributes (such as TSIG keys) can be bound to specific APL RRs by an additional label below the property label.
The prefixes (with or without attributes) will be processed in {{Section 6 of !RFC4034 (canonical order)}}, which means that the RRsets at the `allow-transfer` property label will be processed first, followed by the RRsets with the additional label in canonical order.
When a catalog consumer encounters an APL RRset containing more that a single APL RR, MUST be interpreted as an APL RRset containing a single APL RR denying all IP addresses, i.e.: `APL !1:0.0.0.0/0 !2:0:0:0:0:0:0:0:0/0`.

### TSIG Key Name

The `allow-transfer` property MAY also have a TXT RRset, which will (further) restrict the zone to be transferable only with the TSIG key indicated in any of the TXT RRs in the set.
The key(s) MUST be defined elsewhere, such as in the configuration file of the consumer.
If a TXT RRset is present together with an APL RR, then first the policies in the APL are applied, and if that results in transfers being allowed for the IP address, then in addition a TSIG key MUST match any of the TXT RRs in the TXT RRset.
If a TXT RRset is present without an APL RRset, then only a TSIG key MUST match in any of the TXT RRs in the TXT RRset, regardless of the querying IP address.

If an `allow-transfer` property is present *and* contains APL RRsets and/or TXT RRsets (either directly below the property label or below the additional label), *and* none of the APLs and/or TSIG keys matched or could be found, then the consumer MUST NOT allow transfers of the member zone to which the property applies.

~~~ ascii-art
ZONELABEL5.zones.$CATZ  0                IN PTR example.local.
00-internal.allow-transfer.ZONELABEL5.zones.$CATZ 0 IN APL 1:10.0.0.0/8 2:fd00:0:0:0:0:0:0:0/8
50-external.allow-transfer.ZONELABEL5.zones.$CATZ 0 IN TXT "keyname"
~~~

# Implementation and Operational Notes

One of the rationales for allowing CNAMEs and DNAMEs is that a large and complex catalog may have large and complex access lists repeated a million times. But if there are only a few different access lists, they could be defined separately and then be referenced a million times, reducing both the size and processing effort of the catalog zone.

# IANA Considerations {#IANA}

IANA is requested to add the following entries to the "DNS Catalog Zones Properties" registry under the "Domain Name System (DNS) Parameters" page:

| Property Prefix | Description              | Status          | Reference         |
|-----------------|--------------------------|-----------------|-------------------|
| primaries       | Primary name servers     | Standards Track | \[this document\] |
| notify          | Send DNS NOTIFY behavior | Standards track | \[this document\] |
| allow-transfer  | Allow zone transfer from | Standards track | \[this document\] |
| allow-query     | Allow queries from       | Standards track | \[this document\] |

# Implementation Status

**\[NOTE to the RFC Editor: Please remove this section before publication\]**

This section records the status of known implementations of the protocol defined by this specification at the time of posting of this Internet-Draft {{?RFC7942}}.

The existing BIND 9 implementation of `primaries`, `allow-transfer` and `allow-query` was a major inspiration for writing this draft.

# Security and Privacy Considerations

The original RFC for Catalog Zones already covers a lot of security and privacy considerations, and they are all still valid, but there are also new security considerations introduced by this document.

## Private Zone Exfiltration

If the Catalog Zone producer and consumer are different organizations, the producer may be able to use a crafted Catalog Zone to exfiltrate a private zone on another server within the consumer's network by listing it in the Catalog Zone with more permissive query or transfer access lists. The producer needs to know the exact name of the private zone and an address of the primary where the consumer may fetch it from.f

There are two ways to approach this security issue. One is to make sure that the consumer organisation does not allow zone transfers for private zones on the consuming server. Another approach is to sanitize the incoming Catalog Zone before consuming it, removing anything sensitive from it.

--- back

# Example Catalog with One of Everything

~~~ ascii-art
primaries.$CATZ   0                  IN A 192.0.2.53

ZONELABEL1.zones.$CATZ  0            IN PTR example.com.
primaries.ZONELABEL1.zones.$CATZ  0  IN AAAA 2001:db8:35::53

ZONELABEL2.zones.$CATZ  0                IN PTR example.net.
ns1.primaries.ZONELABEL2.zones.$CATZ  0  IN AAAA 2001:db8:35::53
ns1.primaries.ZONELABEL2.zones.$CATZ  0  IN TXT "keyname-for-ns1"
ns2.primaries.ZONELABEL2.zones.$CATZ  0  IN AAAA 2001:db8:35::54
ns2.primaries.ZONELABEL2.zones.$CATZ  0  IN TXT "keyname-for-ns2"

notify.$CATZ  0                          IN A 192.0.2.49

ZONELABEL3.zones.$CATZ  0                IN PTR example.org.
notify.ZONELABEL3.zones.$CATZ  0         IN AAAA 2001:db8:35::53
notify.ZONELABEL3.zones.$CATZ  0         IN TXT "no default notifies"

ZONELABEL4.zones.$CATZ  0                IN PTR sub.example.org.
notify.ZONELABEL4.zones.$CATZ  0         IN AAAA 2001:db8:35::54
notify.ZONELABEL4.zones.$CATZ  0         IN TXT ""

ZONELABEL5.zones.$CATZ  0                IN PTR example.local.
allow-query.ZONELABEL5.zones.$CATZ  0    IN APL 1:10.0.0.0/8 !1:0.0.0.0/0 !2:0:0:0:0:0:0:0:0/0
allow-transfer.ZONELABEL5.zones.$CATZ  0   IN APL !1:0.0.0.0/0 !2:0:0:0:0:0:0:0:0/0
~~~

# Acknowledgements
{:numbered="false"}

Thanks everybody who helped making this work possible.

# Contributors
{:numbered="false"}

Thanks to all of the contributors.

