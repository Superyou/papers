

# Common

* QCM1: The effect of transient state, only one case? (Rev-B/D)

# Rev-A

* QA1: 3-level cache. 

We added the discussion in Section XXX. 

* QA2: Non-inclusive cache. 

We added the discussion in Section XXX.

* QA3: Potential attacks to speculative buffer. 

We strengthened the discussion in Section XXX.

* QA4: SPEC 2017. 

We added the results in Figure XXX.

* QA5: 8-core. 

We added the results in Figure XXX. 


# Rev-B

Thank you for the detailed comments, in retrospect, we indeed provided too much information without sufficient explanation. We have added paragraphs to explain in detail different cases in Figure 1 and a selected of examples in Figure 8. Also We added the explanation for case study based on the example in Figure 1. To accomodate the additional content, we reduced the relatively plain protocol descriptions based on the state transition graphs. 

* QB1: SpectrePrime/MeltdownPrime. 

Our design can defend these two attacks, we added the discussion in Section XXX. 

* QB2: Speculative load changes other lines?

Explain why it is not possible. 

* QB3: Transitions among SS by NSR and then transition back to NSS correctly, any proof?

* QB4: More information on verification, did it end?

* QB5: More than one copy of data for a given address from each core? Memory consistency implication?

* QB6: Corresponding programs for load/store sequence in Figure 1. 


 
* QB8: "mostly"..

* QB9: Reverse only happens at XSpec->X?

Indeed, your understanding is correct. This is the essence of the lazy speculative state transition. Even this simple modification incurs considerable extra complexity as shown in the paper. There are indeed an alternative eager approach as discussed in Section 5.1. We actually designed such a protocol as well, which indeed contains more extra states and transitions. Based on previous reviewers' feedback, we have removed that design in this submission. 

* QB10: Clarify the difference compared to InvisiSpec and CleanupSpec.

As acknowledged by other reviewers, the difference is very clear. 
-- InvisiSpec: No change to coherence protocol. Perform most speculative loads twice (redo). Install the cache line in correct state and location in cache hierarchy on by safe loads.
-- CleanupSpec: Small change to coherence protocol. Perform speculative loads just once, when they are squashed, perform clean up operations. 
-- RCP: Decouple the speculative loads into two steps, first step installs the cache line in the correct locations (cache components), trigger speculative state transitions; the second step "finalizes" the state transition to non-speculative states. The key is that: the second step can be performed concurrently with processor execution, even after the load is retired from the processor. 

* QB11: How exactly does CleanupSpec violate Property 3?

See revised Section XXX.

* QB12: "commit despite invalidation", Peekaboo or RC?

On page 191 of "a primer on memory consistency and cache coherence (first edition)", Peekaboo is a coherence problem in which "a block is prefetched, invalidated before permission is received, and then a demand reference occurs". A simple example is the following:
Core 0          Core 1
store A(=1)     load B              
store B(=1)     load A
init: A=B=0

If Core 1 prefetch A, but the line is invalidated before load A is performed, peekaboo problem occurs. For SC/TSO, load A must be "replayed" because otherwise, load B and load A are reordered---prohibited by SC/TSO. But in RC, such reordering is allowed, thus the load A does need to be replayed, this is what we mean by "commit despite invalidation". So you correctly mentioned it is a Peekaboo problem, and different MCM can resolve the problem differently. Thus, "commit despite invalidation" is indeed the correct resolution in RC. There is no ambiguation. 

# Rev-C

* QC1: Area and power overhead. 

We currently do not have a measurement for power overhead but the extra coherence traffic can contribute to that. Thus the traffic results can provide some indirect clue. 

Regarding area, the main overhead is due to speculative buffer, similar to InvisiSpec except the counting bloom filter. We provide the summary of the overhead in Table XXX. 

* QC2: More Parsec benchmark?

We made an effort to run XX more program, the results are included in Figure XX. 

* QC3: Source of performance improvement, cycle breakdown. 

We added the results and analysis in Section XXX. 

# Rev-D

* QD1: Why we need to maintain equivalent cache behavior?

Good question. Indeed, the execution can be non-deterministic. But the recent attacks such as Spectre is due to the inequivalent cache state and cache block locations between two cases: 1) a load is executed speculatively and squashed; and 2) the squashed load does not execute at all. Based on this, maintaining the equivalence between 1) and 2) is a directly way to mitigate such attacks. 

As you suggested, it might be possible for two inequivalent executions to be both safe (not vulnerable to such attacks), but it is unclear how and whether it is possible to design such a solution. So far, all existing approaches try to ensure the equivalence as the goal, no matter whether the designs indeed achieve it. As we analyzed in the paper, only InvisiSpec achieves it. 

\rev{{\em Rationale for maintaining equivalent behavior. }
The properties ensure the equivalence because 
recent attacks such as Spectre~\cite{} is due to the {\em inequivalent} cache states and cache block locations between two scenarios: (1) a $SpecRd$ is executed and squashed; and (2) the $SpecRd$ does not execute. 
Therefore, maintaining the equivalence between (1) and (2) is an
intuitive way to mitigate such attacks. 
It might be possible for two inequivalent executions to be both not vulnerable to such attacks, but it is unclear how and whether it is possible to design such a solution.
At this point, all existing approaches try to ensure the equivalence we formalized as the goal. }

* QD2: Verifying security property automatically?

If can be done, it would be very nice. But we haven't figured out how to do it. 

* QD3: Extension of proof based on two instructions to any number of instructions. 

* QD4: RCP based on RC. 

# Rev-E

* QE1: Attacks based on network bandwidth usage?

\rev{For example, with bounded NoC bandwidth,
 more traffic in NoC may increase the response
latency due to queuing. This paper assumes that
the attackers cannot sense such difference. 
In fact, it is the assumption of {\em all} existing solutions. 
Our results on InvisiSpec and CleanupSpec indicate that
both solutions incur extra coherence traffic. 
%InvisiSpec incurs more traffic due to redo of speculative loads---just slightly lower than \projectname. 
To the best of our knowledge,
no speculative execution based attacks have been demonstrated
by leveraging latency increase due to NoC contention. }

* QE2: Speculative buffer size and its implication on performance?


# Rev-F


* QF1: Further analysis on the "speedups"?

* QF2: Why CleanUP suffers from 4x more slowdown in Parsec than SPEC?

We used their original code, so it is not a part of our implementation. But we looked into the execution and find ...

* QF3: 5% slowdown with random replacement, really?



