# Charter proposal for a LOOPS WG, version 0.4.0

## Background

The Internet has a long history of employing performance enhancing
proxies (PEPs, RFC 3135) to improve performance over paths with links
of varying quality (e.g., pilc wg).  Today's PEPs often interact deeply and
"transparently" (intrusively) with end-to-end transport and
application layer protocols.  This practice is coming to an end with
the increasing deployment of encryption.

At the same time, networks are becoming more complex, and
network nodes are becoming more powerful.  It is thus more viable
to trade processing power in network nodes against path quality, in
particular for expensive path segments.  Transport protocols and their
implementations are moving towards playing better with forwarding node
functions such as ECN marking and AQM.

## LOOPS

LOOPS (Local Optimizations on Path Segments) attempts to capture
opportunities opened by these developments, enabling optimizations
within some segments of an end-to-end path.  Typically, these segments will be delimited
by tunnels maintained by dedicated overlay nodes (tunnel endpoints), which allows a local
optimization protocol to run between these nodes.  Many end-to-end
flows can be aggregated into such tunnel being optimized.

LOOPS will focus first on path segments that do not include
either end hosts, and where the primary congestion signaling method is
ECN (which typically requires all routes on the path segment to be
ECN-enabled).  Also, multipath forwarding will not be addressed and is left for future consideration.

The selection of the nodes that will run LOOPS optimization is deployment-specific, and is as such out of the scope.

The functions to be addressed by LOOPS include:

* Local recovery:  Packet losses on the path segments are recovered
  autonomously, removing the need to burden the entire end-to-end path
  with the recovery, and decreasing the latency by which these
  recoveries can be effected.  The protocol will be designed so that
  local recovery can be based on forward error correction (FEC) and/or
  (non-persistent) retransmission. The initial focus will be on
  retransmission, but at least one FEC scheme will be included in
  order to exercise the protocol mechanisms and to provision for better integration of future FEC schemes.

* Local measurement: To properly parameterize the LOOPS algorithms
  (e.g., RTO, FEC rate, acceptable overhead) and to trigger state transitions, measurements
  are performed on a LOOPS segment. The frequency for conducting such measurements will be further detailed as part of the protocol specification.

* Interaction with end-to-end congestion control:  Based on
  configuration and measurement information, losses can possibly be
  categorized into congestion and non-congestion losses.  As well as
  Congestion Experienced (CE) events on the path, the losses that cannot be positively
  identified as non-congestion losses are relayed as congestion events
  to the end-to-end congestion control. Initially, LOOPS will focus on
  the use of ECN for this and therefore only supports ECN-enabled
  end-to-end flows.  Circuit breakers (RFC 8084) will be employed on
  the LOOPS tunnel to further protect against congestion collapse.

The LOOPS protocol will need to run embedded into a tunneling
protocol.  Initially, this will be Geneve (Generic Network Virtualization Encapsulation).  To ease support of further encapsulations in the future, LOOPS will be defined with
a separation in mind between a generic information model level, and a
set of protocol bindings that will start out as the Geneve embedding.

In order to drive the protocol specification work, the working will consider developing a problem and use cases document. The working group may decide to not publish the use cases document as an RFC. Likewise, the WG may decide whether to publish the Geneve LOOPS extension as a separate document or as an applicability of the generic LOOPS information model specification. These decisions will be made during the lifetime of the working group.


## Relationship to IRTF RGs

LOOPS will actively consult ICCRG, for congestion control issues, and
NWCRG, for FEC encoding and encapsulation issues.  In particular,
documents of the latter will serve as a basis for the FEC-based
components of LOOPS.

## Relationship to IETF WGs

In addition to NWCRG mentioned above, TSVWG will be consulted with
respect to their FEC-related work.
(TSVWG and TCPM will also be important sources of information on
recent developments on mechanisms used in LOOPS itself, such as ECN,
time-based recovery, as well as, with QUIC and AVTcore, on the
characteristics of end-to-end transport flows.)

LOOPS will interact with NVO3 as the home of
the Geneve tunneling protocol.

## Milestones

* LOOPS specification(s), WG document adopted, October 2020
* LOOPS specification(s), PS to IESG, October 2021
