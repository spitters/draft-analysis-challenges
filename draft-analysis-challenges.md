---
title: "Challenges, Opportunities, and Directions for Formal Analysis in the IETF"
abbrev: "Formal Analysis Challenges"
category: info

docname: draft-analysis-challenges-latest
submissiontype: IRTF
v: 3
venue:
#  group: UFMRG
#  type: Usable Formal Methods Research Group
#  mail: ufmrg@ietf.org
  github: "chris-wood/draft-analysis-challenges"

author:
 -
    fullname: Christopher A. Wood
    organization: Cloudflare
    email: caw@heapingbits.net

normative:

informative:
   ProVerif: DOI.10.1561/3300000004
   Tamarin: DOI.10.3233/JCS-210053


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
analysis into the process by which the specifications are ratified.

Formal analysis gives us confidence in the protocols we specify and deploy. It
advocates for a rigorous approach to defining protocols and describing their security
properties. Failure to apply formal analysis does not definitively yield protocols
with known flaws, but it can lead to security failures in practice. Infamous examples
as of late include Matrix, Telegram, PGP, and various configurations of TLS 1.2 and
earlier versions. In contrast, applying formal analysis helps demonstrate that certain
classes of attacks are not applicable. While this does not necessarily rule out
attacks entirely, since modeling and analysis is always imprecise and requires
simplifying compromise, it has the potential to more consistently yield better outcomes.

However, there are still plenty of examples of technical specifications that lack
any sort of formal analysis. Lack of formal analysis is problematic for a number of
reasons, including, though not limited to, the following:

1. Real world vulnerabilities. Protocol specifiations may be developed with security
   or privacy vulnerabilities. Such vulnerabilities can lead to real problems for
   end users. In this sense, publishing specifications without formal analysis is
   akin to testing software in production, yet is uniquely worse since updating
   specifications requires IETF processes to enact and takes time.
1. Specification misuse. Formal analysis requires a certain degree of precision and
   rigor in which specifications for algorithms, protocols, and systems are described.
   This precision helps refine the interface for these types of objects. Lack of
   analysis can therefore lead to misuse of the object. This can in turn lead to
   undesired outcomes, including even security vulnerabilities.
1. Audience confusion. Protocol specifications without analysis may lack precision
   or specificity that often comes from formal analysis, yielding complex documents
   that are difficult to understand, use, and deploy in practice.

Lack of formal analysis comes from a variety of reasons. This document summarizes some
of those problems and discusses opportunities for potential solutions and new directions
the community can explore to improve the situation. It is meant to encourage discussion,
not be published as an RFC.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Analysis Overview

As the IETF is primarily tasked with producing technical specifications for protocols,
analysis of these protocols can benefit greatly from computer-aided tools. For example,
{{ProVerif}} and {{Tamarin}} are both tools that protocol designers can use to model
their protocol, attacker, and prove (or disprove) certain properties of the protocol.
As with any modeling tool, these are limited both by the specificity of the model as well
as the questions that are asked of the model. For example, security protocol models often
make simplifying assumptions about the underlying cryptographic objects used in the protocol,
whereas in practice these objects may break over time. Hand-written proofs complement
computer-aided, machine-checked proofs. These proofs are often more complicated since they
more accurately model all aspects of the protocol. As a result, these proofs yield
mathematically precise statements of security.

Regardless of the mechanism, analysis generally follows the following recipe:

1. Specify the threat model, including the attacker and its capabilities.
2. Specify the security goals with respect to this threat model.
3. Describe whether the security goals are met and, if and when they are not met, a
   description of how they are not met through an exemplary attack that violates the goal.
   Applicable attacks are often accompanied with explanations of why they are out of scope
   or how applications and deployments can deal with the attack in practice.

The order of these tasks is not always the same, but the content is generally consistently applied.
What differs across technical specifications is the rigor through which these tasks are achieved.
As an example, TLS 1.3 ({{Appendix E of ?TLS13=RFC8446}}) includes a list of security properties
and, for each property, describes the attacker model in which these properties are satisfied.
It additionally points to relevant, peer reviewed formal analysis work that demonstrates validity of these claims.
QUIC {{?QUIC=RFC9000}} lists various types of attacker models and, within each attacker model,
the types of attacks that are possible. However, unlike {{TLS13}}, does not have backing formal
analysis for all of claims about attack resistance. As another example, Oblivious
HTTP {{?OHTTP=I-D.ietf-ohai-ohttp}} takes an even different approach by informally describing
the threat model and security goals of the protocol, with a reference to backing analysis
(that did not receive peer review) with Tamarin.

No two analyses are the same in breadth or depth. The following section describes a number of
challenges that lead to this inconsistency, and includes recommendations for IETF contributors
to help improve the consistency with which analysis is done.

# Formal Analysis Challenges and Open Problems

This section describes some high level problems that impact the scale at which
the IETF and IRTF applies formal analysis to technical specifications.

## Timelines

Sometimes formal analysis does not occur due to mismatched timelines. Formal analysis
takes time, and that time may not align with the desired pace at which a technical
specification moves towards publication. The general requirement for advancing technical
specifications in IETF and IRTF groups is rough consensus and running code, and those
are often easier to achieve than formal analysis. In many cases, the pressure to ship
or otherwise advance specifications often trumps the desire for formal analysis.

Analysis is time consuming, perhaps even more so than the development
of the specification itself. While the iterative process TLS 1.3 established between
specification, analysis, and experimentation is ideal, often times many protocols are
fully specified prior to analysis. Applying analysis after specification in sequence,
rather than in applying analysis in parallel with protocol specification, means that
the result on publishing time is worsened.

