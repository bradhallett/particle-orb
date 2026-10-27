# Particle Orb — a particles-only live avatar for EnConvo

A state-reactive particle sphere (`KINDS.particles`) for EnConvo's procedural
avatar runtime, plus the runtime fixes it needed. 9,000 GPU sprites render four
conversation states in one vertex shader:

- **idle** — slow axial spin, calm breathing
- **listening** — voice ripples ride the mic level
- **thinking** — contraction in travelling pulse rings
- **speaking** — latitude swirl + stream pull, driven by speech energy

Identity colors are derived, not hardcoded: two OKLab anchors per state blend
in linear space, giving a perceptually even spread that also survives the
deuteranopia check baked into the palette audit.

## What's here

| Path | What |
|---|---|
| `src/particles-kind.js` | The authored `KINDS.particles` block (drop between the markers) |
| `patches/procedural-avatar.js.diff` | Unified diff vs pristine EnConvo 2.5.10 — block, registry entry, and runtime fixes |
| `patches/procedural-characters.json.patch` | One registry entry appended |
| `harness/orb.html` | Local harness: state buttons, poke, synthetic voice |
| `thumbs/particles.webp` | Island thumbnail |

## Applying

EnConvo's `AvatarRuntime` is proprietary; this repo ships only the diff
against it. Either apply both patches to the runtime's `web/` and `vendor/`
files, or paste `src/particles-kind.js` between
`// >>> enconvo-particle-orb begin/end` in `web/procedural-avatar.js` and
append the registry entry. Run `harness/orb.html` from a static server with
the runtime's `procedural-avatar.js` next to it
(`?slug=particles`).

## Runtime fixes in the diff

- sprite `CanvasTexture` leak on re-create (`textures: [sprite]`)
- Reduce-Motion / sleep pacing now reaches ripples, rings and jitter (`uT += dt * pace`)
- shader uniforms seeded from the derived palette instead of duplicated hex
- two inverted one-pole smoothing factors fixed (frame-rate-independent form, factors preserved)
- point-size units corrected (pixel-identical after the `SSF` fold)
- `bands()` computed lazily (one consumer of fourteen kinds)
- input guards + OKLab ΔE / deuteranopia separation audit on palette derivation

## Credits

- Concept adapted from AI Canvas' [Particle Sphere](https://aicanvas.me/) (MIT).
- State vocabulary from the VoiceOrbs project.
- Host API: EnConvo's `AvatarRuntime` (procedural-avatar.js), and the
  gpt-live-avatar lineage it vendors.

## License

MIT — see [LICENSE](LICENSE).
