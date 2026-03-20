# Terminology

## Security goals

Desirable features of communications and networks
- **Confidentiality**: Information is available to intended receiver only
- ***Integrity***: Information is received exactly as sent
- **Availability**: Service is always available even if someone intends to disturbt the network
- **Accountability**: It is always possible to identify who is responsible for any information event
- **Privacy**: Information is not disclosed (*shared*) to anyone
## General Attacks
- **Eavesdropping**: learning confidential information
- **Modification**: modify a message in transit
- **Denial of service**: make the service unavailable
- **Forgery**: build a fake message, pretending it was sent by someone else
- **Masquerade**: posing as someone else in a single message transmission or interactive protocol
- **Repudiation**: denying having sent or received some message
- **Profiling**: gathering information about a single user
- **Fingerprinting**: identifying the user associated with some message
- **Traffic analysis**: learning origin, destination, length, times, types of communication but not the content
### Security services
Services that should protect communication and networks protocol from attacks
- **Secrecy**: makes message unintelligible for eavesdroppers
- **Integrity protection**: makes it possible to detect whether a message was intercepted and modified
- **Access control**: can protect from denial of service
- **Message authentication**: allows to detect forged messages
- **Entity authentication**: protects from masquerades
- **Non repudiation**: prevents repudiation by source and/or destination
- **Anonymization**: prevents consistent association between message and user
- **Key management**: ancillary services to cryptographic mechanisms
- **Randomness generation**: ancillary service to most mechanisms
### Security mechanisms
Is a way to implement some security service
- **Encryption**: message is transformed and only the intended receiver can reverse the transform
- **Digital signature**: a string is appended (*or added*) to the message that can only be **created by the legitimate source** but **can be PUBLICLY verified**
- **Message authentication codes**: a string is appended to the message that can only be created by the legitimate transmitter or receiver
- **Intrusion detection**: identify characteristic of *malicious behaviour* (**Signature**) or *non-typical legitimate behaviour* (**Anomaly**)
- **Randomization**: events of the same category are randomly permuted to break dependences
- **Key agreement**: several agents cooperate to produce a key from inputs provided by each of them
- **Random number generation**: produce a sequence of integers each unpredictable from the others  
![[Screenshot 2026-03-13 alle 20.01.31.png]]
## Network security protocols
A **layer $N$ security protocol** is a **mechanism** that offers one or more security service to protect the information in the **layer $N$ data unit and above (NOT BELOW) of a network**
![[Screenshot 2026-03-13 alle 20.02.30.png]]
![[Screenshot 2026-03-13 alle 20.02.56.png]]

# Quantitative Definition and Evaluation of Security
## Random system
A *random system* with input $x \in X$ and output $y \in Y$ can be described by the *conditional probability distribution* $p_{y|x} : X \times Y \rightarrow [0,1]$ of the output given the input.
$$
p_{y|x}(b|a) = P[y=b|x=a], \forall a \in X, b\in Y
$$
or is better to describe by:
- an *auxiliary random source* $v$, with alphabet $V$ and probability distribution $p_v : V \rightarrow [0,1]$ 
- a deterministic *input-output relationship* $y= f(x,v)$

A *randomized algorithm* is the computational implementation of a random system composed of an auxiliary pseudo-random source $v$, and a *deterministic computation* $A[x,v] \rightarrow y = f(x,v)$

We model a single attack as a *randomized algorithm* $A$ characterized by:
- a **success event** $S_A$ -> *the attacker reach his goal (change of a message)*
- **execution time** $T_A$ -> random variable and usually indicated in seconds even if we don't consider as physical time unit
Similarly, we model a security mechanism as a *randomized algorithm* $M$ characterized by its *execution time* $T_M$

