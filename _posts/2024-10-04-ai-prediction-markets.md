---
layout: post
title: "AI Prediction Markets"
date: 2024-10-04
permalink: /blog/ai-prediction-markets/
---

Prediction markets allow participants to bet on outcomes of future events. They are historically [accurate, canonical](https://www.astralcodexten.com/p/prediction-market-faq), and valuable financial instruments when [constructed well](https://thezvi.wordpress.com/2018/07/26/prediction-markets-when-do-they-work/).

Crypto lends itself nicely to prediction markets because decentralization enables global participation and removes the need for trusted intermediaries. Early on-chain prediction markets, like Gnosis in 2017 and [Augur](https://en.wikipedia.org/wiki/Augur_(software)) in 2018, failed to gain traction — unclear exactly why but probably because they were too early. Crypto was not widely adopted and infrastructure was still too slow and too expensive to be usable by consumers. 

Fast forward to today, the crypto-powered prediction market Polymarket has witnessed explosive growth with all-time trading volumes surpassing $2 billion. A whopping $1.4 billion of that total volume belongs to its single most popular market: the [2024 U.S. Presidential Winner](https://polymarket.com/event/presidential-election-winner-2024/will-donald-trump-win-the-2024-us-presidential-election?tid=1728266145595). This is all the more impressive considering the platform is banned in the U.S. (‘merica loves its politics but politicians do not love crypto markets). It is still unclear whether or not Polymarket will be able to have user retention in the long-run but I see a few big reasons for its success up until now:

1. **Timing**. Prediction markets are often popular when they revolve around high-stakes events like presidential elections or crypto price movements.

2. **Usability**. Running on Polygon (an EVM L2) is cheap, fast, and generally scalable. Polymarket wouldn’t be able to succeed on Ethereum L1. Markets also depend on a stable currency, USDC.

3. **Playing the regulation game well**. Polymarket operates outside the U.S. and their legal fees seem to simply be part of their operational costs.

It’s an exciting time for prediction markets, but I think the space is entering an even larger era of innovation. We’re at a unique point in time where prediction markets have lots of momentum (thanks to Polymarket) _and_ a new set of tech tools at our disposal to experiment with novel mechanisms and creative form factors.

**

One extremely interesting yet underexplored idea are AI prediction markets. In this world, prediction markets can either be **played by AI** (the traders are replaced) or **judged by AI** (the classic settlement oracle is replaced).

Vitalik has some [creative ideas](https://vitalik.eth.limo/general/2024/01/30/cryptoai.html) for the former. Imagine if Twitter implemented a Community Note prediction market played by LLMs that are incentivized via on-chain pricing mechanics. Fact-checking AI prediction markets could be generalized to livestreams, debates, or other social media. These constructions are unique because they take advantage of both the hyperfinancialization of crypto and knowledge-rich AI models. 

<div class="align-center">
    <img src="/public/prediction-markets/diagram.png" width="500px"/>
</div>

A fun implementation of the 'judged by AI' prediction market is [tmr.news](http://tmr.news), a market for the next day’s NY Times front page headline. LLM sentence embeddings are used to measure semantic similarity between predicted headlines to the true headline and users are proportionally rewarded. There are two interesting implementation details that makes tmr.news compelling:

1. **Predictions can be non-binary**. Most prediction markets trade on a binary “yes” (X event will happen) or “no” (X event will not happen), or on a multiple choice bet (e.g. X person will be chosen for Y). tmr.news is able to trade on a non-binary market because LLMs are actually advanced enough to be good judges now.

2. **Settlement is trustless**. Instead of a designated oracle, the market is settled using web proofs. Web proofs prove data governance using a protocol over TLS [1] to prove that web data was fetched from the correct origin and remained untampered with. The major unlock here is that off-chain data can now be ported on-chain in a permissionless and verifiable way. tmr.news settles its daily market by directly querying the front page headline from [https://www.nytimes.com/](https://www.nytimes.com/), eliminating the need for trusted intermediaries.

But these details are indicative of a broader theme: both crypto and AI landscapes have matured enough for AI prediction markets to work well and to be interesting.

Past crypto x AI applications did not make sense because crypto infrastructure was not yet consumer-friendly (especially pre-L2 era), LLMs could not yet add real value, and cryptographic protocols that bridge off-chain and on-chain data (like web proofs) did not yet exist. But now, we have all the right tools to launch multidimensional experiments and uncover fresh discoveries. The design space for AI prediction markets is promising and the timing is finally right to pursue meaningful explorations.

--

[1] There are multiple ways to implement proof of data provenance operating over the Transport Layer Security (TLS) protocol. Protocols like [TLS Notary](https://pluto.xyz/blog/how-tlsnotary-works) and [Reclaim Protocol](https://drive.google.com/file/d/1wmfdtIGPaN9uJBI1DHqN903tP9c_aTG2/view) use a proxy attestor placed between the client and the server to sign off on the exchanged encrypted messages to prove data provenance. These protocols also allow for selective disclosure, a way to only reveal selective parts of fetched data (e.g. revealing your bank balance but not your account number). Another approach is using a Trusted Execution Environment (TEE) to authenticate TLS data. The TEE assumes the role of the TLS client, such that it receives and signs server data before relaying that data to the actual client. A relayer is used to efficiently reduce messages between the client and server to keep TEE work to a minimum.

**Appendix** 

Sam Bankman-Fried allegedly wanted to work on [political prediction markets](https://www.dwarkeshpatel.com/p/brett-harrison) after his departure from Jane Street. 

A number of famous economists have [advocated](https://comments.cftc.gov/PublicComments/ViewComment.aspx?id=70761&SearchText=) for the legalization of prediction markets.

Polymarket markets have proven to be quite accurate; they predicted that Biden would drop out of the presidential race a few weeks before it actually happened.

[This article](https://worksinprogress.co/issue/why-prediction-markets-arent-popular/) suggests that even if regulatory chokeholds were lifted, prediction markets would still not be popular because they have “little natural demand.” People who trade on markets can be classified into one of three types of traders — savers, gamblers, and sharps — and it is not in any type’s best interest to participate in the market. 