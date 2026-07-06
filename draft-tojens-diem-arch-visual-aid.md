---
title: "DIEM Architecture Example"
category: info

docname: draft-tojens-diem-arch-visual-aid-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Applications and Real-Time"
workgroup: "Digital Emblems"
keyword:
 - DIEM architecture
venue:
  group: "Digital Emblems"
  type: "Working Group"
  mail: "diem@ietf.org"
  github: "tojens-ietf/draft-tojens-diem-arch-visual-aid"

author:
 -
    fullname: Tommy Jensen
    organization: Cloudflare
    email: tojens.ietf@gmail.com

normative:

informative:

...

--- abstract

This document defines the architecture for Digital Emblems. Standards
that define Digital Emblems are expected to do so by mapping their
mechanisms to the required and optional componented defined by this
document.


--- middle

# Introduction

Various use cases for Digital Emblems and the requirements those use
cases present are defined in (link to it). This document builds on those
requirements to define an architecture that can accommodate a diverse
set of use cases (hopefully generalized such that as-of-yet unknown use
cases can reuse this same architecture).

The subsections of the next two sections outline the checklist of
things a Digital Emblem standard MUST define to be considered a
Digital Emblem (even if to say that the optional element is being
omitted).


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Foundation and Required Components of a Digital Emblem

This section defines the elements that are common to all Digital
Emblems. Therefore, standards that define a Digital Emblem MUST
define how that emblem fulfills each part of this section.

## Digital Emblem Format

TODO: the current text presumes some custom binary format for
simplicity of defining an example architecture. In reality, this should
probably be some existing data structure format standard, but the
proposed fields remain the same

To accommodate Digital Emblem standards that may require extensive data
payloads, the base format of a Digital Emblem is purposefully kept as
small as possible with an extensible payload. Digital Emblem standards
MUST define the semantics of that payload.

Format:
- FQDN: the asset identifier, a length-prefixed UTF-8 string
- Digital Emblem Type: 16-bit integer from IANA Digital Emblem Type registry
- Payload: length-prefixed data field whose semantics are defined by the Digital Emblem Type

The Digital Emblem Type of 0 is defined by this document to provide
standards with a reusable parsable format. This format of the payload
is defined as:
- a length-prefixed string: this is a MIME type identifier
- the rest of the payload: in the format defined by the MIME type

## Digital Emblem Discovery

Because Digital Emblems are defined as being identified by FQDNs,
discovering them via the DNS is likely to be a common scenario.
This document defines such a mechanism that Digital Emblem standards
can use. They MUST define how their emblems are discovered either by
using this mechanism, building on this mechanism, or defining a new
mechanism of their own. Use of this mechanism is simply defined as using
standard DNS queries to retrieve the DIEM records associated with the
FQDN of the asset bearing the emblem.

<< Normative Reference: second doc defining new DIEM RR type >>

Use cases that wish to use the DIEM RR type can. Others may see fit
to define use of an existing record for (reasons). Others may use
the DIEM record but signal in a SVCB/HTTPS record that it is
available. Some may use specific labels (like a future "_de" label)
while others may not. This architecture does not define or limit
these choices. This document requires that Digital Emblem standards
MUST define how their emblems are discovered and retrieved, even
if it is as simple as "QTYPE=DIEM for the asset's FQDN".

# Optional Architecture Elements

This section defines the elements that a Digital Emblem might have if
its use case requires them, but otherwise does not have to have just
because it is a Digital Emblem. Standards that define a Digital Emblem
MUST indicate whether each of these apply or not.

## Authorization

Some Digital Emblem standards MAY require bearers to have and
demonstrate proper authorization to bear their emblem. If so, they
MUST define the following:
- what parties are authorities
- how trust in an authority is established by a validator
- how a bearer presents its authorization within its emblem's payload

Because FQDNs are used to identify assets bearing emblems, a natural
solution can be the use of DNSSEC. This inherits the trust model
already defined for DNSSEC, which then requires validators to perform
DNSSEC validation. The use of PKI is another possibility. This document
does not define how this or other approaches could be done.

## Verification

Some Digital Emblem standards MAY require some specific verification
of emblem integrity, such as having subsets of their payload signed
and verified through a different source of authority than the
authorization. If so, they MUST define how this is to function. As
this document defines the generic Digital Emblem format, there is
no data integrity verification provided by the format.

## Detection of Validation

Some Digital Emblems MAY require that bearers cannot reasonable
detect when validators are checking for their asset's emblem (or
who is checking). Others MAY require that this is known because they
have a strong audit requirement of when emblems were verified. Many
will require neither, which is why this component is optional.

If a Digital Emblem standard does either require or forbid
bearer awareness of validators checking for their asset's emblem,
it MUST define how this is to be achieved, including any necessary
modifications to other components (such as discovery). Otherwise, it
MUST simply declare that neither case applies.


# Security Considerations

The security considerations of Digital Emblem standards will be
dependent on the way they choose to implement the elements defined
in this document. However, this document uniquely introduces


# IANA Considerations

This document defines a new registry called the Digital Emblem Type
registry. This is intended to be a low bar to entry, FCFS, so that
implementors can tell which parsers to use for the emblem payload.
The zero value is reserved for a special case where the payload is
the MIME type payload defined earlier.


--- back

# Acknowledgments
{:numbered="false"}

N/A
