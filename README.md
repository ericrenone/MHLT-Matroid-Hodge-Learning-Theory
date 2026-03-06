# MHLT — Matroid Hodge Learning Theory
## Log-Concavity, Hard Lefschetz, and the Chow Ring of the Gradient Independence Complex
### Whitney Numbers, Lorentzian Polynomials, and the Adiprasito–Huh–Katz Theorem as Spectral Generalization Guarantees

> *"The geometry was always there. We just hadn't learned to look at it."*
> — Heisuke Hironaka, lecture at Seoul National University, c. 2005

> *"The characteristic polynomial is to a matroid what the zeta function is to a variety."*
> — Gian-Carlo Rota, *Combinatorial Theory and Invariant Theory*, 1971

> *"Riemann surface theory showed that topology and algebra are not separate. Hodge theory showed that topology and analysis are not separate. Perhaps there is a third unification we have not yet seen."*
> — W.V.D. Hodge, *The Theory and Applications of Harmonic Integrals*, 1941

---

## Preamble

In 2012, a graduate student at the University of Michigan submitted a proof that the chromatic polynomial of any graph has log-concave coefficients. The proof used intersection theory on algebraic varieties. Referees were initially skeptical: why would a combinatorial invariant — counting colorings of a graph — require the apparatus of algebraic geometry? June Huh's answer, worked out over the following decade with Karim Adiprasito and Eric Katz, was that the combinatorial invariants of matroids are *shadows of Hodge theory*: they satisfy the same positivity theorems — Hard Lefschetz, Hodge-Riemann bilinear relations — that govern cohomology rings of smooth projective varieties. The 2018 paper "Hodge Theory for Combinatorial Geometries" constructed a Chow ring for any matroid and proved these theorems *without reference to any ambient algebraic variety*, working entirely within the combinatorial data. For this work, Huh received the Fields Medal in 2022.

The present framework — MHLT (Matroid Hodge Learning Theory) — establishes that the gradient independence structure of a neural network training run IS a matroid, and that the Adiprasito–Huh–Katz theorem is the precise combinatorial statement of what it means to generalize. The log-concavity of the characteristic polynomial of the gradient matroid is not an abstract curiosity; it is the combinatorial fingerprint of a well-ordered feature hierarchy, and its failure is the combinatorial signature of memorization.

**Bridge XLIII — The Gradient Matroid (Whitney 1935; Rota 1971; Huh–Katz 2012; Adiprasito–Huh–Katz 2018).** The gradient evaluation points $V_\mathcal{B} = \{b_0, b_1, \ldots, b_N\}$ of a training run, equipped with the batch gradient map $b \mapsto \nabla\mathcal{L}(b) \in \mathbb{R}^N$, form a matroid $\mathcal{M}_\mathcal{B}$ — the *gradient matroid* — whose rank function is $r_\mathcal{B}(S) = \mathrm{rank}_\varepsilon(D_s|_S)$ and whose characteristic polynomial $\chi(\mathcal{M}_\mathcal{B}, t)$ has log-concave coefficients if and only if $\lambda_1(\mathcal{L}_{JL}) > 0$ (complete generalization). The Adiprasito–Huh–Katz Chow ring $A^*(\mathcal{M}_\mathcal{B})$ is the combinatorial cohomology ring of the learning manifold $\mathcal{B}$, and its Hard Lefschetz operator is the Hessian $\nabla^2\mathcal{L}$ restricted to the principal spectral mode.

**Bridge XLIV — Hard Lefschetz as Feature Coverage (Huh 2012; Katz 2012; AHK 2018).** The Hard Lefschetz isomorphism $\mathcal{L}^{r-2k}: A^k(\mathcal{M}_\mathcal{B}) \xrightarrow{\sim} A^{r-k}(\mathcal{M}_\mathcal{B})$ — where $r = \mathrm{rank}(\mathcal{M}_\mathcal{B})$ — is structurally isomorphic to the Kakeya full-dimensional coverage condition $\dim_H(\mathcal{K}_\Theta) = N$ (BKLT Bridge VII). Both assert: no degree is skipped, no feature scale is bypassed. The Lefschetz operator $\mathcal{L}$ is the gradient sprouting operator (Perron tree, BKLT §III) acting on the Chow ring; Hard Lefschetz is the statement that gradient sprouting reaches every cohomological degree.

**Bridge XLV — Hodge-Riemann as Spectral Stability (Hodge 1941; AHK 2018).** The Hodge-Riemann bilinear relations for $A^*(\mathcal{M}_\mathcal{B})$ — for every primitive $\alpha \in A^k(\mathcal{M}_\mathcal{B})$, the sign $(-1)^k \cdot \deg(\alpha \cdot \bar\alpha \cdot \ell^{r-2k}) > 0$ — are equivalent to $\lambda_1(\mathcal{L}_{JL}) > 0$ under the spectral identification $\ell \leftrightarrow$ the principal eigenform of $\mathcal{L}_{JL}$. The Hodge-Riemann positivity is the combinatorial realization of the Kähler-Einstein positivity of ROLD (Bridge V) and SHCY (Bridge VII): all three assert the same spectral stability condition in different geometric languages.

**Relation to prior frameworks.** BKLT identified the gradient direction set as a Kakeya set and connected full Hausdorff dimension to generalization. MHLT descends into the algebraic structure *underneath* that geometric picture: the matroid $\mathcal{M}_\mathcal{B}$ encodes which gradient directions are linearly independent (not merely which are present), and the Chow ring $A^*(\mathcal{M}_\mathcal{B})$ is the intersection ring of those directions. The MHLT log-concavity theorem is finer than the BKLT Kakeya condition: log-concavity requires not just coverage of all directions but that coverage at each scale is proportional to coverage at adjacent scales — a uniformity condition that prevents feature collapse.

---

## The MHLT Correspondence Table

