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

We will have our next after 26/6, which is the date I return to Edinburgh.

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
   ``satisfy'' message.

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
