---
layout: post

title: "Inverting matrices the right way"

date: 2025-02-05
---

Let $V$ be a finite-dimensional vector space over a field $\mathbb{F}$. Let $A : V \to V$ be a linear map with nonzero determinant. Why does $A$ have an inverse?

Here is the standard proof presented in a linear algebra course:

1. Choose a basis $e_1, \dots, e_n$ for $V$ and write $A$ in matrix form $(A_{ij})$ with respect to the basis.

2. Define the determinant $\det A=\sum_{\sigma \in S_n}(-1)^\sigma\prod_iA_{i,\sigma(i)}$.

3. Define the adjugate matrix $\operatorname{adj} A$ (which everyone thinks is called the adjoint) as the $n \times n$ matrix with entries

   $$
   (\operatorname{adj} A)_{ij} = (-1)^{i+j}\det M_{ji}
   $$

   where $M_{ji}$ is the $j,i$-minor of $A$; that is, $M_{ji}$ is the resulting matrix when we delete the $j$th row and $i$th column from $A$.

4. Show by a direct calculation that $A \cdot \operatorname{adj} A = \operatorname{adj} A \cdot A = \det A \cdot I_V$, and therefore $\frac{1}{\det A}\operatorname{adj} A$ is the inverse of $A$.
Thus, $A$ has an inverse.

The adjugate matrix proceeds to never get mentioned again.

This proof is bad. It's very computational,
and it involves defining lots of things by complicated formulas that appear out of nowhere.

The root of the proof's evil comes from its first three words. You don't need to mention a basis to say "linear maps (with nonzero determinant) have inverses", so an ideal proof would not use one. 
A base-free proof will also allow us to generalize the theorem to more general contexts, like vector bundles. But even before learning of such things, one's heart strives for a better proof.
 <!-- but even if you haven't heard of those, don't you have a heart? We can do better. -->

---

I will start with an informal reminder on the _exterior power_,
to hopefully make the post comprehensible for people who only sort-of understand tensor products.

Given a vector space $V$, its $k$th exterior power $\bigwedge^k V$ is a strange vector space,
containing the strange vectors $v_1 \wedge v_2 \wedge \cdots \wedge v_k$,
where each $v_i$ is some vector in $V$.
In other words, there are vectors in $\bigwedge^k V$ that are themselves built from a sequence of $k$ vectors from $V$, separated by wedge symbols.

This doesn't mean that every vector of $\bigwedge^k V$ has the form $v_1 \wedge v_2 \wedge \cdots \wedge v_k$; general vectors of the exterior power are messier, and can only be expressed as a sum of those pure vectors. For instance, 

$$\pmatrix{1 \\ 2 \\ 3} \wedge \pmatrix{4 \\ 5 \\ 6} + \pmatrix{7 \\ 8 \\ 9} \wedge \pmatrix{10 \\ 11 \\ 12}$$

is a perfectly valid vector in $\bigwedge^2 \mathbb{R}^3$, and there is no reason to believe we can write any shorter expression for it.

There are two axioms satisfied by the pure vectors of $\bigwedge^k V$.
The first one is multilinearity, meaning that

$$
v_1 \wedge \cdots \wedge (\lambda \cdot u + \lambda' \cdot u')  \wedge \cdots \wedge v_k = 
\lambda \cdot \left(v_1 \wedge \cdots \wedge u  \wedge \cdots \wedge v_k\right) + \lambda' \cdot \left(v_1 \wedge \cdots \wedge u'  \wedge \cdots \wedge v_k\right)
$$

and the second is antisymmetry, which says that replacing two components changes the vector by a factor of $-1$:

$$
v_1 \wedge \cdots \wedge v_i  \wedge \cdots  \wedge v_j  \wedge \cdots v_k = -\left(v_1 \wedge \cdots \wedge v_j  \wedge \cdots  \wedge v_i  \wedge \cdots v_k\right).
$$

Note how our (sloppy) definition for $\bigwedge^k V$ did not invoke any choice of a basis.

