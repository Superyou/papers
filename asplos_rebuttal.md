We thank all reviewers for the high quality comments.

#Common

* **QCM1: Security thread from speculative transmission of non-speculatively accessed secret data? (Rev-C/D)**

Use Chris MICRO'21 paper.

Interestingly, Rev-D (even if the score is D) even mentioned this in the summary "The paper has an example of constant time cryptographic code that might be followed by some sort of a Spectre gadget that would speculatively transmit the cryptographic key. Prior work does not consider this case because the memory access was performed architecturally." but he still asks for an example....

* **QCM2: Cannot measure the latency change due to the amount of coherence traffic? (Rev-E/G)**

While it might be possible to develop attacks based on that, however, all current ISLE schemes increase coherence traffic, and if it is the case, all schemes are vulnerable. So we believe it is fair to include that. In fact, it should be the implicit assumption for all other work, we just try to be more rigorous. 

* **QCM3: "block the transmission of secretes accessed non-speculatively.."? (Rev-D/E)**

Sorry, we should be more specific: block the **speculative** transmission.. since the paper is about invisible speculative execution. ISE does nothing to block the transmission due to non-speculative execution. 

* **QCM4: Simply an extension of InvisiSpec? (Rev-C/F)**

-- The problem is far more challenging than InvisiSpec, we argue that InvisiSpec is not designed in the way since it is too difficult. Such a complex protocol cannot be a minor contribution, otherwise, why other papers didn't take this approach?

-- For F, the specBuf is similar but in order to support the contribution of our protocol operations, we have some modifications. The main contribution of our paper is, as you said, the protocol. 

* **QCM5: Very hard to follow.(Rev-C/D/E/F/G)**

Yes, we agree that the content is dense and sometimes we can do nothing about it. However, since the previous versions, we have made a big effort to address this, by adding examples shown in Figure 1and discuess one of them in Section 6. 
The MICRO'21 reviewers appreciated that and they can understand the key ideas. 

Could you follows our ideas based on these examples?


* **QCM6: Dolma from Usenix Security'21. (Rev-D/E/F)**

When we submitted this paper to ASPLOS we did not realize a similar literature is accepted in Usenix Security'21 thus we did not cite it in our paper. 
After reading the paper, we think both Dolma and us have the similar idea to selectively issue instructions/micro-ops during transient execution and ensures that it may not cause observability by the attacker. 

Compared with us, Dolma has a similar design to evict a low-priority instruction when a high-priority instruction competing the resource with it. Different from us, Dolma 
uses Delay-On-Miss strategy to selectively issue the instructions that is hit on the L1 cache, whiling delaying those got cache misses. Using our RCP protocal, we could selectively issue all of the instructions without delays and leverage
 RCP to hide the observability. 

Another different point is the assumption about speculative stores. In our paper, we assume that speculative store will not be issue to the memory backend since the implementation of store buffer will isolate it from transient speculation.
However, Dolma think the speculative store will leverage the TLB as a side channel even if the cache end is invulnerable.  


# Rev-A
Score: B

* **QA1: Compare with STT+SDO?**

-- We can add results in final version. 
-- Even STT+SDO is weaker than ISE

* **QA2: 3-level cache?**


RCP can be extended to 3-level cache with a shared L3 and private L1 and L2. The cache state and transitions of the
two private cache levels can be developed in a similar manner
as the private L1 in RCP; and the L3 shared level cache is
similar to L2 in RCP. The interactions between L1 and L2 in
RCP resemble the interactions between L2 (the last level of
private cache) and the shared L3. The operations in L1 and
L2 are similar. While there must be special cases, we do not
see fundamental obstacles.


The design principles can work in the new setting: (1)
speculative load requests will be downward propagated from
L1 to a lower level, which can serve or forward the request.
Whenever such request travels to a cache component, specBuf
entry is allocated and the state transitions are triggered. (2)
PrMerge and L1Merge follow the same downward path to
trigger the delayed final state transition and merge of specBuf
entry. The shared L3 may forward L1Merge upward to the
relevant cache components (e.g., to the current owner so that
it transitions to shared). (3) PrPurge and L1Purge follow the
same downward path to trigger state rollback and specBuf
data elimination but are never sent upward. The key observation is, the downward and upward message propagation path
and the relevant cache components are known according to
how a normal load is performed.


RCP can be in principle extended to non-inclusive cache as well.
The difference between inclusive and non-inclusive is, the
insertion of a line in a cache may cause the line to be deleted
from other caches. Based on RCP, we can augment the
state of a cache line with the destination location, which
provides the information on which cache the line will be
moved to on merge. It should be incorporated to the actual
non-inclusive policy implementation which should already
have such structure [12]. In general, non-inclusive policy
may incur less extra traffic for merge and purge, because
less cache components keep the data. We leave the concrete
extension to non-inclusive cache as future work.


# Rev-C
Score: C (this is Moin, again)

We appreciate the fact you have reviewed the paper 4-5 times, and all comments you provided. As you can see we have tried the best to improve the quality. 
This submission might be new to you since we changed the claim and design to be a comprehensive one to support ISE, not ISLE, since the key reason the previous rejection is that ISLE is insufficient. 
Perhaps that comment is from you, since you repeated that in the end. However, please note that our current solution, which use RCP as a part of ISE is comprehensive. 
You mentioned STT is more relevant, we agree, and we have shown that: ISE is more comprehensive than STT. 

So instead of rejecting and reviewing the paper again in ISCA, can you carefully consider our argument, to see whether it addresses your concern? 

