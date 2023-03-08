---
title: "Challenges, Opportunities, and Directions for Formal Analysis in the IETF"
abbrev: "Formal Analysis Challenges"
category: info

docname: draft-analysis-challenges-latest
submissiontype: IRTF
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
 
The process by which TLS 1.3 was developed was unique for its time. In the wake of
one too many security problems with prior versions of the protocol, TLS 1.3 was
developed in close collaboration with security researchers to ensure that the resulting
design and specification provided provably secure properties that one would expect
from such a protocol. The process took years and resulted in dozens of academic
publications and interoperable implementations, further improving the community's
confidence in the result. And in the end, the work paid off: TLS 1.3 has be safely
deployed at scale for years without any significant issues.

Several IETF protocols since TLS 1.3 have built on the process by which it was
developed, including QUIC {{?RFC9000}}, MLS {{?MLS=I-D.ietf-mls-protocol}},
Oblivious HTTP {{?OHTTP=I-D.ietf-ohai-oblivious-http}}, and Privacy Pass {{?PRIVACYPASS=I-D.ietf-privacypass-architecture}}.
Each of these published and developing specifications have incorporated security
analysis into the process by which the specifications are ratified. However,
there are still plenty of examples of technical specifications that lack
any sort of formal analysis.
 
- Not all IETF drafts get analysis before publication
- Why is this bad? Vulnerbailities. Like testing in production, except without the ability to patch easily.
- Why is analysis good? Confidence -- see TLS 1.3
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