## Security measure
The security provided by mechanism $M$ against attack $A$ can be measured by the probability
$$
P[S_A ; A,M]
$$
that the success event $S_A$ is achieved by attack $A$ with mechanism $M$. it is also customary to measure the *security level* of $M$ against $A$ in *bits* as:
$$
SL(M;A) = log_{1/2}P[S_A;A,M] \space [bits]
$$
The security of $M$ against the class of attacks $\mathcal{A}$ is measured by
$$
sup_{A\in \mathcal{A}}P[S_{\mathcal{A}}; A,M]
$$
and the *security level* of $M$ against $\mathcal{A}$ in bits:
$$
SL(M,\mathcal{A})= log_{1/2}sup_{A \in \mathcal{A}}P[S_{\mathcal{A}}; A,M] \space \space [bits]
$$
But typically a mechanism is designed to aim of being secure against widest possible class of attacks.
So we said a *security mechanism* is $\epsilon$-uncoditional security against class $\mathcal{A}$ of attacks for some $\epsilon > 0$ if $P[S_{\mathcal{A}};A,M] \leq \epsilon, \forall A \in \mathcal{A}$ all attacks in $\mathcal{A}$ succeed against $M$ with probability no more than $\epsilon$ 
A *security mechanism* is said to offer ($\epsilon, T_0$-*computational security*) against a class $\mathcal{A}$ of attacks, for some $\epsilon > 0, T_0 > 0$ if $P[S_{\mathcal{A}}\cap \{T_A \leq T_0 \};A,M]\leq \epsilon, \space \space \space \forall A \in \mathcal{A}$ 
that is all attacks in $\mathcal{A}$ succeed against $M$ within time $T_0$ with probability no more than $\epsilon$. The last definition can also be written as $P[S_{\mathcal{A}};A,M]\leq \epsilon \space, \space \forall A \in \mathcal{A}$ such that $T_A \leq T_0$ surely. That is all attacks in $\mathcal{A}$ that take up to $T_0$ time succeed against $M$ with probability no more than $\epsilon$ .
![[Pasted image 20260314200451.png]]
The problem with the formulation is that the definition depends on the state of *technological maturity*. So we allow the mechanism to depend on a *security parameter* $n \in \mathbb{N}$ (e.g. length of crypto keys, number of rounds in a iterative protocol...) that can be **increased at will**
- The legitimate operation is *still feasible* -> depends on $n$ polyomially
- The adversary operation soon become *infeasible* -> superpolynomial complexity
A sequence of security mechanisms $\{M_n\}$ indexed by some parameter $n \in \mathbb{N}$ is said to offer *asymptotic computational security* against a class $\mathcal{A}$ of attacks if:
1. $\exists$ polynomial $p(\cdot)$ such that $$\forall n \space\space,\space\space T_{M_{n}}\leq p(n)$$
2. $\forall q(\cdot),s(\cdot)$ polynomials, and sequence of attacks $\{A_n\} \subset \mathcal{A} \space, \space \exists n_0$ such that $$\forall n > n_0 , P[S_a \cap \{T_{A_{n}} \leq q(n)\};A_n,M_n] > \frac{1}{s(n)}$$
*The value of $n_0$ dependes on $q(\cdot), s(\cdot)$ and $\{A_n\}$* 
It is also said that the probability of the attack succeeding in polynomial time vanishes super polynomially so that it is *asymptotically negligible*
![[Screenshot 2026-03-15 alle 16.36.06.png]]
![[Screenshot 2026-03-15 alle 16.36.40.png]]
A sequence of security mechanisms $\{M_n\}$ indexed by some parameter $n \in \mathbb{N}$ is said to offer *asymptotic unconditional security* against a class $\mathcal{A}$ of attacks, if $$\lim_{n \to \infty}P[S_{\mathcal{A}}; A_n, M_n] = 0 \space , \space \forall A_n \in \mathcal{A}$$
that is all the sequences of attacks in $\mathcal{A}$ succeed against the corresponding $M_n$ with probability that vanishes as $n$ grows.

