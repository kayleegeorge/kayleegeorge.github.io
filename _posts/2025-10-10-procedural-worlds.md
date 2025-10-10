---
layout: post
title: "Generative worlds"
date: 2025-10-10
permalink: /blog/generative-worlds/
---

I'm very excited about immersive worlds. Worldbuilding is the "process of constructing an imaginary world or setting." 

My interest in worldbuilding was actually sparked in a Classics seminar I took at Stanford my last year called _The Pastoral Ideal_ [1]. It's something that filmmakers, game creators, sci-fi writers, even philosophers explore. Iconic fantasy universes like J.R.R. Tolkien's _Lord of The Rings_ and George R.R. Martin's _A Song of Ice and Fire_ (adapted into _Game of Thrones_) have deep histories, invented languages, and intricate political systems that make their worlds feel truly lived-in. Game devs balance this same depth with interactivity, allowing players to explore and shape these universes firsthand.

Some of _my_ favorite worlds are Fullmetal Alchemist, Attack on Titan, Studio Ghibli films, and Avatar: the Last Airbender. One day I want to be able to step into a complex visual world like Arcane (praised for its stunningly unique art style).

**

Graphics programming is also worldbuilding, just with different tools. Things like real-time rendering and procedural generation (think Minecraft's infinite terrain) let us translate imagination into tangible explorable universes. The next era of these imaginative worlds will be unlocked by AI models that can generate not just terrain, but rich worlds with coherent physics and cultures. 

To that end, I've been building some toy projects to explore some basic building blocks for procedural world generation like SDFs [3]. This evolved into... 

## Procedural Planets

Procedural Planets is simple browser-based galaxy where anyone can create procedurally generated planets using mathematical noise functions. **Make a planet [here](https://planets.kay-r-george.workers.dev/)!!**

<div class="align-center">
    <img src="/public/worlds/planet.png"  width ="600px"/>
</div>

This project is built on Three.js (a popular JS library that acts as the core WebGL abstraction layer used to render 3D graphics in a browser) and uses [React Three Fiber](https://github.com/pmndrs/react-three-fiber) for React compatibility.

When you create a planet, you can configure different parameters like terrain detail and noise octaves to create a unique terrain surface. The preview modes allow you to explore different planet features like noise (raw noise values as contour lines), wireframe (actual 3D mesh geometry), and normals (surface gradients). 

The terrain generation uses procedural displacement mapping, which basically means noise functions determine how far to push each vertex outward from a base sphere.

The flow:
1. Start with a base sphere (96x96)
2. Sample noise functions (3D Perlin noise with domain warping)
3. Displace vertices (push each vertex outward or inward based on its noise value which creates varying terrain like mountains and valleys)
4. Render the mesh (the displaced geometry is rendered as a standard Three.js polygon mesh using efficient GPU rasterization)

<div class="align-center">
  <img src="/public/worlds/create.png"  width ="600px"/>
</div>

Then, when you're exploring the galaxy, the system uses a level-of-detail (LOD) pipeline that adapts the terrain detail complexity based on camera distance. 

### Why Procedural Noise?

Traditional 3D graphics use polygon meshes, which are collections of triangles that approximate curved surfaces. This is great for static models but it has limitations for procedural generation (i.e. memory intensive, limited variety). Instead of storing fixed geometry, procedural noise allows you to simply store function parameters, like `noise(x, y, z, frequency=2.3, octaves=5)`, to generate terrain. This has advantages like tiny memory usage (just ~5KB of parameters vs ~50MB of mesh data) and diversity from different parameter combos.

Noise functions like Perlin noise take a 3D coordinate `(x, y, z)` and return a pseudo-random value. Using 3D noise (sampling the volume around each point rather than 2D noise mapped onto a sphere) allows the terrain to look more naturally continuous. To create more realistic planetary terrain, I use multiple mathematical functions that simulate natural randomness:

1. **3D Perlin Noise** creates smooth, natural-looking height variations
2. **Domain Warping** distorts regular patterns for more organic-looking surfaces 
3. **Fractal Brownian Motion (FBM)** allows multi-level details
4. **Fractal Cracks** generates surface features like canyons

Let's look at how these functions work:

**3D Perlin Noise** generates smooth, continuous random values that look natural rather than chaotic. Whereas 2D noise mapped onto a sphere (which creates seams), true 3D noise samples the volume around each point. This noise implementation uses smootherstep interpolation for C² continuity to make sure terrain normals look smooth and don't have unnatural-looking jumps:

```glsl
float noise3D(vec3 p, float freq) {
  p *= freq;
  vec3 i = floor(p);
  vec3 f = fract(p);

  // Smootherstep interpolation (C² continuous)
  vec3 u = vec3(
    smootherstep(f.x),
    smootherstep(f.y),
    smootherstep(f.z)
  );

  // 8-corner gradient sampling with trilinear interpolation
  // ... (gradient calculations)

  return mix(nxy0, nxy1, u.z);
}
```

This is combined with domain warping, which is a technique where you distort the coordinate space before sampling your noise function. Instead of getting noise at position (x,y,z), you first warp those coordinates using another noise function, then sample at the warped position:

```javascript
  // Without domain warping (regular sample)
  const height = noise3D(x, y, z, frequency);

  // With domain warping (distort coordinates first)
  const warpX = x + noise3D(x, y, z, warpFreq) * warpStrength;
  const warpY = y + noise3D(x+5.2, y+1.3, z+8.7, warpFreq) * warpStrength;
  const warpZ = z + noise3D(x+9.1, y+2.8, z+4.6, warpFreq) * warpStrength;
  const height = noise3D(warpX, warpY, warpZ, frequency);
```

Domain warping creates the flowing, organic base terrain that mimics natural geology (like rivers through mountains). **Fractal Brownian Motion** builds on this by layering multiple octaves of noise at different frequencies, adding realistic detail at multiple scales (like large mountains with smaller hills and tiny surface textures).

```javascript
const fbm = (x, y, z, octaves, frequency, lacunarity, persistence) => {
  let value = 0;
  let amplitude = 1;
  let totalAmplitude = 0;
  let freq = frequency;

  for (let i = 0; i < octaves; i++) {
    value += noise3D(x, y, z, freq) * amplitude;
    totalAmplitude += amplitude;
    amplitude *= persistence;
    freq *= lacunarity;
  }

  return value / totalAmplitude;
};
```

Finally, **fractal cracks** use inverted noise to make features like valleys, canyons, and fault lines with natural-looking branching patterns:

```glsl
float cracks(vec3 p, float scale) {
  vec3 scaledP = p * scale;

  // fractal noise
  float noise1 = abs(fbm(scaledP, 3.0, 2.0, 2.1, 0.6));        // Large branching 
  float noise2 = abs(fbm(scaledP * 2.3 + offset, 2.0, /*...*/)); // Medium
  float noise3 = abs(fbm(scaledP * 4.7 + offset, 1.0, /*...*/)); // Fine details

  float cracks = min(min(1.0 - noise1, 1.0 - noise2), 1.0 - noise3);
  return 1.0 - smoothstep(0.75, 0.9, cracks);
}
```

These noise functions combine to create planets that feel more natural but also allow each world to have its own character!

### Level of Detail (LOD) System

LOD is a dynamic rendering technique that improves performance by rendering varying levels of detail based on the user's viewpoint. If you're far away from a planet, it will just look like a simple sphere (there's no point in rendering all the planet's details if it's only a tiny pixel on your screen). If a planet is _really_ far, then it's culled (aka not rendered).

_(That's why it can be a bit hard to find planets — if you're too far away, then you can't really see them.)_

I use some basic spatial streaming logic to selectively pull planets from the DB based on where you are in the galaxy. This is known as **spatial indexing**: efficiently organizing and querying objects based on their positions in space. More complex systems will use data structures like Octree/Quadtrees (divides 3D/2D space into hierarchical regions), spatial hashes, or R-trees (group nearby planets into bounding boxes). 

```javascript
const lodLevel = useMemo(() => {
  const { HIGH, MEDIUM, LOW, ULTRA_LOW } = GALAXY_CONFIG.LOADING.LOD;
  if (distance < HIGH) return 'high';      // Full detail
  if (distance < MEDIUM) return 'medium';  // Reduced complexity
  if (distance < LOW) return 'low';        // Simple sphere
  if (distance < ULTRA_LOW) return 'ultra_low'; // A dot
  return 'culled';                         // Not rendered
}, [distance]);
```

**

Obviously this is nowhere near being able to step into the world of Arcane or a fully immersive, generative world, but it's been a fun exploration of procedural generation in the browser :-)

**Footnotes**

[1] I particularly took interest in a world crafted by the unknown ancient Greek novelist Longus in his Hellenistic romance novel _Daphnis and Chloe_. Inspired by a painting Longus encountered in a "sacred grove of the Nymphs", the story explores love as experienced through the eyes of children. I was very fascinated by how Longus essentially acted as a translator between the visual and written worlds of the pastoral.

[2] [https://en.wikipedia.org/wiki/Worldbuilding](https://en.wikipedia.org/wiki/Worldbuilding)

[3] I initially explored [Signed Distance Functions](https://en.wikipedia.org/wiki/Signed_distance_function) but found that noise-based displacement was more practical for real-time rendering. My ex-coworker showed me [this guide on 3D SDFs](https://iquilezles.org/articles/distfunctions/) by Inigo Quilez a while ago. This guy has a bunch of cool posts/videos at the intersection of graphics and math (like [live coding a Greek temple!](https://www.youtube.com/watch?v=-pdSjBPH3zM&t=5011s)). 
