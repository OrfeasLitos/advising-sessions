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
