---
title: "Bulk Report Submission for Distributed Aggregation Protocol (DAP)"
abbrev: "Bulk DAP Submission"
category: info

docname: draft-thomson-ppm-dap-bulk-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Privacy Preserving Measurement"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Privacy Preserving Measurement"
  type: "Working Group"
  mail: "ppm@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ppm/"
  github: "martinthomson/dap-bulk"
  latest: "https://martinthomson.github.io/dap-bulk/draft-thomson-ppm-dap-bulk.html"

author:
 -
    fullname: "Martin Thomson"
    organization: Mozilla
    email: "mt@lowentropy.net"
 -
    name: Alex Koshelev
    organization: Meta
    email: koshelev@meta.com

normative:

informative:


--- abstract

A bulk report submission endpoint and format are described
for the Distributed Aggregation Protocol (DAP).
This provides modest, but meaningful, efficiency benefits
over the core protocol for cases where an intermediary
is able to collect large numbers of reports.


--- middle

# Introduction

The Distributed Aggregation Protocol (DAP) {{!DAP=I-D.ietf-ppm-dap}}
accepts reports from many clients
and aggregates them in a manner that protects individual contributions.
The core protocol requires that each report be submitted to the DAP leader.

The assumption implicit in this design is that reports are submitted
directly by clients.
This is not necessary for security reasons
due to the encryption used
(this encryption is necessary for the portion of reports
that is intended for DAP helpers).
Clients could instead pass their reports to an intermediary for forwarding.

Use of an intermediary reduces the availability requirements of aggregators
and might remove the need for anonymizing proxy
(such as the use of Oblivious HTTP {{?RFC9458}}).
It also creates an opportunity to amortize the overheads
involved in report submission.

This document defines a bulk submissions endpoint for DAP
and defines a report submission format for use with that endpoint.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Bulk Submissions Endpoint

`{leader}/tasks/{task-id}/reports`.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
