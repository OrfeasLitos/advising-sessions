## 31/8/2020

Today we had an 1-hour meeting with Prof. Kiayias. I presented the overarching
architecture of the recursive channels construction, along with the low-level details and
implementation problems that have been at the centre of my work these weeks. In
particular, we decided that we should avoid the multiple alternative modes for opening
virtual channels for the sake of clarity and brevity, and leave such optimisations as
implementation details. Prof.  Kiayias agreed with the overall architecture, timeline and
structure of the paper. It was mentioned that the recursive nature of the channels should
be emphasized in the final paper.

## 22/5/2020

Today we had a 30' meeting with Prof. Kiayias. I explained that the current communication
between a virtual and its parent channel makes the protocol not subroutine respecting. We
discussed three possible solutions: a tree structure of the protocols, a "local
communication" functionality and [Multi-Protocol
UC](https://eprint.iacr.org/2019/065.pdf). The functionality was rejected as it adds an
assumption and is somewhat non-standard. We decided to try going forward with the tree
structure and, if an unforseen problem or complexity comes up, we will consider the
Multi-Protocol UC approach.

## 15/5/2020

Today we had a 1h10' meeting with Prof. Kiayias. We discussed the progress in the virtual
payment channels project. We delved into the details of which machines are subsidiaries of
which ones and which machines can pass input to which. A flat and a tree-like addressing
scheme were discussed. We agreed to use the flat addressing sheme so as to avoid imposing
any unnecessary structure. We highlighted that a virtual channel party should be considered as
"resident" of the same computer as the channel that funded it and they can therefore pass
freely inputs and outputs to one another. This does not contradict the necessary
"subroutine-respecting" property of UC protocols.

We also came to the conclusion that, in a specific case, independent channel parties
should be considered as "residents" of the same computer, because the balance security
guarantees are given with respect to the sum of their funds, not with respect to the funds
of each channel party individually. Specifically, this happens when an intermediary D
facilitates the opening of a virtual channel on top of its own channel: D may have to pay
one of the two parties of the virtual channel, but these funds would be replenished in
another channel that exists in the same computer as D, and thus belong to the same "user".
In this case balance security should not be violated. Therefore, balance security should
be defined with respect to groups of parties and not with respect to individual parties.

## 2/3/2020

Today we had a 35' meeting with Prof. Kiayias. We discussed future steps in my research.

* LN privacy: We reiterated a research direction here: what is the most private possible
  version of an LN functionality, given an anonymous communication channel as hybrid?
* Virtual payment channels: We reviewed the idea of building channels on top of channels
  and how that can be modelled in UC. If it cannot be modelled in a modular way, there may
  be room for a new generic UC theorem for combining small protocols into big ones more
  freely. This is the primary focus for now.
* Hydra & Ethereum rollup: Prof. Kiayias proposed that I work on allowing various Hydra
  heads to work with each other. Rollup from Ethereum may be of interest here.

Bitcoin snarks, rational miners at the tx-fee regime, a potential talk at the Stirling
University for Andrea Bracciali and the project on selling data over Bitcoin were
mentioned as well.

## 6/2/2020

Today we had a 30' meeting with Prof. Kiayias. We went over the response to the LN paper
reviewers in CSF and agreed on the last edits to this response and to the paper before
submission.

## 24/1/2020

Today we had a 35' meeting with Prof. Kiayias. We discussed the reviewers' comments on the
[Lightning
Network](https://github.com/OrfeasLitos/PaymentChannels/blob/master/paymentChannels.pdf)
paper and decided on an answer to each. We are optimistic.

## 2/12/2019

Today we had an 1-hour meeting with Prof. Kiayias. I described my latest idea, in which we
aim for unlinkability (ideal privacy) of layer-2 payments in Bitcoin, i.e. layer-2 money
transfers should not leak any information on neither the Bitcoin public key from which the
layer-2 coins came, nor the IP address of anyone involved. Furthermore, multi-hop payments
should maintain sender and receiver privacy from intermediate hops.

To achieve this, a party transfers its coins to a special address with a special tx and
obtains a public-secret keypair. He can use this key to open a channel with another LN
public key, by publishing another transaction. Off-chain payments can now happen, and one
more on-chain tx closes the channel. One last on-chain tx moves the money from the
aforementioned special address back to a usual bitcoin address.

All transactions that happen inside the described system (both on- and off-chain) involve
SNARKs, so that only minimal information about the validity of the tx is leaked, much like
Zcash, therefore whatever happens in the system is unlinkable to bitcoin addresses. All
messages sent inside the described system are routed exclusively through a perfect
anonymous network, thus it is impossible to link layer-2 addresses to IP addresses.
Furthermore, surbs (single use response blocks) are used to route a message back to the
sender anonymously, so privacy is maintained within the layer-2 network. Last, to avoid
linkability due to the use of a single hash per payment route, we would have to employ a
scalar/payment point solution instead of the one used currently in LN.

Prof. Kiayias thought this construction is sensible, but there are various parts (e.g. the
CRS update procedure) that may make it impractical. He also expressed concern regarding
whether there is any substantial contribution, or if this constitutes just a
straightforward combination of existing technologies. In order to verify this, he
suggested that I read [Bolt](https://acmccs.github.io/papers/p473-greenA.pdf), [ZK
contingent payments](https://eprint.iacr.org/2019/964) and [Decentralized Anonymous
Micropayments](https://eprint.iacr.org/2019/964).

In the last 5' I mentioned briefly the progress of the atomic asset swap project, on which
we are enabling atomic selling of digital goods for Bitcoin, while optimising the
pessimistic case.

## 17/10/2019

Today we had an 1-hour 30' meeting with Prof. Kiayias. In the first hour we had a video
conference with Claudia and Harry. We there discussed various privacy desiderata and
constraints of the Lightning Network, along with broad ideas on how the Nym mixnet would
mitigate some of the problems. In particular, the mixnet would greatly help in mitigating
attacks by a global passive adversary, but, due to the (mostly) public nature of LN nodes
and the fact that multi-hop payments cannot be avoided, mixnets cannot provide
satisfactory protection against active local adversaries.

The resolution of the last 30' was that as a taster, I should write the protocol that
realises the [leakage-free
F_PayNet](https://github.com/OrfeasLitos/privacy-ln/raw/90c9ef83c75560cf1f4887f384f9a962d1312b87/privacy-ln.pdf)
assuming a pairwise anonymous network channel functionality, just to show that it is
possible.

Prof. Kiayias also mentioned that I should give a brief explanation of simulation-based
security and highlight how much simpler and more abstract F_PayNet is compared to the full
Lightning specification in my talk at the [Lightning
Conference](https://thelightningconference.com).

## 11/10/2019

Toady we had a 50' meeting with Prof. Kiayias. We discussed some new ideas on the modular
ledger design and a new project regarding Lightning and privacy.

### Modular Ledger Design

Our discussion focussed on the idea that a virtual channel protocol that uses two basic
channel functionalities F_bch as hybrids. Prof. Kiayias was skeptical of the decision to
also intercept the communication between F_bch and G_Ledger with the virtual channel
protocol Pi_vch, especially because this would either mean that its messages should
possibly be changed so much to make them suitable for the blockchain, that it would make
more sense to build them from scratch in Pi_vch, or that we would have to somehow allow
F_bch to sign messages for transactions that don't yet exist on-chain, which could
possibly have a negative effect on the security of F_bch. Nevertheless, we should still
push towards that direction.

### Private LN

We discussed about a new project regarding privacy in Lightning. In this work, as a first
step, we would aim to modify F_PayNet so as to leak as little data as possible to the
simulator for each payment, in an effort to formalise what privacy guarantees are
conceivable and what is the minimum amount of data that can be leaked. F_PayNet should not
use cryptography internally or inject cover traffic -- we should find a more elegant way
to achieve the desired leakage minimisation instead. The tough problem to solve here would
be how to ask from the simulator to build and sign a particular transaction without it
undestanding who is paying whom. To that end, it may be helpful to use a private
underlying ledger. Additionally, Prof. Kiayias recommended that I read [A Universally
Composable Framework for thePrivacy of Email
Ecosystems](https://eprint.iacr.org/2018/848.pdf) in preparation for the meeting with Nym
next week.

## 27/9/2019

Today we had an 1-hour meeting with Prof. Kiayias, which we dedicated to discussing the
attack against the perfect ledger (i.e. the ledger that provides instant finality). This
will become a theorem that will be part of of the Payment Channels paper, supporting our
choice to use the complicated but realistic G_Ledger instead of the perfect ledger. In
particular the theorem will claim that there exists no protocol that realises the perfect
ledger, given a synchronous multicast network.

Our discussion revolved around the attack and the proof, which was not yet complete. In
particular, I had already pinpointed that the attack should exploit the fact that an
honest player that finds a new transaction on the network cannot afford not to add it to
her local ledger (otherwise it would break instantaneousness), but at the same time risks
to add to her ledger a transaction that the Adversary has sent only to her. Prof. Kiayias
pointed out some mistakes in the current formulation and we looked for some ways to
formally tackle the issue, but we did not manage to reach a specific attack. We
nevertheless came to the conclusion that such an attack would be a matter of just a little
thought to come up with.

## 13/9/2019

Today we had a 40' meeting with Prof. Kiayias. I let him know of my
[Cerberus](https://github.com/orfeaslitos/cerberus-script) work with Zeta Avarikioti which
we plan to submit to FC and he told me that he will not go over the paper, but he allows
me to go ahead and submit (without him being a co-author). We then moved on to discussing
the last pending issues of the Payment Channels paper, which we plan to resubmit to CSF. I
showed him the transaction figures, the prose explaining F_PayNet and my attempt to
showing that our approach is general enough. I promised to explain the LN protocol in
prose, to add the cooperative closing process and to attempt to find a simple way in which
the "too ideal" ledger functionality breaks. I will also have to move to a 2-column
format.

## 26/7/2019

Today we had an 1-hour meeting with Prof. Kiayias. Initially, we briefly discussed our
response to an email we received from Tyler Wellener from BlockVenture Coalition, who
expressed interest in meeting us regarding our work on Lightning Payment Channels
formalisation. We decided that I should schedule a meeting with the two of us.

### Virtual Payment Channels

We first discussed why the candidate model where the virtual channel protocol uses one or
more base channel functionalities as hybrids cannot be expressed in
[JUC](https://eprint.iacr.org/2002/047) terms. The reason is because, in order to
manipulate the base channel directly, the Environment needs a direct communication channel
with the corresponding functionality. Unfortunately such a channel is not allowed by JUC.
We also came to the same conclusion for the case where there exists one generic channel
functionality for both the virtual and the base ones. In this case, the needed
dependence/communication between a virtual and a base channel functionality is not allowed
by JUC.

We then tried to draw architectural inspiration from the way such a project would be
modularised in the case of a software implementation, especially given the structure of
the virtual channels construction. After some thought, I proposed the following structure:
a channel module has an interface towards its user and one towards its foundation.
If it is a base channel, the foundation is the Ledger. If it is a virtual channel (or a
channel that previously was a simple base channel but now "supports" a virtual on top of
it), the foundation is a "bridge" module. The foundation of the "bridge" module itself is
the Ledger. This way, the channel module does not have to explicitly know whether it
directly "sits" upon a ledger or if it is proxied by a "bridge" module instead.

It now remains to be seen whether the bridge and the channel modules can be combined in an
intuitive way in order to achieve recursive building of channels and how easily such an
architecture can be made compatible with UC.

### Ouroboros Predict-Time

I brought to Prof. Kiayias' attention a minor issue in the proof of existence of
predict-time_OG. In particular, it claims that one MAINTAIN-LEDGER message is enough to
reach the end of the LedgerMaintenance() procedure, which is incorrect. In order to
rectify the mistake, F_VRF has to send "restricting" requests for signatures to the
Adversary (in order to avoid a situation where the Adversary does not necessarily
immediately sign and thus the number of needed MAINTAIN-LEDGERs is unknowable) and the
correct number of MAINTAIN-LEDGERs needed for LedgerMaintenance() to complete a run has to
be calculated and put in the proof.

## 24/7/2019

### Virtual Payment Channels

Today we had an 1-hour meeting with Prof. Kiayias. I described to him the desired
architecture where each payment channel is a separate functionality. The main problem
that we met was that the traditional encapsulation that UC imposes between functionalities
could not be respected: UC demands that the Environment cannot communicate with a local
hybrid functionality of a protocol, but in our case the Environment should be able to
directly communicate with a base channel functionality (e.g. to do a payment in the base
channel) that is also used as a hybrid by a virtual channel protocol.

Prof. Kiayias recognised the issue and proposed that I read [Universal Composition with
Joint State](https://eprint.iacr.org/2002/047) as a possible solution.

## 3/7/2019

Today we had an 1-hour meeting with Prof. Kiayias. We added the final touches to the
Payment Network paper before submitting to eprint, which consisted of minor rephrasings
and clarifications of the explanation of negligence for multi-hop payments. I also
presented my scheme for virtual payment channels on Bitcoin. Prof. Kiayias understood it
and did not see an issue with it, but suggested that I should start writing up.

## 28/6/2019

Today we had a 50' meeting with Prof. Kiayias. We discussed the latest changes to the
Payment Network paper and my progress in the Virtual Payment Channels for Bitcoin.

### Payment Network

We reviewed the latest changes: related work on combined signatures and UC payment
channels, negligent behaviour definition on F_PayNet. The first two do not need any
changes, but the latter is still too cryptic. Prof. Kiayias suggested expanding it and
explaining each term separately to avoid confusing the reader, without removing any part
of the information given. After this change the paper will be ready for submission to
eprint.

### Virtual LN

I briefly presented the main idea behind virtual payemnt channels and started describing
my candidate solution for the case where one virtual channel is established over two
normal ones. We did not have enough time to go over the whole system. Prof. Kiayias
commented that it looks like these are simple enough ideas and it is rather surprising
that they have not been pursued by the creators of Lightning yet.

## 4/6/2019

Today we had an 1-hour meeting with Prof. Kiayias. We discussed some changes to the
Payment Network paper before uploading it to eprint and the candidate upcoming projects.

### Payment Network

I will do the following edits before submitting to eprint. First, I will give a clearer
and more detailed but still high-level explanation of the security guarantees and the
specific time limits for non-negligence in the overview of F_PayNet. This may subsequently
lead to a simplification of the Introduction. Second, I will do a more thorough literature
review and argue why their version of the world is too simplistic for each paper
separately. Third, I will add a reference (or more) to [12] in the Combined Signatures
overview.

### Future projects

The candidate projects for my thesis are, in rough order of completion probability:
* G_Ledger simplification
* Virtual channels for bitcoin
* Privacy for payment and state channels
* New blockchain with layer 2 in mind
* Steemit on channels
* Content curation classification
* Optimal decentralized content curation with Machine Learning

## 27/5/2019

Today we had an 1-hour meeting with Prof. Kiayias. We discussed all loose ends of the
submission. In particular we rephrased some parts and reviewed the structure and contents
of both the main part and the appendix. Prof. Kiayias focussed then on the exact polling
time bounds on players and in the process uncovered an artefact in the current
functionality and protocol: some delays use the parameter "k" from GKL, even though this
parameter does not appear in G_Ledger. I thus promised to review these details and
rephrase the proof accordingly where needed. We figured out that the only other remaining
parts are the Introduction, the Conclusion, the reduction of our security to the 4
signature games and, if time permits, the proof of security of the IBS construction in LN.

## 24/5/2019

### Payment Network

Today we had a 1:10' meeting with Prof. Kiayias. We decided on the general paper
structure and the contents and ordering of each section. We also discussed the need for an
explicit description of our version of the IBS, the addition of a new algorithm for
generating a public key from a private key for the combined signature scheme and the proof
technique to reduce a rogue transaction during execution to a forgery attack.

## 21/5/2019

### Payment Network

Today we had a 1:45' meeting with Prof. Kiayias. We went over several parts of the paper,
identifying the core parts that still need improvement. In particular, we discussed some
parts of the functionality that could be objectionable. The most interesting part was the
fact that F_PayNet simply trusts the simulator when the latter announces new members to
`closedChannels` and `updatesToReport`. We figured out that the main motivation and the
way it is done is not perfect (still F_PayNet has to keep track of promised updates and
closes and halt if they're not delivered, but it currently doesn't), but is certainly on
the right track. It also gives interesting intuition on the guarantees given, specifically
on the fact that an adversary may trick the functionality into believing anything and the
former can only figure out the issue upon the closing of the channel.

The second part of the discussion focussed on the core parts of the proof of security of
payments. I walked Prof. Kiayias through several interesting parts of the proof. We found
out some minor mistakes, but the most important observation was that the security against
forgeries is currently explained in a very informal fashion. We dedicated a lot of time
understanding how the proof should be properly done. In particular, I should define an
adversary that, given forgery in an LN execution is possible, would win the forgery game
with non-negligible probability. This should be repeated for each of the three forgeries
possible and characterize thus all bad events. Subsequently, whenever we encounter a spot
in the proof where such a forgery would break security, we move to the conditional space
where such a forgery does not happen. In this way it will be possible to prove security
completely formally and give explicit security bounds (instead of merely proving
computational security).

In the process of the previous discussion, various other deficiencies of the proof were
uncovered, such as the fact that a number of possible forgeries are not mentioned. We aim
to submit to TCC.

## 13/5/2019

### Payment Network

Today we had an 1-hour meeting with Prof. Kiayias. We went over a couple of points where I
wasn't sure that my approach was correct. In particular, I mentioned that several checks
that would happen upon receiving a message from the environment have been removed from
F_PayNet and instead the message is always forwarded to the simulator. The actions of the
simulator are checked after the fact and, if any check fails, the F_PayNet halts.
Furthermore, I showed the approach taken in the simulator, according to which it simulates
only some of the lines of the protocol and obtains the missing information from the
activating message of F_PayNet. This happened in an attempt to mimic exactly the (READ)
messages in the ideal world, which turns out to be superfluous but nonetheless would
achieve security with an even weaker G_Ledger. Third, I showed him a sample of the proof
in order to double-check that it takes a right approach that covers all bases.

Subsequently we moved on to discussing the proposition involving the G_Ledger guarantees
concerning when a party has to submit a transaction in order for it to be placed in a
particular block range. Prof. Kiayias has provided this proposition with exact bounds and
a formal proof. Unfortunately the proposition needs the party to act in reaction to it
state, i.e. its k-pruned blockchain. We couldn't make it work with the party reacting to
the latest block it sees.

Lastly we discussed the final structure of the paper and which parts are missing
(introduction, related work, etc.) A list of those is being written and filled in by Prof.
Kiayias.

## 30/4/2019

### Payment Network

Today we had an 1-hour meeting with Prof. Kiayias. We looked into the exact cases when
F_PayNet decides an honest player has been wrongly charged, which led us to me explaining
why the particular minimum block height in which Alice must poll (i.e. OutgoingCltvExpiry
\+ 2k + fu - 1) was chosen.

We assume that a transaction that is broadcast for u rounds when the chain has height h
(as seen by the broadcasting party) is added to the blockchain at most at block h + fu +
k. If an honest party wants to timeout an HTLC and starts broadcasting the relevant tx
when she sees block (OutgoingCltvExpiry - 1), the Adversary can postpone this timeout
until block (OutgoingCltvExpiry - 1 + k + fu). Thus it is possible for him to include his
HTLC-success transaction in this block, preventing the honest party from timing out and
postponing disclosure of the preimage as much as possible, until (OutgoingCltvExpiry - 1 +
2k + fu).

Prof. Kiayias told me that, since we are using G_Ledger, we cannot use low-level
parameters from GKL. Furthermore, the assumption above is not correct. Prof. Kiayias will
find the exact block that Alice has to start broadcasting a transaction in order to have
it enter in at most a particular block in a separate lemma. In the meanwhile I'll continue
with the proof.

Lastly, we'll have to add G_Ledger to our paper. I'll add it once I receive the latex
sources.

## 16/4/2019

### Payment Network

Today we had an 1-hour meeting with Prof. Kiayias. We focussed on the pay-simulator. I
noticed that the second condition in line 3 is superfluous and wrong, it has to be
removed. Prof. Kiayias commented that in line 7 the exact time intervals when it is
dangerous for a player to be (relay-)negligent are not clear, they should be explicitly
specified. A minor typo that we found is that the simulator communicates with the
functionality, not with itself.

Given that this proof step covers the cases in which the Adversary could exploit any abuse
of the primitives we use (hashes, simple signatures, combined signatures), we have to
explicitly consider these events. In particular, the Simulator must keep tables with:

 * All the hashes for which the Adversary doesn't know the preimage. A hash is added when
   a party creates the preimage and is removed the first time the party sends the preimage
   anywhere.
 * All the messages that haven't been signed and the adversary could exploit. This should
   probably contain all txs that spend an unspent output to the Adversary. Three different
   tables should be kept: one for messages that need simple signatures, one for messages
   that need forgery of combined signatures where the Adversary knows the master key and
   one for messages that need forgery of combined signatures where the Adversary knows the
   shared key.

Four different bad events have to be defined and we have to prove that in each of these
bad events, the relevant primitive is broken (thus the bad events have negligible
probability). Under the other event, which happens with overwhelming probability, the real
and the ideal execution must be proven identical.

I also briefly mentioned that I have an idea that could possibly allow us to create
virtual payment channels without a finite horizon on top of lightning.

## 3/4/2019

### A puff of Steem

Today we had a 50' meeting with Prof. Kiayias, Prof. Livshits and Andrés Mosteiro
Monteoliva. We discussed the reviews of the papers and decided which suggestions to act
upon and which not to. In particular we decided to expand our related work with a
comparison with some anti-sybil mechanisms on social networks that were pointed to by one
reviewer, as well as add a brief history of Steem and Steemit. We resolved not to add more
simulations, or propose more attacks and their respective defenses.

## 27/3/2019

### Payment Network

Today we had an 1-hour and 15' meeting with Prof. Kiayias. We started our discussion with
the approach to proving Lemma 4 (pay). Apparently the approach of simulating all honest
parties' communications in the Simulator is right and, since the benefits provided by the
signature primitives are not crucially enforced by the Functionality here (but in "poll"),
the statement of Lemma 4 will hopefully be proven in its current form (i.e. with exact
equality). The only change that has to be made in the Functionality is that it should be
informed whether the payment was charged on Alice or the Adversary (instead of a concrete
player), or if it did not go through at all.

Almost the entire remainder of our meeting was dedicated to the proof strategy of the next
steps. In particular we overviewed several points in the idealization of pay, close and
poll that possibly require special care. As a prime example, we vaguely specified what
kinds of bad events can happen if any of the signature schemes is broken and we figured
out that under these events the ideal world would be distinguishable from the real only
with respect to the "poll" message. Therefore the step of idealizing the "poll" message
will not prove exact equality, but computational indistinguishability (assuming that
signatures break with negligible probability).

Other minor proposed corrections are to use the protocol symbol in the pay Simulator
description and have the Functionality only bookkeep on the balances of honest players.

We finally discussed practical matters of travelling to Paris for Tokenomics 2019,
possible visit to Greece in May and summer vacation.

## 13/3/2019

### Payment Network

This week we had an 1-hour meeting with Prof. Kiayias. I reported on the meeting with and
the work of Mario Larangeira and Maxim Jorenko. We resolved that we should not rush to
collaborate with them, given that there isn't a part of LN with which they can help.

We then moved on to discuss the combined key primitive. In particular I explained how the
"label" is irrelevant for the key combination. I showed the steps I took to remedy that
and Prof. Kiayias approved. He also told me how to fix the random coins of the KeyGen()
procedures (using the randomness from the PRF) in the most straightforward manner. Another
minor point that was mentioned was that the dummy functionality should only forward to the
simulator messages that conform to the syntax expected by the protocol.

The greatest part of our discussion was then focussed on the main proof. I walked Prof.
Kiayias through the idealisation of the "register" phase. He verified and approved of the
proof. The rest of the discussion was dedicated to the way that the "open" part will be
idealised. We walked through each step in the protocol and the functionality in parallel
and we figured out that every step in the protocol can be executed by the Simulator
without issue. Prof. Kiayias pointed out that the Simulator should treat the 4
combinations of the honest/malicious status of the two players separately and send the
"channelAnnounced" message (renamed from "channelOpened") to F_PayNet at a very carefully
selected moment, especially when the initiator of the channel is malicious.

## 8/3/2019

### Payment Network

This week we held a 45' meeting with Prof. Kiayias. We dedicated our discussion on the
combined signature primitive definition. We first focussed on whether we should define the
primitive with games or a functionality. After showing Prof. Kiayias my attempt for a
functionality, we decided that it is much more straightforward to go with game-based
security. What's more, since this primitive will live entirely within the LN protocol
(which in turn will be proven UC-secure), so going the UC way for this primitive is not
needed.

I then described the interface of the 6 algorithms (MasterKeyGen, KeyShareGen,
CombineKeyGen, CombinePubKeyGen, Sign, Verify) and the two correctness requirements
(correct signature and correct public key generation). They did not need any corrections.
The last part of our discussion focussed on the security of the scheme. Two games were
defined, largely in the lines of the ones I had defined during the week, with minor
corrections. Intuitively, each game guarantees the security of the two types of keypairs.

Finally, we resolved that my next step should be to prove the LN combined key construction
secure under these definitions.

## 26/2/2019

### Payment Network

Today we had a meeting with Prof. Kiayias that lasted 1 hour and 45'. We discussed Payment
Network. We first discussed how the proof will proceed to the next step. In particular we
resolved to move the ``register'' steps to the functionality, adapt the simulator
accordingly and prove that the modified setup generates for the Environment the same view
as the one that is generated by F_Dummy and S_protocol.

We then focussed on the way PRFs will be used in the protocol and the functionality side.
In particular, the functionality will use perfect randomness, whereas the protocol will
use a PRF. The security will hinge on the fact that the PRF is good. It will inevitably
introduce a some negligible probability of distinguishing.

Subsequently we dedicated a big chunk of our time discussing the way keys are generated.
Prof. Kiayias figured out that the generation of ``pay'', ``dpay'' and ``htlc'' keys could
be abstracted by an Identity Based Signature scheme, augmented with a function that, given
a master public key and a label, produces the corresponding public key. Furthermore, he
prototyped a new cryptographic primitive for generating keypairs out of distributed
secrets. In particular, this primitive consists of 3 functions:
 - One that generates keypairs of shares.
 - One that combines a master secret key with a label and the secret part of a share to
   produce a signing-verification keypair.
 - One that combines a master public key with a label and the public part of a share to
   produce a signature verification key.
This primitive can be used to model the revocation keys generation. The specific
functional and security properties will be specified in the future, but in principle we
would like that the verification key generated by a master secret and a secret share
coincide with the one generated by the corresponding master public and public share.

After that, we discussed how several details of the current pseudocode could be improved.
Various minor corrections were introduced. Lastly, in preparation for the next proof step,
we started looking at the way a channel is opened. Unfortunately we did not have the time
to come to any concrete conclusions.

## 22/2/2019

Today we had an 1-hour meeting with Prof. Kiayias. I first asked a series of miscalleanous
questions before discussing the Payment Network.

### Various questions

1. Why is A analysed in many sub-A (a vector) in https://eprint.iacr.org/2015/574.pdf, p. 30?
   This question was not resolved.
1. In https://eprint.iacr.org/2003/239.pdf, p. 12 it is said that "Z corrupts no party".
   This makes sense because a particular distinguisher Z is defined.
1. In https://eprint.iacr.org/2000/067.pdf, p. 57 we did not figure out the exact meaning
   of each EXEC subscript. Nevertheless, Prof. Kiayias gave me a thorough high-level
   explanation of the proof technique for the central theorem universal composition.
1. Prof. Kiayias confirmed that proving equivalence of property-based and simulation-based
   security is a valid way to translate a complex functionality to a more understandable
   list of desired properties.
1. There are at least two ways for a functionality to clearly signify that something "bad"
   (that doesn't happen in the protocol) has taken place: either to halt or to return a
   special error message.
1. Finally, regarding the fact that in https://eprint.iacr.org/2017/149.pdf, p. 38 many
   messages are sent as a result of (ostensibly) receiving a single "register" message can
   be explained by the fact that the actions described actually correspond to 3 received
   identical messages. After receiving one input, only one output is sent and state is
   kept to then send the next corresponding output after receiving the next identical
   input.

### Payment Network

As most of the time was used for the questions, we did not have a lot of progress here. In
particular, we did not have the time to check the "proof" that the dummy functionality
together with the simulator that runs all protocols is indistinguishable from the actual
protocol.

What we did discuss about was the following: First, I showed how Lightning generates the
per-commitment points. We figured out that this could be substituted with a simple PRF. In
our analysis, we will assume that per-commitment points are indeed generated by a PRF.

Second, I explained that the Lightning specification does not protect an intermediate hop
that does not constantly check the ledger from losing money on timed-out HTLCs. We decided
that the most robust way for us to go would be to have two types of negligence: plain and
relay-negligence, with different delays each. We should then prove that a player that
polls as often as the relay delay will not lose any money, but a player that polls only
between the two delays may lose on some relaying HTLCs.

Last, we figured out that a simple way to modularize the protocol and F_PayNet is to split
them in three parts: one for opening, one for closing and one for paying. Furthermore, the
protocol could be split in even smaller chunks.

An important detail: The ledger functionality leaks all reads to the Adversary. This means
that any reads done by the Lightning protocol must be replicated by F_PayNet or the
Simulator.

## 8/2/2019

### Payment Network

Today we had an 1-hour meeting with Prof. Kiayias. We went to the fine details of what
pieces of data are held by the two parties of a channel (in-depth discussion of the
contents of local and remote commitment and HTLC transactions, along with their
signatures) and the sequence of things that have to happen before an HTLC payment goes
through (irrevocably revoke all states that do not contain the HTLC output on both sides).
We also discussed the bad paths that can be taken in every step.

For the first time, we discussed without going into much detail the difference between the
absolute delays of the HTLCs and the relative delays of the delayed payment outputs. We
understood that we have to look into it in more detail.

One target we set for the next meeting is to have a clear representation of the state,
defined separately from the protocol. This has the potential to simplify the protocol.

## 4/2/2019

### Payment Network

Today we had a 50' meeting with Prof. Kiayias. We discussed message flow in the protocol
(with complications that also affect F_PayNet) and the proof methodology. We also
discussed how to tackle the issue of time in the protocol.

First, in order to solve the issue of needing to send arbitrarily many messages after
receiving a single message (checkNew), we decided that the Environment should ask the
_broadcasting_ player (the one who first received openChannel by the Environment) to check
if _one specific_ funding transaction has been added to the ledger and, if so, to inform
the other party. This ensures that one received message corresponds to one sent message.

Second, we decided to limit the number of ITMs that are registered to the clock by
demanding that protocol instances only take the time from the information obtained by the
ledger. This in turn means that the new, simplified version of the F_Ledger cannot be used
(since does not return any information connected to time) and we will fall back to the old
and established F_Ledger to use the block number as time indication. This also hinted that
we should add a way for the new F_Ledger to signal time-related information along with
transactions, possibly through a parametrizing procedure.

Lastly, we discussed what steps should be taken to prove the desired indistinguishability.
There are two options: either directly writing the Simulator, or to first write a dummy,
pass through functionality that just pushes all messages to and from the Simulator, which
in turn simply emulates all the protocol instances. We will prove indistinguishability
there and gradually pull out to the functionality the various responsibilities, proving
indistinguishability between every pair of subsequent versions as we progress. A new
meeting should be scheduled once I have prepared the first, dummy version of the ideal
world to discuss the technicalities of how to prove the desired indistinguishability.

## 22/1/2019

### Payment Network

Today we had a 3-hour meeting with Prof. Kiayias. We dedicated the entire time to
F_PayNet. We parsed the entire Functionality and made several corrections. We transferred
several responsibilities to the Simulator and reduced the amount of checks done by
F_PayNet to the essential minimum. In order to enforce compliance from the Simulator, the
Functionality checks the Ledger for the necessary (or equivalent) transactions every time
they are supposed to be found there.

## 18/1/2019

### Payment Network

Today during the weekly meeting I made a presentation of the off-chain transactions Alice
keeps stored while an HTLC payment is in flight. I gave a detailed description of this
particular snapshot of the state and answered several explanatory questions. As a
follow-up to the presentation, we had a brief discussion with Prof. Kiayias. We decided
to greatly simplify the Functionality, because having the Functionality to store all the
transactions and communicate with G_Ledger defeats the purpose by bringing it too close to
the protocol. The Functionality should only ensure that the security and functional
properties of payment networks are enforced, not actually "do the dirty work" that the
Simulator is supposed to do.

## 10/1/2019

### Payment Network

Today we had a 45' meeting with Prof. Kiayias. We focussed on the changes that F_PayNet
requires as a result of its separation with G_Ledger and to allow for active corruptions
during a multi-hop payment.

Regarding the channel opening, the funding transaction should be built and sent by
F_PayNet. After submission (in Alice's name) wait for the transaction to be added to
Alice's state in the G_Ledger (i.e. k blocks) and update internal channel state
afterwards. Breach remedy transactions (or whatever invalidating primitive is defined in
BOLT) should be also kept in the state of F_PayNet, so that it can always dispute honest
players' channels that were maliciously closed on the ledger.

Regarding an off-chain payment, There will be one exchange with the Simulator per hop. The
relevant message should be defined. The timeouts for each HTLC output will be chosen as in
spec (minimum safe values) and will not be tunable by the environment (for simplicity).

Another point of interest was that, along with onChainBalance, we should define a
offChainBalance map from players to coins and prove in every step that a coin-preserving
invariant holds. This will give the desired money saving properties.

We also discussed the proving technique that will be used for proving that the protocol
realizes the functionality. One approach is to consider each possible "knowledge state"
the Environment could be in. Another approach that is also used in other cryptographic
proofs is to start with a dummy F_PayNet that pushes all messages to the Simulator, who in
turn simply executes a separate instance of the protocol for each player (this obviously
is indistinguishable) and then incrementally extract parts of the logic to F_PayNet and
bound the increase to the advantage of the Adversary in each extraction step. After the
last step, the functionality will be identical to the defined one. We will then prove that
the sum of the incremental advantages do not result in a non-negligible total advantage.

## 10/12/2018

### Payment Network

Today we had a 35' meeting with Prof. Kiayias. We focussed on F_PayNet. Several
implementation details regarding the opening of a channel and the way that multi-hop
payments are made were discussed.

First of all, we changed the structure of the functionalities once more, probably for the
last time. From now on, the Ledger will be a global functionality, separate from F_PayNet.
Honest players interact only with F_PayNet, but the Environment, the Adversary and
corrupted players can interact directly with G_Ledger. As a result, channel opening and
closing transactions have to be submitted to G_Ledger. (It is still unclear whether these
transactions will be submitted by the Simulator or by F_PayNet directly, but most probably
the latter.)

In order to maintain the desired balance guarantees, the Environment should not know the
private keys that can spend the on-chain funds of the players. Thus in the registration
phase a player should not submit the public key, but it should be generated along with the
public key by an ideal entity. Given that malicious parties must be allowed to spend their
on-chain funds by directly interacting with G_Ledger, these keypairs have to be created by
the Simulator. If the registering player is honest, the Simulator will not reveal these
keypairs to the Adversary. Otherwise, the keypairs will be generated by the Adversary and
the Simulator will pass them on to F_PayNet. F_PayNet will poll G_Ledger for the addition
of funds to these public keys on every activation and thus keep track of the on-chain
balances of each player.

Regarding an off-chain payment, we realised that we need to treat the case when pairs on
the payment path are both honest/both malicious/one malicious and one honest separately.
In the case of the protocol, one successful payment needs one message from the payer to
the first hop, one from the first hop to the second and so on until the beneficiary; then
the opposite path must be followed all the way to the payer. Only then will the atomic
payment successfully complete. The same series of messages must be exchanged between
F_PayNet and the Simulator; in the case of two honest parties, the Simulator is simply
informed of the step and gives a simple OK. In the case that there is one honest and one
malicious party, the Simulator must give its consent as well, along with some additional
data from the malicious party. Lastly, if a hop (or contiguous hops) consist only of
malicious players, the Simulator simply lets F_PayNet know of the end result. Furthermore,
to enable concurrency, a payment does not have to complete (or fail) before F_PayNet can
serve another message, but we create a new message named nextHop (or similar) that can be
received from the Simulator. This message enables the next step of a multi-hop payment to
take place. This way the real-world case of a multi-hop payment failing while in-flight
can be successfully simulated.

Lastly, a minor correction on notation was proposed: Instead of using a lowercase v for
each player, an uppercase V is preferred (e.g. in Initialisation).

## 6/12/2018

### Post voting system

This time we had a 35' meeting with Prof. Kiayias. We discussed the transition of the
"Puff of Steem" paper to the dynamic setting. I described the modified model:
1. The execution pattern lasts for L rounds. In each round all players are activated. Each
   post can be voted for R rounds. The Environment can now instruct players to post
   instead of voting; Prof. Kiayias commented that we should have a more fine-grained
   control over when the Environment instructs players to post, since posting on the last
   round ensures very low visibility.
1. The theorem does not contain the number of posts or the number of rounds anymore. It
   is stated in such a form that L is sent to infinity, i.e. we care about arbitrarily
   long executions.
1. Since posts are not an input to the system but are created ostensibly by players, I
   proposed to add an Oracle which provides "encrypted" posts to players, who in turn
   submit them to G_Feed for "decryption" and addition to the list of posts. Prof. Kiayias
   simplified this setup by having each player simply send the `post` message to G_Feed
   and then G_Feed asks the Oracle for a post for the player. The specific parameters
   passed to the Oracle and whether the latter maintains state is not yet decided. The
   Oracle is the vehicle through which we will carry out the desired attack, by generating
   specifically crafted posts.
1. I proposed a method for exponential decay of both the ideal score and the real score of
   every post as time passes. Prof. Kiayias argued that this method is too specific and
   arbitrary; he instead proposed that we specify some properties these two functions
   should satisfy, instead of exactly defining them. We should choose these properties so
   that they seem plausible for a system like that (and ideally include the Steemit
   system), but are attackable as well. We thus aim for a negative result that invalidates
   a class of post ordering methods. Prof. Kiayias set as an ideal target to decide the
   properties such that they cover all the interesting post ordering methods and prove
   them unsuitable, leaving only "dumb" ordering methods that would obviously not lead to
   useful orderings. I responded that such a proof would in practice show that there
   exists no good post ordering system, a result that is intuitively very unlikely to
   hold.

In a followup email next morning, Prof. Kiayias commented that a positive result regarding
decentralized content curation can possibly be achieved through a machine learning
approach, so we should pursue this direction with some help from the ML group in the
future. Until then, we should focus on the negative result and send to a small(er)
confernece. He added that for February he recommends focussing on the payment channels
paper and attempt submission to Usenix or most probably Crypto.

## 22/11/2018

Today we had a 30' meeting with Prof. Kiayias. We dedicated half of the time discussing
the fate of "Puff of Steem" and the other half to the progress of the F_PayNet and the
related protocol.

### Post voting system

We first discussed where we should aim to submit next. Since [The Web
Conference](https://www2019.thewebconf.org/) deadline is already past, we decided to aim
for either [Usenix Security](https://www.usenix.org/conference/usenixsecurity19) or
[PODC](https://www.podc.org/). We agreed that in order to have a substantial probability
of being accepted in either of these conferences, we have to:
1. Add coins to our current model and prove our theorem there
1. Expand to the dynamic setting, where the execution continues and posts keep coming
   indefinitely.
In case we submit to PODC, we will have to focus on expanding our theory, whereas in
Usenix, simulations of real-world scenarios will be more relevant. In the latter case, we
will have to decide upon one (or several) suitable likeability distribution(s).

### Payment Networks

Here we focused on two issues. Firstly, we discussed once more whether F_PayNet should
allow only for channels initially funded by only one of the parties (as in LND) or permit
the more robust case of allowing funding by both parties. I explained that allowing for
the second case would make us diverge very far from the BOLT specification. Prof. Kiayias
then suggested that we write two separate functionalities, one with the first and another
with the second channel opening method. Only the first will be realised by the Lightning
protocol.

Secondly, we decided upon the cleanest way of introducing and managing the coins of each
player. We decided that each player registers their public key as a first interaction with
the functionality. The functionality reads the state of said player and decides this
player's balance based on the coins she can spend with the corresponding private key. All
subsequent keys for all players are generated and stored by the simulator. The coin
balance guarantees that will be given by the functionality are maintained as long as
players do not interact directly with G_Ledger but only through F_PayNet.

## 16/11/2018

### Payment Networks

Today we had a 40' remote meeting with Prof. Kiayias. We focussed entirely on Payment
Networks. I first showed him a candidate validate(state, tx) specification, which would
simplify the operations currectly offered by Bitcoin without effectively reducing the
types of transactions that can be made. Furthermore, it suits our use case very well,
without adding unneeded complexity. Prof. Kiayias found the specification sound.

We then delved into the details of the newly introduced changes to F_PayNet. In
particular, I first explained that the LND implementation only supports opening channels
that are funded from one of the two participants; this is a limitation that simplifies the
code and improves the user experience, as the other party need not manually confirm or
reject the opening of the channel. Our model however supports channels that are initially
funded by both participants. This introduces one more message for the functionality to
accommodate, namely acceptChannel. We agreed to keep our model this way, but if this
addition makes the analysis too hard, we should be ready to simplify it. Prof. Kiayias
also proposed allowing for both types of channel opening as a future extension.

Afterwards, we discussed the actions the functionality takes upon receiving an
acceptChannel message. Prof. Kiayias had two important objections. Firstly, the
functionality does not have to return to the players a detailed set of elements needed to
reproduce the protocol externally (such as public-private keys), just a unique receipt and
the end-user information (such as channel balance). Secondly, the functionality should not
add transactions to the buffer, but confine itself to keeping track of what channels exist
locally. It is the job of the simulator to submit transactions to the ledger in order to
simulate the real world as needed, so whenever the functionality now builds a transaction
and adds it to the ledger, it should instead simply send a message with the relevant
information to the simulator. Another minor comment was that I should better use the
command "return" instead of "send" for messages that are transmitted to the players by the
functionality. Given the semantics of "return", it should happen en masse as the last
action before the functionality returns the control to other ITMs.

We then briefly discussed whether we should also employ the invoicing system of LND. I
described how a payment in LND is made: First the receiver creates an invoice through LND
and sends it out-of-band to the payer. Then the payer completes the payment through LND.
On the other hand we avoid the first step; the payer directly specifies who and how much
to pay, and through which nodes to route the payment. We agreed that our approach is
simpler and clearer, so we should not change it to look like LND.

Lastly, we agreed to meet early next week to discuss further progress on the functionality
and our course of action regarding the "Puff of Steem" paper.

## 22/10/2018

### Ledger Functionality

Today we had a 2:15' meeting with Prof. Kiayias, Thomas Kerber and Dimitris Karakostas on
the implementation of a new, simplified Ledger Functionality. We started from scratch and,
based on previous work that Thomas had done, designed a complete, new functionality with
clearer and simpler semantics, along with a new liveness guarantee. The functionality is
parametrised by only one algorithm, namely isValid(state, tx). Dimitris was assigned the
responsibility of writing down the functionality.

### Payment Networks

Following the previous meeting, me and Prof. Kiayias had a 15' discussion on the progress
of F_PayNet. In particular, we decided that the handling of the "pay" message is now
done correctly. On the other hand, the handling of the "close" message has to be
improved. In particular, the true mechanics of parties being able to close to an old
channel state, whilst other parties are given the chance to broadcast a newer state within
a predefined time frame (as read from G_Clock) should be explicitly described. The time
frame should be specified upon opening the channel.

Lastly, we agreed that it is not needed to give the parties the option to decide the
minimum_depth parameter (which defines how deep the opening transaction should be buried
before considering the channel open), but instead rely on the states of each individual
player for that.

## 12/10/2018

Today we had a 40' meeting with Prof. Kiayias. We dedicated most of the time on payment
networks and briefly discussed DAG-based transaction systems.

### Payment Networks

First, I let Prof. Kiayias know of my progress in formalising the channel open protocol in
Lightning. In particular, I showed him the figure of the messages of the protocol and
ensured that the `broadcast` and `fundingCreated` messages do not involve the Environment,
but F_Ledger. I also mentioned the many layers of non-intuitive complexity that is
leveraged to enable the Lightning protocols and justified it with the need for
backwards-compatible changes.

We then discussed the state of F_PayNet. Prof. Kiayias observed that the opening and
closing of channels express the desired properties, but the channel update does not. In
particular, the update procedure should reflect the fact that each successful update done
by a player that aims to achieve a `tx` results in the functionality sending a
`receipt` to each relevant party. The process should be provably fast in the
semi-synchronous setting, e.g. O(\[payment path length\] \* \[network delay\]). Any party
should then be able to use her receipts and txs to create and send a single transaction to
F_ledger which spends the opening transaction in a way that ensures that at a certain
predetermined moment in the future she will be able to spend the amount of coins promised
by the txs.

Afterwards, we focussed on some details around payment routing. We specifically decided
that each payment should be initiated with the Environment specifying (among other things)
the exact path to the paying party; that subsequent hops should accept to route the
payment if they have enough capacity in the respective channel (i.e. they will _not_ ask
the Environment whether to route or not, but decide locally). This ensures that payments
happen in an atomic manner. We thus avoid the case of concurrent mutually-invalidating
multi-hop payments. A corrupt hop can of course refuse routing and thus possibly partition
the network. A relevant guarantee should be that no honest participant can lose money
whatever the corrupt hops do.

### DAG systems

I then briefly exposed my ideas on DAG-based transaction systems. In particular I
mentioned the two properties that I deemed relevant: First, that each tx creator decides
how much PoW to provide and pays or is paid according to this choice. Second, that each tx
creator chooses to validate all "new" tip transactions, i.e. all transactions that in
turn validate transactions that were "new" at the time.

Prof. Kiayias commented on the difficulty of creating a new protocol from scratch and
added that he rather had in mind attacking a DAG-based cryptocurrency. We however agreed
that this could be a futile effort, since these protocols are under-documented and thus
require a great effort to understand their inner workings, the level of formalism is such
that most claimed attacks could be debunked as "not pertaining to the model" or "highly
unlikely" (except for a very specific and possibly expensive double-spend attack) and
finally the foundations controlling such cryptocurrencies can easily change the protocol,
making the whole process a moving target.

Lastly, we discussed briefly on Prof. Kiayias's "parallel chains" work. He briefly let
me know of the optimal network usage results and some of its techniques. He then added
that the as-of-yet unsolved issue (and the possible relevance of DAG-based systems) is
that the number of chains is decided before the system starts its operation, based on the
network conditions. The latter are assumed to be stable. Nevertheless, in real-world
circumstances, the network conditions could vary and thus the number of chains too. This
idea makes the concept of DAG-based systems relevant. It is still unclear how to leverage
them though.

We decided that these two projects (payment networks and DAG systems) are rather
incompatible and that I should choose to focus on payment networks given the intuition and
previous work I have already made on this front. We should however have in mind the other
project for the future.

## 19/8/2018

### Post voting system

Today we had a 30' meeting with Prof. Kiayias. We clarified the way in which we can carry
over parts of Andrés's dissertation and that Andrés should carry on with Related Work.

We then focussed on a question I had on the proof: There is a degenerate edge-case that
needs separate treatment. The choice was between explicitly excluding this edge-case and
dedicating more time to prove non-convergence. We decided that excluding it is easier; a
relevant Remark should explain these limitations after the Steem system definition.

Lastly, we laid out the details of the content Simulation section. In particular, it will
contain:
1. One or two plots (t-convergence and kendall-tau) for the case of likeabilities of
   uniform distribution and (maybe) a different set of likeabilities (e.g.
   player-affinity)
1. A plot (size of voting ring - relative/absolute gain in position) for the case when
   there exist selfish players.

## 18/9/2018

### Post voting system

Today we had a four-way meeting with the four co-authors: me, Andrés, Prof. Kiayias and
Benjamin Livshits. We gave Ben a rough outline of the current state of the paper,
describing our approach and model. He commented on the many rough edges of the paper and
proposed that we carry over some parts from Andrés's dissertation. We also distributed
responsibilities, with Prof. Kiayias focussing on the Introduction, me on the proof (which
will go to the appendix) and the theory section and Andrés on the related work and the
simulation. I will also help with coding the Greedy part of the simulation if needed.

## 11/9/2018

### Post voting system

This week we had a 30' meeting with Prof. Kiayias, which we dedicated to the post voting
system paper. I presented to him the new definitions of an activation message, Execution
Pattern and of (N, R, M, t)-convergence, with which he agreed. He proposed that I change
the definition of the activation message (and as a result that of Execution Pattern) to
contain the target player's pid.

I then showed him the latest progress in the proof of convergence, focussing on the most
complex counter-example that breaks convergence when the number of rounds is not enough
for a player who fully likes all posts to vote all of them with full voting power. He
found the approach reasonable and approved of the progress.

## 31/8/2018

### Post voting system

This week we had an 1-hour meeting with Prof. Kiayias, which was dedicated to the post
voting system paper. I mentioned a series of design choices, such as the definition of a
general post voting system and the five parametrising algorithms. We went through most of
the pseudocodes in detail. I also let Prof. Kiayias know of the problem with letting the
Environment be an arbitrary ITM: it may never activate any player.

Prof. Kiayias proposed a series of improvements. First of all, he suggested to define a
post voting system as a tuple of four algorithms: Init, Aux, HandleVote and Vote. These
algorightms parametrize the already-defined G\_Feed and honest player. He also suggested
that we do away with the Init algorithm for the honest player. He also highlighted two
minor mistakes in the pseudocode.

The most important architectural change that he proposed is that of restricting the
Environment to very specific Execution Patterns that suit our need. This way we can assume
that the Environment behaves as expected and does not fight the evolution of the system.
Since we make no security claims, this is acceptable. This change resolves a series of
other problems as well, such as the fact that having G\_feed output its final post list
becomes redundant and the number of rounds parameter becomes a "global variable" that is
implicitly known by all algorithms and functions, therefore simplifying the writeup.

## 27/8/2018

This meeting had an initial 30' part where me, Prof. Kiayias and Dr. Vassilis Zikas were
present and a second that lasted 45' and was only between me and Dr. Zikas. We discussed
the UC-Trust model and particularly asked for Dr. Zikas's expertise on the [Rational
Protocol Design](https://eprint.iacr.org/2013/496.pdf) framework and how to apply it to
our setting.

### UC-Trust

The first 15' we talked about a misunderstanding of mine with respect to the definition of
the set of all "good" simulators, C\_A. The current definition given is correct, I will
have to look more into it to clear up my misunderstanding. We then gave a thorough
explanation of our current approach to using the framework. Dr. Zikas at first commented
that giving the Environment the ability to choose payoffs would mean that the notion of
optimality ceases to exist and proving that a protocol is optimal in this case would be
equivalent to proving that it is secure in the usual sense.

We then moved on to discuss without Prof. Kiayias, delving deeper into the details of our
approach. I explained how payoffs are currently implemented and why they are needed in the
cryptographic game as well, along with how they are separated and combined with the
game-theoretic utilities. As the rest of the architecture was being explained, Dr. Zikas
realized that the problem we are trying to tackle is indeed complex and that there exist
two intertwined mechanics that could be analysed more easily individually. In particular,
F\_SAT currently implements both the auction-matching and the fair swap mechanisms, both
of which, if not securely realized, allow for strategic behaviour from agents. Thus, Dr.
Zikas's suggestion was to split this functionality into two, more easily analysable parts.
In particular, he suggested that I rewrite F\_Trade to never cheat and be used as a helper
functionality by F\_Match. The latter functionality is closer to the problem Trust is Risk
tries to solve, thus for now it makes more sense to focus on this. I will try to split
F\_SAT into these two parts and make the presentation reasonable for our next meeting.

## 24/8/2018

### Post-voting

This time we had another three-way meeting on the post voting paper with Prof. Kiayias and
Andrés Mosteiro Monteoliva, which lasted for one hour. We discussed on how to define a
post-ordering system and did a short review of the simulation code, written in Python.

After some deliberation, we concluded that the best approach for the definition of the
system would be to define a global functionality G\_Feed which has as hard-coded
parameters the number of players, the initial list of posts, number of voting rounds,
possibly the coins of each player and possibly more. Alongside this functionality, we will
define a corresponding Honest Strategy for the players that interact with this
functionality. The post-voting system would be defined as G\_Feed and the Honest Strategy.
A particular post-voting system can be put to action as follows: The environment activates
a player, who requests to read the current list of posts from G\_Feed and, upon receiving
the current list, it responds with a Vote message. At some point G\_Feed completes
execution and outputs to the environment the final state of the list. We are thus able to
define t-convergence under honesty for an arbitrary system. Afterwards it will be
straightforward to define Steemit as a particular post-voting system and prove the bounds
within which it t-converges.

We also decided to completely drop the concept of the Likeability Distribution of players
and only keep the Likeability of posts. This makes sense since players cannot create posts
during the execution, but posts are already defined in G\_Feed. Other small corrections
include replacing IdealOrder() function from set to list with isIdealOrder() property
(from list to {True, False}). Its name should also be shortened. Finally I should use
macros for variables to facilitate future changes in notation.

We dedicated the rest of the meeting to the simulation code. We showed Prof. Kiayias the
current state of the code and he requested that Andrés create a series of simulations in
order to plot the t of t-convergence dependent on the various twekable parameters, such as
rounds, attention span, etc. Similar plots with the Kendall-tau or Spearman metrics would
be useful. Ideally we would like to have a simple function that takes as inputs the
parameters and which parameter we want to use as x, runs several simulations and outputs
the plots.

## 22/8/2018

### Post-voting

This time we had a three-way meeting on the post voting paper with Prof. Kiayias and
Andrés Mosteiro Monteoliva, which lasted for one hour. We showed Prof. Kiayias the current
state of our model. Apart from many minor corrections, he proposed to separate the
description of the general voting system, the specifics of Steem and the players'
strategy. He then formulated the basics of the desired definitions and theorems we would
like to prove.

## 2/8/2018

This week we had an online meeting that lasted 45'. We focussed on the Steem-related
project.

### Steem

I let Prof. Kiayias know of the new model we designed with Andrés. In this model, there
exists a set of n players, each of which has an initial SP (Steem Power) and a quality
distribution that consists of n distributions in \[0, 1\], one for each player. After some
discussion, we decided that the simplest way to make a model that is both simple and
captures Steem well is as follows.

We keep the concept of voting power, voting weight and Steem Power (SP), but we do not
consider simple Steem coins and Steem Dollars. We also allow each player to create exactly
one post at the beginning of the execution. Players then vote for a number of rounds.
Players cannot vote twice for the same post. The game ends when the voting period for the
posts is over (i.e. one week). Each player has a strategy, either "honest" or "greedy".
Each greedy player is substituted by m sybil players, with each of them taking one-mth of
the SP of the initial player. The value m is chosen such that if a player votes m times
overall in all rounds, no voting power is wasted. Each honest player has a minimum voting
power limit, under which it never falls. At the beginning of the execution, each player
creates one post. This post has a quality value in \[0, 1\] for each player, drawn from
the respective distribution of the creator. The posts are ordered in a random fashion.
Then a predetermined number of voting rounds starts.

In each round (which, in Steem terms, lasts 3 seconds), each player processes the list of
posts from the beginning. An honest player votes for each post, giving it a weight equal to
how much they like it. The player completes their turn when they reach their minimum
voting power. Sybil players only vote for posts generated by sybils of the same initial
greedy player. They vote in such a way so as to boost the posts of the "voting-ring" early
on, but also they take care not to have too low voting power by spending it all at the
beginning. A minimum voting power limit (similar to the honest players) is applied. After
a player completes its round, the votes are tallied and the list is reordered accordingly.

At the end of the game, the designer wins if the order of the posts is similar to the best
order (as given by the average quality of each post, possibly weigthed by each player's
SP) and greedy players take points according to the height of their posts.

The target of our analysis will be double: On the one hand, see how much SP and how many
rounds are needed to converge to the best order if only honest players play. As a
generalisation, we may focus only on the top k posts of the list. If convergence is
impossible, we would like to prove it as well. On the other hand, we would like to prove
that, if greedy players exist, the structure of the game is not incentive-compatible.

A Python simulation of a simplification of the above will be employed to get intuition and
give practical results. The main simplification will be with respect to the quality
distributions.

### Payment Networks

We also had a short discussion on my progress on the Payment Networks. I asked Prof.
Kiayias a question with respect to direct communication between a Functionality and the
Environment. Specifically, I asked how it is possible for these two entities to
communicate. Prof. Kiayias answered that the Functionality can indeed send messages to the
Environment, however the Environment can then distinguish. Thus the Functionality sends
such a message only in case of a bad event.

Furthermore, we discussed on the simplification I did and entirely stripped the Onion
encryption from the Lightning Protocol. Prof. Kiayias disagreed and rightly argued that it
makes more sense to attempt to understand the exact security properties that Sphinx (or
Onion routing in general) provides and incorporate it into our model. He encouraged me to
send him an email on the subject once I have looked more into it.

### Outstanding issues

1. Application-layer F_Ledger
2. Progress in UC-Trust
3. Wrapper Functionalities seem to restrict the behaviour of the wrapped Functionality. We
   would like the opposite for F_PayNet.
4. How to treat Onion routing?

### I have to study
1. [KZZ](https://eprint.iacr.org/2015/574.pdf)
2. [Specification of Cryptographic Tasks](https://eprint.iacr.org/2008/132.pdf)
3. [Onion Routing
   Formalisation](https://link.springer.com/content/pdf/10.1007%2F11535218_11.pdf)

## 26/7/2018

This week we had an online meeting that lasted almost an hour. We used most of this time
for the Payment Networks.

### Payment Networks

I explained to Prof. Kiayias that I did not work on the HTLC because there were other
priorities, namely to precisely describe the lightning protocol for opening a channel. I
gave a high-level description on the procedure (players exchange keys, then exchange
the unsigned funding transaction and signed commitment and revocable delivery
transactions) and justified the middle ground between [the
specification](https://github.com/lightningnetwork/lightning-rfc/) and [the
paper](https://lightning.network/lightning-network-paper.pdf) I have chosen. In
particular, only one player funds the channel for simplicity (as in the specification) but
the channel opening protocol follows the paper paradigm, since the full complexity this
version provides will be used either way when updating the channel (and also because this
procedure is better documented).

I then asked whether the opening protocol should also be matched to a relevant
functionality and Prof. Kiayias answered negatively. To facilitate readability, the
protocol should be simply abstracted away as a separate function. He also suggested that I
add Alice's sid to the opening message sent by the Environment, just to follow convention.

I also let him know that the protocol had to have an additional case when paying to
account for the event that the two parties already have a channel between them. This will
be the next part of the protocol to be fully specified (after the channel opening is
done).

Furthermore, we discussed the fact that some user-defined time limits are needed in the
channel opening phase. Also relevant was the fact that if Alice tries to cheat by
broadcasting an older transaction, Bob may catch her and take all the funds of the channel
if he is activated within the time limit, or lose his share if he is late. Prof. Kiayias
pointed out that, if we want the protocol to realise the functionality, both the timeouts
and the possible events in case of cheating should also be added to F_PayNet. I then
objected that taking such an approach would eventually make F_PayNet (a) much more complex
and (b) too specific to be compatible with other payment networks protocols apart from
Lightning. Prof. Kiayias agreed and proposed the "wrapper" approach as used in
[KZZ](https://eprint.iacr.org/2015/574.pdf). This way, we would have a simple ideal
functionality just like F_PayNet is now and a wrapper that makes it less ideal but
realisable. Also, it would be the task of the Simulator to decide what cut of the funds
Bob will take if Alice cheats, with the (less ideal) functionality only checking that, in
case of timely activation, Bob takes at least as much as asserted in the latest channel
state - this could solve the issue of the functionality losing compatibility with other
payment network protocols, e.g. [Perun](https://eprint.iacr.org/2017/635.pdf).

Another important issue we discussed is that of the integration with F_Ledger. This issue
also affects UC-Trust. The problem is that F_Ledger covers the consensus layer, but leaves
as parameter predicates the content of the blocks and the rules imposed on them,
essentially abstracting away the application layer. Unfortunately, the point of using a
ledger functionality in our case is to have a concrete application layer where
transactions can be submitted and retrieved in a secure way. I will write an email to
Prof. Kiayias elaborating on this point.

Lastly, I proposed an alternative approach to the handling of functionalities. My proposal
was motivated by the fact that we will probably have to define several versions of
F_PayNet with slight variations to accommodate for different payment network protocols,
but also by the fact that modern functionalities (e.g. F_Ledger) have become large and
complex and as a result it is not evident anymore that they provide specific security
properties. The proposal was a two-level approach: First define the desired security
properties, then define a functionality that provides them and finally create a protocol
that realises the functionality. This way we can have a list of security properties for
payment channels and systematically prove which protocols provide which properties,
instead of only proving that they realise specific versions of F_PayNet. This approach
could also be used to prove that F_Ledger provides liveness and persistence. Prof. Kiayias
recognised the issue, but was hesitant as to whether the approach I proposed is the
correct one because it may hurt composability. We will return to this point at a later
date.

### UC-Trust

I briefly let Prof. Kiayias know that in UC-Trust we should go after showing under which
conditions is Pi_TIR is *attack-payoff optimal* (as defined in
[RPD](https://eprint.iacr.org/2013/496.pdf)) since it does not seem that Pi_TIR will be
*attack-payoff secure* for interesting cases.

I also told him that I spent a lot of time trying to unpack the definition of the utility
function in RPD (also discussing with others) and that I think there are some notational
issues. Prof. Kiayias proposed that we do a joint meeting with Dr. Vassilis Zikas to
clarify these points. I also promised to ask Dr. Zikas for clarifications on the crucial
points through email.

### Steem

As far as the [Steem](https://steem.com/) project with MsC student Andrés Mosteiro
Monteoliva goes, I told Prof. Kiayias that I now have a rather concrete understanding of
how the rating system of Steem works. I let him know that I had some more meetings with
Andrés (one of which was a coding session for the voting simulation) and that there exists
tension between making the model and the simulation robust enough to really compare with
the real-world Steem setting and keeping it simple and within the necessary time frame.
Prof. Kiayias proposed once more a joint meeting with the three of us. I promised to have
at least a basic formal model for Steem ready by then.

## 16/7/2018

This week we dedicated our time on the latest changes to UC-Trust for 30'.

We went through the current iteration of the strong/weak F_SAT functionality (the two are
combined, with the weak version added to the strong as comments) and the utilities of the
Designer and the Attacker (as per the [RPD](https://eprint.iacr.org/2013/496.pdf)
framework).

Prof. Kiayias recommended that the "utility" within the Functionality be renamed to
"payoff" to avoid confusion with the RPD utility. I proposed that several "Seller"
variables be replaced with "Offer" where suitable. Moving on to more important points, we
discussed on the specific strengths that are provided to the Adversary by the weak <F_SAT>
and they seem reasonable. In particular, since the Adversary does not know the payoffs of
the players, it must be able to influence the list of offers separately from changing the
actual offer selected. Specifying distList() and distSeller() (a.k.a. distOffer()), as
well as solving the issue that the current punishment against cheats is both too harsh
(grim trigger) and too restricted (friends of the cheated player are not informed) are
deferred to the future.

We then focussed on the Designer and Attacker utilities. After understanding the rationale
behind the formulation, Prof. Kiayias agreed that there is nothing ill-defined and the
general idea seems reasonable. The only change proposed (but deferred to the future) is to
remove from the Environment the ability to assign payoffs to the players and externally
quantify over all possible payoffs (or restricted classes of them).

Prof. Kiayias suggested that the next steps to be taken should be to exactly specify what
an execution looks like (taking inspiration from [Bitcoin
Backbone](https://eprint.iacr.org/2014/765.pdf) and the way the players' views are
concatenated and handled) and to precisely explain what is the target of the RPD analysis
\- i.e. to at least prove that Pi_Trust = Trust is Risk is a Nash Equilibrium (like the
Bitcoin protocol in [BGMTZ](https://eprint.iacr.org/2018/138.pdf)), maybe prove that it is
*optimal* (according to the RPD definition) and ideally to prove that it actually realizes
F_SAT (especially the last part is probably impossible).

## 12/7/2018

This time we discussed on payment networks (as we now call the payment channels) for 30'.

More in detail, we mostly discussed on the Lightning Protocol, which I have been writing
in a more UC-friendly form (Pi_LN). Prof. Kiayias proposed a series of improvements. First
of all, he recommended that all channels, both those including the current player and
those involving others, should be directly added to the network graph upon
creation/learning of a new channel.

Secondly, after reading the crucial part of the off-chain payment, where Alice sends a
correctly built Onion-encrypted message to Bob that contains a valid HTLC payment to Bob
with the specified payment (currently l. 36), Prof. Kiayias recommended that I use an
existing Onion primitive and that I build a suitable HTLC primitive to exactly model the
desired behaviour. In more detail, the HTLC primitive should reflect an NP relation
between a statement (public key and hash, known to the sender) and a witness (signature
and preimage, known to the recipient until the moment the payment is sent).

Lastly, he pointed out that in the current formulation, no information is leaked to the
Adversary. I will have to make an informed decision on what exactly to leak to it. Prof.
Kiayias suggested that I use [A Framework for the Sound Specification of Cryptographic
Tasks](https://eprint.iacr.org/2008/132.pdf) to approach more methodologically this issue.

## 14/6/2018

This time we had a 30' online meeting with Prof. Kiayias.

### UC-Trust

Regarding the trust-related project, I showed Prof. Kiayias the first version of the
weakened <F_SAT>. He agreed with the general direction of the strengths given to the
Adversary there. He resolved my issue of how to punish the Adversary in case of a breach
(e.g. a cheat) by adding the correct punishment to the strong functionality F_SAT. It is
still unclear how the functionality will notice the more subtle attacks, such as the
`chooseMe` breach.

We did not have any progress with respect to the utilities of the players. Currently, the
best idea is to make the utility of the Adversary equal to the utility it is granted by
the Environment at the beginning of the execution, a solution which unfortunately is not
compatible with the RPD framework where the utility is chosen beforehand and then the
Environment is allowed to be any ITM.

### Payment Channels

Regarding the Payment Channels, I showed Prof. Kiayias the first formulation of the
payment network functionality, F_PayNet. The current formulation consists of reaction to
three messages: `open`, `pay` and `close`. He liked the overall idea and had no obvious
corrections to make. He advised me to integrate the F_PayNet with the most modern
formulation of F_Ledger (as described in [Ouroboros
Genesis](https://eprint.iacr.org/2018/378.pdf)) as the next target. This will connect
F_PayNet with existing UC functionalities and facilitate the subsequent proof steps. The
following step (which will be taken after the aforementioned integration has taken place)
will be to provide some power to the Adversary, who now is completely ignored.

We will have our next meeting after 26/6, which is the date I return to Edinburgh.

## 6/6/2018

This meeting was between me and Prof. Kousha Etessami; Prof. Kiayias was not present. In
this meeting, we discussed my high-level ideas about formulating a game (in the
game-theoretic sense) where each player has a particular belief system over the world and
possibly a different prior on the state of the world. The players would be polynomially
constrained, and thus would have to cleverly allocate their resources for every decision.
The horizon of their knowledge would be complemented with their *trust* towards decisions
of other players and the possible states of the world farther from the horizon.
Epsilon-equilibria would be the interesting target of analysing such a game.

Unfortunately Prof. Etessami showed me the state of the art: There exist no results on
even the existence of equilibria in such complex games. The best results we have regard
much simpler games with a very limited number of players. Even worse, Prof. Etessami's
intuition was that constraining the players will not much facilitate the analysis of such
games; much tighter constraints should be placed on the power of the players in order to
sucessfully argue about the existence of equilibria in such games, let alone compute them.

My take-home message was that this line of research should be put on hold. It is true that
my knowledge on both the field of Cryptography and on that of Game Theory could help me to
improve the state of the art in this direction, but this is something I will only pursue
if my other threads do not yield the desired results.

## 1/6/2018

This week we spent half of the time discussing UC-Trust and the rest for the Payment
Channels.

### UC-Trust

This week we focussed on understanding the methodology of [Rational Protocol
Design](https://eprint.iacr.org/2013/496.pdf). In this model, we aim to find a protocol
that realises a particular functionality of interest in such a way that rational
attackers will maximise their utility by following the protocol. We discussed the
necessary elements for an *attack model*, which are:
1. A "secure" functionality, the one we want to find a protocol for.
2. A "weakened" functionality, which explicitly allows the adversary to achieve security
breaches.
3. A utility function for the *protocol designer* and another for the *attacker*.
A protocol that keeps the utility of any attacker within negligible distance from the
utility of the attacker interacting with the secure functionality is *payoff secure*.

After deliberating on the techniques and the details used in [But why does it
work?](https://eprint.iacr.org/2018/138.pdf), we concluded that the next target should be
to write a weakened F_SAT functionality (e.g. one that explicitly allows an adversary to
cause cheats) and utility functions for both the game designer and the attacker. We also
concluded that my previous efforts to formulate a weakened F_Trust were somewhat
misguided.

### Payment Channels

As for the Payment Channels, I described the intuition behind my current model. More
precisely, I model a Payment Network which consists of players and their "assured funds",
i.e. the minimum funds that they will certainly obtain if the entire network is settled
for any reason, and endpoints and their associated "tied funds". Each endpoint corresponds
to a single on-chain transaction with locked funds that can be settled by some players of
the Network. Additionally, the Network is equipped with a transition function that takes
the current state of the Network and some actions by players and returns a new state of
the Network.

Prof. Kiayias agreed with the intuition behind this and told me that it is time to
formulate a specific functionality that serves requests from players to pay other players.
It should read the ledger to deduce the current state of the Network. If it can find a
path and make the payment, it does so without touching the blockchain. If it is unable, it
returns a relevant message to the players. I will start formulating this functionality
this week.

## 25/5/2018

### UC-Trust

In this week's meeting we mainly discussed about the new trust protocol Pi_Trust that
leverages Trust is Risk, along with the game that assesses the ability of an adversarial
player to accrue a high utility. More specifically, in the current (informal) description
of Pi_Trust, Alice engages in a trade with Bob only if the price p of the good does not
exceed her indirect trust to Bob. She then pays him using the Trust is Risk method. If the
trade succeeds and Bob got r, 0 <= r <= p of her direct trust/payment, she reverts to her
previous trust towards her friends and adds an additional p - r direct trust to Bob. If
the trade fails and Bob got r, 0 <= r <= p of her direct trust/payment, she takes back the
rest of the p - r coins from Bob and reduces her direct trust to the responsible friends
by r.

Prof. Kiayias liked this version of the game and told me that the next step in this
direction is to define an F_Trust that can idealise the properties such a Pi_Trust has. We
then realised that it will be hard to define such a functionality in a way that greatly
simplifies on the design of Pi_Trust, especially since such an F_Trust must interact with
the global functionality G_Ledger in an indistinguishable way from Pi_Trust. Putting the
ledger inside the functionality does not seem to be an option either, since the ledger
must be accessible by F_Trade (or Pi_Trade) as well. We have to look into this issue more
thoroughly.

Prof. Kiayias also suggested that I define F_Trust on graphs and an accompanying utility
function on graphs such that when parties cheat, the change that happens on the graph
reduces their utility.

The game takes as parameter the length, the trust graph, the set of desires, the common
prior for the utilities, the length of the game and the corrupted node. It then starts
copies of Pi_SAT for the honest nodes and lets the adversary start with the malicious node
and the system parameters (apart from the length of the game) as input (N.B.: currently
only the malicious node is given to the adversary. This is a mistake). It then hands out
utilities drawn from the common prior to all players. Then the main part of the game
starts, which repeats for the length of the game: A random buyer, a random desire and a
random subset of possible vendors is chosen. The player is then instructed to satisfy this
desire. The algorithm finally outputs the utility of the Adversary. It may make sense to
change this output to the node that has the maximum utility and then try to argue that our
protocols ensure that the Adversary does not have the maximum utility more often than the
rest of the nodes.

### First year review

Prof. Aggelos gave me some feedback on the [first year
report](https://github.com/OrfeasLitos/first-year-review/blob/master/first_year_report.pdf).
First of all, he asked me to rewrite the "Game Theory approach" section to include only
the relevant background on Game Theory and move the justification of the need for Game
Theory elsewhere. Secondly he suggested that I explicitly state the design choice in
F_Trade that the buyer pays before the vendor sends the object, thus becoming the one
susceptible to cheating. The other way around could be chosen as well, but we have decided
not to in order to more closely resemble the real-world case.

### Miscalleanous

1. I commented on my new direction on defining trust using a bayesian game. I briefly
   explained that this idea encapsulates the computational limits of agents, who have to
   choose whether to trust and save time or devote more time to make more informed
   decisions. I will further read on the related Game Theory topics, organise a meeting
   with Kousha and present this idea in the first year review.
2. Prof. Kiayias proposed that I start a new project with Dionysis Zindros to look into
   IOTA and other hashgraph cryptocurrencies and analyse their security properties.
3. We agreed that I write an improved version of the signatures chapter in the [lecture
   notes](http://www.kiayias.com/Aggelos_Kiayias/Introduction_to_Modern_Cryptography_files/Cryptograph_Primitives_and_Protocols.pdf),
   especially the proof of security.
4. We agreed to devote some time at our next meeting to browse the existing resources on
   cryptographic and game theoretic combined approaches.

## 4/5/2018

This week we did not have a meeting. The email with the weekly update that I sent to Prof.
Kiayias follows:

I tried to describe a portion of a Pi_SAT protocol that interacts with F_Trust = Trust is
Risk. We skip the part where Alice has the desire and searches for someone to satisfy it
and we suppose that Bob offers to satisfy her desire for price p (t=0). The idea is as
follows: If (indirect trust from Alice to Bob)_0 >= p, then Alice redistributes her direct
trust (t=1) so that (indirect trust from Alice to Bob)_0 = (indirect trust from Alice to
Bob)_1 and (direct trust from Alice to Bob)_1 = p. She has now paid Bob.

If Bob goes through with the trade and does not touch her direct trust, then Alice
redistributes her direct trust identically to the initial state (t=0) and additionally
adds p more direct trust to Bob. If he sends her the asset but takes away her direct
trust, then she only reverts her direct trust to the initial state (t=0). (e.g. if Charlie
was the friend who linked her with Bob, then she replenishes her trust to Charlie.) To do
either, she uses her exclusive coins if she has any, otherwise she uses the least recently
used direct trust. The LRU trust is the direct trust that has been leveraged the farthest
back in the past to make a successful trade.

If Bob does not go through with the trade, then Alice tries to take her money back. If Bob
has not taken her money, she reverts her direct trusts to their initial state. If Bob has
taken her money, then she reduces her direct trust to the friends that introduced her to
Bob by p and takes this money for exclusive use.

Bob can attack Alice if he colludes with all of Alice's friends that made her trust him.
To succeed, they have to coordinate and steal at the same time. This attack is beneficial
to Alice's friends only if they have to obey Bob for some other reason; otherwise, they
would have made more money if they stole from Alice before Bob was implicated at all.

It would be interesting to see what exactly can Bob do to pressure Alice's friends to
betray her. Intuitively, Bob could be a high-level mafia member and Alice's friends the
lower-level accomplices who prefer to stay in the mafia in the hopes of making
money/increasing their rank instead of being honest or stealing for themselves before Bob
gets implicated.

## 27/4/2018

### UC-Trust

This week I updated Prof. Kiayias regarding the plan of action. Every player is assinged a
utility function by the Environment following a distribution over the class of all utility
functions at the beginning of the game. This distribution is common knowledge to the
players. Alice's utility function takes as input the time, the assets and the money that
Alice possesses. We can consider the assets as continuous if this helps. At any moment in
time (unknown to the players) the Environment may stop the game and evaluate the
utilities.

Our target will be to either find a particular protocol which constitutes a Nash
Equilibrium for all reasonable utilities or prove that no such protocol exists, because
e.g. there exist some utilities such that for any particular protocol-strategy, Alice can
profitably switch to a more advantageous protocol-strategy.

One such protocol to consider is outlined here: When the Environment asks Alice to satisfy
a desire, Alice broadcasts that desire. Bob, who has a satisfying asset, makes an educated
guess about the utility Alice will gain by the asset (according to the common knowledge
utility distribution and possibly previous interactions with Alice) and proposes to her
this price. If she accepts, he updates his guess on Alice's utility by assuming it is more
profitable for her to get this asset and vice versa if she rejects the offer.

Prof. Kiayias thought this plan of action seems reasonable and asked how this setup
interacts with F_Trust. I proposed we simply substitute F_Trust with Trust is Risk in
order to see if there exists a standardised interaction with Trust is Risk under the
setting described above such that the entire protocol is a Nash Equilibrium. This will be
next week's task.

### First Year Review

I shortly described the structure of the first year report, which is "Motivation",
"Problems to solve", "Tools used", "Plan of action" (and contigency plan), "Work to date"
and "Literature review". Prof. Kiayias approved.

### Payment Channels

We discussed my ideas on a general description of a payment channel. For now the idea is
that a payment channel is a tuple consisting of two sets and a function. The first set
contains pairs of players (public keys) and the "assured funds" which the players can
withdraw unilaterally and are ensured to receive if other players settle on-chain. The
second contains pairs of on-chain endpoints and "blocked funds" such that if some of the
endpoints are settled, then only their funds will be redistributed according to a
specified rule to the players. It holds that the sum of "assured funds" is at most equal
to the sum of "blocked funds". Lastly the transition function takes a set of actions (as
many as the players) and returns a new payment channel.

First, Prof. Kiayias added that we should define the concatenation of two payment
channels. I proposed that this concatenation should only exist if the transition functions
are the same. In this case, the concatenation is simply a new payment channel with the
same transition function (extended accordingly to accomodate for more players) and the
concatenation of the two first and the two second sets. More specifically, if some players
exist in both payment channels, then these two occurences are substituted with one
occurence and final "assured funds" equal to the sum of the initial "assured funds".

Second, we discussed on the routing of payments, and particularly whether we could
leverage our knowledge on trust to speak about who assumes any risk involved.
Specifically, we would like to see if any risk the intermediary assumes could be
transferred to the paying party. I pointed out that virtually all current systems provide
a completely trustless functionality and thus trust does not currently play a role here.
We walked though a particular example based on the Lightning network to show that if there
is a conflict between the routing of two payments, then it is in the discretion of the
party controlling the funds in conflict to resolve the issue; if she commits money to both
payments and all other players follow the protocol, then she will lose her funds for sure:
there is no risk at all, only certain loss.

We also briefly discussed the results of [Concurrency and Privacy with Payment-Channel
Networks](https://secpriv.tuwien.ac.at/fileadmin/t/secpriv/Papers/ccs2017a.pdf) that show
that it is impossible to have a payment routing system that is both non-blocking and
private.

Lastly I gave a rough list of properties that may be of interest (or should be abstracted
away) regarding payment channels: Local storage, memory, CPU, number of messages,
necessary online time, various timeouts, different needs for creation, update and channel
settlement, privacy for on-chain observers and other channel participants, (non-)blocking
routing, fees, multi-hop channels rules (directionality, collusion-resistant privacy) use
of underlying blockchain tools (e.g. SNARKs in
[Bolt](https://eprint.iacr.org/2016/701.pdf)), time to cashout in case of non-cooperation,
possibility for faster cashout in case of cooperation, need for trust, possibility of
arbitrary contracts (e.g. [PERUN](https://eprint.iacr.org/2017/635.pdf)) and others.

We agreed that I will focus on the first year report and on the e-commerce trust system
for now.

## 5/4/2018

### UC-Trust

This week we discussed the implications of the current formulation of the utility function
and, in light of the arising complexity and non-generality of the description, we opted
for a different approach. More specifically, we decided that insisting on Pi_SAT being a
utility-maximising algorithm is an idea that unnecessarily increases complexity, because
it needs the utility function to be defined as a series over myopic utilities of every
possible event weighted by the probability that this event takes place; all this projected
for every moment in the future.

Instead of this approach, for now we allow Pi_SAT to be in principle any algorithm; The
Environment will simply provide to each player a utility function that takes as input
(time, money, assets); the players may even ignore the utility function and act
arbitrarily. At a moment in time not known to the players, the Environment will stop the
game and check (a function of) the utility of each player.

This approach ensures that the current evaluation of the utility function in F_SAT remains
unchanged and that we will be able to argue about a variety of Pi_SAT strategies without
too much notational and semantic burden. More specifically, it will be easier to argue
about e.g. the social welfare in case no one ever cheats or the utility of a player that
always cheats in a world that nobody cheats. A big assumption may be introduced to
facilitate this goal, namely that the utilities handed out by the Environment follow a
distribution that is prior common knowledge to the players.

## 30/3/2018

### UC-Trust

This week we focused on the broader picture of the model, with the aim of revising current
design choices. Our discussion focused on how to differentiate between the various
possible F_Trust functionalities. We went together through a detailed tracing of a
possible flow of messages, rethinking things that did not make sense as we went.

The most prominent problem we faced was the way in which the (as of yet not conclusively
specified) reputation parameter influences the utility functions of the players. Dr.
Kiayias pointed out that normally players' utilities should depend only on their money and
assets, thus we decided to remove the reputation from the list of parameters of the
utility function. This in turn means that we have to rethink the way in which the optimal
F_Trust can threaten sellers to cooperate, without creating an incentive for buyers to
falsely accuse competitive sellers of fraud. Ultimately, our goal is to find an F_Trust
formulation that can be proven (in)distinguishable from a Pi_Trust that possibly has
oracle access to the "reputations" of the players (whatever that ends up to mean).

Other comments made were that it may be cleaner for F_Assets and F_Ledger to be global
functionalities (i.e. the Environment should always be able to exchange messages directly
with them) and that the current formulation of F_Trust does not correctly maximise amongst
all possible alternatives (even if we ignore the issue of reputations in the utility
functions). Furthermore, it became obvious that a cleaner formulation of how players
decide their current utility given their knowledge, their money and their assets is
needed. More specifically, it is currently unclear how the "myopic" score of each moment
is distinguished from the (infinite) series representing the utility that a
forward-looking player enjoys and how the score of each moment adds up for the current
utility. No changes were proposed to the current message flow.

## 22/3/2018

This week we did not have a meeting. The email with the weekly update that I sent to Prof.
Kiayias follows:

So, the last two weeks I refined and added more detail to the distinguishability proof in
uc-trust, I think it is now complete. You can read it @
https://github.com/OrfeasLitos/UC-Trust/blob/master/what_is_trust.pdf page 15.

Furthermore, I have been thinking how to set up the "reputation" parameter. I kept hitting
dead-ends, because every idea I had was hitting our favourite wall: it seems that all
global-valued reputation systems are attackable. So I decided to change the parameters to
the utility function of Alice. Instead of a single reputation number, I am thinking of
putting the following tuple: each element will correspond to one player other than Alice,
say Bob. This element will itself be a tuple of Powerset(Assets) elements. Each element of
this tuple will correspond to a particular subset of Assets. More concretely, each element
will be the probability that Bob asks for the satisfaction of this particular combination
of Assets. Here is an example:

We live in the world of P = {Alice, Bob, Charlie}, where there exist Assets = {bicycle,
game boy} (and of course money). Alice's utility function for time t is the following:

U_Alice(t)(x coins, y Assets, ((Pr[Alice receives message (canYouSatisfy, {bicycle}) from
Bob at time t], Pr[Alice receives message (canYouSatisfy, {game boy}) from Bob at time t],
Pr[Alice receives message (canYouSatisfy, {bicycle, game boy}) from Bob at time t]),
(Pr[Alice receives message (canYouSatisfy, {bicycle}) from Charlie at time t], Pr[Alice
receives message (canYouSatisfy, {game boy}) from Charlie at time t], Pr[Alice receives
message (canYouSatisfy, {bicycle, game boy}) from Charlie at time t]))) = z

where x Natural, z Real, y a multiset of elements of Assets

How does this sound?

Finally, I have been compiling a list of properties for the payment channels. I aim to
gradually make it complete. Now it looks like this:

1. Number of participants in the channel
1. On-chain connection(s) between participants
1. Actions: open, update, execute, close
1. Who needs to sign for each action, who is notified, how many rounds of communication?
1. Can a participant unilaterally commit on-chain?
1. Up to how much money can a participant unilaterally obtain?
1. What can a malicious party do? If it corrupts more participants it can do more?
1. How much slower is the process in case of a malicious party?
1. How expensive are the actions? (CPU, memory, storage)
1. How expensive are interactions with the blockchain? (fees, time, etc.)

Tell me your opinion.

## 8/3/2018

### UC-Trust

This week Dr. Kiayias commented on the first iteration of the proof that Pi_SAT using a
bad quality F_Trust (which chooses a random player from the input list) and F_SAT are
distinguishable. He verified the generally correct form of the proof and gave several
minor notation and presentation suggestions, along with the following more important
comments:

1. In the case of the ideal world, the Adversary must be substituted by the simulator.
2. Still referring to the ideal world, I will have to reason about the way the specific
   utilities chosen along with the F_SAT specification ensure that Alice's desire will be
   satisfied by Bob (possibly referring to specific code lines).
3. Replace utilities with more general ones which only satisfy a specific property.
   (optional) 
4. Change F_SAT to still choose a player if the Adversary does not return a valid seller
   to ensure that F_SAT is not too weak.
5. Argue more specifically and formally on why the desire will never be satisfied in the
   case of the hybrid world.

### Payment Channels

I briefly commented on the money-pot modelling idea where each coin belongs to a pot
controlled by several people; possibly more than one person can acquire the same coin. The
pot contract may create relationships ranging from fully-trusted (e.g. 1-of-n multisig) to
trustless (signature from all members for each update).

## 2/3/2018

### UC-Trust

This week we reviewed the first concrete(ish) setup for the utility functions of rational
Pi_SAT players, which involves discount functions, reputation and the probability that a
player assigns to a future condition. It seems that the basic idea makes some sense, but
there are still various unfinished details and debatable design choices. We have decided
to postpone the exact description of these functions and the relation between them for a
later time.

My task for next week will be to go as far as I can in proving that the Environment can
currently distinguish F_SAT (which never allows any cheating) from a set of Pi_SATs that
use a bad F_Trust' which returns a random player from the input list. This is a simple
sanity check of the model and an exercise in understanding, building and using the
necessary syntax for such a proof, as well as seeing how such proofs look like.

## 22/2/2018

### UC-Trust

This week we reviewed for the first time the various functionalities and corresponding
protocols. More precisely, of the functionalities F_SAT, F_Trust, F_Trade and F_Assets and
the protocols Pi_SAT and Pi_Trade, we reviewed in detail F_SAT, Pi_SAT, F_Trust, F_Trade
and Pi_Trade. More precisely, we resolved to make the following changes, ordered vaguely
from most to least important:

1. Give a description for the utility function, at least inputs and axioms/properties.
   The inputs should consist of assets and money (money has the special property that is
   monotone with the utility value and this is common knowledge), as well as the
   "reputation" (its precise form remains to be seen, possibly something in the lines of
   "the code of Pi_Trust").
2. Work for a proof that if F_Trust is as described, no cheats ever happen. This will
   serve as a sanity check for an ideal market, which however is unrealisable because of
   the big powers of F_Trust.
3. Instead of having F_SAT choosing the best possible seller (which is unrealisable),
   have it create a list of possible sellers and pass it on to the Adversary, who will
   then choose the seller from this list.
4. Change all occurrences of the utility function to follow the same notation, especially
   F_Trust, where the notation is very unclear.
5. Change the desire (sent by the Environment) to be a set of assets that would satisfy
   said desire.
6. Change message(a, b, c) to (message, a, b, c) for readability.
7. Fix a small semantic problem in the last 5 lines of the reaction of Pi_SAT to a
   `satisfy` message.

## 8/2/2018

### UC-Trust

This week the meeting was shorter than usual. Initially I gave a high-level explanation of
the structure of and interaction between the various functionalities and protocols.
Afterwards, I pointed out some core elements of each protocol and functionality.

For F_Trade and Pi_Trade, Dr. Kiayias proposed no corrections. However, he proposed that
access control be removed from F_Assets and substituted with a simple mechanism where a
player can query ownership only for his own assets and not for other players' assets. For
the next week I will have to write down F_Trust.

## 2/2/2018

### UC-Trust

This week we dedicated our entire meeting to figuring out the high-level overview of all
the functionalities and protocols. Here is the description we decided upon:

The Environment inputs to F_SAT a particular desire. F_SAT outputs either "satisfied" or
"not satisfied".

The realisation of F_SAT is Pi_SAT, which consults F_Trust to decide who to ask for the
satisfaction of the desire and decides which player is the best choice given a particular
utility function. Then Pi_SAT inputs the asset to be bought, the player who sells it and
the payment amount to F_Trade, which returns either "payment failed" or ("payment went
through" and ("asset received" or "asset not received")). F_Trade does not have any
utility semantics.

Pi_Trade is a rather simple "plumbing" protocol, which makes the payment with the help of
F_Ledger and changes the ownership of the asset with the help of F_Assets. F_Ledger could
be e.g. bitcoin backbone and F_Assets is a simple functionality that keeps track of
asset ownership. F_Assets will remain without realisation.

It is possible that F_SAT changes with respect to which F_Trust is used by its
corresponding Pi_SAT.

## 25/1/2018

### UC-Trust

This week, we focused on the protocol description between the players. We went over the
pseudocode and several improvements were proposed:
1. Avoid strict programmatic writing style and switch to a more natural language,
   specifically for the responses to messages sent as part of a reaction to an earlier
   message.
2. Change the message sent to the Environment after a response to a payment has been
   received (or the wait has timed out). Instead of sending "satisfied(desire, string,
   seller)" or "partiallySatisfied(desire, string, seller)" it is better to return
   "satisfied(desire, string, seller, additionalUtility)", so that there is a continuous
   range of satisfaction levels instead of just three.
3. Rewrite the F_SAT functionality to better integrate with the newly written protocol.
   Furthermore, Dr. Kiayias proposed that we go over parts of some UC papers (e.g. the
   [Canetti 2004 signature paper](https://eprint.iacr.org/2003/239.pdf)) to help me better
   understand how and why things are proven.

### Payment Channels literature overview

I showed the progress on the review of the Lighning Network to Dr. Kiayias. We did not go
into much detail, however he was satisfied by the overall structure. It is possible that
this work will lead to a paper.

## 12/1/2018

### UC-Trust

This week we held the first meeting of the year with Dr. Kiayias, in person. We explored in
more detail how the mechanism for the Protocol and the Satisfaction Functionality can work
in a reasonable way while staying compatible with one another. We managed to push important
decisions (where the alternative would be the use of a utility function) to the
Environment. We determined to give substantial flexibility to the Functionality in order to
avoid making it too perfect for any Protocol to correctly simulate it. We also introduced
the Adversary.

The messages the Environment can now send to or receive from a player are the following:
1. Environment → Alice: "Satisfy desire d through a player in list L" (maybe the list is
   redundant)
2. Environment → Bob: "Obtain and advertise the ability to satisfy desire d in exchange
   for value x"
3. Alice → Environment: "Payment x sent to player Bob for desire d"
4. Environment → Bob: "Fully satisfy Alice's desire d" or "Send inferior good to Alice for
   her desire d" or "Take no action upon Alice's payment x for her desire d"
5. Alice → Environment: "Bob fully satisfied my desire d" or "Bob sent an inferior good
   for my desire d" or "Bob did not take any action upon payment x for my desire d"
6. Environment → Alice: "Increase your direct trust towards Bob by x" or
7. Environment → Alice: "Decrease your direct trust towards Bob by x"
8. Environment → Bob: "Steal x from Alice's direct trust"

As we have determined earlier, the Satisfaction Functionality does not need any trust
semantics, therefore the last two messages are simply forwarded to the Simulator (which
contains the Adversary). The Simulator (or the player who received the message in the case
of the real Protocol) updates the Ledger Functionality accordingly.

## 31/12/2017

### UC-Trust: Thoughts on player and trust definitions

Here are some ideas on how to describe the game in the case that the satisfaction
functionality is missing.
1. Player **Alice** is an ITM that tries to maximise a utility function, known only to
   her.  All other players have their own utility functions, which are unknown to
   **Alice** (she may infer some things about them as the game is evolving though). The
   utility function depends on all the exchanged messages up to the present moment and the
   state **Alice** is in. It also depends on the amount of money **Alice** has; it is
   common knowledge that the utility function of any player is monotonically increasing
   with respect to the amount of tokens this player owns.
2. As for the tokens, it holds that the sum of all tokens of all players is a function of
   time. This function is common knowledge. This is to ensure that money supply is
   independent of the decisions of individual players.
3. Since she is rational, **Alice** will choose to pay some tokens to **Bob** when she
   expects that her utility will thus eventually increase more than in the case that she
   would keep the tokens. Put simply, **Alice** will choose to pay **Bob** when she has
   reason to believe that he will deliver a product or service which is worth for her more
   than the tokens spent.
4. We say that **Alice** is paying trustlessly when (at the moment of payment) she knows a
   strategy that, in case **Bob** tries to commit fraud, she is sure to recover her money.
   On the other hand, if she undertakes some risk due to lack of such a strategy, we say
   that **Alice** *trusts* **Bob**. More specifically, we say that **Alice** *trusts*
   **Bob** *X* tokens when **Alice** knows a strategy such that the maximum amount of
   tokens **Bob** can potentially steal from her is *X*.

## 28/12/2017

Christmas break

## 21/12/2017

This week we did not have a meeting since Dr. Kiayias was not in town. The topic of this
week was the overview of payment channels. More specifically, I have finished reading
[PERUN](https://eprint.iacr.org/2017/635.pdf) and wrote a summary of its functions. I then
proceeded to read [Lightning
Network](https://lightning.network/lightning-network-paper.pdf) and to write a summary for
it as well. This procedure is not complete for the latter paper, but the basic use case is
almost described. The next paper to review is
[Teechan](https://arxiv.org/pdf/1612.07766.pdf).

As a first impression, I believe that the strategies employed by these papers do not fit
well with the approach required by
[LocalChains](https://github.com/OrfeasLitos/LocalChains/blob/master/localchains.pdf), but
may give the necessary starting point to build upon. Further research is required to find
a suitable model.

## 14/12/2017

This week we did not have a meeting since Dr. Kiayias was not in town. The topics of the
day were UC-Trust, LocalChains and the [Whose data is it
anyway?](https://www.whosedataisitanyway.com/) discussion sessions.

### UC-Trust

I have described the "Trust" oracle, which takes as input:
1. a desire,
2. a list of candidate players that can satisfy this desire,
3. the player that wants this desire satisfied.

and outputs:
1. the one player from the list that provides the highest increase in utility on
   expectation,
2. the aforementioned increase in utility on expectation.

Maybe this oracle should be called "market research".

### LocalChains

I have read most of [PERUN](https://eprint.iacr.org/2017/635.pdf).  The relevant part for
us is the way that a virtual payment channel between **Alice** and **Bob** can be created
such that the two preexisting on-chain **Alice** ↔ **Ingrid** and **Ingrid** ↔ **Bob**
payment channels are leveraged, replacing the need for on-chain transactions with two
interactions with **Ingrid**, one for the channel creation and another for closing the
channel.

> **Ingrid**'s role [...] is similar to the role that the blockchain takes for the
> basic channels.

Nevertheless, it is not assumed that **Ingrid** is honest, since the blockchain is always
the last resort for **Bob** if both **Ingrid** and **Alice** are malicious. A high level
description of a virtual can be found at p. 45, with a more detailed exposition at pp.
47-52, which also include most of the relevant protocols and contract functions.

### Whose data is it anyway?

Lastly, I attended the [Whose data is it anyway?](https://www.whosedataisitanyway.com/)
event, where I learned about the new "open banking" regulations that will be put in effect
next year, obliging banks to use a specific API to share customer data with each other.
This effectively creates a common state-controlled data pool. The new laws also describe
finer access control and accountability rules and try to safeguard the citizens' right to
privacy. Having a common data pool with many different agents accessing it with a
standardized API and protecting individuals' privacy seem contradictory, so the concepts
of trust and privacy were discussed to some extent, however without any noteworthy
arguments.

## 7/12/2017

This week we briefly discussed about the form the [satisfaction
functionality](https://github.com/OrfeasLitos/UC-Trust/blob/master/what_is_trust/algorithms/satfunc.tex)
is taking. More specifically, Dr. Kiayias saw two things: first, that the functionality
does not in any way inform the player with the minimum price (Bob) about him facilitating
the satisfaction of a desire of another player who expressed this desire (Alice) and
second, that the functionality attempts to maximize the overall utility even if that means
that specific players may see their utilities slightly reduced. Thus Dr. Kiayias proposed
that we should aim for some form of this functionality that will allow us to use both a
normal financial ledger and a [Trust is
Risk](https://github.com/OrfeasLitos/TrustIsRisk/blob/master/fc17.pdf)-like ledger as its
realisations and eventually argue formally that the latter realisation offers more
attractive properties, financially speaking.

Note: this is the first log entry, but not the first meeting. Aggelos Kiayias has been my
supervisor since May 2017 and we have had several meetings since.
