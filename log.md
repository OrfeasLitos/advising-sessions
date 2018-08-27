### 27/8/2018

This meeting had an initial 30' part where me, Prof. Kiayias and Dr. Vassilis Zikas were
present and a second that lasted 45' and was only between me and Dr. Zikas. We discussed
the UC-Trust model and particularly asked for Dr. Zikas's expertise on the [Rational
Protocol Design](https://eprint.iacr.org/2013/496.pdf) framework and how to apply it to
our setting.

### UC-Trust

The first 15' we talked about a misunderstanding of mine with respect to the definition of
the set of all ``good'' simulators, C\_A. The current definition given is correct, I will
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
