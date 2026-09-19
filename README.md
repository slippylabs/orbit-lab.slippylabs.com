# Orbit Lab

An n-body gravity sandbox — drag to launch a world and watch it orbit, slingshot or get thrown out. Kepler ellipses, binary stars and the figure-eight three-body orbit, with live energy and momentum. Runs entirely in your browser.

**Live:** <https://orbit-lab.slippylabs.com/>

## What it does

- Seven scenarios: the inner solar system, an elliptical comet, the figure-eight three-body choreography, a binary star with a planet, a gravity assist, three stars with no plan, and a small cluster that collapses.
- Drag anywhere to launch a body — where you press is the position, the drag is the velocity. Scroll to zoom, right-drag to pan.
- Live readouts for total energy, its drift, angular momentum and simulation time, plus a table of orbital elements (a, e, predicted period) for every body against the heaviest one.

## How it works

Velocity Verlet, which is **symplectic**: its energy error oscillates inside a bound instead of accumulating, so an orbit closes on itself indefinitely. Forward Euler on the same problem spirals the planet into the star — smoothly enough to look plausible, which is exactly why it is the wrong choice.

Gravity is softened (the denominator is `(r² + ε²)^(3/2)`) so a dead-centre collision is a fast slingshot rather than a division by zero, and the potential energy in the readout is softened to match. ε is per scenario: the figure-eight uses almost none, because its bodies never come closer than 0.69 and it is a published exact solution that softening would move.

The solar system runs in AU, years and solar masses, where G = 4π² and Kepler's third law reads `T² = a³` with no constant at all.

## Verification

Checked against a NumPy reference step for step, and then against the things only a correct symplectic integrator can do:

- the figure-eight (Chenciner–Montgomery initial conditions) returns to its start after its published period 6.32591398, to within 3e-8, with energy drift 5e-14;
- measured orbital periods match `a^1.5` to 2e-4 across six radii;
- the energy error stays in a band over 60,000 steps while an explicit-Euler control on the same system climbs monotonically to 31% — the test is run against both, because a check nothing can fail is not a check.

## Run it locally

A static site. No build step, no package manager, no dependencies:

```
git clone git@github.com:slippylabs/orbit-lab.slippylabs.com.git
cd orbit-lab.slippylabs.com
python3 -m http.server 8000
```

Then open <http://localhost:8000>.

---

Part of [Slippy Labs](https://slippylabs.com). Every tool is indexed at
[projects.slippylabs.com](https://projects.slippylabs.com).
