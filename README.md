# tenfold

One phrase, ten art styles, all of it code. Each style is a single self-contained HTML page that draws `a little faith in impossible things` on a Canvas 2D context, with no images, fonts or libraries.

![The ten styles](poster.png)

## How they are built

Every page follows the same contract. The letterforms are hand-coded for each style as stroke skeletons, bitmap charts or vector strokes, so each letter is made of the medium itself. Slow surfaces such as paper, cloth, walls and terrain are baked once at load into float buffers; each frame is then composed per pixel and written with one `putImageData`, or drawn with Canvas paths and composite modes where the medium is linear. Randomness comes from an FNV string hash seeding mulberry32 (xmur3 in Flip-dot), keyed by names like `disc:12:4`, so `seek(t)` draws the same frame every time and no page calls `Math.random`. Each build finishes between 4.3 and 4.9 seconds and holds a hero frame near 5.2 seconds.

## The styles

### Calder mobile (`11-calder`)

The phrase is bent from single-stroke wire: a glyph table of polylines in x-height units, joined into one continuous wire per word, smoothed with a Catmull-Rom spline and finished with curled ends. Everything an arm carries becomes point masses (rod segments, wire chunks, enamel paddles weighted by area), and `solve()` works bottom-up, finding each arm's one free length by bisection until the net moment is zero. That solve is what places the red a so it levels the top rod. Motion is a damped spring integrated at 1/240 s from t=0 on every seek, and its rest angle counts only the mass built so far, so each arm tips and recovers as parts hook on. Shadows are projected onto the wall from a lamp with a 275 px radius, binned by penumbra width and blurred at half resolution. Each paddle is ray-cast per pixel with 3×3 supersampling and shaded with Lambert light, a sheen term and Schlick Fresnel over a baked enamel brush-stroke texture.

### Kintsugi (`12-kintsugi`)

The phrase is set in a hand-coded cursive skeleton across a black raku bowl seen from above, and each line becomes a crack: resampled at 2 px, pushed sideways by three octaves of Perlin noise, and extended by lead cracks that march to the rim or stop in a T-junction where they meet another. Every crack is rasterised into per-pixel fields (signed distance, half-width, seam id, arc-length fraction), and a flood fill labels the shards between them, each baked as three sprites: bare break, parted and lacquered. The bowl is a height field (rim harmonics, spatula cuts, an off-centre well) shaded in linear light with Blinn-Phong, Fresnel, ambient occlusion and Voronoi crazing; the linen under it is a procedural plain weave. After the shards part and return, gold flows along each seam's arc-length fraction, glossy when fresh and drying to satin as exp(−Δt/0.5). The last line stays wet, and a makie brush slides in to rest on it.

### After Felice Varini (`13-varini`)

This page is a small ray tracer. The glyphs are geometric (ring arcs with square-cut ends, bands and discs) laid out in the image plane of a projector at one seat in a stair hall built from boxes. A surface point takes paint only if it projects back inside a letter and an occlusion ray reaches the projector, so the phrase breaks across walls, treads and a handrail and reassembles only from that eye. Lighting combines a sampled sun disc, an analytic sky term through a glass roof, and one traced bounce of 16 cosine samples on a quarter-resolution probe grid, blurred within each face; penumbra and silhouette pixels get extra samples, and the result goes through an ACES tone curve. Paint arrives in roller patches ordered by hash and weighted by area, vertical on walls and horizontal on treads, with ragged edges and creep under the tape. After the build the camera rises 42 cm and the phrase fractures, which shows it was painted on the room.

### Specimen drawer (`16-specimen`)

Six moths in an entomology drawer each carry one word. Each word is a centre-line skeleton stroked onto an offscreen canvas, bent to follow the wing, and read back as an alpha mask that recolours the wing's pale crossband. Wings are Catmull-Rom outlines converted into a 1440-entry polar table, so pattern lines are iso-lines of the radius and veins are angle interpolations. The cover scales are individual lozenges, drawn from the margin inward so they shingle, each shaded by its own tilt to the light; the word mask is thresholded per scale, so letter edges follow the scale rows. The blue moth's colour is a structural ramp indexed by radius, tilt and noise. The drawer, oak frame and pin holes are baked per pixel under a raking light. Each moth is lowered 54 mm onto its pin with depth-of-field blur and a shadow whose offset and softness grow with height, under a glass lid with a sliding reflection and dust.

### Swiss relief map (`18-relief`)

The phrase is engraved as place names on a Swiss-style relief map. The terrain is a 992×1262 height field at 25 m per pixel, grown from 8 peaks with a branching ridge tree, domain warp, fbm and a three-octave gully pass that carves erosion along the fall line; each summit is then rescaled to its set height. The river and the trail are routed by Dijkstra searches, the trail with a grade-squared cost, and streams follow steepest descent with flow accumulation. Contours come per pixel from the distance to the nearest 100 m level divided by the gradient, rock hachures are streamlines along the slope, and the hillshade blends two normal fields lit at −45° azimuth and 42° altitude. Names are cut with a flat-graver model that strokes each skeleton repeatedly along the nib angle, set along arcs and crest lines. Eight ink plates print with sub-pixel misregistration onto mottled paper, and per-pixel start-time maps let the contours climb each massif from its foot.

### Gilded manuscript (`09-manuscript`)

The text is written in textura quadrata by a broad-nib model. Glyphs are built from minims as point lists, and `nibStroke` stamps a rotated rectangle every 0.55 px at a fixed 40° pen angle, so thick and thin come from the direction of travel, with noise in nib width, angle and tremor. Strokes add with the `lighter` mode so overlaps build density, the quill runs dry on a 13-stroke cycle, ink maps to an iron-gall ramp that stays glossy for 0.6 s, and an animated goose quill rides each stroke. The vellum is baked from follicles, scrape marks, cockle, ruling, prickings and the verso showing through. The gilded A sits on gesso whose height comes from an exact distance transform; 49 squares of leaf are laid and brushed off, and the shading blends wet gesso, crinkled leaf and burnished mirror. The mirror reflects each ray into a modelled room with one mullioned window, and the agate pass is that transition sweeping across the letter.

