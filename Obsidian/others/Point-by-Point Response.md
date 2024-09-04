## Reviewer #1

C1-O1. While the motivation of this work is clear, the definition of the query is very specific and its linkage to the actual problem is weak.
- First of all, the problem definition requires that |S|+|T| >= k, whereas different size constraints could be put (e.g., |S|>=k1 and |T|>=k2). Do S and T have to be disjoint?
- More importantly, there is no definition of "dense flow" and "dense flow maximization" in the introduction, making obscure the entire definition.
- The concept of time or order (i.e. when do the transaction take place, in what order, etc.) is not discussed at all.
- Finally, the definition is not linked to definitions used in previous work on money laundering detection. The authors confine the discussion to just whether "least k" or "most k" would be more appropriate.
Hence, I'm not convinced about the appropriateness of the proposed query for the intended application.

R1-O1.
- In the problem definition we require |S'|+|T'|>=k where S' is subset of S and T' is subset of T. S and T have to be disjoint, and thus S' and T' are disjoint. Add disjoint to Section 2.2, kSTDF.
- Explain dense flow in Section 1.
- Formally define timestamp and its order in Section 2.1, Timestamp.
- Dense flow is motivated in Introduction, but not defined in Introduction. Explain dense flow in Section 1.

C1-O2. The definitions in Sec. 2 extend corresponding definitions in the classic max-flow problem. However it is not clear how your problem which applies on committed transactions is related to the max-flow problem which finds for the maximum potential flow in a network with capacities on edges. Hence the problem is ill-defined and it is unclear whether it can realistically capture the real money flow in a transactions network. More specifically: 
- The design choice described in Footnote 1 is unclear. Why not use a multi-graph representation with parallel edges? Otherwise, why don't you introduce one intermediate node per transaction, i.e., edges from u to v become u-uv1-v, u-uv2-v, etc.?
- The capacity of one edge in this context is unclear. In a flow network, such as road network or utility network there is a maximum capacity per edge which models the maximum flow that can pass through that edge (e.g. per second). In the transaction graphs that you work on what does capacity correspond to? Is it the maximum amount of money that can be transferred on one edge? Why do we have such a constraint and why is it important for the fraud detection problem?
- The TFN definition is somewhat inconsistent to the previous definitions in that the concept of capacity is incompatible with the concept of time-labeled edges, as capacity denotes potential whereas time-labeled edges denote actual quantities that have been transferred at specific times. More importantly, if we focus on a given source and a given sink, the flow conservation property may not hold in the actual data, as there could be incoming transactions to a given node with quantities not obtained indirectly from the source. For example a path s->a->b may contribute to b, but a path x->a->b may not contribute to b. In this case, for an a->b edge, how can you know whether the funds originate from s or x? Example 2.1 is only an example and does not include a clear definition of flow or maximum flow from s to t. For example, how can you know that the funds transferred from v4 to t originate from s and they do not originate from v2? If the objective is to find the maximum possible flow, how do you know that this realistically corresponds to the real flow from s to t? Is there any theoretical guarantee about the real flow from s to t if one knows the maximum possible flow?
- The kSTDF is not properly defined, because MFlow(S',T') for a subset S' of S and a subset T' of T is not properly defined. Specifically, if you work on such a subset S',T' what network would you use? Would it be some induced subgraph? Which edges should be excluded? I'm not sure whether flow problems have been studied for arbitrary subgraphs.

R1-O2.
- It is correct.
- Explain capacity in Section 2.1, Flow Network.
- The flow from s to t is the maximum possible money transferred from s to t, and it is highly likely to be money laundering if such possible transfer is huge. Explain it in Section 2.1?
- Add a sentence about multi-source multi-sink maximum flow.

C1-O3. Due to the holes and inconsistencies in the problem definition in sec. 2, I'm no longer sure what is being computed in Section 3 and onwards and whether this would correspond to a realistic problem.

R1-O3. N/A

C1-O4. There has been previous work, like [4], on how to transform a temporal flow network to a (static) flow network. So, the contribution of Section 4 is questionable.

