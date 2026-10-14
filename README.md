<!-- solve-problem:b1f0a8f7-f6f5-416f-9289-bf8c3120ac2b -->
# Polynomial kernel for Directed Feedback Vertex Set

A [Tangle Solve problem](https://solve.tangle.tools/problems/b1f0a8f7-f6f5-416f-9289-bf8c3120ac2b).

## What is the problem?

**Directed Feedback Vertex Set (DFVS):** given a directed graph \(G\) and integer \(k\), decide whether there is a set \(S\) of at most \(k\) vertices whose removal makes \(G\) acyclic. Parameterize by \(k\).

A **kernelization** is a polynomial-time algorithm mapping \((G,k)\) to an equivalent instance \((G',k')\) with \(k' \le k\) and \(|G'| \le f(k)\). The problem asks: does DFVS admit a kernel with \(f(k) = k^{\mathcal{O}(1)}\) (a *polynomial kernel*)? Equivalently (by standard lower-bound frameworks), the question is whether the size of a YES-instance, after polynomial-time preprocessing, can be bounded polynomially in \(k\) — the class NP[poly] ∩ coNP[poly]-style compressibility question. Note: DFVS is NP-hard (generalizes Vertex Cover-like obstructions on tournaments), so the kernel, not an exact poly-time algorithm, is the target. Standard assumption: the kernel need only work for the decision problem, and vertex-weighted variants where weights are polynomially bounded are an accepted intermediate goal.

## Why does it matter?

DFVS is a flagship open problem in kernelization; its undirected analogue has a \(4k\)-vertex kernel, and resolving the directed case would be the strongest structural-preprocessing result for cycle deletion. Practically, deleting cycles models deadlock breaking, deadlock recovery in OS databases, network control (controllability of directed networks), and Bayesian-network simplification, where real instances have small \(k\) and preprocessing directly reduces solve time. A polynomial kernel (or a lower bound via composition/AND-cross-composition) would resolve a major entry on the parameterized-algorithms agenda.

## What's already known?

- **FPT:** DFVS is solvable in \(2^{\mathcal{O}(k \log k)} \cdot n^{\mathcal{O}(1)}\) time via iterative compression (Chen, Liu, Lu, O'Sullivan, Razgon 2008) — this is the celebrated result separating FVS from Directed Feedback Arc Set's earlier frontier.
- **No polynomial kernel is known**, even with heavy restrictions.
- **Partial progress (Bergougnoux, Eiben, Ganian, Ordyniak, Ramanujan, Algorithmica 2021):** polynomial kernels for DFVS parameterized by \(k\) *plus* a structural parameter — notably \(k + \ell\) where \(\ell\) is the size of a minimum vertex cover of the underlying undirected graph, and for graphs of bounded split deletion number / bounded independence-ish structure. They introduce the technique of compressing to an annotated "cycle-with-subdivision" structure and using Integer Linear Programming with \(k^{\mathcal{O}(1)}\) variables (Frank–Tardes /totally-unimodular-flavored LPs for skew-supermodular cut-covering) to re-expand.
- **Related:** Directed Feedback Arc Set in tournaments has a quadratic-ish kernel (related LP approach); DFAS in general digraphs has a \(k^{\mathcal{O}(k)}\)-type "kernel" via branching that is not polynomial. Lokshtanov et al. proved several problems lack polynomial kernels under NP ⊆ coNP/poly; DFVS is *not* among them — no lower bound is known either.
- The main obstacle: no known reduction rule handles the interplay of many "almost disjoint" cycles through few high-degree vertices; undirected techniques (expansion lemma, \(P_3\)-packing style reductions) fail because directed reachability doesn't support the matching-style exchanges.

## What would count as solving it?

1. **Positive resolution:** a polynomial-time algorithm reducing any \((G,k)\) to an equivalent DFVS instance (or an equivalent problem, i.e., a *compression*) of size \(k^{\mathcal{O}(1)}\). A referee accepts: correctness proof + running time + explicit size bound.
2. **Negative resolution:** a polynomial-parameter transformation from a problem conjectured kernel-incompressible (e.g., an NP-hard problem with no poly kernel under NP ⊆ coNP/poly) to DFVS, or an OR/AND-cross-composition, proving no polynomial kernel unless NP ⊆ coNP/poly.
3. **Intermediate acceptable results:** poly kernel for DFVS on tournaments, on bounded-genus digraphs, or parameter \(k + \text{(small structural measure)}\) improving Bergougnoux et al.; or a \(k^{\mathcal{O}(\log k)}\) kernel.

## How to start

1. Read **Bergougnoux et al. 2021** thoroughly — especially the \(k+\ell\) (vertex cover of the underlying graph) kernel; understand exactly which reductions break when \(\ell\) is large. This is the state of the art; Section on "skew-supermodular cover" LP machinery is the technical core.
2. Read **Chen, Liu, Lu, O'Sullivan, Razgon (JACM 2008)** for the iterative-compression structure — a kernel might be found by "de-compressing" what the compression step learns.
3. Try the natural approach: bound the number of vertices in a YES-instance that lie on many vertex-disjoint-cycle-relevant positions, via a directed analogue of the expansion lemma; the known failure mode is that directed "expansion" needs a bipartite robust-reachability structure that doesn't closure properly.
4. Alternatively attack the lower bound side: attempt OR-cross-composition from a hitting-set-type problem onto cyclic structure of a digraph; check whether DFVS's high expressiveness (each gadget cycle encodes a clause) can be constrained.

## Key references

- Bergougnoux, Eiben, Ganian, Ordyniak, Ramanujan. *Towards a Polynomial Kernel for Directed Feedback Vertex Set*, Algorithmica 83, 2021. DOI: 10.1007/s00453-020-00777-5. Gives polynomial kernels for DFVS parameterized by \(k\) plus vertex-cover number of the underlying graph and related structural measures, using skew-supermodular LP-based tools. https://doi.org/10.1007/s00453-020-00777-5
- Chen, Liu, Lu, O'Sullivan, Razgon. *A Fixed-Parameter Algorithm for the Directed Feedback Vertex Set Problem*, JACM 55(5), 2008. Establishes FPT for DFVS via iterative compression over "cycle with one chord"-style graph sequences. (verify exact journal details)
- Lokshtanov, Marx, Saurabh. *Known Algorithms on Graphs of Bounded Treewidth Are Probably Optimal / lower-bound literature on kernels* — for the cross-composition machinery needed for a negative resolution. (verify which survey/paper best covers kernel lower bounds; Bodlaender–Downey–Fellows–Hermelin, JCSS 2009, is the foundational kernelization-lower-bounds paper.)

## Warnings

- Do not confuse DFVS with Directed **Feedback Arc Set** or with the tournament variants — kernel status differs.
- Vertex-weighted reductions must be handled carefully: losing weights by subdivision is invalid in DFVS because subdivisions create cycles through new vertices.
- The undirected kernel techniques (expansion lemma, crown reduction) do not transfer; multiple groups have failed here — understand why before adapting.
- No kernel lower bound is known either, so both directions are genuinely open; the census statement is accurate but terse on the fact that even \(k^{\mathcal{O}(k)}\) → polynomial for general digraphs has no published attack beyond the structural-parameter line.

---
*`ALGO-000095` · flagged for review*

## Acceptance guidance

1. **Positive resolution:** a polynomial-time algorithm reducing any \((G,k)\) to an equivalent DFVS instance (or an equivalent problem, i.e., a *compression*) of size \(k^{\mathcal{O}(1)}\). A referee accepts: correctness proof + running time + explicit size bound.
2. **Negative resolution:** a polynomial-parameter transformation from a problem conjectured kernel-incompressible (e.g., an NP-hard problem with no poly kernel under NP ⊆ coNP/poly) to DFVS, or an OR/AND-cross-composition, proving no polynomial kernel unless NP ⊆ coNP/poly.
3. **Intermediate acceptable results:** poly kernel for DFVS on tournaments, on bounded-genus digraphs, or parameter \(k + \text{(small structural measure)}\) improving Bergougnoux et al.; or a \(k^{\mathcal{O}(\log k)}\) kernel.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/b1f0a8f7-f6f5-416f-9289-bf8c3120ac2b?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000095.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000095)