Using the axioms, we can calculate the dimension of the exterior product. Say that $V$ has dimension $n$, and let $e_1, \dots, e_n$ be some basis for $V$. Then we can use the first axiom to express any pure vector $v_1 \wedge \cdots \wedge v_k$ using only pure vectors of the form $e_{i_1} \wedge \cdots \wedge e_{i_k}$. Then, we can use the second axiom to sort the $e_i$'s, and force $i_1 \le i_2 \le \cdots \le i_k$. However, a double appearance of some $e_i$ makes the entire vector vanish, since $v \wedge v = -\left(v \wedge v\right)$ by the second axiom! So actually, we can assume that $i_1 < \cdots < i_k$.
The amount of tuples satisfying $i_1 < \cdots < i_k$ is $\binom{n}{k}$,
so we have $\operatorname{dim}(\bigwedge^k V) = \binom{n}{k}$.

(In the last paragraph I assumed that the characteristic of $\mathbb{F}$ is not $2$; when the characteristic is $2$ we consider the $v \wedge v = 0$ property as an axiom)

---

Now, let us define the determinant in a more sensible way.

Suppose that $V$ is an $n$-dimensional vector space.
Consider the $n$th exterior power $\bigwedge^n V$. This is a one-dimensional vector space, just like $\mathbb{F}$, but there is no natural isomorphism between it and $\mathbb{F}$. Our operator $A : V \to V$ defines an operator $\bigwedge^nA : \bigwedge^nV \to \bigwedge^nV$ via

$$
v_1 \wedge v_2\wedge \cdots\wedge v_n \mapsto Av_1\wedge Av_2\wedge \cdots\wedge Av_n.
$$

Since $\bigwedge^nV$ is one-dimensional, the only operators that can be defined on it are multiplications by scalars. Therefore, there must be a unique scalar such that $\bigwedge^n A$ is multiplication by it. We call this scalar $\det A$.

The standard properties of determinants follow naturally. For instance, 
we can prove that $\det(AB) = \det(A) \cdot \det(B)$ by the computation

$$
\det(AB) \cdot (v_1 \wedge \cdots \wedge v_n) = ABv_1 \wedge \cdots \wedge ABv_n = \det A \cdot (Bv_1 \wedge \cdots \wedge Bv_n) = \det A \cdot \det B \cdot (v_1 \wedge \cdots \wedge v_n).
$$

<!-- to compute $\det(AB)$, we need to consider the action of applying $AB$ on all the vectors of $v_1 \wedge  v_2 \wedge  \cdots \wedge  v_n$. But applying $B$ is equivalent to multiplying the entire thing by $\det B$, and then applying $A$ is equivalent to multiplying the entire thing by $\det A$. -->

---