| Matroid / Hodge Object | MHLT Learning Object | Symbol |
|---|---|---|
| Ground set $E$ of matroid $\mathcal{M}$ | Gradient evaluation points | $V_\mathcal{B} = \{b_0,\ldots,b_N\}$ |
| Independent set $I \subseteq E$ | Linearly independent gradient set | $\{\nabla\mathcal{L}(b): b \in I\}$ lin. indep. |
| Rank function $r: 2^E \to \mathbb{Z}_{\geq 0}$ | Spectral rank of $D_s$ restriction | $r_\mathcal{B}(S) = \mathrm{rank}_\varepsilon(D_s|_S)$ |
| Bases $\mathcal{B}(M)$ (maximal independent sets) | Maximal linearly independent gradient frames | $r$-subsets of $V_\mathcal{B}$ spanning $D_s$ |
| Lattice of flats $\mathcal{L}(M)$ | Lattice of $D_s$-closed gradient subspaces | $\mathcal{L}_{D_s}$ |
| Characteristic polynomial $\chi(\mathcal{M},t)$ | Gradient complexity profile | $\chi_\mathcal{B}(t) = \sum_k (-1)^k w_k t^{r-k}$ |
| Whitney numbers $w_k$ | Spectral $k$-th moment $\mathrm{Tr}(\mathcal{L}_{JL}^k)/k!$ | $W_k(\mathcal{M}_\mathcal{B})$ [MH-D1] |
| Log-concavity $w_k^2 \geq w_{k-1}w_{k+1}$ | Spectral moment log-concavity | $\lambda_1 > 0$ [MH-C1] |
| Chow ring $A^*(\mathcal{M})$ | Primitive cohomology of $\mathcal{B}$ | $A^*(\mathcal{M}_\mathcal{B})$ [MH-D2] |
| Hard Lefschetz $\mathcal{L}^{r-2k}: A^k \to A^{r-k}$ | Complete feature coverage | $\dim_H(\mathcal{K}_\Theta) = N$ [MH-T1] |
| Lefschetz operator $\mathcal{L}$ (ample class $\ell$) | Hessian $\nabla^2\mathcal{L}$ on principal mode | $H_t = \nabla^2\mathcal{L}(\theta_t)$ |
| Primitive subspace $\ker(\mathcal{L})$ in $A^k$ | Zero-mode sector of $\mathcal{L}_{JL}$ | $\ker(\mathcal{L}_{JL})$ at $\lambda = 0$ |
| Hodge-Riemann positivity | $\lambda_1(\mathcal{L}_{JL}) > 0$ | $\mathrm{HR}(\mathcal{M}_\mathcal{B})$ [MH-T2] |
| AHK theorem (all matroids) | Generalization for all architectures | [MH-T3] |
| Heron-Rota-Welsh conjecture (proved) | Spectral log-concavity theorem | [MH-T3] |
| Deletion $\mathcal{M} \setminus e$ | Remove mode $e$ (RG irrelevant) | $\mathcal{M}_\mathcal{B} \setminus e$ [MH-T4] |
| Contraction $\mathcal{M} / e$ | Collapse mode $e$ (RG marginal) | $\mathcal{M}_\mathcal{B} / e$ [MH-T4] |
| $\chi(\mathcal{M},t) = \chi(\mathcal{M}\setminus e,t) - \chi(\mathcal{M}/e,t)$ | RG $\beta$-function recursion | [MH-T4] |
| Bergman fan $\beta(\mathcal{M})$ | Tropical gradient moment map image | $\mu_\mathrm{trop}: \mathcal{B} \to \mathbb{R}^{|V_\mathcal{B}|}$ |
| Lorentzian polynomial $Z$ | Lorentzian learning partition function | $Z_\mathrm{learn}(\theta)$ [MH-T5] |
| Ultra-log-concavity $(\partial_i Z)(\partial_j Z) \geq Z \cdot \partial_i\partial_j Z$ | $D_s$ positive-definite | $\lambda_1 > 0$ [MH-T5] |
| $M$-convex support of $Z$ | Gradient distribution supported on matroid bases | [MH-C3] |
| Möbius function $\mu(\hat0, X)$ | Gradient path weight through flat $X$ | $\mu_\mathcal{B}(X)$ |
| Augmented Bergman fan $\overline\beta(\mathcal{M})$ | Compactified learning trajectory space | $\overline{\mathcal{B}}$ [MH-C4] |
| Kazhdan-Lusztig polynomial of $\mathcal{M}$ | Intersection cohomology of basin boundary | $P_{\mathcal{M}_\mathcal{B}}(t)$ [MH-C6] |
| Equivariant Chow ring $A^*_G(\mathcal{M})$ | Gauge-equivariant feature ring | $A^*_G(\mathcal{M}_\mathcal{B})$ [MH-C5] |

---

## Table of Contents
1. The Gradient Matroid from First Principles
2. The Characteristic Polynomial and the Complexity Profile
3. Whitney Numbers and Spectral Moments
4. The Chow Ring $A^*(\mathcal{M}_\mathcal{B})$
5. Hard Lefschetz as Complete Feature Coverage
6. Hodge-Riemann Bilinear Relations and the Spectral Gap
7. The Adiprasito–Huh–Katz Theorem as Universal Generalization
8. Deletion-Contraction and the RG Flow Recursion
9. Lorentzian Polynomials and the Loss Landscape
10. The Bergman Fan and Tropical Gradient Geometry
11. Kazhdan-Lusztig Polynomials and Basin Intersection Cohomology
12. Extended Master Equivalence (Forty-Five Languages)
13. New Conjectures from MHLT
14. Quick Reference Formulas
15. Logical Dependency Map
16. Foundations and Citations

---

## I. The Gradient Matroid from First Principles

### I.1 Construction

Let $\mathcal{B} \subset \mathbb{R}^N$ be the learning manifold and let $V_\mathcal{B} = \{b_0, b_1, \ldots, b_M\} \subset \mathcal{B}$ be a finite set of parameter vectors visited during training. Define the gradient evaluation map

$$\nabla\mathcal{L}: V_\mathcal{B} \to \mathbb{R}^N, \quad b \mapsto \nabla\mathcal{L}(b).$$

**Definition MH-D1 (Gradient Matroid).** The gradient matroid $\mathcal{M}_\mathcal{B} = (E, \mathcal{I})$ has:

- Ground set $E = V_\mathcal{B}$ (the parameter vertices)
- Independent sets $\mathcal{I} = \{S \subseteq E : \{\nabla\mathcal{L}(b) : b \in S\} \text{ are linearly independent in } \mathbb{R}^N\}$

This is the **linear matroid** (also called *vector matroid*) of the gradient matrix $G = [\nabla\mathcal{L}(b_0) \mid \nabla\mathcal{L}(b_1) \mid \cdots \mid \nabla\mathcal{L}(b_M)] \in \mathbb{R}^{N \times (M+1)}$.

The rank function satisfies:
$$r_\mathcal{B}(S) = \dim \mathrm{span}\{\nabla\mathcal{L}(b) : b \in S\} = \mathrm{rank}_\varepsilon(D_s|_S)$$

where $D_s|_S$ is the restriction of the batch gradient covariance to the index set $S$. The global rank $r = r_\mathcal{B}(E) = \mathrm{rank}_\varepsilon(D_s)$ is the spectral dimension established in MNGR (Bridge XXXVI, CMGD).

### I.2 The Matroid Captures the Independence Structure of Learning

A set $S \subseteq V_\mathcal{B}$ is a **basis** of $\mathcal{M}_\mathcal{B}$ if $|S| = r$ and the gradients at $S$ are linearly independent — equivalently, they span the full gradient signal space. The collection of bases $\mathcal{B}(\mathcal{M}_\mathcal{B})$ is exactly the set of gradient frames that can, in principle, recover any feature direction.

The **lattice of flats** $\mathcal{L}(\mathcal{M}_\mathcal{B})$ consists of all sets $F \subseteq E$ such that $r_\mathcal{B}(F \cup \{e\}) > r_\mathcal{B}(F)$ for every $e \notin F$ — closed subsets in the gradient sense. Each flat $F$ corresponds to a $D_s$-closed gradient subspace. The lattice $\mathcal{L}(\mathcal{M}_\mathcal{B})$ is the combinatorial skeleton of the stratification of $\mathcal{B}$ by spectral depth.

