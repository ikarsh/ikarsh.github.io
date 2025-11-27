---
layout: post

title: "Inverting matrices the right way"

date: 2025-02-05
---

$$
\newcommand{\nc}[3]{\newcommand{#2}[#1]{#3}}
\nc0{\(}{\left(}
\nc0{\)}{\right)}
\nc0{F}{\mathbb{F}}
\nc0{s}{\sigma}
\nc0{End}{\text{End}}
\nc0{adj}{\text{adj}}
\nc0{Tr}{\text{Tr}}
\nc0{Wedge}{\bigwedge}
\nc0{^}{\wedge}
\nc0{mto}{\mapsto}
\nc0{a}{\alpha}
\nc2{Hom}{\text{Hom}\(#1, #2\)}
\nc0{\ox}{\otimes}
$$

Let $\F$ be a field and $V$ be an $n$-dimensional vector space over $\F$. Let $A \in \End(V)$ be a linear operator with nonzero determinant. Why does $A$ have an inverse?

Here is the standard proof presented in a linear algebra course:

1. Choose a basis $B=(e_i)$ for $V$ and write $A$ in matrix form $(A_{ij})$ with respect to the basis.

2. Define the determinant $\det A=\sum_{\s \in S_n}(-1)^\s\prod_iA_{i,\s(i)}$.

3. Define the adjugate matrix $\adj A$ as the $n \times n$ matrix with entries

   $$
   (\adj A)_{ij} = (-1)^{i+j}\det M_{ji}
   $$

   where $M_{ji}$ is the $j,i$-minor of $A$, that is, the resulting matrix when we delete the $j$th row and $i$th column.

4. Observe via direct calculation that $A \cdot \adj A = \adj A \cdot A = \det A \cdot I_V$, and therefore $\frac{1}{\det A}\adj A$ is the inverse of $A$.

This adjugate matrix proceeds to never get mentioned again.

This proof is bad, as seen immediately by its first three words. Choosing a basis should be a last resort; A base-free proof is better if one wants to generalize the case of a vector space to vector bundles, or if one simply has a heart. We can do better.

---

Let us start by defining the determinant in a more sensible way.

Consider the $n$th exterior algebra $\Wedge^n V$. This is a one-dimensional vector space over $\F$, but there is no natural isomorphism between it and $\F$. Our operator $A$ defines an operator $\Wedge^nA$ on $\End(\Wedge^nA)$ via

$$
v_1 \^v_2\^\dots\^v_n \mto Av_1\^Av_2\^\dots\^Av_n
$$

Since $\Wedge^nV$ is one-dimensional, the only operators that can be defined on it are multiplications by scalars. Therefore, there must be a unique scalar such that $\Wedge^n A$ is multiplication by it. We call this scalar $\det A$.

The standard properties of determinants follow naturally. For instance, to compute $\det(AB)$, we need to consider the action of applying $AB$ on all the vectors of $v_1 \^ v_2 \^ \dots \^ v_n$. But applying $B$ is equivalent to multiplying the entire thing by $\det B$, and then applying $A$ is equivalent to multiplying the entire thing by $\det A$.

---

The construction of the adjugate is a bit more complicated. Consider the vector space $\Wedge^{n - 1}V$. This space has dimension $n$ (but it isn't naturally isomorphic to either $V$ or $V^*$). For every $v \in V$ we define a natural linear map $\phi_v:\Wedge^{n-1}V \to \Wedge^nV$ by $\phi_v(T) = T \^ v$. Since $\phi_{\a v+\a'v'} = \a\phi_v+\a'\phi_{v'}$, we actually get a linear map

$$
\phi:V \to \Hom{\Wedge^{n-1}V}{\Wedge^nV}
$$

Note that $\phi$ is a map between two vector spaces of dimension $n$. It has to be injective, since for every nonzero $v$ we can find $v_1, \dots, v_{n - 1}$ so that $v_1 \^ \dots \^ v_{n - 1} \^ v \ne 0$. This implies $\phi$ is an isomorphism.

We are now ready to define the adjugate. Hooray! We want it to be an operator on $V$, but we know $V$ is naturally isomorphic to $\Hom{\Wedge^{n-1}V} {\Wedge^nV}$. As before, $A \in \End(V)$ defines $\Wedge^{n-1}A \in \End(\Wedge^{n-1}V)$ by applying $A$ on all the vectors, and pre-composition with $\Wedge^{n-1}A$ is an operator on $\Hom{\Wedge^{n-1}V}{\Wedge^nV}$ that depends on $A$. This turns out to be the adjugate!

Making this more concrete: $(\adj A)v$ is the unique vector such that, for all $v_1, \dots, v_{n - 1}$, there is equality

$$
v_1 \^ v_2 \^ \dots \^ v_{n - 1} \^ (\adj A)v = Av_1 \^ Av_2 \^ \dots \^ Av_{n - 1} \^ v
$$

From this property, we can deduce that $(\adj A) \cdot A = (\det A) \cdot I_V$, because of

$$
(\det A)v_1 \^ \dots \^ v_n = Av_1 \^ \dots \^ Av_n = v_1 \^ \dots \^ v_{n - 1} \^ (\adj A)Av_n
$$

Which shows that if $\det A$ is nonzero, $A'=\frac1{\det A}\adj A$ is a left-inverse for $A$.

If $A'$ is the left-inverse of $A$, then $A'$ must also have nonzero determinant and by the same argument have a left-inverse $A''$. We then have

$$
AA'=A''A'AA'=A''A'=I_V
$$

proving that $A'$ is also a right-inverse for $A$. This finishes our natural construction of the inverse.

---

This proof was presented to me a few years ago by a friend and I really like it. Doing base-free constructions forces us to find the better definitions to objects like the determinant or the adjugate.

Another base-free definition worth noting, that is more famous, is that of the trace. For finite-dimensional vector spaces $V$ and $W$ there is a natural homomorphism $\Phi:V^* \ox W \to \Hom VW$, defined via

$$
\Phi(\phi \ox w)(v) = \phi(v)w
$$

This map is an isomorphism (to see that you kind of have to use a basis - but using a basis to _prove_ a property is far more legitimate than using a basis to _construct_ an object). This implies that $\End(V)$ is isomorphic to $V^* \ox V$. But there is a natural functional on this space, sending $\phi \ox v$ to $\phi(v)$, and this turns out to be equivalent to the trace. It is a fun exercise to see that properties of the trace (such as $\Tr AB = \Tr BA$) follow naturally from this definition.

---

Regarding the end of the proof, note that we showed $(\adj A)\cdot A = (\det A)\cdot I_V$, but not the other direction $A \cdot (\adj A) = (\det A) \cdot I_V$. We did prove it when $\det A\ne 0$, since in that case $\frac1{\det A}\adj A = A^{-1}$, however this fails in the case $\det A = 0$. There are a few options to complete this: we can view the case of zero determinant as a limit case of the nonzero determinant case, or we can use the definition of $\adj A$ directly: Take $u \in \ker A$, and note that in the formula

$$
v_1 \^ v_2 \^ \dots \^ v_{n - 1} \^ (\adj A)v = Av_1 \^ Av_2 \^ \dots \^ Av_{n - 1} \^ v
$$

replacing $v_{n - 1}$ by $v_{n - 1}+u$ does not alter the right hand side. The implication is that $v_1 \^ \dots \^ v_{n - 2} \^ u \^ (\adj A)v = 0$, meaning that $v_1,\dots,v_{n-2},(\adj A)v$ have a combination that is a nontrivial element in $\ker A$. Since this is true for every choice of $v_1,\dots,v_{n - 2}$, it must be true that either $\dim \ker A>1$ or that $(\adj A)v$ is itself in the kernel. The first case implies that $\adj A$ itself vanishes, and the second is what we needed to prove.

I find it curious that $(\adj A)\cdot A = (\det A)\cdot I_V$ followed so directly from the definition, but the other direction $A \cdot (\adj A)=(\det A) \cdot I_V$ was so indirect. Both formulas are true for any square matrix with entries in any commutative ring.