The sensible construction of $\operatorname{adj} A$ is a bit more complicated. Consider the vector space $\bigwedge^{n - 1}V$. This space has dimension $n$ (but it isn't naturally isomorphic to either $V$ or $V^*$). For every $v \in V$ we define a natural linear map $\phi_v:\bigwedge^{n-1}V \to \bigwedge^nV$ by $\phi_v(T) = T \wedge  v$. Since $\phi_{\lambda v+\lambda'v'} = \lambda\phi_v+\lambda'\phi_{v'}$, we actually get a linear map

$$
\phi:V \to \operatorname{Hom}\left(\bigwedge^{n-1}V, \bigwedge^nV\right).
$$

Note that $\phi$ is a map between two vector spaces of dimension $n$. It has to be injective, because for every nonzero $v$ we can find $v_1, \dots, v_{n - 1}$ so that $v_1 \wedge  \cdots \wedge  v_{n - 1} \wedge  v \ne 0$. This implies $\phi$ is an isomorphism.

This isomorphism is crucial to our definition of the adjugate. 
We want $\operatorname{adj} A$ to be a map $V \to V$,
but we have a natural isomorphism between $V$ and $\operatorname{Hom}\left(\bigwedge^{n-1}V,  \bigwedge^nV\right)$. As before, $A:V \to V$ defines a map $\bigwedge^{n-1}A\colon \bigwedge^{n-1}V \to \bigwedge^{n - 1}V$ 
that applies $A$ to each term of $v_1 \wedge \cdots \wedge v_{n - 1}$. 
Precomposition with $\bigwedge^{n-1}A$ is a linear map from $\operatorname{Hom}\left(\bigwedge^{n-1}V, \bigwedge^nV\right)$ to itself, that depends on $A$. It turns out that under the isomorphism $\phi$, this precomposition corresponds to $\operatorname{adj} A$!

Making this more concrete: $(\operatorname{adj} A)v$ is the unique vector such that, for all $v_1, \dots, v_{n - 1}$, there is equality

$$
v_1  \wedge  \cdots \wedge  v_{n - 1} \wedge  (\operatorname{adj} A)v = Av_1 \wedge  \cdots \wedge  Av_{n - 1} \wedge  v.
$$

From this property, we can deduce that $\operatorname{adj} A \cdot A = \det A \cdot I_V$, because

$$
\det A \cdot (v_1 \wedge  \cdots \wedge  v_n) = Av_1 \wedge  \cdots \wedge  Av_n = v_1 \wedge  \cdots \wedge  v_{n - 1} \wedge  (\operatorname{adj} A \cdot A) v_n.
$$

It follows that if $\det A \ne 0$, then $A'=\frac1{\det A}\operatorname{adj} A$ is a left-inverse for $A$.

If $A'$ is the left-inverse of $A$, then $A'$ must also have nonzero determinant 
(since $\det A' \cdot \det A = 1$), so by the same argument it must have a left inverse $A''$. 
We then have

$$
AA'=(A''A')AA'= A''(A'A)A' = A''A'=I_V
$$

proving that $A'$ is also a right-inverse for $A$. 
This finishes our natural construction of the inverse.

---

This proof was presented to me a few years ago by a friend and I really like it. Doing base-free constructions forces us to find the better definitions to objects like the determinant or the adjugate.

Another base-free definition worth noting, that is more famous, is that of the trace. For finite-dimensional vector spaces $V$ and $W$ there is a natural homomorphism $\Phi:V^* \otimes W \to \operatorname{Hom}(V, W)$, defined via

$$
\Phi(\phi \otimes w)(v) = \phi(v)w.
$$

This map is an isomorphism (to see that you have to use a basis - but using a basis to _prove properties_ is very different from using a basis to _construct objects_). This implies that $\operatorname{End}(V)$ is isomorphic to $V^* \otimes V$. But there is a natural functional on this space, sending $\phi \otimes v$ to $\phi(v)$, and this turns out to be equivalent to the trace. It is a fun exercise to see that properties of the trace (such as $\operatorname{Tr} AB = \operatorname{Tr} BA$) follow naturally from this definition.

---

Regarding the end of the proof, note that we showed $(\operatorname{adj} A)\cdot A = (\det A)\cdot I_V$, but not the other direction $A \cdot (\operatorname{adj} A) = (\det A) \cdot I_V$. We did prove it when $\det A\ne 0$, since in that case $\frac1{\det A}\operatorname{adj} A = A^{-1}$, however this fails in the case $\det A = 0$. There are a few options to complete this: we can view the case of zero determinant as a limit case of the nonzero determinant case, or we can use the definition of $\operatorname{adj} A$ directly: Take $u \in \ker A$, and note that in the formula

$$
v_1 \wedge  v_2 \wedge  \cdots \wedge  v_{n - 1} \wedge  (\operatorname{adj} A)v = Av_1 \wedge  Av_2 \wedge  \cdots \wedge  Av_{n - 1} \wedge  v
$$

replacing $v_{n - 1}$ by $v_{n - 1}+u$ does not alter the right hand side. The implication is that $v_1 \wedge  \cdots \wedge  v_{n - 2} \wedge  u \wedge  (\operatorname{adj} A)v = 0$, meaning that $v_1,\dots,v_{n-2},(\operatorname{adj} A)v$ have a combination that is a nontrivial element in $\ker A$. Since this is true for every choice of $v_1,\dots,v_{n - 2}$, it must be true that either $\dim \ker A>1$ or that $(\operatorname{adj} A)v$ is itself in the kernel. The first case implies that $\operatorname{adj} A$ itself vanishes, and the second is what we needed to prove.

I find it curious that $(\operatorname{adj} A)\cdot A = (\det A)\cdot I_V$ followed so directly from the definition, but the other direction $A \cdot (\operatorname{adj} A)=(\det A) \cdot I_V$ was so indirect. Both formulas are true for any square matrix with entries in any commutative ring.
<!-- but I suspect that in some more general setting, it really  that one of them holds and the other doesn't. -->