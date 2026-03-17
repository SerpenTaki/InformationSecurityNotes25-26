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
|            |                                                                                          |                                                                                                                                                                      |
# Source distinguishers
![[Pasted image 20260317114708.png]]
A **distinguisher** between two random variables $x_0$ and $x_1$ is a system $D$ that is allowed to observe a realization of $y$ without knowing in advance if $b=0$ or $b=1$ and should then guess which one holds
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
> The **Total variation distance** between 2 rvs $x_0,x_1$ with alphabet $\mathcal{A}$ is defined as: $$$$

$$$$