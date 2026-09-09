# MI_sim — M1: Local Information Network Universe

An interactive sandbox that asks one question:

> If a universe is given **only** nodes, edges, and a local update rule — with space, time and
> the speed of light deliberately **not** put in — do those three things grow back out of it?

Everything runs in the browser. No build step, no dependencies, no server. Open the file.

| | |
|---|---|
| **English** | [`index.html`](index.html) |
| **中文** | [`index.zh.html`](index.zh.html) |

Both files are standalone and functionally identical; only the interface language differs.

---

## The entire model

A graph `G = (V, E)`. Each node `i` carries a state `s_i`. One rule:

```
s_i(n+1) = F( s_i(n), { s_j(n) : j ∈ N(i) } )
```

That is all. The one hard constraint:

> **No global reads.** A node may only look at its immediate neighbours.

There is no metre, no second, no `c` anywhere in the setup. What we *do* have is:

- **distance** = graph distance `d_G(i,j)` (fewest edges between two nodes)
- **time** = the update counter `n`
- **causal ball** `B(i,n) = { j : d_G(i,j) ≤ n }` — everything a disturbance at `i` could
  possibly have reached after `n` updates

Introduce a lattice spacing `a` and a tick `τ` and you get `Δx/Δt ≤ a/τ`, i.e. a maximum
speed `c = a/τ` that nobody typed in.

## Four experiments

| Tab | Question | What it actually plots |
|---|---|---|
| 1 · Causal speed | Is there a stable propagation speed? | Front radius vs update count, for both the causal support and the amplitude front |
| 2 · Light cone | Does a light-cone structure appear? | A real spacetime diagram (space slice horizontal, updates downward) + `\|B(n)\|` growth |
| 3 · Particles | Are there stable localised structures? | rms width, centroid speed, and `Σφ²` conservation over time |
| 4 · Continuum | Does a continuous spacetime emerge at large scale? | Measured dispersion `ω(k)` against the continuum limit, plus front anisotropy |

Update rules available: discrete wave equation, Klein–Gordon (massive), sine-Gordon
(nonlinear, supports breathers), and Conway's Game of Life (`s ∈ {0,1}`).

---

## Read this before trusting any number

This is the part that matters most, and it is why the interface looks the way it does.

**Most of the "results" in a model like this are analytically predictable.** They are not
discoveries. If you run a simulation and it reports `causal speed = 1.000`, that is not the
universe revealing something — the rule *was* "at most one edge per step", so measuring it
is a unit test, not an experiment.

So every number in the readout is tagged:

- **ANALYTIC** — computable with pen and paper. Measuring it only verifies the implementation.
- **MEASURED** — no closed form exists. The number itself is the result.

And there are deliberately **no verdicts**. Earlier versions of this tool printed things like
"✓ maximum speed emerged". That was wrong twice over: the threshold defining "✓" was chosen
by the author, and the fallback message explained genuine anomalies away as "still converging".
Both are confirmation bias written into code.

Instead the readout lists **cases you might see and what each would mean**, in four categories:

- substantive result
- expected, but tautological
- trap, easy to misread
- bug or numerical problem

You look at the plots and decide.

### What is genuinely built in (and therefore cannot "emerge")

- **Two dimensions.** The lattice is 2-D by construction, so `|B(n)| ~ n²` and the measured
  effective dimension ≈ 2.000 are consequences of the setup.
- **The causal bound of 1 edge/step.** This is the update rule restated.
- **`a` and `τ`.** Only the *combination* `c = a/τ` is emergent. The two scales themselves are
  still inputs.
- **The 45° cone in the spacetime diagram.** That is a plotting convention (equal pixel scale
  on both axes), not a measurement.

### What is actually interesting

- **The causal bound and the signal speed are different quantities.** Information can reach
  `n` edges away, but the wave front only travels `√κ`. Nothing forced them to coincide,
  and they don't — the same way vacuum `c` differs from the speed of light in glass.
- **Break locality and the light cone dies.** Drag "long-range edges" up to a few dozen and
  the causal front stops advancing linearly. This is stronger evidence that the cone comes
  from "neighbours only" than measuring `c = 1` ever was.
- **Continuum spacetime is only a long-wavelength approximation.** The measured dispersion
  matches `ω = √κ·k` at small `k` and departs from it badly as `k → π`, saturating at
  `ω = 2·asin(√κ)`. At κ=0.25 the deviation at `k=π` is 33.3%.
- **Isotropy is emergent; the lattice underneath is not isotropic.** Wave-front anisotropy
  drops as the seed gets wider, but the causal ball stays a diamond with 34.3% anisotropy at
  *every* scale. That residual preferred direction is real and has not been removed.

## Known limitations

- **Not Lorentz invariant.** The 4-neighbour lattice keeps a preferred frame (see the diamond
  causal ball above). Fixing this needs a different substrate — causal sets, random graphs —
  not different parameters.
- **`a` and `τ` are still external.** Making them self-organise requires the *connectivity*
  itself to evolve (graph rewriting), which this model does not do.
- **No gravity, no quantum mechanics.** Nothing here derives `ħ` or `G`.
- **2-D sine-Gordon breather stability is unresolved.** Derrick-type scaling arguments suggest
  collapse. The tool measures it and refuses to tell you the answer, because there isn't a
  confident one.

## Usage

Clone or download, then open `index.html` (or `index.zh.html`) in any modern browser.

Controls: `Space` run/pause · `S` single step · `R` reset. Everything else is in the left panel.

Suggested first pass:

1. **Tab 1** — press Run, watch the two front speeds separate. Then push "long-range edges"
   to 200 and press Reset to see the cone break.
2. **Tab 2** — let it fill; the V-shaped light cone is drawn directly from the simulation.
3. **Tab 3** — the default glider measures exactly 0.250 = c/4. Then switch rule to
   sine-Gordon with the breather seed and watch the width curve instead.
4. **Tab 4** — click "Measure full ω(k) spectrum". Then sweep σ and watch anisotropy fall.

## Roadmap (M2)

Add graph-rewriting rules so that connectivity itself evolves, and test whether dimension,
`a` and `τ` can self-organise. Those outcomes are genuinely unknown, which is exactly when
the "no pre-written verdicts" discipline above stops being pedantic and starts being necessary.

## License

Not yet specified. Ask before reusing substantially.
