<div id='section-id-3'/>

# Circles Entropy: An Anonymous Trust and Credit System

David Dashyan, Ajinkya Kulkarni, Julio Linares and Kaustubh Srikanth

*v0.1 (License: [CC-BY-ND](https://creativecommons.org/licenses/by-nd/4.0/))*


<div id='section-id-10'/>

## Abstract

This black paper introduces Circles Entropy, a system that facilitates anonymous trust and credit relations, enabling private  transactions among its users. Trust and credit relations are expressed in the form of graphs, that form the fundamental data structures over which secure computation takes place. Circles Entropy comprises the following key components: a p2p intent gossip network, a graph solver network, a settlement layer and a utility token.

The following sections provide a technical overview of the various components of Circles Entropy. We explore how it can be deployed to anonymize relations within CirclesUBI, an existing mesh credit protocol designed to implement a non-state basic income system. Further, we highlight future research directions and discuss other potential applications.

<div id='section-id-18'/>

#### Table of Contents

- [Introduction: Islands of Low Entropy](#section-id-22)
- [System Architecture](#section-id-42)
  - [Example: Anonymous CirclesUBI](#section-id-64)
    - [Protocol Description](#section-id-78)
  - [Salient Features of the Taiga Execution Model](#section-id-90)
  - [Solving](#section-id-101)
  - [Incentives and the Utility Token (Ensō): Gossip and Solvers](#section-id-108)
- [Privacy Guarantees](#section-id-126)
  - [Network Level Anonymity](#section-id-133)
  - [Account Anonymity](#section-id-144)
  - [Function Privacy](#section-id-148)
- [Research Directions](#section-id-152)
  - [Privacy and Security](#section-id-156)
    - [Collaborative zkSNARKs for Encrypted Solving](#section-id-158)
    - [Fully Homomorphic Encryption](#section-id-166)
    - [Privacy-preserving Graph Analytics](#section-id-172)
  - [Society and Technology](#section-id-176)
    - [Mesh Credit Networks](#section-id-178)
    - [Credit Clearing](#section-id-184)
  - [Potential Applications](#section-id-188)
    - [Social Media Moderation](#section-id-190)
    - [Spam Protection](#section-id-194)
- [Conclusion](#section-id-198)

<div id='section-id-22'/>

## Introduction: Islands of Low Entropy

Entropy is a physical concept that measures the degree of disorder or randomness in a system. Physicist Erwin Schrödinger once argued that living organisms, including human beings, are able to maintain a relatively low level of entropy or disorder within their bodies, despite being subject to the second law of thermodynamics, which states that the entropy of a closed system tends to increase over time. Schrödinger used the term "islands of low entropy" to describe living organisms, which are able to maintain a highly ordered state in the face of the increasing disorder of the surrounding environment. He believed that this ability to maintain low entropy was linked to the ability of living organisms to store and utilize information, and he suggested that the genetic code might be the basis of this information storage and processing.

As individuals become seamlessly entangled with the data they produce and in the process form their own data simulacra, a lack of autonomy over it has led to these simulacra becoming increasingly commodified and entropic with the emergence of an unscrupulous marketplace where data is owned by third parties and sold to the highest bidder. Thus, it becomes important to reduce entropy of this data in order to not completely abstract it from the human being it represents. Transposing the ideas of Schrödinger, "islands of low data entropy" must actively be maintained by providing complete data autonomy and zero-compromise privacy and security guarantees for users.

Attempting to address this problem leads to questions such as: what if it were possible to perform meaningful computations on data where the individual inputs of each person are hidden, and where the outputs enabled people to perform meaningful actions without revealing identities in the network, thus preserving data autonomy and privacy?
<!-- Thus protecting individual + collective relations (from the last point). -->
<!-- Perhaps we can make a stronger point to these questions (and the "why" of Entropy) if we mention the issue with the current credit/CirclesUBI system and we need an AWoT in the first palce -->

This blackpaper outlines the efforts of Circles Entropy to provide answers to these questions. Circles Entropy was initially conceived as a project to implement an anonymous web of trust (AWoT). Ruminating on the concept of an AWoT from a technical viewpoint, Circles Entropy broadened its focus as a project that seeks to anonymize social relationships at the data computation level.


The use case for anonymous credit and payment systems using Circles Entropy is that it can enhance the privacy and security of credit networks by enabling participants to transact without revealing their identity or transaction history. A user can prove that they have the necessary creditworthiness or collateral to participate in the network, without revealing any sensitive information about their identity or financial history. Our aim is to create a framework where individuals can transact with zero compromises on privacy.

<!-- This is a great paragraph (potentially bring this higher up if it helps contextualise earlier) -->
<!-- Can credit/trust transactions be interpreted as more than just financial transactions? The big legal implications surround activity that is just financial, perhaps zoom out of financial and emphasise that credit/trust are beyond that (ofcourse this needs proper legal advice) -->

<div id='section-id-42'/>

## System Architecture

 In practice, Circles Entropy is composed of a suite of cutting-edge cryptographic protocols and privacy-preserving technology. While Circles Entropy can be applied to various computational scenarios requiring robust security and privacy guarantees, our current emphasis lies in graph computations. Graph data structures emerge when modeling socio-economic phenomena, as social relations can be represented as graphs with nodes for entities (individuals, accounts, etc.) and edges representing interactions (trust relations, transactions etc.) between them.
 <!-- Possibly allude to or acknowledge the limitations that come with introducing graph data structures -->

Circles Entropy comprises the following key elements:

1. **P2P Intent Gossip Network:** This component serves as a virtual overlay network, facilitating the transmission of user-defined partial state transitions. It enables users to share and exchange information without revealing their identities.
2. **Graph Solver Network:** The graph solver network plays a crucial role in matching the shared state transitions to form atomic transactions. It ensures secure computation over trust and credit graphs.
3. **Settlement Layer:** The settlement layer serves as a platform where the solver nodes publish transaction proofs. It provides a mechanism for verifying the validity and integrity of transactions, enhancing the overall security of the system.
<!-- there was a comment/discussion from people at the Informal Co-Fi team that we should look into the SUAVE model by Flashbots and apply it somehow with the intent architecture, whereby we have a blockchain that pays out solver fees in ensos. I asked around and I was sent the following two blog posts:
 This is the MEV-Boost builder in SGX https://writings.flashbots.net/block-building-inside-sgx
- This is the MPC-based backrunning  https://writings.flashbots.net/backrunning-private-txs-
 -->
5. **Utility Token:** To support the functionalities of the network, a utility token is utilized as a means of payment for the services provided by the gossip and solver nodes.



This section provides a high-level overview of Circles Entropy's architecture. Circles Entropy builds on the [Anoma](https://anoma.net) protocol stack to provide a solution for anonymizing trust relations encoded as commitments submitted by users. As an illustrative example to describe the Circles Entropy architecture in practice, we consider CirclesUBI, an universal basic income project based on mesh-credit. We then take a brief look at Taiga, a framework for generalized shielded state transitions that Circles Entropy builds on. Finally, we briefly describe the role of graph solvers, a concept specific and critical to Circles Entropy.



<div id='section-id-64'/>

### Example: Anonymous CirclesUBI


To illustrate the practical utility of Circles Entropy, consider a *web of trust (WoT)*, a network of individuals possessing digital certificates, which serve as endorsements of trust towards others in the network. Suppose two parties within this network wish to exchange money and need to establish the existence of a chain of certificates between them before proceeding. Circles Entropy enables computing the cryptographic proof of such a certificate chain without divulging any information about the specific trust connections to the computing parties. By validating the presence of a trust path, the transacting parties can proceed with their exchange. In this setting, individuals only possess knowledge of their own localized trust network[^ltn], and no additional information about the network is available except for the existence of valid trust connections, facilitating monetary transactions.

[^ltn]: In Circles Entropy people can only see their own personal trust network and not the complete WoT.

[CirclesUBI](https://handbook.joincircles.net/docs/developers/whitepaper) is a mesh-credit system that is based on individualized cryptocurrencies and a WoT between these currencies. When new users join CirclesUBI, a new personal Circles (CRC) cryptocurrency is created for them on a smart contract-enabled blockchain. This currency is then regularly minted and added to their account, forming the basis of CirclesUBI's properties. Users have the ability to trust the personal currencies of other users, which requires them to treat this personal currency as identical to any other Circles (CRC) currencies that they hold. <!-- Reminds me of our current digital banking system, issuing fiat 1:1 to other banks (it's not all one monolith) --> As the WoT becomes more interconnected, this network of personal currencies converges to a single planetary monetary system.


![](https://hackmd.io/_uploads/HkpRwUXS2.jpg =350x350) ![](https://hackmd.io/_uploads/Hy2CDLmr2.jpg =350x350)

*The trust graph (left) and the payment graph (right) for CirclesUBI. (Credit: Teodoro Criscione)*

<div id='section-id-78'/>

#### Protocol Description

With CirclesUBI deployed as an application using the Anoma protocol stack, user interaction involves submitting intents to the gossip network. Within the Taiga framework, users of CirclesUBI express two kinds of intents:
- *Trust commitments*: Trust commitments allow a user to commit to exchanging their personal token with certain other users (which collectively constitute the user's trust circle or trust network). Trust commitments are persistent but can be revoked by the user at any time.
- *Exchange requests*: Exchange requests initiate an exchange along the transitive trust path, the existence of which must be proved before the exchange happens. Exchange requests expire once they have been fulfilled (or if they cannot be fulfilled).

Together, these two kinds of intents constitute enough information for solvers to find a transitive <!-- Is this a known term? Don't know what it means --> trust path and compose a valid state change transaction involving several parties.

Intents are routed through a [mixnet](https://constructiveproof.com/posts/2020-02-17-a-simple-introduction-to-mixnets/) to provide network-level anonymity.  The gossip network disseminates intents among peers, which are monitored by solvers looking to match these intents. Once a complete transaction involving exchange requests and transitive trust commitments has been crafted by the solver, it may submit the transaction proofs along with transaction metadata <!-- What might this be?  -->  to a settlement layer, effectively changing the application state.  Both solvers and users check the state changes against the settlement layer.

To provide account privacy and hide transaction amounts, application state is stored in a [shielded pool](https://specs.namada.net/masp.html) of notes, while a public [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree) contains note commitments. Partial function privacy is provided by the fact that the CirclesUBI user's intents are encoded by [validity predicates](https://github.com/anoma/taiga/blob/main/book/src/validity-predicates.md), which are used to authorize application state changes.

<div id='section-id-90'/>

### Salient Features of the Taiga Execution Model

The project adopts the intent-centric architectural model of [Taiga](https://github.com/anoma/taiga/tree/main), developed by Anoma. One of the key aspects of this model is the declarative nature of execution, which allows one the expression of the logic of a computation without stating its control flow. This provides room for context-independent expression and tailor-made optimizations.  Taiga also provides modularity and a clean separation between computation and verification. Clients submit state transition requirements and delegate the task of composing possible state transitions to solvers. <!-- Do solvers all have the same approach to solving state transitions? --> Intents are binding but not guaranteed to be fulfilled.

In the context of a credit system, intents can be either simple or composed. In a simple intent, credit relations between two people can be computed directly to express changes of obligations. Intents can be composed to form new intents. In a composed intent, credits can ripple based on the trust network in order to settle payments along a trust path in order to settle obligations, even if the parties involved don't trust each other directly.

Solvers do not know if the intent they are solving is simple or composed. For example, when a trust path between two users is computed, a particular solver does not know if it is completely or partially solving for the trust path. <!-- Perhaps mention why this is advantageous -->


Execution semantics of solvers and the structure of intents are not restricted, enabling development of applications with a desired security model. Taiga itself provides account unlinkability for transactions as well as partial function privacy.

<div id='section-id-101'/>

### Solving

All trust commitments submitted by participants in a network constitute a graph that we call the web of trust. For Circles Entropy, the solvers are tasked with computing on this graph data; most importantly, finding paths between two given nodes. More graph-related operations such as cycle detection, network flow algorithms and Sybil defence will be supported in the future.

Given that computations involving the complete trust graph are impractical in a model where each solver computes incrementally by composing state transitions, algorithms that use heuristics and do not traverse large sections of the graph for calculating paths are more effective. <!-- Perhaps restructure this sentence (break down into two shorter ones possibly). One on complete trust graph then the other on heuristic algorithms --> As a consequence, along with ability to observe the next hops from each node, solvers need to maintain a cache to provide heuristics data for tagged node clusters. For example, the [A* algorithm](https://en.wikipedia.org/wiki/A*_search_algorithm) uses heuristics data for computing the shortest path while traversing only a small neighbourhood of the graph along the way. This data can be cached by solvers for repeated use.


<div id='section-id-108'/>

### Incentives and the Utility Token (Ensō): Gossip and Solvers

In the Circles Entropy network, both gossip[^gossip] nodes and solver nodes have distinct incentives to participate.

Every completed state transition will include a way to unlock funds to pay every participant who made this transaction possible. Solvers are incentivised to search for full state transitions. The incentive model for gossip nodes is based on the assumption that it will be statistically more advantageous to cooperate.  More specifically, relaying messages further and taking part of the fee is going to be statistically more profitable rather than claiming the full fee and not relaying messages further.

<!-- TODO: add details about incentives -->

[^gossip]: The word "gossip" has a complex history, rooted in the persecution of women during the witch hunts of the 16th and 17th centuries. It has its roots in Old English, where it referred to a godparent or sponsor at a baptism. In her book "Caliban and the Witch," Silvia Federici argues that gossip was a powerful tool of resistance for women during this time period. During the witch hunts, women were often accused of engaging in "gossip" or spreading malicious rumors. This was used as evidence of their supposed witchcraft and was often enough to condemn them to death. However, gossip was actually a form of collective resistance for women. Women would gather together and share information about their lives, their struggles, and their oppressors. This allowed them to build solidarity and support each other.  Over time, it came to be associated with the sharing of personal information and rumors. Today, the word "gossip" still carries negative connotations, but we have to remember it can also be a powerful tool of resistance and solidarity for marginalized communities.

To incentivise participants and encourage them to contribute their computing power to the network, we propose the use of a utility token called Ensō, after Japanese word that refers to a circular symbol used in Zen Buddhism[^enso]. Participants can earn Ensō tokens by providing their computing power to assist other participants within the network. Participants can also utilize their tokens to request the services of solvers and gossip nodes from other participants.

[^enso]: The symbol is created with a single brushstroke and represents enlightenment, strength, elegance, and the universe. The simplicity and fluidity of the Ensō symbol are said to represent the essence of Zen philosophy, which emphasizes the importance of living in the present moment and letting go of attachment to material possessions and ego.

The cost per solver operation and the maintenance of gossip nodes within Circles Entropy will be determined by several factors, including the complexity of the computation, the computing power required, the supply and demand dynamics of computing resources within the network, as well as the labor costs associated with maintaining and developing the Circles Entropy protocol.

To facilitate the implementation of this incentive system, we plan to issue an initial supply of 1.2 billion Ensōs on the [Evmos](https://evmos.org/) blockchain.

<div id='section-id-126'/>

## Privacy Guarantees

In any system, the overall security and privacy are determined by its most vulnerable component. Therefore, our focus is on developing a system that provides security and anonymity at all levels.

It's important to note that the Circles Entropy architecture, as described in the previous section, does not guarantee network-level anonymity by itself. Instead, it relies on the use of mixnets to achieve this goal.


<div id='section-id-133'/>

### Network Level Anonymity

While the Anoma protocols offer privacy guarantees, user activity on the network can still be linked to their transactions. Relay networks like Tor are not sufficient to protect users from a global passive observer and long-term statistical disclosure attacks. To ensure unlinkability and unobservability, all user interactions will be routed through a mixnet.

Two currently operating mixnets, Nym and Katzenpost, both generate too much cover traffic for low bandwidth applications such as ours. <!-- What do you mean by this exactly? What's the issue with the amount of cover traffic? Are you thinking about the cost?  --> The Nym project has plans to support different bandwidth profiles, and we eagerly anticipate their implementation. Alternatively, we are exploring the possibility of parameterizing a Katzenpost mixnet specifically tailored to the average bandwidth profile of our users.

<!-- Wouldn't this make the size of you anonymity set the size of your userbase? -->
<!-- Also would you be running this parameterised mixnet yourselves? Or getting people to run a different instance of Katzenpost? -->



<div id='section-id-144'/>

### Account Anonymity

Intents do not reveal any account data when they are gossiped among peers or solved. Transaction anonymity is ensured by the fact that solvers generate zero-knowledge proofs for solved intents. Parties involved in exchanges and their amounts are hidden from third party observers, as all state transitions reside in a shielded pool.

<div id='section-id-148'/>

### Function Privacy

Application logic is declarative and encoded in form of validity predicates. Although they are hidden from third party observers, user intents need to be revealed to solvers. Despite the fact that accounts are hidden, trust commitments effectively encode a trust graph and it is easy to imagine how such information could potentially be used to deanonymise users. This difficult problem is of central importance for our further research.

<div id='section-id-152'/>

## Research Directions

Circles Entropy is a research and development project that involves two threads of inquiry.  The first thread is concerned with security and privacy, which involves developing better privacy-preserving protocols. The second thread relates to society and technology, which involves researching and implementing better socio-economic systems such as  mutual credit, mesh credit and credit-clearing structures.

<div id='section-id-156'/>

### Privacy and Security

<div id='section-id-158'/>

#### Collaborative zkSNARKs for Encrypted Solving

Collaborative zk-snarks (or "coSNARKs", introduced by [Boneh-Ozdemir](https://eprint.iacr.org/2021/1530.pdf)) propose a method to generate SNARK proofs involving a witness that is distributedly held among several parties. They exploit the linear nature of polynomial commitments and elliptic curve operations (addition and scaling) to implement an effective secret-sharing scheme among mutually distrusting parties.

A system where solvers also play the role of participants in a coSNARK to match intents could prove to be an effective strategy to provide complete intent hiding. The latest research in multiparty computation permits the [detection and penalization of DoS attacks](https://eprint.iacr.org/2016/611) even by multiple participants, which can be linked to economic incentives to disincentivize malicious behavior.

In terms of performance, coSNARKs offer surprisingly little overhead compared to single party proofs, with a slowdown of around ~4x for ~64 participants (using PLONK). An optimized implementation coupled with realistic scaling solutions should be able to bring down this overhead even further.

<div id='section-id-166'/>

#### Fully Homomorphic Encryption

Often described as the "holy grail" of cryptography, fully-homomorphic encryption promises to allow arbitrary computation on encrypted data. Fully-homorphically encrypted intents would provide complete function privacy, making it impossible for solvers to divine any information about intents while still being able to compute over them.

Various FHE implementations are currently available, including [OpenFHE](https://www.openfhe.org/) and Microsoft's [SEAL](https://www.microsoft.com/en-us/research/project/microsoft-seal/) but there seems to be almost universal consensus on their lack of efficiency and usability. Usability of FHE schemes is hampered by a lack of clean abstractions, rendering them inaccessible except to serious cryptographers deeply entrenched in the subject. However, there exist actively-developed general purpose FHE compilers (e.g. [Concrete](https://github.com/zama-ai/concrete) or [Sunscreen](https://sunscreen.tech/)). Studying the suitability and viability of various FHE solutions for Circles Entropy will be a topic of active research as the project progresses.

<div id='section-id-172'/>

#### Privacy-preserving Graph Analytics

Circles Entropy aims to be a system that is smart enough to quantify trust relations between users based on behaviour, and do so without jeopardizing user privacy. Computations will work with limited localized views of the global trust data, necessitating heavy use of local graph algorithms and heuristics. Designing arithmetic circuits for algorithms that work with such restrictions on available data necessitates research into optimizing and/or bettering convential graph algorithms for this setting. A similar need resulted in a barrage of novel hash algorithms optimized for arithmetic circuits developed in recent years (such as [Poseidon](https://www.poseidon-hash.info/), [Concrete](https://www.rc-hash.info/) and [Rescue-Prime](https://www.esat.kuleuven.be/cosic/sites/rescue/)).

<div id='section-id-176'/>

### Society and Technology

<div id='section-id-178'/>

#### Mesh Credit Networks

A mesh credit network is a distributed credit network that enables users to issue, pay, borrow and lend money in the network without the need for a central authority or financial institution. In a mesh credit network, each participant acts as a lender and a borrower, creating a network of interconnected credit relationships. Mesh credit networks provide a flexible monetary fabric that can be strategically deployed to build new economic systems. Examples of contemporary mesh credit networks include protocols like CirclesUBI, and the Trustlines Network, MyCHIPs and legacy ones like original Ripple protocol. CirclesUBI, a system co-built by some of the authors of the present blackpaper, uses a mesh credit architecture in order to deploy an Unconditional Basic Income system. The initial reason for making the Circles Entropy protocol was to anonymize the web of trust that underlies the current infrastructure of CirclesUBI.

[^1]: Thanks to Andrew Miller for this point.

<div id='section-id-184'/>

#### Credit Clearing

[Credit clearing systems](https://www.mdpi.com/1911-8074/13/12/295) designed to facilitate transactions between individuals or small businesses without the need for a centralized intermediary, such as a bank. These systems rely on a network of participants who agree to accept each other's credit, which enables transactions to be processed without the need for cash or traditional banking services. Circles Entropy can be used by credit clearing systems to provide an additional layer of security and privacy to the transactions, particularly in the verification of credit balances and the processing or offsetting of transactions. In a credit clearing system, each participant has a credit balance that represents the amount of credit they have earned by providing goods or services to other participants. Verifying the correctness of these credit balances and offsetting them is critical to ensuring the integrity of the system.  Participants can prove the correctness of their credit balances without revealing the exact value of their balance to other participants in the clearing matrix. This can help to ensure the privacy of the transaction, while still allowing participants to verify that the transaction is valid.

<div id='section-id-188'/>

### Potential Applications

<div id='section-id-190'/>

#### Social Media Moderation

With the decadence of platforms like Facebook, we see that there is a need to have social media platforms that bring back the joy of interacting with one's trusted peers online. Protocols like Scuttlebutt and ActivityPub prove that alternatives are possible that strive for distributed architectures and are people-focused. Leveraging the anonymity of trust graph data  we can enable new modes of decentralized content moderation without compromising on privacy.

<div id='section-id-194'/>

#### Spam Protection

Circles Entropy can be useful in preventing spam or unwanted messages from being sent through a communication network. This is particularly so for email filtering and in preventing unwanted messages on a messaging app. Email filtering is an essential feature for modern email clients and is used to prevent spam and malicious emails from reaching the user's inbox. Traditional email filtering approaches rely on sender reputation or content analysis, which can be ineffective against sophisticated spamming techniques. Circles Entropy  can be used to create a more secure and privacy-preserving email filtering system.

<div id='section-id-198'/>

## Conclusion

Today, the mechanics and incentives of the technological landscape are shaped by a business model based on trusting a third party with potentially-sensitive data. Current technological imaginations posit that data exploitation is a necessary precondition for accessing services online, with the alternative being having no services at all.

Powerful entities continue to leave no stone unturned to indulge in a debauched degeneration of the human identity to "just some data" that may be circulated, manipulated and used for malicious purposes with impunity.  The question appears as one between utility opposed to privacy, one between transparency versus anonymity, between the public and the private spheres of life. This, we think, is a false dichotomy perpetrated by the logic upon which technology is conceived today.

Circles Entropy points towards an autonomous technological imagination where people are able to trust and interact with each other knowing there is no third party watching, where they are not seen as data to be exploited but as a co-creators of their own worlds.

<!-- Minor point but I think there is room in here - perhaps in places I've mentioned that examples could be useful - for more footnotes, instead of just one on its own  -->
