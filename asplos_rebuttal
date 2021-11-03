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

-- For F, the specBuf is similar but our main contribution is, as you said, the protocol. 

* **QCM5: Very hard to follow.(Rev-C/D/E/F/G)**

Yes, we agree that the content is dense and sometimes we can do nothing about it. However, since the previous versions, we have made a big effort to address this, by adding examples, in XXX and XXX, the MICRO'21 reviewers appreciated that and they can understand the key ideas. Did you check these examples?

* **QCM6: Dolma from Usenix Security'21. (Rev-D/E/F)**


# Rev-A
Score: B

* **QA1: Compare with STT+SDO?**

-- We can add results in final version. 
-- Even STT+SDO is weaker than ISE

* **QA2: 3-level cache?**

Use the paragraph in MICRO revision

# Rev-C
Score: C (this is Moin, again)

We appreciate the fact you have reviewed the paper 4-5 times, and all comments you provided. As you can see we have tried the best to improve the quality. This submission might be new to you since we changed the claim and design to be a comprehensive one to support ISE, not ISLE, since the key reason the previous rejection is that ISLE is insufficient. Perhaps that comment is from you, since you repeated that in the end. However, please not that our current solution, which use RCP as a part of ISE is comprehensive. You mentioned STT is more relevant, we agree, and we have shown that: ISE is more comprehensive than STT. 

So instead of rejecting and reviewing the paper again in ISCA, can you carefully consider our argument, to see whether it addresses your concern? 

You can also see that other reviewers actually buy our argument. I think your comments are majorly due to the fact that you reviewed our previous version several times, I hope you can avoid the stereotype and serious consider our new way of presenting the idea. It is exactly changed in this way to accomodate previous comments. 


* **QC2: Novel support for ISE? Key contribution.**

The ISE and ISLE discussion is not misplaced, since you have reviewed the paper 4-5 times, you should remember a key reason for rejection is because RCP is not comprehensive defense. This is the reason we integrate RCP with ASPLOS'21's processor mechanism to achieve ISE, which is comprehensive--even stronger than STT. Hope it is clear. 

ISE=processor mechanism (ASPLOS'21)+ISLE
ISLE: InvisiSpec (correct+high overhead), CleanupSpec (incorrect), MuonTrap (incorrect), RCP (correct+low overhead)
So the novel part is RCP for ISE: all prior solutions are either incorrect or incur high overhead. 

It is also the first paper to really implement correct ISE using either InvisiSpec or RCP. Note that ASPLOS'21 just highlight the processor mechanisms. 

Even more, Ghost paper also tries to achieve ISE, but incorrect, for two reasons.  

* **QC3: How to architect L2SpecBuf? Conflict misses?**

* **QC4: Only focus on cache hierarchy?**

Agree, but please do not simply tone down our design as just containing RCP, it is ISE, stronger than STT. Note that you already mentioned "more principled approaches (such as STT) have become more relevant."
Your comment actually explains why do we change the contribution from a new protocol for ISLE to ISE. 

# Rev-D
score: D

We appreciate your insightful questions. But frankly the D is too harsh based on your comment. 

* **QD1: New data structures may expose CPU to new side-channels?**

He mentioned this without evidence, we have to think. If indeed no evidence, we have to report to the chair so that he can provide more details. 

# Rev-E
score: B



* **QE2: MuonTrap's L0 optimization?**

TODO

* **QE3: How does L1 keep load order?**

No need?

* **QE4: How large is specBuf in Fig2?**

TODO

* **QE5: constant CBF check time?**

Search some document and give a time, as if you modeled this in the simulator. 

* **QE6: L2: how to get the #of spec core?**

Briefly describe. 

* **QE7: RISC-V or IBM Power MCM?**

The main feature of these models is the non-atomic writes, which we discussed in XXX.

# Rev-F
score: C (Don't understand much)

* **QF1: Comment on NDA?**

* **QF2: Why RCP needs specBuf?**

Keep speculative value: we may miss in L1/L2 cache. Also replay the replacement, etc. Point to the section. 

* **QF3: Why not run multithreaded version of SPEC17?**

Since we need to make sure the results are comparable to other papers. InvisiSpec uses PARSEC. We can add the results. 

* **QF4: Why mention ISLE?**

Because it is what InvisiSpec, CleanupSpec, MuonTrap, SafeSpec could provide, it turns out to be insufficient. But ISLE is indeed a component of ISE. Mentioning that could cleanly separate the processor mechanisms with cache mechanisms for ISLE.

# Rev-G
score: C (Dmitry Ponomarev)

* **QG1: Impact on the directory structure?**

* **QG2: Size of CBF?**

* **QG3: Branch predictor?**

* **QG4: Why CleanupSpec performs poorly on "facesim"?**












