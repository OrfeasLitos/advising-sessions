## 8/2/2018

### UC-Trust

This week the meeting was shorter than usual. Initially I gave a high-level explanation of
the structure of and interaction between the various functionalities and protocols.
Afterwards, I pointed out some core elements of each protocol and functionality.

For F_TRADE and Pi_TRADE, Dr. Kiayias proposed no corrections. However, he proposed that
access control be removed from F_ASSETS and substituted with a simple mechanism where a
player can query ownership only for his own assets and not for other players' assets. For
the next week I will have to write down F_TRUST.

## 2/2/2018

### UC-Trust

This week we dedicated our entire meeting to figuring out the high-level overview of all
the functionalities and protocols. Here is the description we decided upon:

The Environment inputs to F_SAT a particular desire. F_SAT outputs either "satisfied" or
"not satisfied".

The realisation of F_SAT is Pi_SAT, which consults F_TRUST to decide who to ask for the
satisfaction of the desire and decides which player is the best choice given a particular
utility function. Then Pi_SAT inputs the asset to be bought, the player who sells it and
the payment amount to F_TRADE, which returns either "payment failed" or ("payment went
through" and ("asset received" or "asset not received")). F_TRADE does not have any
utility semantics.

Pi_TRADE is a rather simple "plumbing" protocol, which makes the payment with the help of
F_LEDGER and changes the ownership of the asset with the help of F_ASSETS. F_LEDGER could
be e.g. bitcoin backbone and F_ASSETS is a simple functionality that keeps track of
asset ownership. F_ASSETS will remain without realisation.

It is possible that F_SAT changes with respect to which F_TRUST is used by its
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