You can also see that other reviewers actually buy our argument. I think your comments are majorly due to the fact that you reviewed our previous version several times, 
I hope you can avoid the stereotype and serious consider our new way of presenting the idea. It is exactly changed in this way to accomodate previous comments. 


* **QC2: Novel support for ISE? Key contribution.**

The ISE and ISLE discussion is not misplaced, since you have reviewed the paper 4-5 times, you should remember a key reason for rejection is because RCP is not comprehensive defense. 
This is the reason we integrate RCP with ASPLOS'21's processor mechanism to achieve ISE, which is comprehensive--even stronger than STT. Hope it is clear. 

ISE=processor mechanism (ASPLOS'21)+ISLE
ISLE: InvisiSpec (correct+high overhead), CleanupSpec (incorrect), MuonTrap (incorrect), RCP (correct+low overhead)
So the novel part is RCP for ISE: all prior solutions are either incorrect or incur high overhead. 

It is also the first paper to really implement correct ISE using either InvisiSpec or RCP. Note that ASPLOS'21 just highlight the processor mechanisms. 

Even more, Ghost paper also tries to achieve ISE, but incorrect, for two reasons.  

* **QC3: How to architect L2SpecBuf? Conflict misses?**

The L2SpecBuf is a per-core buffer. We will not use associativity to search or replace like a cache. 
The counting bloom filter(CBF) are proposed to speedup the search among this buffer structure. Thus we believe we do not have the conflict issue like you said. 


* **QC4: Only focus on cache hierarchy?**

Agree, but please do not simply tone down our design as just containing RCP, it is ISE, stronger than STT. Note that you already mentioned "more principled approaches (such as STT) have become more relevant."
Your comment actually explains why do we change the contribution from a new protocol for ISLE to ISE. 

# Rev-D
score: D

We appreciate your insightful questions. But frankly the D is too harsh based on your comment. 



* **QD1: New data structures may expose CPU to new side-channels?**

He mentioned this without evidence, we have to think. If indeed no evidence, we have to report to the chair so that he can provide more details. 

I think he mentioned this as weakness with no evidence. Better to ask him to provide more details. 

* **QD:  Important to block speculative transmission of non-speculatively accessed secrets

Actually we shared the similar threat model with Dolma. A secret value could be accessed by a non-speculative instruction and store into a register before the start of the speculation. 

Thus, if the attacker wishes to leak this
register-based secret, he could still execute and transmit
the secret using side channels.  Dolma also considers this as vulnerability and metions it as register leakage. 


# Rev-E
score: B



* **QE2: MuonTrap's L0 optimization?**

Muontrap's MSI protocol may not have the problem pointed out in Figure1(d). But first of all, the downgraded MSI coherence protocol will slowdown 
the performance. At the same time, Muontrap still need to stall when accessing remote M (in Fig1(f)), which could be avoided by RCP. 


* **QE3: How does L1 keep load order?**


There is no need to track the order information since the location of an instruction in LSQ can directly reveal this. To complete a local purge, the L1SpecBuf will clear all the entries starting from the recived instruction to the tail.

* **QE4: How large is specBuf in Fig2?**

2×(# o f cores)×(# o f LQ entries) x (# of size of each entry)
In our evaluations, 2x4x32x(64B+1B+1B) =  16.5MB


* **QE5: constant CBF check time?**


It is an assumption that we made because we do not want the check time to be part of the side channel. 


* **QE6: L2: how to get the #of spec core?**

It could be easily down by using CBF structure. We could check all the entries in L2SpecBuf to get this counter since every speculative instructions are recorded here. Please refer to Section 4.2.  



* **QE7: RISC-V or IBM Power MCM?**

The main feature of these models is the non-atomic writes, which we discussed in Section 6, at the bottom of page 9.

# Rev-F
score: C (Don't understand much)

* **QF1: Comment on NDA?**

We indeed included the discussion about NDA and cited in the paper. At a high level, NDA has the similar idea to STT. But since NDA does not have an open-source implementation, we just evaluate STT-futuristic during evaluation. 


* **QF2: Why RCP needs specBuf?**

Keep speculative value: we may miss in L1/L2 cache. Also replay the replacement, etc. Section 4.3 describes how we complete memory operations in all cases along with the SpecBuf structure.

* **QF3: Why not run multithreaded version of SPEC17?**

Since we need to make sure the results are comparable to other papers. InvisiSpec uses PARSEC. We can add the results. 

* **QF4: Why mention ISLE?**

Because it is what InvisiSpec, CleanupSpec, MuonTrap, SafeSpec could provide, it turns out to be insufficient. But ISLE is indeed a component of ISE. Mentioning that could cleanly separate the processor mechanisms with cache mechanisms for ISLE.

# Rev-G
score: C (Dmitry Ponomarev)

* **QG1: Impact on the directory structure?**

We also made corresponding changes in directory to support the speculative states in cache hiearchy. We did not show all the details 
because of the limit on pages. 


* **QG2: Size of CBF?**

The size of the L2SpecBuf is about (# o f cores)×(# o f LQ entries) x (# of size of each entry) which is around 8MB in our setting. 
The overhead of CBF are mainly due to the hash number, which is negligible compared with the size of L2SpecBuf. 


* **QG3: Branch predictor?**

We use the default Tournament Predictor in gem5. We can also change it into bi_model or 2-bit predictor in the setting but the results are indeed different. 


* **QG4: Why CleanupSpec performs poorly on "facesim"?**

It is indeed a weird data point. The performance of CleanupSpec is highly relied on the misprediction rates and we found that the misprediction rate 
of facesim is  about 0.8%, the highest among other benchmarks. Another resonable guess is the concurrent speedup operation among different
cores incurs consecutive stalls during the execution. 











