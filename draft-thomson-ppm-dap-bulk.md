---
###
# Internet-Draft Markdown Template
#
# Rename this file from draft-todo-yourname-protocol.md to get started.
# Draft name format is "draft-<yourname>-<workgroup>-<name>.md".
#
# For initial setup, you only need to edit the first block of fields.
# Only "title" needs to be changed; delete "abbrev" if your title is short.
# Any other content can be edited, but be careful not to introduce errors.
# Some fields will be set automatically during setup if they are unchanged.
#
# Don't include "-00" or "-latest" in the filename.
# Labels in the form draft-<yourname>-<workgroup>-<name>-latest are used by
# the tools to refer to the current version; see "docname" for example.
#
# This template uses kramdown-rfc: https://github.com/cabo/kramdown-rfc
# You can replace the entire file if you prefer a different format.
# Change the file extension to match the format (.xml for XML, etc...)
#
###
title: "Bulk Report Submission for Distributed Aggregation Protocol (DAP)"
abbrev: "Bulk DAP Submission"
category: info

docname: draft-thomson-ppm-dap-bulk-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: AREA
workgroup: WG Working Group
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: WG
  type: Working Group
  mail: WG@example.com
  arch: https://example.com/WG
  github: USER/REPO
  latest: https://example.com/LATEST

author:
 -
    fullname: Your Name Here
    organization: Mozilla
    email: your.email@example.com
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