R1-O4.

C1-O5. It is unclear how the "investigators" have access to the identities of bitcoin and ethereum network and how they can identify true criminal cases from the transactions. The NFT network study is not adequately presented. Who is the data owner in this case and who guarantees the criminal activity cases? In addition, the authors present precision results but recall is also important.  

R1-O5. Give more information about case study.
## Reviewer #2

C2-O1. Problem formulation is incomplete and incorrect.  
C2-O2. Hence, I am unable to assess the proposed contributions.  
C2-O3. Empirical networks studied in experiments are drawn from only one domain. Hence, it is unclear if the method is restricted to this single domain in practice, i.e., the method fails elsewhere.  

I am not an expert on this or related topics. I can, however, appreciate the informal motivation for the current study. Unfortunately, the problem is not specified clearly enough to understand precisely what needs to be solved. Hence, I am unable to understand clearly enough, and hence appreciate enough the correctness and potential value of the proposed contributions.  

While section 1 makes sense to me at an informal high level, once we delve in to section 2, I find the technical details to be fundamentally broken. In Definition 2.1, what is the role of s and t? They do not even appear in the definition of f. In the following definition of "maximum flow", I am at a complete loss to make sense of the two sentences. In Definition 2.3, I have the same confusion. Furthermore, I do not find here any temporal structure imposed on flows, i.e., edges can appear out of temporal order in any flow, as far as I can tell. Then in Example 2.1, I'm at a complete loss to try to use the example to repair the broken definitions. Why does MFlow(s, t) equal 7? Why does the path from v2 to v5 via v3 "breach" the temporal flow constraint? I don't see how the flow constraint has any restriction on this path. Why does MFlow(s, t) equal 2, later on in the example?  

Consequently, I am unable to follow the proposed solution in Section 3. I have no idea what problem is being solved.

## Reviewer #3

C3-O1.
- More intuition should be provided as to why the problem is NP-hard.
- Why does a greedy approach not work, for example? Intuitively, growing the number of sources and sinks will still wanting a maximum flow does not seem to be intractable.
- More background on the difficulties should be provided, since this problem is being proposed here by the authors for the first time.

R3-O1.
- The NP-hardness is shown in lemma 2.1, and the detailed proof is in Appendix B.
- The peeling algorithm we use is a greedy approach with 2-approximality, and growing the number of sources and sinks has no approximality guarantee.
- The problem is NP-hard.

C3-O2. The formal treatment is lacking in some respects.
- On page 2, it is bizarre to state that MFlow(s',t') = MFlow(S,T), since you never defined what MFlow(S,T) is; you immediately reduce it without first defining it.
- On page 6, it is baffling to see a time complexity stated of O((S+T)^2), which seems to be in contradiction with the NP-hardness of the problem.

*R3-O2.
- Add a sentence about multi-source multi-sink maximum flow.
- This part is the preprocessing of the problem, not solving the problem.

C3-O3 Some more details on the temporal domain would be desirable.
- How is it determined in practical applications?
- How large should we expect it to be?
- The transformation blows up the graph by a factor equal to the size of the temporal domain. In the experimental section, I didn't see that clearly discussed.

R3-O3.
- As described in definition 2.3, the timestamp indicates the moment the transaction occurred.
- In this paper we only consider the relative order of timestamps, so the size of the domain doesn't matter. We treat it as Z+ as described in definition 2.3.
- Add a sentece in Section 4, Transformation Algorithm about the size of transformed graph, |V'|=|V|+2|E| and |E'|=3|E|.

C3-M1. Please clarify the application to financial transactions. What are the capacities of the edges? The amounts transferred, I assume? This may be obvious but you never really say it. This also occurs in Definition 2.3.

R3-M1.
- Clarify the application and explain capacity in Section 2.1, Flow Network.
- Explain the practical significance of flow in Definition 2.3.

C3-M2. Page 3, top, you say that most existing studies ignore the temporal aspect, which is misleading since in the Related Work you do discuss various temporal approaches.

R3-M2. Emphasize that most existing studies about *money laundering* ignore the temporal aspect.