Overcoming this challenge may require a change in the way in which we establish
consensus for technical specifications. Some possibilities are below.

1. Establish a set of common questions and formal process for formal analysis. This may include,
   for example, a template by which analysis findings may be documented, as well as key
   questions to ask. It may also include standardized notation that document editors can
   use for noting analysis-relevant questions and issues in their protocol documents.
1. Seek out formal analysis for working group documents as soon as possible after adoption.
   This helps ensure that analysis occurs in line with protocol specification to the maximum
   extent possible.
1. Encourage protocol editors and expert reviewers to share analysis results with the relevant
   working group(s) as early as possible. If possible, these findings can be shared during
   in-person meetings, but may also be shared on the mailing list.

## Tooling

Another barrier to scaling formal analysis is the state of tooling. While there exists
a wide variety of tools by which protocol designers can model and analyze their protocol,
including {{ProVerif}} and {{Tamarin}}, not all tools are easy-to-use for protocol
designers or provide the necessary set of features required for analysis. For example,
modeling privacy -- for some definition of privacy -- is often a difficult task with
tools like Tamarin and ProVerif.

There may be multiple ways by which these obstacles are overcome. For example,
the community might invest or otherwise contribute to the ongoing development and
maintenance of these tools. In general, they are often maintained by academic researchers,
including graduate students, who leave the project or move on to other things over time.
Like an critical open source project, these tools require consistent and reliable
support to encourage continued maintenance and development to better fit the needs
of the community.

Beyond explicit tooling support, the amount of educational resources available for
consumption, including examples, demos, running code, etc., could be improved and made
more approachable for newcomers. While there exists plenty of learning material for
these tools, they are often oriented towards those with a background or familiarity
with formal methods. Educational material oriented towards engineers is needed.

## Incentives

Lack of incentives is a large barrier to adopting formal analysis in practice.
From a research perspective, there is often little incentive to work on formal analysis
for "uninteresting" or "overly simple" protocols, including those that seem to be
"obviously correct." Publishing formal analysis with negative results, i.e., which
demonstrate that there are no problems with a technical specification, is often
challenging. And without proper publishing incentives, there is little incentive
for academic researchers to spend time on this type of work. Addressing this
problem may require establishing a venue for more easily publishing formal analysis
without compromising on the quality of peer review. Such venue should make clear
that publishing analysis of any type is appropriate, including formal models, pen
and paper proofs, and so on.

From the specification editor's perspective, sometimes analysis can seem unnecessary,
perhaps because the object being specified is indeed "obviously correct" or that
security considerations with proven interoperability seem sufficient. However, as
has been demonstrated, security considerations are not a suitable replacement for
security analysis. In general, the problem here seems to be that the type of analysis
that would be sufficient to provide confidence in the specification is not clear.
The IETF and IRTF community needs guidance on what type of analysis is appropriate.
As obvious examples, cryptographic algorithms require analysis. Network protocols
such as TLS also require analysis, but of a different type. The community could
work on collecting examples of formal analyses to use as guidance in determining
the suitable amount of analysis.

## Complexity



- Specifications are complex, leading to large analysis learning curves.
- Specifications are imprecise. English language is a barrier to non-native speakers. Although it's good for intuitition, it's not the best for details.

## Resource Limits

Analysis is also impeded by a limitation of resources. Formal analysis is not an
easy task. The tools, techniques, and methodologies for applying formal analysis
vary in maturity, and the people generally capable of contributing such analyses
are far fewer than the number required to analyze all known protocols. Moreover,
the domain expertise required to formulate and represent and describe a suitable
threat model is often difficult to acquire for those that do have the skills to
perform formal analysis. For better or worse, specification editors are often not
equipped to formulate problem statements, threat models, and security definitions.

Specification editors can take steps to help bridge the gap between themselves and
those capable of performing protocol design and analysis. Some possible actions are below.

3. Identify and collaborate with domain experts who might offer expertise for their
   protocol. If domain experts are unknown, consult the WG chairs or security area
   contributors for assitance.
1. Include the protocol's desired security properties and threat model in the protocol
   document. This can be done in the security considerations or elsewhere, but including
   it somewhere helps give domain experts and reviewers a target for analysis.
1. Include known implementations of protocols somewhere alongside the protocol being developed.
   Some analysis efforts focus on the gap between specifications and implementations, and
   reviewing implementations is an important part of assessing that gap.
1. Document all known gaps between the protocol specification and known formal analysis.
   This helps ensure that the results of the analysis are properly represented to consumers
   of protocol specification documents.
1. Identify open issues or aspects of the protocol that require formal analysis during development
   of the protocol specification. One way to do this is to note that the entire draft is still a
   work in progress, whereas another way is to highlight specific properties of the protocol that
   require more analysis.

Conversely, protocol designers and researchers can also take steps to bridge the gap
between their skill set and the actual technical specifications. Some possible actions are below.

1. Collaborate with protocol document editors to determine what are ongoing analysis
   tasks. The end goal here is to minimize conflicts and competition that might emerge
   with different experts performing reviews.
1. Ensure that analysis work that requires running code, such as formal analyses using
   modeling tools such as ProVerif and Tamarin, are understandable and reproducible.
   It should be possible for any contributor in the IETF to re-run the analysis and
   understand its results.

# Security Considerations

This document discusses challenges for applying formal analysis to technical specifications
produced by the IETF and IRTF. It does not raise new security considerations.

# IANA Considerations

This document has no IANA actions.


--- back

# Acknowledgments
{:numbered="false"}

This document was influenced by conversations with many people that led to the UFMRG formation.