### Scanimation (`07-scanimation`)

A barrier-grid animation printed and then revealed. The letters are skeletons of lines, superellipse arcs and balls, stamped with a flat 32×9 px nib. Two poses, upright and sheared, are rendered to offscreen masks and interleaved into 120 strips of 9 px; the print arrives in two sweeps, one per pose, with ink running down each strip. Board ink and card are baked per pixel with noise-driven edges, voids and fibres. The acetate is a square wave with an 18.018 px period (two strips plus 0.1% film stretch), and its coverage is integrated analytically for exact pixel-area antialiasing and defocus. The bars cast a shadow on the card at a slightly different pitch, which produces the moiré, and once the sheet lands it creeps one period left so the phrase moves between its poses.

### Flip-dot sign (`06-flip-dot`)

Three panels of 6,184 discs set the phrase in a hand-coded 5×7 proportional bitmap font, each panel sized so its line fills the width. A disc's turn is an eased half-rotation that strikes its stop at 46% of the turn, followed by a damped sine rebound; columns write left to right, each coil row a beat after the one above, and one disc is jammed while another is stuck black. Each disc is projected through a pinhole camera with slight yaw and keystone, drawn as an ellipse squashed by the cosine of its angle, and lit in linear sRGB with sun, sky and a Blinn sheen. A disc caught mid-turn is drawn three times for motion blur. The housing casts a hard shadow, discs and housing get an orange-peel texture from a per-pixel pass, and a blur masked to the left edge gives depth of field.

### Cross-stitch (`02-cross-stitch`)

The phrase is charted as a bitmap of blocks on 12 px Aida cloth, with three-quarter stitches added at the stepped corners. Everything renders in software into a linear-light float buffer. The cloth is baked once as basket-woven threads, each pixel a shaded cushion with ply noise and ambient occlusion, mapped through a slight rotation, warp and keystone. Each stitch leg is two strands rasterised as capsule distance fields and shaded as twisted tubes with groove occlusion, pinched ends and Blinn-Phong sheen; seeded tension makes some stitches loose, tight or twisted. Stitches follow the Danish method, bottom legs out along a row and top legs back, and each settles after it lands while its shadow is offset by lift and blurred. A needle with a modelled eye works the motto, birds and borders, then parks. One sprig stitch is crossed the wrong way on purpose, and a beech hoop, brass clamp and walnut table finish the frame.

### Banknote engraving (`03-banknote`)

The legend is cut in hand-coded roman capitals: thick, thin and stressed strokes turned into width-varying ribbons, spaced optically by binary search over 28 profile levels, and bent onto a circular band. The guilloché is lathe curves r(θ): a 12-lobed rose of crossing families, a dark ring with a braid cut out, a four-strand border, a sine-wave tint and a corona of 84 curves, each pass on its own offscreen layer with its own registration matrix. Inside the letters, two blurs separate hairlines from thick-stroke interiors, and the interiors fill with concentric rules whose width follows tone. The paper is three noise layers with felt and security fibres. Offset inks multiply as transparent tint and intaglio ink lies on top as opaque relief; a height field of paper, emboss and ink is lit with Lambert, ray-marched shadows and specular, and over the last 1.3 seconds of the build the lamp drops from 50° to 14° so the raised ink catches the light.

## How it was made

- **One session, one model.** A single Claude Code lead session took about 29 hours from the first prompt to this repo. The lead and every subagent ran on Opus 5.5; the lead's context filled and was summarised 3 times.
- **107 agents under the lead:** 24 style builders, 5 phrase-page builders, 11 builders for a revision round, 1 film builder, 64 short-lived art-director reviewers that each saw only the render at feed size, and 2 caption writers.
- **The feedback loop:** the lead sent its agents 199 messages of review notes, fixes and redirects. Builders opened their own renders about 2,000 times to check their work.
- **Iterations:** 20 styles were explored in two batches, each taking 1 to 4 review rounds. 10 were picked to carry the phrase; 4 phrase pages passed their first review and 6 their second. The film took 9 drafts plus 2 alternate openings, across 76 commits.
- **Specific asks made it less creative:** a round that put the same motif in every style weakened all ten pages, so it was reverted. Open briefs gave the best work.
- **Tokens:** about 10 million output tokens, including thinking, over about 6,900 model calls. Input was about 1.6 billion tokens, almost all of it cached context re-read on each call.
- **Human input:** about 27 prompts over the whole project.
- **Output:** 10 self-contained HTML pages, 7,395 lines and 540 KB in total. The film is 1,996 frames at 1080×1350, each rendered by headless Chrome on a 64-vCPU cloud machine; the whole shoot takes about 2 minutes. A GPU host was tried and dropped, since Canvas 2D runs on the CPU.

## Run it

Open any `index.html` in a browser to see the finished piece. Each page builds itself from a blank surface over six seconds; add `?t=2.5` to the URL to see any moment of the build. Every frame is deterministic: `window.__riso.seek(t)` draws frame `t`, with all randomness seeded by stable strings.

## Credits

Written and rendered in code by Claude Opus 5.5. The phrase is the title of [an essay](https://www.linkedin.com/pulse/little-faith-impossible-things-sanju-sunny-ed83c/). The seeded random helpers `xmur3` and `mulberry32` in the Flip-dot page come from [bryc/code](https://github.com/bryc/code).

## License

MIT