|            | unconditional                                                                            | computational                                                                                                                                                        |
| ---------- | ---------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| finite     | $$\forall A \in \mathcal{A} $$ $$\newline \space P[S_{\mathcal{A}}; A,M] \leq \epsilon$$ | $$\forall A \in \mathcal{A}$$ $$P[S_{\mathcal{A}}\cap\{T_A\leq T_0\};A,M]\leq \epsilon$$                                                                             |
| asymptotic | $$\forall \{A_n \in \mathcal{A}\}$$ $$\lim_{n \to \infty} P[S_\mathcal{A};A_n,M_n] = 0$$ | $$\forall\{A_n \in \mathcal{A}\}, \space \forall q(\cdot),s(\cdot), \space \forall n>n_0$$ $$P[S_{\mathcal{A}}\cap\{T_{A_{n}}\leq q(n)\};A_n,M_n] < \frac{1}{s(n)}$$ |
# Source distinguishers
![[Pasted image 20260317114708.png]]
A **distinguisher** between two random variables $x_0$ and $x_1$ is a system $D$ that is allowed to observe a realization of $y$ without knowing in advance if $b=0$ or $b=1$ and should then guess which one holds. #distinguisher
- $x_0$ and $x_1$ are characterized by their [^1]PMDs $p_{x_{0}}, p_{x_{1}}$
- a *deterministic* $D$ is composed of a decision function $g \space : \space \mathcal{Y} \rightarrow \{ 0,1\},$ i.e. $b=g(y)$ 
- more generally, a *probabilistic* $D$ is characterized by a conditional PMDs $P_{b'|y}(\cdot|\cdot)$ 
[^1]: Probability Mass distribution

The performance of a *distinguisher* $D$ is given by the pair of *correct decision probabilities* $$(P_{b'|b}(0|0), P_{b'|b}(1|1))$$
or a complementary by the pair of *error probabilities* $$(P_{b'|b}(1|0),P_{b'|b}(0|1))$$
The *distingushing advantage* of $D$ between $x_0$ and $x_1$ is 
$$adv_D(x_0,x_1)= |P_{b'|b}(0|0)-P_{b'|b}(0|1) = |P_{b'|b}(1|1)-P_{b'|b}(1|0)|$$
$$= |P_{b'|b}(0|0)+P_{b'|b}(1|1)-1|=|1-P_{b'|b}(1|0)-P_{b'|b}(0|1)|$$
		                 ![[Pasted image 20260317115936.png]]
> [!Warning] NB
> - for a **perfect** distinguisher $adv_D(x_0,x_1) = 1$
> - for a **dumb** distinguisher $adv_D(x_0,x_1) = 0$
#### Properties
For any [^1]rvs $x,y,z$ and any distinguisher $D$ between them, the following hold:
- **symmetry**: $adv_D(x,y) = adv_D(y,x)$
- **bounds**: $0 \leq adv_D(x,y) \leq 1$ 
- **Triangular inequality**: $|adv_D(x,z)-adv_D(y,z)| \leq adv_D(x,y) \leq adv_D(x,z)+adv_D(y,z)$
[^1]: Random variables
## Unconditional indistinguishability
It is not always possible to find a perfect distinguisher:
> [!Abstract] (unconditional, finite)
> 2 variables $x_0$ and $x_1$ are said to be $\epsilon$-*unconditionally indistinguishable* if for any distinguisher $D$ , it is $adv_D(x_0,x_1) \leq \epsilon$

![[Pasted image 20260317121345.png]]
### Performance of unconditional distinguishers
For a deterministic distinguisher $$D \space : \space b'=g(y), p(0|1) = \sum_{a \in g^{-1}(0)} p_{x_{1}}(a) \space , \space \space p(1|0) = \sum_{a \in g^{-1}(1)}p_{x_0}(a)$$
![[Pasted image 20260317121844.png]]

> [!Abstract] Optimal unconditional distinguisher
> The distinguisher that maximizes $adv_D(x_0,x_1)$ is the **maximum likelihood (ML)** estimator of $b$ from the observation of $y$
> $$g_{ML}(a) = \begin{equation}
\left\{\begin{split}
 0 \space , \space if p_{x_0}(a) \geq p_{x_1}(a)\\
1 \space , \space if p_{x_0}(a) < p_{x_1}(a) \\ 
\end{split}\right.
\end{equation}$$
it is often *computationally infeasible*
## Total variation distance

> [!Abstract] Total variation distance
> The **Total variation distance** between 2 rvs $x_0,x_1$ with alphabet $\mathcal{A}$ is defined as: $$d_V(x_0,x_1) = sup_{S\subset A}(P[x_0 \in S]-P[x_1\in S]) = sup_{S \subset A} \sum_{a \in S} [p_{x_0}(a)-p_{x_1}(a)]$$
> the supremum is achieved with $$S^*=\{a \in \mathcal{A} \space : \space p_{x_0}(a) > p_{x_1}(a)\}$$ and can be computed as  $$d_V(x_0,x_1) = \frac{1}{2}\sum_{a\in S^*}[p_{x_0}(a)-p_{x_1}(a)] + \frac{1}{2}\sum_{a\notin S^*}[p_{x_1}(a)-p_{x_0}(a)] = \frac{1}{2}\sum_{a \in \mathcal{A}} | p_{x_0}(a)-p_{x_1}(a)| $$

> [!info] Relationship with distinguishability
>$$sup_D adv_D(x_0,x_1) = sup_D |P_{b'|b}(0|0)-P_{b'|b}(0|1)| = sup_{g:\mathcal{A} \rightarrow \{ 0,1 \}}|P[x_0 \in g^{-1}(0)]-P[x_1 \in g^{-1}(0)]| $$
> $$= \sum_{a \in g^{-1}_{ML}(0)} p_{x_0}(a)-p_{x_1}(a) = d_V(x_0,x_1)$$
> Thus $d_V(x_0,x_1) < \epsilon  \iff x_0,x_1$ are $\epsilon$-*unconditionally indistinguishable* 
#### Properties
- **Symmetry** $d_V(x,y) = d_V(y,x)$
- **Positivity** $d_V(x,y)\geq 0$ and $d_V(x,y)=0 \iff p_x =p_y$
- **Triangular inequality** $|d_V(x,y) - d_V(y,z)| \leq d_V(x,y) \leq d_V(x,z) + d_V(y,z)$
- **rv difference probability** if $d_V(x,y) = \epsilon$ there exists a joint distribution $p_{xy}$ having $p_x$ and $p_y$ as its marginals, such that $P[x \neq y] \leq \epsilon$ 
- **monotonicity** $d_v(x,y) \leq d_v((x,z),(y,v))$ for any rvs $x,y,z,v$
- **subadditivity with independent rvs**: if $x_1,x_2$ are independent and so are $y_1,y_2$ then $d_V((x_1,x_2),(y_1,y_2)) \leq d_v(x_1,y_1)+d_V(x_2,y_2)$
## Asymptotic indistinguishability
> [!abstract] (unconditional, asymptotic)
> Two sequences of random variables (or vectors) $x_{0,n}$ and $x_{1,n}$ are said to be *unconditionally indistinguishable in the asymptotic formulation* for any of distinguishers $D_n$ 
> $$lim_{n \rightarrow \infty} adv_{D_n}(x_{0,n},x_{1,n}) = 0$$

It is easy to relate this notion to the total variation distance as

> [!Tip] Proposition
> Two sequences of random variables (or vectors) $x_{0,n}$ and $x_{1,n}$ are *unconditionally indistinguishable in the asymptotic* if and only if $$lim_{n \rightarrow \infty} d_V(x_{0,n},x_{1.n}) = 0$$

## Computational indistinguishability
Sometimes even if a good distinguisher exist in principe, it may not be possible to find a computational efficient one
>[!Abstract] Definition (computational, finite)
>$x_0$ and $x_1$ are said to be $(\epsilon , T_0)$-*computationally indistinguishable* if for any distinguisher $D$ with complexity $T_D \leq T_0$ it $adv_D(x_0,x_1) \leq \epsilon$

We can also introduce the asymptotic version
>[!Abstract] (computational, asymptotic)
>Two sequences of random variables (or vectors) $x_{0.n}$ and $x_{1.n}$ are said to be *computationally indistinguishable in the asymptotic formulation* if for any polynomials $p(\cdot), q(\cdot)$ and any sequence of distinguishers $D_n$ with complexity $T_{D_n} \leq p(n)$, there exist a $n_{p,q}$ such that $adv_{D_n}(x_{0.n},x_{1.n}) \leq 1/q(n), \space \forall n > n_{p,q}$

![[Pasted image 20260317192006.png]]
![[Pasted image 20260317192035.png]]
## System distinguishers
Consider 2 *probabilistic systems* $S_0$ and $S_1$ where:
- $S_0$ is characterized by the conditional PMD $p_{y_0|x}$
- $S_1$ is characterized by $p_{y_1}|x$
A *Distinguisher* between $S_0$ and $S_1$ is third system $D$ that is allowed to interact with $S_b$ without knowing in advance if $b=0$ of $b=1$ and
- can feed any input $x$ to $S_b$
- can observe the corresponding output $y$
- should then guess whether $b=0$ or $b=1$
$D$ is composed of:
- an *input selection* strategy $p_x$ (possibily adaptive, $p_{x|y}$)
- a decision function $g : \mathcal{X} \times \mathcal{Y} \rightarrow \{ 0,1\}$ i.e. $b' = g(x,y)$
- or a probabilistic decision rule with conditional probabilities $P_{b'}
![[Pasted image 20260317193839.png]]
### Distinguisher performance
The performance of a distinguisher $D$ is given by the pair of correct decision (*or complementary error*) probabilities $$(p_{b'|b}(0|0),p_{b'|b}(1|1)) \space \space, \space \space (p_{b'|b}(1|0),p_{b'|b}(0|1))$$
The *distinguish advantage* of $D$ between $S_0$ and $S_1$ is $$adv_D(S_0,S_1) = |p_{b'|b}(0|0)-p_{b'|b}(0|1)| = |p_{b'|b}(1|1)-p_{b'|b}(1|0)|$$
$$= |p_{b'|b}(0|0)+p_{b'|b}(1|1)-1| = |1-p_{b'|b}(1|0),p_{b'|b}(0|1)|$$
- for a **perfect** distinguisher $adv_D(S_0,S_1)=1$
- for a **dumb** distinguisher $adv_D(S_0.S_1)=0$
## Unconditional indistinguishability
>[!Abstract] (unconditional, finite)
>Two systems $S_0$ and $S_2$ are said to be $\epsilon$-*unconditionally indistinguishable* if for any distinguisher $D$ it is $adv_D(S_0,S_1) \leq \epsilon$

>[!Abstract] (computational, asymptotic)
>Two sequences of systems $S_{0,n}$ and $S_{1,n}$ are said to be *unconditionally indistinguishable in the asymptotic formulation* if for any sequence  of distinguishers $D_n$  $$lim_{n \rightarrow \infty} adv_{D_n}(S_{0,n},S_{1_n}) = 0$$

## Relationship with total variation distance
For two systems $S_0,S_1$ with input $x$ and outputs $y_0,y_1$ respectively we can relate their unconditional distinguish-ability to the total variation distance of the joint input-output pairs $$sup_D adv_D(S_0,S_1) = sup_{p_x}sup_g|P[(x,y_0) \in g^{-1}(0)]-P[(x,y_1)\in g^{-1}(0)]|$$
$$=sup_{p_x}|P[(x,y_0)\in g^{-1}_{ML}(0)]-P[(x,y_1)\in g^{-1}_{ML}(0)]|=sup_{p_x}d_V((x,y_0),(x,y_1))$$
Alternatively we can relate it to the total variation distance between conditional distribution of the outputs given the same input value
$$sup_D adv_D (S_0,S_1) = sup_{p_x} \frac{1}{2} \sum_{a,b} |p_{y_0|x}(b|a)p_x(a)-p_{y_1|x}(b|a)p_x(a)|$$
$$=sup_{p_x} \frac{1}{2} \sum_{a} p_x(a) \sum_b |p_{y_0|x}(b|a)-p_{y_1|x}(b|a)|$$
$$=sup_{a} \frac{1}{2} \sum_b |p_{y_0|x}(b|a)-p_{y_1|x}(b|a)|=sup_a d_V(p_{y_0|x=a},p_{y_1|x=a})$$
## Computational indistinguishability
>[!Abstract] (computational, finite)
>$S_0$ and $S_1$ are said to be $(\epsilon , T_0)$-*computationally indistinguishable* if for any distinguisher $D$ with complexity $T_D \leq T_0$ it is $adv_D(S_0,S_1) \leq \epsilon$
>

>[!Abstract] (computational, finite)
> Two sequence of systems $S_{0,n}$ and $S_{1,n}$ are said to be *computationally indistinguishable in the asymptotic formulation* if, for any polynomials $p(\cdot),q(\cdot)$ and any sequence of distinguishers $D_n$ with complexity $T_{D_n} \leq p(n)$,  $$\exists n_0 \space \space : \space \space adv_{D_n}(S_{0,n},S_{1,n} \leq \frac{1}{q(n)} \space \space \space, \space \forall n>n_0$$
>the value of $n_0$ depends on $q(\cdot), s(\cdot)$ and $\{D_n\}$
> $$lim_{n \rightarrow \infty}q(n)adv_{D_n}(S_{0,n},S_{1,n}) = 0$$
### Ideal counterparts of mechanisms
We define the *security measure* for a mechanism $M$ in terms of indistinguishability from its *ideal counterpart* $M^*$
The ideal counterpart of a mechanism is another (possibly unrealistic) mechanism $M^*$ that:
- provides the same security service as $M$ in an ideal manner
- has the same interfaces as $M$
>[!Example]
>| **Real-world mechanism** $M$ | **ideal counterpart** $M^*$                                                   |
| ---------------------------- | ----------------------------------------------------------------------------- |
| secure random generator      | source of uniform independent symbols                                         |
| cryptographic key agreement  | 2 identical copies of random uniform string, garbage to the eavesdropper      |
| encryption/decryption        | identical copy of message to legitimate receiver, garbage to the eavesdropper |
## Unconditional security definitions
>[!Abstract] (unconditional, finite)
>A mechanism $M$ is said to be $\epsilon$-*unconditionally secure* if it is $\epsilon$-*unconditionally indistinguishable* from its ideal counterpart $M^*$ 

>[!Abstract] (unconditional, asymptotic)
>A sequence of mechanisms $\{M_n\}, n \in \mathbb{N}$ is said to be *unconditional secure* in the asymptotic formulation if it is *unconditionally indistiguishable* from its ideal counterpart $\{M^*_n\}$ is the asymptotic formulation
## Computational security definitions
>[!Abstract] (conditional, finite)
>A mechanism $M$ is said to be $\epsilon$-*conditionally secure* if it is $\epsilon$-*conditionally indistinguishable* from its ideal counterpart $M^*$ 

>[!Abstract] (conditional, asymptotic)
>A sequence of mechanisms $\{M_n\}, n \in \mathbb{N}$ is said to be *asymptotically computationally secure* if:
>- $\exists$ polynomial $p(\cdot)$ such that $\forall n,T_{M_n} \leq p(n)$
>- the sequence $\{M_n\}$ is *computationally indistiguishable* from its ideal counterpart $\{M^*_n\}$ in the asymptotic formulation
## Multiple ideal counterparts
For some mechanisms $M$ there may be multiple $M^*$ (each characterized by a different $p_{y^*|x}$) all equally valid, forming a set $\mathcal{M}^*$ of ideal counterparts.
Then the security definitions should be revised as
>[!Abstract] (unconditional, finite)
>A mechanism $M$ is said to be $\epsilon$-*unconditionally secure* if it is $\epsilon$-*unconditionally indistiguishable* from at least one ideal counterpart $M^* \in \mathcal{M}^*$
>

and *similarly for the asymptotic* and the *computational* security definitions. Observe that $\epsilon$-unconditional security can then be written via the relationship with total variation distance as:
$$ min_{M^*\in\mathcal{M}^*} sup_D adv_D(M,M^*) \leq \epsilon \iff min_{P_{y^*|x}\in \mathcal{M}^*} max_{p_x} d_V ((x,y), (x,y^*)) \leq \epsilon$$
$$ \iff min_{P_{y^*|x}\in \mathcal{M}^*} max_a d_V (p_{y|x=a}, p_{y^*|x=a}) \leq \epsilon$$

|            | unconditional                                                                     | computational                                                                                                                             |
| ---------- | --------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| finite     | $\forall D$ distinguisher $$adv_D(M,M^*)\leq \epsilon$$                           | $\forall D$ with $T_D \leq T_0$ $$adv_D(M,M^*) \leq \epsilon$$                                                                            |
| asymptotic | $\forall \{D_n\}$ sequence $$lim_{n \rightarrow \infty}adv_{Ð_n}(M_n,M^*_n) = 0$$ | $\forall q(\cdot), s(\cdot)$ polynomials, $\forall \{D_n\} : T_{D_n} \leq q(n)$ $$lim_{n \rightarrow \infty}s(n) adv_{D_n}(M_n,M^*_n)=0$$ |
# Composable Security Definitions
## Security analysis
A **security analysis** is a method to establish the security of a mechanism $M$ by:
- considering several attacks on $M$ 
- model the mechanism $M$ and each attack $A$
- estimate the success probability $P[S_A;A,M]$
It provides no absolute guarantees that all possible attacks have been considered
## Security proofs
A **security proof** for a mechanism $M$ is a mathematical derivation that allows to establish bounds on distinguishability or attack success probability. It requires:
- a precise and complete adversarial / #distinguisher /  attacker model
- compute values or derive upper bounds on $adv_D(M,M^*)$ or $P[S_A;A,M]$
## Security reduction
A **security reduction** is a particular technique for **security proofs** where the security (*to be proved*) of $M_1$ is derived from another (*assumed or established*) mechanism $M_2$

> [!info] Proposition (general statement)
> If $M_2$ is secure (*according to some definition*), then $M_1$ is secure (*according to some possibly different definition*)

>[!Success] Proof by contradiction
>1. Pretend to assume that $M_1$ is **not secure** 
>2. Then there must be a distinguisher $D_1$ yielding (*cedevole*) high $adv_{D_1}(M_1,M^*_1)$, or (an attack $A_1$ yielding high $P[S_{A_1};A_1,M_1]$ )
>3. Show that $D_1$ (or $A_1$) can be used to build a $D_2$ yielding high $adv_{D_2}(M_2,M_2^*)$, or (an attack $A_2$ yielding high $P[S_{A_2};A_2,M_2]$)
>4. Deduce that $M_2$ would not be secure, which contradicts the direct assumption
>5. Therefore the contradiction assumption must be false, and $M_1$ must be secure
# Composition of security mechanisms
More complex security mechanisms or protocols are typically implemented by combining simpler elementary mechanisms 
![[Screenshot 2026-03-20 alle 16.08.10.png]]
Now let's consider a security mechanism $S$ that use another mechanism $M$ , and denote this by $S[M]$. Let $S[M^*]$ denote the same mechanism $S$ where $M$ is replaced by its ideal counterpart $M^*$ and let $S^*$ denote the ideal counterpart of $S$ (which need not use $M$ or $M^*$ )
![[Screenshot 2026-03-20 alle 16.19.54.png]]
If we know the security level of $M$ and $S[M^*]$, is possible to derive the security of $S[M]$?
## Composition Lemmas
>[!info] Lemma: Distinguishability of inner mechanism
>Consider system $S$ including a subsystem $M$ or $M'$. 
>Then for any distinguisher $D_S$ on $S$ there exist a distinguisher $D_M$ between $M$ and $M'$ such that $$adv_{D_S}(S[M],S[M']) = adv_{D_M}(M,M')$$

![[Screenshot 2026-03-20 alle 16.25.46.png]]
>[!info] Lemma: triangular inequality
>For any systems $S_1,S_2,S_3$ and distinguisher $D$ $$adv_D(S_1,S_2) \leq adv_D(S_1,S_3) + adv_D(S_3,S_2)$$
>and by taking the supremum over $D$ $$sup_D adv_D (S_1,S_2) \leq sup_{D'}(adv_{D'} (S_1,S_3)+ adv_{D'}(S_3,S_2))$$
>$$\leq sup_{D'} adv_{D'} (S1,S3) + sup_{D''} adv_{D''} (S_3,S_2)$$

>[!Abstract] Proof
>Follows from the triangular inequality property of distinguishing advantage

## The composition theorem
>[!info] Theorem: unconditional
>If $M$ is $\epsilon_1$-*unconditionally* secure and $S[M^*]$ is $\epsilon_2$-*unconditionally* secure then $S[M]$ is $(\epsilon_1 + \epsilon_2 )$-*unconditionally* secure

>[!Abstract] Proof
>Follows from the above lemmas: $$sup_{D_S} adv_{D_S} (S[M],S^*) \leq sup_{D_S} adv_{D_S} (S[M],S[M^*]) + sup_{D_S} adv_{D_S} (S[M^*], S^*)$$
>$$\leq sup_{D_M} adv_{D_M}(M,M^*) + sup_{D_S} adv_{D_S}(S[M^*],S^*) \leq \epsilon_1 + \epsilon_2$$
By repeatedly applying the above result we can generalize to $N$-*fold* uses of $M$ in $S$

>[!Example] Corollary
>If $M$ is $\epsilon_1$-*unconditionally secure* and $S[m^*]$ is $\epsilon_2$-*unconditionally secure*, then $S[M^N]$ is $(N \epsilon_1 + \epsilon_2)$-*unconditionally secure*
>

We can state without proof 
>[!info] Theorem: unconditional asymptotic
>In the asymptotic formulation if $\{M_n\}$ is unconditionally secure and $S_n[M^*_n]$ is unconditionally secure then $S_n[M_n]$ is unconditionally secure

>[!info] Theorem: unconditional concrete
>If $M$ is $(\epsilon_1 , T_0)$-*computationally secure* and $S[M^*]$ is $(\epsilon_2 , T_0)$-*computationally secure* then $S[M]$ is $(\epsilon_1 + \epsilon_2 , T_0)$-*computationally secure*

In the computational asymptotic form security is retained even if $M$ is used in $S$ a number of times that is upper bounded by a polynomial in $n$ as follows
>[!info] Theorem: computational asymptotic
>In the asymptotic formulation, if $\{M_n\}$ is computationally secure and $S_n[M^*_n]$ is computationally secure, then for any polynomial $p(\cdot)$, $S_n[M_n^{p(n)}$ is computationally secure

### Relationship between finite unconditional security definitions
>[!Info] Proposition
>If a mechanism $M$ is $\delta$-unconditionally secure and its ideal counterpart $M^*$ offers $\epsilon$-unconditional security against a class $\mathcal{A}$ of attacks, then $M$ offers $(\epsilon + \delta )$ -unconditional security against the same class $\mathcal{A}$

>[!Abstract] Proof
>$$sup_D adv_D (M,M^*) \leq \delta \Rightarrow sup_{p_x}d_V(p_{xy},p_{xy^*}) \leq \delta$$
>$$\Rightarrow d_V(p_{xy},p_{xy^*}) \leq \delta \space \space, \space \space \forall p_x$$
>$$\Rightarrow \forall p_x, \exists p_{xyy^*} : P[(x,y) \neq (x,y^*)] = P[y \neq y^*] \leq \delta$$
>Then for all $A \in \mathcal{A}$ and by the total probability theorem
>$$P[S_{\mathcal{A}}; A,M] = P[S_{\mathcal{A}} |y = y^*; A,M]P[y = y^*;A,M] + P[S_{\mathcal{A}}|y \neq y^*; A,M]P[y \neq y^*; A,M]$$
>$$\leq P[S_{\mathcal{A}};A,M^*] \cdot 1 + 1 \cdot P[y \neq y^*; A,M] \leq \epsilon + \delta$$

### Relationship between asymptotic unconditional security definitions
>[!info] Proposition
>If a sequence of mechanisms $\{M_n\}$ is unconditionally secure in the asymptotic formulation and its ideal counterparts $\{M_n^*\}$ offer asymptotic unconditionally security against a class $\mathcal{A}$ of attacks, then $\{M_n\}$ also offer asymptotic unconditionally security against the same class $\mathcal{A}$

>[!Abstract] Proof
>Apply the same reasoning as before to each $M_n,M_n^*,D_n,A_n$ and derive
>$$P[S_{\mathcal{A}};A_n,M_n] \leq P[S_{\mathcal{A}}; A_n,M^*_n] + sup_{D_n} adv_{D_n} (M_n, M^*_n)$$
>$$lim_{n \rightarrow \infty} P[S_{\mathcal{A}};A_n,M_n] \leq lim_{n \rightarrow \infty} P[S_{\mathcal{A}};A_n,M^*_n] + lim_{n \rightarrow \infty} sup_{D_n}adv_{D_n}(M_n,;M^*_n) = 0$$

### Relationship between finite unconditional security definitions