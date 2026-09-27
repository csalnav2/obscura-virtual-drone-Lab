# Scientific scope and interpretation

## Executive statement

OBSCURA is a reduced-order control, visualization, and verification sandbox. It combines stochastic rigid-body motion, an effective rank-3 optical coupling tensor, Monte Carlo photon packets for visualization and detector surrogates, sampled paraxial wave diagnostics, a reduced quantum channel, and projected SPSA control.

It is not a full-wave Maxwell solver, a constitutive-retrieval pipeline, a quantum-electrodynamics simulator, a fabricated-material certificate, or an experimental demonstration of invisibility.

## What low QFI means here

The modeled output density matrix depends on a scalar phase-like parameter \(\phi\). QFI quantifies the local distinguishability of nearby values of that parameter:

\[
F_Q(\phi)=\sum_{i,j:\lambda_i+\lambda_j>0}
\frac{2\left|\langle i|\partial_\phi\rho|j\rangle\right|^2}
     {\lambda_i+\lambda_j}.
\]

Low QFI therefore means the implemented reduced output state is locally insensitive to the chosen parameter. It does **not** mean that every quantum measurement, every parameterization, every input state, or every external detector is unable to distinguish the object.

## What low classical observability means here

The classical gate combines:

- A noise-aware capture ratio relative to the modeled bare object.
- A shadow-error constraint.
- Detector-weighted scattering and absorption checks.

Low capture with high absorption is not sufficient, because a black object can remove signal while remaining conspicuous through shadow and energy loss. The controller includes explicit absorption and throughput guards to reduce that loophole.

## What “dual observability” means

The realized state is considered dual-below only when both quantum and classical conditions pass, and the broader sampled-robust gate is true:

\[
\begin{aligned}
&F_{Q,\mathrm{measured}} \le F_{Q,\mathrm{lim}},\\
&\max_\lambda F_Q(\lambda) \le F_{Q,\mathrm{lim}},\\
&C_\mathrm{measured} \le C_\mathrm{lim},\\
&\max_\lambda C(\lambda) \le C_\mathrm{lim},\\
&\max_\lambda S(\lambda) \le S_\mathrm{lim},\\
&\text{sampled-robust gate}=\mathrm{true}.
\end{aligned}
\]

The sampled-robust gate additionally checks detector scattering, detector absorption, declared material validity intervals, and applicable wave/channel thresholds.

## What can be claimed now

- The code implements a reproducible reduced-order benchmark for simultaneous suppression of modeled quantum and classical observability.
- Thresholds, energy partition, source power, spectral nodes, material state, noise, optimizer decisions, and lifecycle telemetry are explicit and exportable.
- The optimizer uses common frozen candidate conditions and a guard that can retain the unchanged state.
- A state may be described as “below threshold in the configured simulation” when the exported raw data support that statement.
- A state may be described as “sampled robust” only when the source's full implemented gate returns true.

## What cannot be claimed from this code alone

- Physical invisibility.
- Experimental invisibility.
- Continuous-angle or continuous-spectrum cloaking.
- Full-vector Maxwell validation.
- Fabricability or material stability at the displayed macroscopic scale.
- Universal low distinguishability against arbitrary measurements or probe states.
- A measured QFI result.
- A material-specific certificate for graphene, GO/rGO, hBN, or ITO.

## Confidence labels

| Claim | Confidence | Reason |
|---|---|---|
| The source implements the stated reduced-order equations | High | Directly inspectable in code and protected by deterministic checks |
| The dashboard can show below-threshold modeled observables | High, conditional on raw run data | Threshold logic is explicit; a particular run still needs its exports |
| The optimizer caused improvement rather than a favorable seed | Medium until paired multi-seed statistics are archived | Paired banks exist, but publication needs effect sizes and confidence intervals |
| The result predicts a Maxwell solver | Low until cross-solver comparison | The model is intentionally not a Maxwell solver |
| The result predicts a fabricated sample | Low until calibration and experiment | Material constants are illustrative and sample-independent |
