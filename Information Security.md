# Terminology

## Security goals

Desirable features of communications and networks
- **Confidentiality***: Information is available to intended receiver only
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
1. $