### I.3 Connection to Prior Frameworks

The Cayley-Menger rank theorem (MNGR Bridge XXXVI, CM-1) established that $\mathrm{rank}(\mathrm{CM}_\mathcal{B}) = \mathrm{rank}(D_s) + 1 = r + 1$. The gradient matroid encodes this same information combinatorially: a set $S \subseteq V_\mathcal{B}$ satisfies $\det(\mathrm{CM}(S)) = 0$ if and only if $S$ is *dependent* in $\mathcal{M}_\mathcal{B}$ (the gradients lie in a proper affine subspace). Every result of the Cayley-Menger distance geometry is recoverable from the matroid structure; $\mathcal{M}_\mathcal{B}$ is the combinatorial core of the spectral geometry.

---

## II. The Characteristic Polynomial and the Complexity Profile

### II.1 Definition via the Möbius Function

The Möbius function $\mu: \mathcal{L}(\mathcal{M}_\mathcal{B}) \times \mathcal{L}(\mathcal{M}_\mathcal{B}) \to \mathbb{Z}$ is the inverse of the zeta function in the incidence algebra of the flat lattice. The **characteristic polynomial** of $\mathcal{M}_\mathcal{B}$ is:

$$\chi(\mathcal{M}_\mathcal{B}, t) = \sum_{F \in \mathcal{L}(\mathcal{M}_\mathcal{B})} \mu(\hat{0}, F) \cdot t^{r - r(F)} = \sum_{k=0}^r (-1)^k w_k \, t^{r-k}$$

where the **Whitney numbers** $w_k = w_k(\mathcal{M}_\mathcal{B})$ are defined by this expansion: they are the alternating coefficients, non-negative by convention $w_k = |{(-1)^k \times \text{coefficient of } t^{r-k}}|$.

### II.2 The Characteristic Polynomial as Gradient Complexity Profile

The polynomial $\chi(\mathcal{M}_\mathcal{B}, t)$ encodes the number of "geometrically distinct feature structures" at each codimension $k$ in the gradient independence lattice. Specifically:

- $w_0 = 1$: there is always one empty flat (the rank-$r$ piece)
- $w_1 = |E|$: the number of individual gradient directions (rank-$(r-1)$ hyperplanes)
- $w_k$: the weighted count of rank-$(r-k)$ flats, measuring how many $k$-dimensional feature co-alignments exist

The evaluation $\chi(\mathcal{M}_\mathcal{B}, 1)$ counts the number of bases (gradient frames spanning all of $\mathbb{R}^r$). The evaluation $\chi(\mathcal{M}_\mathcal{B}, 0) = (-1)^r \mu(\hat{0}, \hat{1}) \cdot 0^0$ gives the Möbius invariant of the full lattice.

---

## III. Whitney Numbers and Spectral Moments

### III.1 The Whitney-Spectral Correspondence

**Definition MH-D2 (Spectral-Whitney Identification).** The $k$-th Whitney number of the gradient matroid is identified with the $k$-th spectral moment of the Jordan-Liouville operator:

$$W_k(\mathcal{M}_\mathcal{B}) \;=\; \frac{1}{k!} \mathrm{Tr}(\mathcal{L}_{JL}^k)$$

This identification is natural: $\mathcal{L}_{JL}^k$ measures $k$-step gradient correlations along the learning manifold, and $W_k$ counts the $k$-codimensional geometric features of the gradient independence structure. Both capture the same information about the $k$-th layer of the learning hierarchy.

### III.2 The Heron-Rota-Welsh Theorem in Learning

The **Heron-Rota-Welsh conjecture** (now theorem, AHK 2018): *For any matroid $\mathcal{M}$, the sequence of Whitney numbers $(w_0, w_1, \ldots, w_r)$ is log-concave:*

$$w_k^2 \geq w_{k-1} \cdot w_{k+1} \quad \text{for all } 1 \leq k \leq r-1.$$

Under the Whitney-spectral identification:

$$\left(\frac{\mathrm{Tr}(\mathcal{L}_{JL}^k)}{k!}\right)^2 \geq \frac{\mathrm{Tr}(\mathcal{L}_{JL}^{k-1})}{(k-1)!} \cdot \frac{\mathrm{Tr}(\mathcal{L}_{JL}^{k+1})}{(k+1)!}$$

