# Claims matrix

Use this table in the README, release notes, presentation slides, paper drafts, and social posts.

| Claim | Status | Recommended wording |
|---|---|---|
| Low QFI was observed in the model | Allowed when raw data support it | “The reduced channel produced per-probe QFI below the configured 0.020 rad⁻² limit.” |
| Low classical observability was observed | Allowed when raw data support it | “Noise-aware capture and shadow observables remained below their declared limits in the configured run.” |
| Both were low simultaneously | Strongest current claim | “The run achieved simultaneous below-threshold quantum and classical observability in the configured reduced-order benchmark.” |
| The result was sampled across the visible interval | Allowed with node count and gate result | “All enabled checks passed at the declared endpoint-inclusive sampled wavelengths.” |
| The result was broadband | Use only with “sampled” and the interval | “Sampled visible-band robustness over 380 to 780 nm at the configured quadrature nodes.” |
| The result was omnidirectional | Use only as a source-model statement | “A 96-direction equal-solid-angle all-sky intensity quadrature was used.” |
| The object was invisible | Not supported | Replace with “below-threshold modeled observability.” |
| A metamaterial cloak was demonstrated | Not supported physically | Replace with “programmable effective-medium cloaking benchmark.” |
| Quantum invisibility was achieved | Not supported | Replace with “low QFI in the implemented reduced quantum channel.” |
| Maxwell equations were solved | False | State explicitly that this is not FDTD or FEM. |
| A real material was validated | False without calibration | State that named material constants are illustrative comparator priors. |
| A universal proof was obtained | False | State that the tests are finite, sampled, and model-specific. |

## Gold-standard abstract sentence

> We present a reduced-order programmable-metamaterial control benchmark that jointly minimizes a per-probe quantum Fisher information metric and noise-aware classical capture and shadow observables under explicit spectral, energy, degradation, and sampled wave-channel constraints.

## Gold-standard result sentence

> In the reported simulation run, the decisive per-probe QFI, threshold-normalized capture, and threshold-normalized shadow channels remained below their declared boundaries for the stated interval and seed; this result is a model-specific dual-observability suppression result, not experimental invisibility.

## Gold-standard limitation sentence

> The framework does not solve full-vector Maxwell equations, retrieve constitutive tensors from a measured specimen, prove continuous wavelength-angle robustness, or certify a fabricated cloak.

## Language ladder

From most conservative to most promotional while remaining defensible:

1. “Reduced-order dual-observability control benchmark.”
2. “Simulated programmable-metamaterial observability suppression.”
3. “Sampled dual-observability cloaking benchmark.”
4. “OBSCURA simulated cloaking laboratory.”

Do not climb past this ladder into “invisibility achieved.” That would turn a sharp scientific result into a fog machine.
