---
layout: post
title: "Labubus & AI Editors"
date: 2025-09-08
permalink: /blog/ai-editors/
---

AI editor interfaces usually fall into one of two buckets: (1) back-and-forth chat, or (2) drag-and-drop canvases. That's why I wanted to explore building a different AI generation interface at the first Gemini Nano Banana hackathon (and won 4th place!).

We built a novel consumer app for brands to rapidly experiment and iterate on new assets or product lines. Nano Banana uniquely enables apps & interfaces like this because of its style consistency (both between input & output images and across multiple calls) and generation speed.

<video src="/public/ai-editor/labubus2.mp4" controls style="max-width: 800px; width: 100%; height: auto;"></video>

A project is a unified brand or style. The user uploads a few images as the "style guide" for all sets that are within that project. Sets are collections of things that align with that style, generated all at once from a text prompt (e.g. "Jansport backpacks", "hightop sneakers", "letters of the alphabet"). When a set is generated, the platform first generates a diverse set of text prompts using Gemini 2.5 Flash. Then, we feed those prompts into Nano Banana with the style guide — these calls are parallelized. 

The [one style --> one product --> N generated variations] pipeline is great for experimentation or creating a lot of background assets within a particular "universe" (aka brand).

Labubus are an amazing example of a brand that is taking the world by storm and should continue to try to occupy as much mindshare as possible to increase their bottom line. Our platform makes it really easy to launch one-time campaigns (e.g. backpack partnerships for back-to-school season) and visually experiment with new products to guage initial consumer interest without investing too many internal company resources (e.g. design, manufacturing, etc.).

<div style="text-align: center;">
    <img src="/public/ai-editor/home.png" alt="home" style="width: 100%; height: auto; border-radius: 8px;">
</div>

LABUBU-IFY EVERYTHING MWAHAHA!!

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; max-width: 1000px; margin: 20px auto;">
  <div style="text-align: center;">
    <img src="/public/ai-editor/backpack.png" alt="Backpack" style="width: 100%; height: auto; border-radius: 8px;">
  </div>
  <div>
    <div style="text-align: center;">
        <img src="/public/ai-editor/sneakers.png" alt="Sneakers" style="width: 100%; height: auto; border-radius: 8px;">
    </div>
  </div>
  <div style="text-align: center;">
    <img src="/public/ai-editor/stickers.png" alt="stickers" style="width: 100%; height: auto; border-radius: 8px;">
  </div>
  <div style="text-align: center;">
    <img src="/public/ai-editor/rocket.png" alt="rocket" style="width: 100%; height: auto; border-radius: 8px;">
  </div>
  <div style="text-align: center;">
    <img src="/public/ai-editor/font.png" alt="font" style="width: 100%; height: auto; border-radius: 8px;">
  </div>
  <div style="text-align: center;">
    <img src="/public/ai-editor/bees.png" alt="bees" style="width: 100%; height: auto; border-radius: 8px;">
  </div>
</div>

If you're an indie 2D game developer and are bottlenecked by art (e.g. creating pixel art assets), you can use generate a bunch of sets that all match the style of your game universe. For example, for Stardew Valley, I made sets of plants, animals, and houses:

<div style="text-align: center;">
    <img src="/public/ai-editor/plants.png" alt="plants" style="width: 100%; height: auto; border-radius: 8px;">
</div>

If you're a creative and need inspiration for character design or need a bunch of "NPCs", you might generate different sets of characters — like new grass pokemon or background characters for South Park.

<div style="text-align: center;">
    <img src="/public/ai-editor/grass.png" alt="grass" style="width: 100%; height: auto; border-radius: 8px;">
</div>


<div style="text-align: center;">
    <img src="/public/ai-editor/southpark.png" alt="South park" style="width: 100%; height: auto; border-radius: 8px;">
</div>

**

One thing I really disliked about [Google AI Studio](https://aistudio.google.com/prompts/new_chat) is how slow the turn-taking chat interface is. This is a great pattern for when you are iterating on _one_ thing, but horrible for iterating on _many_ things under _one_ theme. It's incredibly slow / impossible to parallelize work, annoying to repeat prompts / image attachments for each generation, difficult to differentiate style vs. content, and no gallery view for seeing all outputs at once.

Even [Sora](https://openai.com/sora/) and [Midjourney](https://www.midjourney.com/) haven't nailed the _best_ flow in my opinion, but I think they are still very good consumer interfaces that have the right pipeline for what they are trying to achieve right now (queues for background generation, social explore gallery, etc.).

As mentioned in an [earlier post](/blog/ai-manga/), I really like Midjourney's separation of image, style, and omni in their prompt editor. It makes it easier to specify what components the image model should focus on for what purpose -- which sets user expectations and guides the model better for more reliable outputs:

<div style="text-align: center;">
    <img src="/public/ai-manga/prompt.png" alt="South park" style="width: 100%; height: auto; border-radius: 8px;">
</div>

Lots of these AI image editors are still in the "toy" stage, but we're entering an era where the human-AI interface will determine winners and losers. As models commoditize, the interface becomes the differentiator. Building at the Nano Banana hackathon made me think about:

- **Parallel creativity**: Most AI tools assume single-threaded thinking, but creativity is about exploring multiple directions simultaneously. We need interfaces for creative worlds, not just individual outputs.
- **Granular intent**: Interfaces should handle the bulk of reasoning about inputs at the _user input_ layer. Midjourney's style/image/omni separation is a clearer mental model for users and offers better input guidance for models. These interfaces will probably vary depending on the use case (e.g. make a comic book will have character input stage, plot stage, etc.). 
- **Sketching speed**: When generation is fast enough, AI becomes a co-collaborator in brainstorming rather than a service you commission. This gets ideas off the ground and lets you iterate faster. 

_Here's the [Github](https://github.com/kayleegeorge/turbo-banana) to our hackathon project!_
