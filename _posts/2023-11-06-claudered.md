---
layout: post
title: "On having an adversarial mindset"
date: 2023-11-06
permalink: /blog/claudered/
---

This past weekend, I went to the Anthropic London Hackathon and built "Claudered" (or ClaudeRed), a gamified LLM red teaming platform. The project applies some practices from security & cryptography land to AI safety — namely CTFs, bug bounties, and blue vs. red teaming. 

_An aside: I was actually supposed to be visiting a friend in London while I was studying abroad in Paris, but he got sick so I went to the hackathon instead haha. I also didn't have housing so I just slept on the floor in one of the hackathon rooms (lol). I was honestly pretty nervous since I didn't know anyone there and didn't know what I was going to build so I hacked on something solo... BUT I ended up being a finalist and presenting to everyone! Cool experience :)_

For the purposes of this piece, I don't want to go too into [implementation details](https://devpost.com/software/claudered) but here are some slides from the finalist presentation to summarize the project a bit:

<div style="display: grid; grid-template-columns: 1fr 0.95fr 0.85fr; gap: 4px; max-width: 940px; margin: 20px auto;">
  <div style="text-align: center;">
    <img src="/public/claudered/what.png" alt="What" style="width: 100%; height: auto; border-radius: 8px;">
  </div>
  <div style="text-align: center;">
    <img src="/public/claudered/why.png" alt="Why" style="width: 100%; height: auto; border-radius: 8px;">
  </div>
  <div style="text-align: center;">
    <img src="/public/claudered/tweet.png" alt="Tweet" style="width: 100%; height: auto; border-radius: 8px;">
  </div>
</div>

Generally, I think games and CTF-style challenges are great educational mediums for teaching AI safety in an approachable way -- covering topics like prompt injection, jailbreaking, alignment, etc. This puzzle-solving, experimental play allows one to adopt an "adversarial mindset" and in turn, helps researchers identify weak spots, ideate creative defense mechanisms, and build more fair and safe systems.

Something that surprised me about working with Claude was how non-deterministic it was. It wasn't _that_ hard to get it to tell me a secret that it wasn't supposed to divulge. This felt fundamentally different than attacking a cryptographic system, where you either have the private key or you don't — either the math holds or it completely breaks down.

This is the dichotomy of generative and defensive systems. Tools that _produce_ output like LLMs, diffusion models, and code generation are generative whereas defensive refers to the likes of cryptography, security, and safety. To _do good_, these need to work in tandem. Generative systems can be incredibly useful for democratizing knowledge and creativity but run the risk of centralization and cultural convergence & decay. Defensive technologies offer an opposing, balancing force. 

A mentor from my applied cryptography sphere describes the latter phenomenon as _minimizing systemic risk_. How can the individual protect themself in worst-case scenario? (Crypto's answer to this question is decentralization — i.e. in theory, no single actor can corrupt a blockchain if it's sufficiently decentralized)

One writing that I have mentally cached is [The Moral Character of Cryptographic Work](https://web.cs.ucdavis.edu/~rogaway/papers/moral-fn.pdf) by Phillip Rogaway. (his version of _systemic risk_ is nation-state surveillance)

> _"Cryptography rearranges power: it configures who can do what, from what. This makes cryptography an inherently political tool, and it confers on the field an intrinsically moral dimension...I plead for a reinvention of our disciplinary culture to attend not only to puzzles and math, but, also, to the societal implications of our work."_

Part 2 discusses a distinction drawn by [Arvind Narayanan](https://www.cs.princeton.edu/~arvindn/publications/crypto-dream-part1.pdf): _crypto-for-security_ is crypto for commercial purposes (e.g. TLS, payment cards, cell phones) vs.  _crypto-for-privacy_ has social or political aims (e.g. privacy, cypherpunk reform). Rogaway then claims that most academic cryptography doesn't fall into either of these buckets, but rather, is simply _crypto-for-crypto_.

That is all to say, Rogaway claims that frontier research can easily become divorced from real-world concerns if not intentional enough about choosing just and useful problems to solve:

> _"Choose your problems well. Let values inform your choice. Many times I have spoken to people who seem to have no real idea why they are studying what they are. [...] Attend to problems' social value. Be introspective about why you are working on the problems you are."_

This introspection becomes even more critical in AI.

LLMs are incredibly powerful and will become widely adopted by the everday consumer. They will unlock a whole new realm of capabilities, impacting how people access/consume information, form opinions, and understand the world at large. But LLMs _will_ evolve and people will increasingly rely on them. This is why AI needs to be built with incredible responsibility.

Learning will be more democratized and this is good. But I am also worried that the upcoming generation will outsource thinking to an AI and we will enter an era of cultural decline. Creativity and diversity of thought will converge because the language that everyone consumes (and outputs) will converge. LLMs already influence thought and sway opinion. This will become increasingly unnoticed as the models improve. I wouldn't doubt that our society's moral standards will be impacted by LLMs. This means that the people training these models have a great responsibility.

I admit this is a bleak framing of the future, but it's intentionally extreme. Cryptographers have long lived in this world: build the most robust system in the most adversarial, worst-case scenario. (Think threat modeling, least privilege, zero trust, red teaming, byzantine fault tolerance, formal verification, etc.)

The same principles from _The Moral Character of Cryptographic Work_ apply to AI: **AI researchers have a moral and political obligation to consider the societal implications of their work.** 

AI systems must be developed responsibly and deployed safely to benefit the public good. People who train models will directly impact what people perceive as what's right and wrong (e.g. moral codes of society). This will be an ongoing challenge, but I am hopeful teams will be intentional with their work, and as a result, AI will be an incredible benefit to humanity. (_See Anthropic's [Constitutional AI](https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback), [Core Views](https://www.anthropic.com/news/core-views-on-ai-safety), [Responsible Scaling](https://www.anthropic.com/news/anthropics-responsible-scaling-policy)._)

**

Some directions that I think are interesting are: 
- AI threat modeling & red-teaming: how can we identify abuse (particularly edge cases) before deployments? (adversarial prompts, data poisoning, etc.)
- Alignment: what are the "right" reward functions from a societal perspective and how are these determined (especially when things like politics come into play)?
- Formal verification in domains like AI for code generation or AI for math (Lean?)
- Algorithmic bias & training data homogeneity: how can we ensure systems are sufficiently diverse and/or neutral? 