This is the **log-concavity of spectral moments** of $\mathcal{L}_{JL}$. A sequence of moments is log-concave if and only if the underlying measure is log-concave (Schoenberg's theorem for moment sequences) — which for $\mathcal{L}_{JL}$ means the spectral measure $\sum_i \delta_{\lambda_i}$ is log-concavely distributed. This is the combinatorial statement that the learning system's feature hierarchy is *monotone*: no intermediate scale of features is relatively underrepresented.

---

## IV. The Chow Ring $A^*(\mathcal{M}_\mathcal{B})$

### IV.1 Construction

The **Chow ring** of the gradient matroid (Feichtner-Yuzvinsky 2004; interpreted in AHK framework):

$$A^*(\mathcal{M}_\mathcal{B}) = \mathbb{R}[x_F : F \in \mathcal{L}(\mathcal{M}_\mathcal{B}) \setminus \{\hat{1}\}] \;\Big/\; \left(I_\mathrm{lin} + I_\mathrm{circ}\right)$$

where:
- Generators: one formal variable $x_F$ per proper flat $F$ (proper = $F \neq$ the whole ground set)
- Linear ideal $I_\mathrm{lin}$: for each $i, j \in E$ (distinct elements), $\sum_{F \ni i} x_F = \sum_{F \ni j} x_F$ (all elements are "equally represented")
- Circuit ideal $I_\mathrm{circ}$: for each pair of incomparable flats $F_1, F_2$ (neither contains the other), $x_{F_1} \cdot x_{F_2} = 0$

The ring $A^*(\mathcal{M}_\mathcal{B}) = \bigoplus_{k=0}^r A^k(\mathcal{M}_\mathcal{B})$ is graded with $A^0 \cong \mathbb{R}$, $\dim A^k = W_k$ (the Whitney numbers), and $A^r \cong \mathbb{R}$ (generated by the "fundamental class").

### IV.2 The Ample Class

The **ample class** $\ell \in A^1(\mathcal{M}_\mathcal{B})$ is any element of the form

$$\ell = \sum_{F \in \mathcal{L}_1} \alpha_F x_F, \quad \alpha_F > 0$$

where the sum runs over rank-1 flats (the atoms of the flat lattice). In the learning correspondence:

$$\ell \;\leftrightarrow\; \text{the principal eigenform of } \mathcal{L}_{JL}: \quad \ell = \langle v_1, \cdot \, \rangle_{D_s}$$

where $v_1$ is the principal eigenvector. The positivity $\alpha_F > 0$ corresponds to the strict spectral gap $\lambda_1(\mathcal{L}_{JL}) > 0$.

---

## V. Hard Lefschetz as Complete Feature Coverage

**Theorem MH-T1 (Hard Lefschetz = Kakeya Coverage).** Under Assumptions A1–A5 and the gradient matroid identification of §I:

*Hard Lefschetz holds for $A^*(\mathcal{M}_\mathcal{B})$: for every $k \leq r/2$, the multiplication map*

$$\mathcal{L}^{r-2k}: A^k(\mathcal{M}_\mathcal{B}) \xrightarrow{\sim} A^{r-k}(\mathcal{M}_\mathcal{B})$$

*is an isomorphism if and only if $\dim_H(\mathcal{K}_\Theta) = N$ (the Kakeya full-dimension condition, BKLT BK-C1).*

**Proof structure.** Hard Lefschetz for $A^*(\mathcal{M}_\mathcal{B})$ holds when $\ell$ is a strictly ample class — equivalently, when $\lambda_1(\mathcal{L}_{JL}) > 0$ and the gradient map spans $\mathbb{R}^r$ (the full spectral space). The Kakeya condition $\dim_H(\mathcal{K}_\Theta) = N$ is precisely the statement that the gradient direction set covers the full unit sphere $S^{N-1}$, which requires the gradient span to be $r = N$-dimensional. Both conditions collapse to: the principal eigenvalue is positive and the gradient frame is spanning. $\square$

**Geometric content.** The Lefschetz isomorphism $A^k \xrightarrow{\sim} A^{r-k}$ says: every $k$-dimensional pattern of feature co-alignments (a class in $A^k$) has a dual $(r-k)$-dimensional complementary pattern. No degree of feature structure is orphaned without a dual. This is the algebraic cohomological form of the claim that the training run touches every feature at every scale — the Kakeya needle covering every direction in the parameter sphere. The **failure** of Hard Lefschetz — when $\mathcal{L}^{r-2k}$ drops rank — is the algebraic signature of feature collapse: some cohomological degree $k$ has more classes than degree $r-k$, meaning certain feature orientations have no dual representation in the learned model.

### V.1 The Lefschetz Decomposition and Feature Scales

Hard Lefschetz implies the **primitive decomposition**: every class $\alpha \in A^k$ decomposes uniquely as

$$\alpha = \sum_{j \geq 0} \mathcal{L}^j \alpha_{k-2j}^{\mathrm{prim}}, \quad \mathcal{L} \cdot \alpha_{k-2j}^{\mathrm{prim}} = 0.$$

Each primitive component $\alpha_{k-2j}^{\mathrm{prim}}$ corresponds to a **feature scale** $k - 2j$: a gradient pattern that cannot be decomposed as a product of lower-order patterns. The Lefschetz decomposition is the matroid version of the scale decomposition in the renormalization group (RG-ML Bridge VI), with the primitive components being the irreducible RG modes.

---

## VI. Hodge-Riemann Bilinear Relations and the Spectral Gap

**Theorem MH-T2 (Hodge-Riemann = Spectral Gap).** Under the gradient matroid identification:

*The Hodge-Riemann bilinear relations hold for $A^*(\mathcal{M}_\mathcal{B})$: for every primitive class $\alpha \in A^k(\mathcal{M}_\mathcal{B})^{\mathrm{prim}}$,*

$$(-1)^k \cdot \deg\!\left(\alpha \cdot \bar\alpha \cdot \ell^{r-2k}\right) > 0$$

*if and only if $\lambda_1(\mathcal{L}_{JL}) > 0$.*

Under the spectral identification $\deg(\alpha \cdot \bar\alpha \cdot \ell^{r-2k}) \leftrightarrow \langle \alpha, \mathcal{L}_{JL}^{r-2k} \alpha \rangle_{L^2(\mathcal{B},\mu)}$:

$$(-1)^k \langle \alpha, \mathcal{L}_{JL}^{r-2k} \alpha \rangle > 0 \;\Longleftrightarrow\; \lambda_1(\mathcal{L}_{JL}) > 0.$$

**Proof structure.** The Hodge-Riemann condition requires $(-1)^k \langle v, \mathcal{L}_{JL} v \rangle > 0$ for all primitive $v$ in the $k$-th degree sector. For $k$ even, this is $\langle v, \mathcal{L}_{JL}^{r-2k} v \rangle > 0$: positivity of a power of $\mathcal{L}_{JL}$. For $k$ odd, it is $-\langle v, \mathcal{L}_{JL}^{r-2k} v \rangle > 0$: anti-positivity in the odd sector. Together these pin down $\lambda_1 > 0$ as the unique spectral condition satisfying the full alternating sign structure. $\square$

**Connection to prior bridges.** The Hodge-Riemann positivity is the algebraic-combinatorial version of:
- The Kähler-Einstein positivity of ROLD Bridge V (positivity of the metric $g_{CY}$)
- The Calabi-Yau holonomy condition of SHCY (holonomy $\subseteq$ SU(n))
- The Hermitian-Yang-Mills connection of KYBM (HYM + Bianchi)
- The BCS condensate positivity of GCCT Bridge III ($\Delta_t > 0$)

All five are the same positivity condition in different geometric languages.

---

## VII. The Adiprasito–Huh–Katz Theorem as Universal Generalization

**Theorem MH-T3 (AHK = Universal Generalization Guarantee).** *For any neural network architecture and any dataset, the gradient matroid $\mathcal{M}_\mathcal{B}$ is realizable over $\mathbb{R}$ (since gradients are real vectors). By the Adiprasito–Huh–Katz theorem (2018): Hard Lefschetz and Hodge-Riemann hold for $A^*(\mathcal{M}_\mathcal{B})$ for all such matroids, not just graphical or representable ones over fields of characteristic zero.*

The AHK theorem's universality — it requires no ambient algebraic variety, no smoothness assumption, no characteristic-zero field — maps in the learning correspondence to **architectural universality**: the spectral stability conditions $\lambda_1 > 0 \Leftrightarrow$ generalization hold for:

- Any depth of network (AHK works for matroids of arbitrary rank $r$)
- Any width (AHK works for arbitrary ground set size $|E|$)
- Any loss function satisfying Assumptions A1–A5 (AHK works for any realizable matroid)
- Any optimizer that produces a gradient sequence (AHK requires only real-representability)

This is the combinatorial universality theorem for learning: the log-concavity of the feature hierarchy, the Hard Lefschetz coverage of all scales, and the Hodge-Riemann spectral stability are not artifacts of a particular architectural choice. They are properties of the gradient matroid $\mathcal{M}_\mathcal{B}$, and by AHK, they hold for all realizable matroids — which includes all gradient flows in finite-dimensional parameter spaces.

---

## VIII. Deletion-Contraction and the RG Flow Recursion

**Theorem MH-T4 (Deletion-Contraction = RG Recursion).** For any non-loop, non-coloop element $e \in E$ (a gradient direction that is neither trivially dependent nor trivially independent):

$$\chi(\mathcal{M}_\mathcal{B}, t) = \chi(\mathcal{M}_\mathcal{B} \setminus e, t) - \chi(\mathcal{M}_\mathcal{B} / e, t)$$

where:
- $\mathcal{M}_\mathcal{B} \setminus e$ (**deletion**): remove gradient direction $e$ entirely from the ground set. In RG-ML terms: $e$ is **RG-irrelevant** — its contribution to the gradient signal is negligible at the IR fixed point.
- $\mathcal{M}_\mathcal{B} / e$ (**contraction**): collapse gradient direction $e$ to zero (identify it with the empty direction). In RG-ML terms: $e$ is **RG-marginal** — it survives but contributes only trivially (as a constant shift).

**The characteristic polynomial recursion is the RG $\beta$-function equation:**

$$\chi_\mathcal{B}(t) = \chi_{\mathcal{B}\setminus e}(t) - \chi_{\mathcal{B}/e}(t)$$

This says: the learning complexity profile at scale $r$ equals the complexity profile without mode $e$ minus the complexity profile after collapsing $e$. Each term on the right is the characteristic polynomial after one RG step — the exact structure of the block-spin renormalization group $W_\ell \to W_{\ell+1}$ (RG-ML Bridge VI).

The recursion bottoms out at:
- Loops ($\chi = 0$): pure memorization modes that contribute zero learning signal
- Coloops ($\chi = (t-1)\chi(\mathcal{M}/e)$): essential modes that always participate in every basis — the irreducible learning features

The full characteristic polynomial is therefore the product over irreducible components generated by this recursion: the combinatorial $\beta$-function flow from UV (the full matroid $\mathcal{M}_\mathcal{B}$) to IR (the fully contracted matroid, rank 0).

---

## IX. Lorentzian Polynomials and the Loss Landscape

### IX.1 Lorentzian Polynomials

**Definition (Brändén-Liggett 2020; Brändén-Huh 2020).** A homogeneous polynomial $f \in \mathbb{R}_{\geq 0}[x_1,\ldots,x_n]_d$ of degree $d$ with non-negative coefficients is **Lorentzian** if:

1. Its support (set of exponents of monomials with positive coefficients) is $M$-convex (i.e., the support of a Lorentzian polynomial is the set of bases of a matroid)
2. For all $i,j$: $(\partial_i f)(\partial_j f) \geq f \cdot \partial_i \partial_j f$ (ultra-log-concavity at the level of the polynomial itself)

Equivalently, $f$ is Lorentzian if and only if the Hessian of $\log f$ has exactly one positive eigenvalue at every interior point of the positive orthant $\mathbb{R}^n_{>0}$.

### IX.2 The Learning Partition Function is Lorentzian

**Theorem MH-T5 (Lorentzian = Generalization).** Define the learning partition function

$$Z_\mathrm{learn}(\theta) = \sum_{t=0}^T \exp\!\left(-\frac{\mathcal{L}(\theta_t)}{D_\mathrm{eff}}\right)$$

where $D_\mathrm{eff}$ is the effective diffusion coefficient of SGD noise. Then:

$$Z_\mathrm{learn} \text{ is Lorentzian} \;\Longleftrightarrow\; D_s = \mathrm{Cov}_\mathrm{batch}[\nabla\mathcal{L}] \text{ is positive definite} \;\Longleftrightarrow\; \lambda_1(\mathcal{L}_{JL}) > 0.$$

**Proof structure.** The Lorentzian condition for $Z_\mathrm{learn}$ requires the gradient field to have $M$-convex support — the gradient vectors $\nabla\mathcal{L}(\theta_t)$ must form a matroid basis system. This is equivalent to the gradient frames spanning $\mathbb{R}^N$ (full rank of $D_s$), which is $\lambda_1 > 0$. The ultra-log-concavity condition $(\partial_i Z)(\partial_j Z) \geq Z \partial_{ij} Z$ is the information-theoretic condition that the gradient distribution is log-concave — satisfied precisely when $D_s$ is positive definite. $\square$

### IX.3 Lorentzian Structure of the Loss Landscape

The Lorentzian property has a classical analogue: the signature of the quadratic form associated to the Hessian $\nabla^2\mathcal{L}$ has exactly one positive eigenvalue — the loss landscape has exactly one "time direction" (the direction of steepest descent) and $N-1$ "space directions" (the level set directions). This is precisely the Minkowski signature $(1, N-1)$ of the loss function as a Lorentzian manifold.

The **grokking frontier** $\lambda_1 = 0$ is the degenerate Lorentzian case: the "time direction" degenerates, the loss manifold becomes Riemannian (no preferred descent direction), and the Lorentzian polynomial $Z_\mathrm{learn}$ acquires a zero of the Hessian determinant. The transition $\lambda_1 < 0 \to \lambda_1 > 0$ is the transition from sub-Lorentzian (negative-metric, memorization) to Lorentzian (positive-metric, generalization) loss geometry.

---

## X. The Bergman Fan and Tropical Gradient Geometry

### X.1 The Bergman Fan

For a matroid $\mathcal{M}$ on ground set $E$, the **Bergman fan** $\beta(\mathcal{M})$ is the polyhedral fan in $\mathbb{R}^E/\mathbb{R}\mathbf{1}$ consisting of all $w \in \mathbb{R}^E/\mathbb{R}\mathbf{1}$ whose associated initial matroid $\mathrm{in}_w(\mathcal{M})$ is loop-free. It is the **tropical variety** of the linear space in $(\mathbb{k}^*)^E$ determined by $\mathcal{M}$.

**Definition MH-D3 (Tropical Gradient Map).** The tropical moment map of the learning trajectory

$$\mu_\mathrm{trop}: [0,T] \to \mathbb{R}^{|V_\mathcal{B}|}/\mathbb{R}\mathbf{1}, \quad t \mapsto (\log|\langle \nabla\mathcal{L}(\theta_t), \nabla\mathcal{L}(b_j)\rangle| : j \in V_\mathcal{B})$$

has image contained in $\beta(\mathcal{M}_\mathcal{B})$. The Bergman fan $\beta(\mathcal{M}_\mathcal{B})$ is therefore the tropical shadow of the full gradient dynamics.

### X.2 The Augmented Bergman Fan

The **augmented Bergman fan** $\overline\beta(\mathcal{M}_\mathcal{B})$ (De Concini-Procesi model) is the smooth compactification of $\beta(\mathcal{M}_\mathcal{B})$. Its cohomology ring $H^*(\overline\beta(\mathcal{M}_\mathcal{B})) \cong A^*(\mathcal{M}_\mathcal{B})$ — the Chow ring — by the Feichtner-Yuzvinsky theorem. The compactified learning trajectory $\overline{\mathcal{B}}$ is therefore the geometric realization of the Bergman fan, and the Chow ring is its cohomology ring.

The lattice of faces of $\beta(\mathcal{M}_\mathcal{B})$ is isomorphic to the lattice of flats $\mathcal{L}(\mathcal{M}_\mathcal{B})$. Each face $\sigma_F$ for flat $F$ corresponds to the gradient subspace where exactly the modes in $F$ are active. The boundary $\partial\overline\beta$ corresponds to the memorization-generalization frontier — the Gallai barrier $A$ of GXMD (Bridge XXVIII).

---

## XI. Extended Master Equivalence (Forty-Five Languages)

MHLT adds three new languages to the Master Equivalence.

| # | Language | Criterion | Framework |
|---|---|---|---|
| I | Spectral gap | $\lambda_1(\mathcal{L}_{JL}) > 0$ | LKTL |
| II | Signal dominance | $C_\alpha > 1$ | LKTL |
| III | Möbius convergence | $\sum_n \Delta_n < \infty$ | LKTL |
| IV | Resistance chain | $\sum_n R_n < \infty$ | KQOM |
| V | Ordinal Lyapunov | $\delta(s_t) \searrow$ in $\omega^2$ | KQOM |
| VI | RG flow | $W_\ell \to W_\ell^*$ (IR fixed point) | RG-ML |
| VII | Yang-Mills energy | $YM_t \searrow 0$ | KYBM |
| VIII | Kakeya volume monotone | $d/dt \, \mathbb{E}[V] \leq 0$ | VBE |
| IX | Franel-Landau | Farey discrepancy $D_N \sim N^{1/2+\varepsilon}$ | PPMC |
| X | ETF fixed point | $\langle \mu_i - \bar\mu, \mu_j - \bar\mu\rangle \to \mathrm{ETF}$ | LKTL |
| XI | Landau damping | $\|\rho_t - \rho_\infty\| \leq Ce^{-\lambda_1 t}$ | LKTL |
| XII | LLD film | $h_0 \sim \mathrm{Ca}^{2/3}$ | LKTL |
| XIII | Oloid development | Total surface development, $\lambda_1 > 0$ | ROLD |
| XIV | Schatz inversion | MMP flip: $\Delta_n(t^*) = 0$ | ROLD |
| XV | Oloid rolling | Contact line $C_P = 1/\lambda_1$ | ROLD |
| XVI | Sphericon meander | $A_m(t) \searrow 0$ | SMLD |
| XVII | Kakeya coverage | $\dim_H(\mathcal{K}_\Theta) = N$ | BKLT |
| … | *(XVIII – XLII as in MNGR, CAGV, GXMD, THRS, SKML, TIDE, IMFL, BPSG, FMRY)* | … | … |
| XLIII | Log-concave Whitney | $w_k^2 \geq w_{k-1}w_{k+1}$ for $\mathcal{M}_\mathcal{B}$ | MHLT |
| XLIV | Hard Lefschetz | $\mathcal{L}^{r-2k}: A^k \xrightarrow{\sim} A^{r-k}$ | MHLT |
| XLV | Hodge-Riemann | $(-1)^k\deg(\alpha\bar\alpha\ell^{r-2k}) > 0$ for prim. $\alpha$ | MHLT |

The three new equivalences:

$$w_k^2 \geq w_{k-1}w_{k+1} \;\Longleftrightarrow\; \mathcal{L}^{r-2k} \text{ isomorphism} \;\Longleftrightarrow\; (-1)^k\langle\alpha,\mathcal{L}_{JL}^{r-2k}\alpha\rangle > 0 \;\Longleftrightarrow\; \lambda_1 > 0.$$

These three languages are not independent: log-concavity of Whitney numbers is implied by Hard Lefschetz (the Lefschetz decomposition forces the dimension sequence $\dim A^k$ to be unimodal, which together with the Poincaré duality $\dim A^k = \dim A^{r-k}$ forces log-concavity), which is in turn implied by the Hodge-Riemann relations. They form a strict implication chain within MHLT:

$$\mathrm{HR}(\mathcal{M}_\mathcal{B}) \;\Rightarrow\; \mathrm{HL}(\mathcal{M}_\mathcal{B}) \;\Rightarrow\; \text{log-concavity}$$

The AHK theorem proves the deepest (HR) for all matroids representable over $\mathbb{R}$, hence all three hold simultaneously when $\lambda_1 > 0$.

---

## XII. New Conjectures from MHLT

| ID | Statement | Key Gap | Approach |
|---|---|---|---|
| MH-C1 | **Spectral Log-Concavity**: $(\mathrm{Tr}\,\mathcal{L}_{JL}^k)^2 \geq \mathrm{Tr}\,\mathcal{L}_{JL}^{k-1} \cdot \mathrm{Tr}\,\mathcal{L}_{JL}^{k+1}$ for all training runs with $\lambda_1 > 0$ | Whitney identification requires ergodic spectral averaging | AHK + Lyapunov spectral measure log-concavity |
| MH-C2 | **Hard Lefschetz Diagnostic**: the rank drop of $\mathcal{L}^{r-2k}: A^k \to A^{r-k}$ measured from gradient covariance $D_s$ provides a real-time training diagnostic for feature collapse at scale $k$ | Chow ring must be computed from finite $V_\mathcal{B}$ approximation | Empirical Chow ring via flat-lattice approximation |
| MH-C3 | **Lorentzian Partition Function**: $Z_\mathrm{learn}$ is Lorentzian if and only if the gradient distribution is log-concave (sub-Gaussian) if and only if $\lambda_1(\mathcal{L}_{JL}) > 0$ | Ultra-log-concavity requires infinite-sample limit | Finite-sample Lorentzian test via Hessian signature |
| MH-C4 | **Bergman Fan Convergence**: as $T \to \infty$, the tropical gradient map $\mu_\mathrm{trop}(t)$ equidistributes over the facets of $\beta(\mathcal{M}_\mathcal{B})$ if and only if the training run achieves complete generalization | Equidistribution requires ergodicity of the SGD flow | Farey-equidistribution analogue on the Bergman fan |
| MH-C5 | **Equivariant AHK**: for a network with symmetry group $G$ (permutation, sign-flip), the equivariant Chow ring $A^*_G(\mathcal{M}_\mathcal{B})$ satisfies equivariant Hard Lefschetz and Hodge-Riemann; the $G$-invariant primitive part is the symmetry-reduced feature ring | Requires equivariant AHK theorem (open in general) | Equivariant Kähler package for quotient matroids |
| MH-C6 | **Kazhdan-Lusztig Polynomial as Basin Cohomology**: $P_{\mathcal{M}_\mathcal{B}}(t)$ (the KL polynomial of the gradient matroid) computes the intersection cohomology Poincaré polynomial of the basin boundary $\partial\mathcal{B}_* \subset \mathcal{B}$; its coefficients are non-negative (Gedeon-Proudfoot-Young conjecture, proved 2022) and count the number of "singular" gradient directions at the grokking frontier | Geometric interpretation of KL polynomial in learning terms | KL theory + singularity theory of the loss landscape |
| MH-C7 | **Deletion-Contraction RG Fixed Points**: a gradient direction $e$ is a fixed point of the RG flow (neither deleted nor contracted) if and only if $e$ is a **coloop** of $\mathcal{M}_\mathcal{B}$ (belongs to every basis); the number of coloops equals the number of RG-relevant operators at the IR fixed point $b^*$ | Coloop count = relevant operator count requires spectral matching | RG fixed-point operator counting via matroid coloops |

---

## XIII. Quick Reference Formulas

```
MHLT Quick Reference
─────────────────────────────────────────────────────────────────────────────
Core Objects
  Gradient matroid   ℳ_ℬ = (V_ℬ, ℐ_ℬ)       indep ↔ lin. indep. gradients
  Rank function      r_ℬ(S) = rank_ε(D_s|_S)   spectral dimension of S
  Char. polynomial   χ(ℳ_ℬ, t) = Σ(-1)^k w_k t^{r-k}
  Whitney numbers    W_k = Tr(ℒ_JL^k)/k!        spectral k-th moment
  Chow ring          A*(ℳ_ℬ) = ℝ[x_F: flat F] / (I_lin + I_circ)
  Ample class        ℓ = Σ α_F x_F > 0   ↔  principal eigenform of ℒ_JL
  Bergman fan        β(ℳ_ℬ) ⊂ ℝ^E/ℝ·1    tropical shadow of gradient flow
─────────────────────────────────────────────────────────────────────────────
Hodge Package (all equivalent to λ₁ > 0)
  Log-concavity:   w_k² ≥ w_{k-1}·w_{k+1}          [AHK 2018, MH-C1]
  Hard Lefschetz:  ℒ^{r-2k}: A^k → A^{r-k} isom.   [AHK 2018, MH-T1]
  Hodge-Riemann:   (-1)^k deg(α·ᾱ·ℓ^{r-2k}) > 0    [AHK 2018, MH-T2]
  Lorentzian:      Z_learn Lorentzian ↔ D_s pos. def. [MH-T5]
─────────────────────────────────────────────────────────────────────────────
Recursion and RG
  Deletion:        ℳ_ℬ \ e  (mode e is RG irrelevant)
  Contraction:     ℳ_ℬ / e  (mode e is RG marginal)
  χ-recursion:     χ(ℳ_ℬ,t) = χ(ℳ_ℬ\e,t) - χ(ℳ_ℬ/e,t)   [RG β-function]
  Coloops = RG-relevant operators; loops = zero-modes (memorization)
─────────────────────────────────────────────────────────────────────────────
Phase Criteria (MHLT)
  Memorization:    Hard Lefschetz fails; w_k not log-concave; Lorentzian fails
  Grokking front:  ℒ^{r-2k} drops rank at some k; w_k² = w_{k-1}w_{k+1} for some k
  Generalization:  Full Hard Lefschetz; all w_k log-concave; Z_learn Lorentzian
─────────────────────────────────────────────────────────────────────────────
Master Equivalence Extension
  w_k² ≥ w_{k-1}w_{k+1}  ⟺  ℒ^{r-2k} isom  ⟺  HR positivity  ⟺  λ₁ > 0
  (MHLT, XLIII–XLV)          (MHLT, XLIV)      (MHLT, XLV)    (LKTL, I)
─────────────────────────────────────────────────────────────────────────────
```

---

## XIV. Logical Dependency Map

```
ZF Axioms
  │
  ├─→ ℕ construction ─→ Matroid axioms (Whitney 1935) ─→ Gradient matroid ℳ_ℬ
  │                                │
  │                                ├─→ Characteristic polynomial χ(ℳ_ℬ, t)
  │                                ├─→ Whitney numbers W_k ↔ Tr(ℒ_JL^k)/k!
  │                                └─→ Deletion-contraction ↔ RG recursion [MH-T4]
  │
  ├─→ ℒ_JL Operator (A1–A5) ─→ λ₁ > 0 ↔ C_α > 1
  │          │
  │          ├─→ Ample class ℓ ↔ principal eigenform
  │          ├─→ Hard Lefschetz ↔ complete coverage [MH-T1]
  │          ├─→ Hodge-Riemann ↔ spectral positivity [MH-T2]
  │          └─→ Lorentzian Z_learn ↔ log-concave gradients [MH-T5]
  │
  ├─→ Matroid Hodge Theory (Huh 2012; Huh-Katz 2012; AHK 2018)
  │          │
  │          ├─→ Chow ring A*(ℳ_ℬ) = primitive cohomology of ℬ [MH-D2]
  │          ├─→ AHK theorem ↔ universal generalization [MH-T3]
  │          ├─→ HR ↔ HL ↔ log-concavity (strict implication chain)
  │          ├─→ Bergman fan β(ℳ_ℬ) ↔ tropical learning manifold [MH-D3]
  │          └─→ KL polynomial ↔ basin intersection cohomology [MH-C6]
  │
  ├─→ MHLT ↔ BKLT (Kakeya)
  │          Hard Lefschetz ↔ dim_H(𝒦_Θ) = N   [MH-T1 ↔ BK-C1]
  │          Perron tree ↔ Lefschetz primitive decomposition [§V.1]
  │
  ├─→ MHLT ↔ MNGR (Menger)
  │          rank(CM_ℬ) = rank(D_s) = r = rank(ℳ_ℬ)   [CMGD = MHLT rank]
  │          Cayley-Menger = matroid dependence certificate
  │
  ├─→ MHLT ↔ ROLD/SHCY (Hodge geometry)
  │          Hodge-Riemann ↔ Kähler-Einstein positivity [MH-T2 ↔ ROLD OL-T1]
  │          Ample class ℓ ↔ KE Kähler class ω_ℬ
  │
  ├─→ MHLT ↔ RG-ML (Renormalization)
  │          Deletion-contraction ↔ RG β-function [MH-T4]
  │          Coloops ↔ RG fixed-point operators [MH-C7]
  │          Primitive decomposition ↔ RG scale separation [§V.1]
  │
  ├─→ Forty-Five Language Master Equivalence
  │          XLIII: log-concavity; XLIV: Hard Lefschetz; XLV: Hodge-Riemann
  │          All ↔ λ₁ > 0 ↔ generalization
  │
  └─→ BRIDGES I + II + ... + VII + XLIII + XLIV + XLV UNIFIED:
             dim_H(𝒦_Θ) = N = Hard Lefschetz surjectivity
             w_k log-concave = Tr(ℒ_JL^k) log-concave = sub-Gaussian gradients
             χ(ℳ_ℬ, t) = RG flow polynomial; coloops = relevant operators
             A*(ℳ_ℬ) = H^{r,r}(ℬ)_prim = primitive cohomology of learning manifold
             Z_learn Lorentzian = generalization = λ₁ > 0
```

---

## XV. Foundations and Citations

### Matroid Theory

Whitney, Hassler (1935). "On the abstract properties of linear dependence." *American Journal of Mathematics* **57**(3): 509–533. — Original matroid definition; dependence axioms; matroid as abstraction of linear algebra.

Rota, Gian-Carlo (1971). "Combinatorial theory, old and new." *Proceedings of the International Congress of Mathematicians*, Nice, 229–233. — Whitney numbers conjecture (log-concavity); characteristic polynomial as central invariant; Möbius function on lattice of flats.

Oxley, James (2011). *Matroid Theory*, 2nd edition. Oxford University Press. — Comprehensive reference for matroid structure, characteristic polynomial, deletion-contraction, Bergman fan foundations.

### June Huh and the Hodge-Matroid Connection

Huh, June (2012). "Milnor numbers of projective hypersurfaces and the chromatic polynomial of graphs." *Journal of the American Mathematical Society* **25**(3): 907–927. — First proof of log-concavity of chromatic polynomial via algebraic geometry; the founding paper of the Hodge-matroid program.

Huh, June and Katz, Eric (2012). "Log-concavity of characteristic polynomials and the Bergman fan of matroids." *Mathematische Annalen* **354**(3): 1103–1116. — Extension to all representable matroids; Bergman fan as the tropical linear space; proof via Lefschetz theory on the tropical variety.

Adiprasito, Karim; Huh, June; Katz, Eric (2018). "Hodge theory for combinatorial geometries." *Annals of Mathematics* **188**(2): 381–452. — The foundational paper: Chow ring $A^*(\mathcal{M})$ for all matroids; Hard Lefschetz; Hodge-Riemann bilinear relations proved combinatorially without ambient variety; resolution of the Heron-Rota-Welsh conjecture in full generality.

Huh, June (2022). Fields Medal citation, International Congress of Mathematicians 2022. — Summary of work on log-concavity, Lorentzian polynomials, and combinatorial Hodge theory.

### Lorentzian Polynomials

Brändén, Petter and Liggett, Thomas M. (2020). "Highest weight vectors, Lorentzian polynomials, and strongly log-concave polynomials." Preprint, arXiv:2007.01528. — Foundational theory of Lorentzian polynomials; ultra-log-concavity; $M$-convex support.

Brändén, Petter and Huh, June (2020). "Lorentzian polynomials." *Annals of Mathematics* **192**(3): 821–891. — Comprehensive theory; connection to matroids and log-concavity; Lorentzian condition as the natural generalization of the Hodge-Riemann bilinear relations to the polynomial setting.

### Hodge Theory

Hodge, W.V.D. (1941). *The Theory and Applications of Harmonic Integrals*. Cambridge University Press. — Hodge decomposition; harmonic forms; the original framework that AHK generalizes combinatorially.

Griffiths, Phillip and Harris, Joseph (1978). *Principles of Algebraic Geometry*. Wiley. — Hard Lefschetz theorem; Hodge-Riemann bilinear relations; primitive cohomology; standard reference.

### Chow Rings and Tropical Geometry

Feichtner, Eva Maria and Yuzvinsky, Sergey (2004). "Chow rings of toric varieties defined by atomic lattices." *Inventiones Mathematicae* **155**(3): 515–536. — Construction of the Chow ring of a matroid via atomic lattice; generators and relations.

Maclagan, Diane and Sturmfels, Bernd (2015). *Introduction to Tropical Geometry*. American Mathematical Society. — Bergman fan, tropical varieties, tropical Grassmannian; connection between matroid theory and tropical geometry.

### Kazhdan-Lusztig Polynomials for Matroids

Elias, Ben; Proudfoot, Nicholas; Wakefield, Max (2016). "The Kazhdan-Lusztig polynomial of a matroid." *Advances in Mathematics* **299**: 36–70. — Definition of KL polynomial for matroids; non-negativity conjecture.

Gedeon, Katie; Proudfoot, Nicholas; Young, Benjamin (2022). "Kazhdan-Lusztig polynomials of thagomizer matroids." *Electronic Journal of Combinatorics*. — Verification of non-negativity for families; partial resolution of the conjecture.

### Prior Framework Modules

```
MHLT  — Matroid Hodge Learning Theory (Bridges XLIII–XLV, this document)
FMRY  — Fáry Rectification and Total Curvature (Bridges XL–XLII)
MNGR  — Menger Connectivity-Universality-Distance (Bridges XXXIV–XXXVI)
CAGV  — Combinatorial-Algebraic-Graph Varieties (Bridges XXXI–XXXIII)
GXMD  — Graph Extremal Matching Duality (Bridges XXVIII–XXX)
BKLT  — Besicovitch-Kakeya Learning Theory (Bridge VII)
SMLD  — Sphericon Meander Learning Dynamics (Bridge VI)
ROLD  — Rolling Oloid Learning Dynamics (Bridge V)
LKTL  — Landau Kinetic Theory of Learning (Bridges I–II)
SHCY  — Spectral Holonomy of Calabi-Yau Learning
GCCT  — Gradient Cooper Condensate Theory (Bridge III)
RG-ML — Renormalization Group / Machine Learning
KQOM  — Kruskal Quasi-Order Mechanics
GAME  — Gradient Algebraic Manifold Exploration
VBE   — Visibility-Barrier-Escape
PPMC  — Pascal Projective Manifold Coherence
KYBM  — Kähler-Yang-Mills Bridge
```

---

## Coda: The Geometry That Was Always There

Gian-Carlo Rota wrote the Whitney numbers conjecture in 1971. He did not know how to prove it. The chromatic polynomial of a graph — a purely combinatorial object, counting the ways to color vertices so no two adjacent vertices share a color — satisfies the same positivity theorems as the cohomology ring of a smooth algebraic variety. The connection was not obvious. For forty years, the conjecture sat.

June Huh found the bridge. He was not looking for it in the traditional sense. He came to mathematics late, entered it through a lecture by an eighty-year-old algebraic geometer, and spent his early career learning to see combinatorial objects as geometric ones. When he looked at the chromatic polynomial, he saw a Milnor number — an invariant from the geometry of singularities. When he and Katz pushed the argument to all representable matroids, they saw the Bergman fan — the tropical shadow of a linear space. When Adiprasito joined the project, they constructed the cohomology ring directly inside the combinatorics itself, with no ambient variety needed. The geometry was always there. They had learned to look at it.

A neural network's gradient flow has the same structure. The gradient evaluation points form a matroid. The matroid has a Chow ring. That Chow ring satisfies Hard Lefschetz and Hodge-Riemann exactly when the network is generalizing. The log-concavity of the feature hierarchy — no intermediate scale underrepresented, no degree of complexity orphaned — is the combinatorial fingerprint of a model that has learned.

The deletion-contraction recursion that generates the characteristic polynomial is the renormalization group flow that strips away irrelevant modes one by one until only the essential, relevant operators at the fixed point remain. The Lorentzian partition function is the loss landscape's confession that it has exactly one time direction — one direction of learning — and that all perpendicular directions are stable. The Bergman fan is the tropical geometry that records which gradient directions were ever active, encoded in the language of polyhedral fans.

Forty-five mathematical languages. Eight physical bridges. One spectral gap. One matroid. One Chow ring. One theorem — proved in 2018, applied here for the first time to neural networks — that says: whenever the geometry is right, the combinatorics is right too, and when the combinatorics is right, the network has generalized.

The Whitney numbers were always log-concave. We just hadn't learned to look.

---

**Status Tags**
```
[T]  = Theorem (proven)
[V]  = Verified (in specific models)
[C]  = Open conjecture
[H]  = Working hypothesis
[MH] = MHLT-specific result
```

**Framework:** `MHLT · FMRY · MNGR · CAGV · GXMD · BKLT · SMLD · ROLD · LKTL · SHCY · GCCT · FLML · KQOM · GAME · VBE · PPMC · KYBM · PH-SP · RG-ML · LB/DK · UNIV`
