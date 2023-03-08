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
title: "Challenges, Opportunities, and Directions for Formal Analysis in the IETF"
abbrev: "Formal Analysis Challenges"
category: info

docname: draft-analysis-challenges-latest
submissiontype: IRTF
number:
date:
consensus: true
v: 3
venue:
  group: UFMRG
  type: Usable Formal Methods Research Group
  mail: ufmrg@ietf.org
  github: chris-wood/draft-analysis-challenges

author:
 -
    fullname: Christopher A. Wood
    organization: Cloudflare
    email: caw@heapingbits.net

normative:

informative:


--- abstract

This document discusses challenges, opportunities, and directions
for formal analysis of protocols developed in the IETF. 

--- middle

# Introduction

The IETF and IRTF produce technical specifications for protocols used on the Internet.
Applications use these technical specifications in shipping software. Often times,
use of these technical specifications has a direct impact on end users of the Internet.
As an obvious example, TLS 1.3 {{?TLS13=RFC8446}} is the de-facto transport security 
protocol used on the Internet, protecting data in transit between different different
parties on the Internet.
 
The process by which TLS 1.3 was developed was unique for its time.
 
- Not all IETF drafts get analysis before publication
- Why is this bad? Vulnerbailities. Like testing in production, except without the ability to patch easily.
- Why is analysis good? Confidence --> see TLS 1.3
- There are lots of reasons analysis does not scale to IETF protocol specifications. This document summarizes some of those problems and discusses opportunities for potential solutions and new directions the community can explore to improve the situation. It is meant to encourage discussion.


# Conventions and Definitions

{::boilerplate bcp14-tagged}
 
# Timelines
 
 - Mismatched timelines. 
    - Too much work to do in the allotted time frame. Specifications may have shipping pressure from industry.
    - Don't want analysis to impede progress
 
# Tooling
 
 - Inadequate tooling.
    - Tooling is inadequate or not fit for purpose. Moreover, tools often die or are not well maintained.
 
# Complexity
 
 - Specifications are complex, leading to large analysis learning curves.
- Specifications are imprecise. English language is a barrier to non-native speakers. Although it's good for intuitition, it's not the best for details. 
 
# Incentives
 
 - Lack of incentives. 
    - For researchers, publishing "negative results," i.e., "this protocol is good" is hard to do as no one is really interested in publishing uninteresting results. 
        - What exactly would be published? The case study? Code in the model?
    - For editors, analysis may seem unnecessary ("too simple" or "we wrote security considerations")
        - "Too trivial" -- some folks may consider analysis to be unnecessary based on the simplicity of a protocol.
        - Type of analysis is unclear. Is testing enough? Security considerations enough? Pen and paper proof enough? Security analysis and security considerations mismatch, e.g., HTTP message signatures. Considerations are not a replacement for analysis. Need guidance on what type of analysis should be considered appropriate.

 
# Scaling 

- Limited expertise. 
    - Editors are not experts or analyzers. Getting people to think abstractly is hard to do.
    - Experts are limited. There are only so many Vincents and Karthiks.


# Security Considerations

TODO Security


# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

This document was influenced by conversations with many people that led to the UFMRG formation.
