# Random Field Shapes

An endless gallery of 3D shapes, each grown from one seed. Nothing is drawn by hand: the seed builds a random graph of operators over 3D space, and the shape is wherever the resulting field is negative.

**Live page:** https://raemon.github.io/random-field-shapes/

## How it works

- **Genome.** A seed picks the structure of an operator graph, not just its parameters. The graph is emitted as GLSL.
  - *Sources:* a coordinate, distance under a random norm, gradient noise, cell noise, a simulated field.
  - *Value operators:* shell, ripple, layers, min/max, smooth blend, multiply, select by a third field.
  - *Space operators:* rotate, scale, fold, repeat, rotational repeat, unwrap, twist, warp, snap to grid, inversion, iterated fold.
- **Simulated fields.** Some graphs include a four-channel grid (64 cells across) whose start state is a small random field and whose update rule is a random expression over local measurements: the cell, its neighbours' mean, min and max, a shell average at a random radius, slope, and the value carried along a flow. The rule runs for 16 to 64 steps.
- **Colour.** A second, smaller random field run through a random cosine palette.
- **Filter.** Each candidate is scored from a tiny classification render. Empty results, solid cubes and speckle are re-rolled, up to six genomes per seed. Simulations are screened on a 32-cell grid first and dropped if they go flat or turn to static.

## Rendering

- Each field is evaluated once per cell into a 128-cell 3D texture. One fixed shader ray-marches that texture for the probes, the tile and the rotating detail view.
- All generation runs in Web Workers on `OffscreenCanvas`, so the page stays responsive. Tiles arrive as `ImageBitmap`s.
- Shaders for the next seed start compiling while the current one renders.

Needs WebGL 2 and `OffscreenCanvas`. Everything is in `index.html`; there is no build step.

## Limits

- Detail is capped by the grid: features thinner than about 1/128 of the cube are lost.
- The same seed should give the same shape, but a borderline re-roll can differ between GPUs.
