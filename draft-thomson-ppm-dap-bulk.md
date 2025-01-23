---
title: "Bulk Report Submission for Distributed Aggregation Protocol (DAP)"
abbrev: "Bulk DAP Submission"
category: info

docname: draft-thomson-ppm-dap-bulk-latest
submissiontype: IETF
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
to protect client identity from the server
(such as the use of Oblivious HTTP {{?RFC9458}}
as described in {{Section 7.4 of DAP}}).
It also creates an opportunity to amortize the overheads
involved in report submission.

This document defines a bulk submissions endpoint for DAP
and defines a report submission format for use with that endpoint.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Bulk Submissions Endpoint

DAP defines the `{leader}/tasks/{task-id}/reports` resource
for report submission for a task.
This document extends DAP
to define the `{leader}/tasks/{task-id}/reports/bulk` resource
for bulk report submission for a task.

Clients can send a HTTP POST request to this resource
with a payload in the bulk submission format ({{format}}).
A bulk request is equivalent to multiple separate submissions.

A DAP leader MAY accept bulk submissions
at the regular "reports" resource.


# Bulk Submission Format {#format}

The "application/dap-bulk-report" format contains a header
that encodes extensions that are common to all reports.
The header is followed by any number of reports,
from which those shared fields are removed.

~~~ tls-syntax
struct {
  Extension common_extensions<0..2^16-1>;
  Report report[REPORT_COUNT];
} BulkReport;
~~~
{: #syn-bulk-report title="Bulk Report Format"}

The `common_extensions` field contains
common extensions that are added
to the set of public report extensions
in each report that follows.
Reports that include values for any common extension
override the value in the common extension.

{:aside}
> It is slightly more intrusive and fragile,
> but we should consider removing the `extensions` field
> from `ReportMetadata` as well.
> Repeating those two bytes isn't a huge problem,
> but it's pretty cheap.
> It also removes the last clause from the preceding paragraph.

The header is followed by any number of reports,
which are encoded exactly as described in {{DAP}},
except as noted in {{changes}}.
The use of `REPORT_COUNT` in {{syn-bulk-report}}
is a small abuse of the TLS syntax to signify
that any number of reports are included.
This "value" could be any positive integer.
Unlike a variable-length field,
as denoted with `&lt;` and `>`,
this format does not require that the size of all included reports
be known before constructing a request.


## Optional Removal of Existing Report Extensions {#removal}

The existing report extensions could also potentially be removed,
as shown in {{syn-plaintext-change}}.

~~~ tls-syntax
opaque PlaintextInputShare<0..2^32-1>;

/* the old format, for reference:
struct {
  Extension extensions<0..2^16-1>;
  opaque payload<0..2^32-1>;
} PlaintextInputShare;
*/
~~~
{: #syn-plaintext-change title="Proposed Plaintext Share Format"}

These private extensions currently have no defined purpose
and add two bytes per aggregator to every report.
The risk of removing them is that a purpose for a generic extension
is discovered at some point in the future.
Though specific VDAF instantiations might
define their own extension container,
this decision might limit
the availability of extensions that apply to any VDAF.


# Security Considerations {#security}

Report metadata,
which would include the extensions
if the recommendations in {{changes}} are adopted,
are included in the additional associated data
for every report.
Bulk submission is therefore strictly a performance optimization
as far as the operation of DAP is concerned.

The potential for a single client
to generate large amounts of work for a DAP service
is a serious threat to service availability.
Any DAP leader SHOULD implement measures to defend
against resource exhaustion attacks through this interface.
This might include strong authentication of the requester.
The logical entity to make this request is a collector,
which is likely to be known to the leader.

The addition of public extensions exposes more information
to the leader.
This might be used by a malicious leader
to selectively remove reports.
A leader is already able to do this without helper awareness,
but the added information might allow this to be more selective.


# IANA Considerations {#iana}

IANA is requested to register
at time of publication
the "application/dap-bulk-report" media type
in the "Media Types" registry at
[](https://iana.org/assignments/media-types){: brackets="angle"},
following the procedures of {{!RFC6838}}.
That registration includes the following:

Type name:

: application

Subtype name:

: dap-bulk-report

Required parameters:

: N/A

Optional parameters:

: N/A

Encoding considerations:

: "binary"

Security considerations:

: See {{security}}

Interoperability considerations:

: N/A

Published specification:

: this document

Applications that use this media type:

: This type identifies a bulk report submission
  for the Distributed Aggregation Protocol.

Fragment identifier considerations:

: N/A

Additional information:

: <t><br/></t><dl spacing="compact">
  <dt>Magic number(s):</dt><dd>N/A</dd>
  <dt>Deprecated alias names for this type:</dt><dd>N/A</dd>
  <dt>File extension(s):</dt><dd>N/A</dd>
  <dt>Macintosh file type code(s):</dt><dd>N/A</dd>
  </dl>

Person and email address to contact for further information:

: <br/>See Authors' Addresses section

Intended usage:

: COMMON

Restrictions on usage:

: N/A

Author:

: See Authors' Addresses section

Change controller:

: IETF
{: spacing="compact"}


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
