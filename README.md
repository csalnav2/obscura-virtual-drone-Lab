# OBSCURA → Virtual Drone Lab

**Programmable observability research, extended into an interactive multi-agent 3-D simulation.**

Christopher Salnave · Public showcase and collaboration portal · Updated September 27, 2026

OBSCURA began as a reduced-order metamaterial/dual-observability simulation.
The Virtual Drone Lab extension brings that research direction into a moving
3-D environment: five agents, changing observation geometry, weather-dependent
perturbations, on-demand simulated cloaking and shared external skill memory.

The full drone application is a private research implementation. This repository
contains public descriptions, standard mathematics, the earlier OBSCURA
showcase, and an independently authored **synthetic preview**. It does not
contain the complete drone notebook, optical control engine or learned memory.

> **Scientific scope:** classical observability and quantum Fisher information
> are evaluated within declared reduced models. A disappearing rendered drone
> is a visual effect, not proof of physical invisibility, universal quantum
> undetectability or validated metamaterial fabrication.

## What the full drone application supports

| Capability | Meaning and boundary |
|---|---|
| Moving 3-D observer geometry | Pose-dependent, sampled optical observations during virtual flight; finite samples do not cover every physical observer. |
| Weather-aware SPSA | Bounded adaptation of a reduced optical surrogate, with guarded comparisons; not the complete original private OBSCURA optimizer. |
| Rain, fog, dust and noise | Configured perturbation levels **0, 25, 50, 75 and 100%**; scenario settings, not guaranteed suppression or calibrated storm severity. |
| Mid-flight changes | Weather can change immediately; landscape replacement uses collision/clearance checks and may adjust placement or reject an unsafe transition. |
| Cloak / reveal | Voice, chat and dashboard controls; requests remain subject to energy, cooldown and runtime state. “Reveal” and “deactivate cloak” restore the uncloaked state. |
| Conversation handoffs | Switch agents while retaining the selected camera mode and zoom in Fleet mode. Private conversations use separate sessions. |
| Camera views | Rear, side, above, FPV and orbit views, with zoom and selected-drone inspection. |
| Meta^n-inspired layers | Bounded, prescribed strategy/reflection layers with recorded provenance; not autonomous code rewriting or model-weight self-improvement. |
| HyperSkill-inspired memory | Hyperedges link subtasks, skills, failures and trajectory evidence, allowing agents to retrieve lessons with source-agent attribution. |
| PCA via SVD | Moving low-dimensional representations of each drone's observed state; compression and explained variance remain explicit. |
| Quaternion rotors | Compass-style latent-direction and physical-attitude diagnostics. The latent display does **not** steer the drone. |

See [capabilities and evidence](docs/DRONE_LAB_CAPABILITIES.md) for the distinction
between implemented behavior, reported demo observations and unverified claims.

## Run the public preview

Requires Python **3.11+** and a modern browser. **No Python packages, API keys or
accounts are required.** From a checkout containing this revision:

```bash
git clone https://github.com/csalnav2/obscura-metamaterial-observability-lab.git
cd obscura-metamaterial-observability-lab
python preview/preview.py --output preview-output --seconds 30 --fps 20 --seed 2026
```

Open `preview-output/index.html` directly in your browser. It is self-contained
and can run offline. Choose a drone and camera view; change weather appearance,
intensity and landscape palette; use **Preview cloak** / **Reveal**.

This is a clearly labeled storyboard with analytic trajectories and a moving
observer. It does **not** run SPSA, quantum optics, a flight controller, live voice,
Meta^n or learned hypergraph memory. Its fade control demonstrates the UI idea;
it does not compute an observability score. Its landscape selector changes the
palette, not terrain geometry. See [preview instructions](preview/README.md).

The existing [OBSCURA public showcase](obscura_public_showcase.html) and
[overview image](obscura_public_overview.png) remain available. They belong to
the earlier project and are not independent validation of the drone extension.

## Mathematical overview

Standard SPSA estimates all components of a gradient from a pair of perturbed
objective evaluations, with $\Delta_{k,i}\in\{-1,+1\}$:

$$
\widehat g_{k,i}=
\frac{J(\theta_k+c_k\Delta_k;\xi_k)-J(\theta_k-c_k\Delta_k;\xi_k)}
{2c_k\Delta_{k,i}},\qquad
\theta_{k+1}=\Pi_{\Theta}(\theta_k-a_k\widehat g_k).
$$

Using the same sampled disturbance $\xi_k$ for candidate comparisons helps
separate a parameter change from a change in weather or pose. Acceptance guards
and separate evaluation runs are still necessary.

For centered observations, PCA via SVD is

$$X_c=U\Sigma V^T,\qquad Z=X_cV_r.$$

For a reduced density operator $\rho(\phi)=\sum_i\lambda_i|i\rangle\langle i|$,

$$
F_Q(\phi)=2\sum_{i,j:\lambda_i+\lambda_j>0}
\frac{|\langle i|\partial_\phi\rho|j\rangle|^2}{\lambda_i+\lambda_j}.
$$

Low QFI means low local sensitivity to the **declared parameter** in that model.
It is not a universal object-detection result. Capture, shadow, throughput and
state distinguishability need separate checks.

[Mathematics, units and assumptions](docs/DRONE_LAB_MATH.md) explains observer
geometry, atmospheric attenuation, flight dynamics, SPSA, PCA, quaternion
transport and external hypergraph memory without disclosing private calibration
maps, objective weights or the complete engine.

## Validation

```bash
python -m unittest discover -s tests -v
node tests/test_preview_viewer.cjs
python audit_public_release.py .
```

Node is optional for running the preview; it is used by the viewer checks.
The public tests verify the synthetic generator, geometry, bounded controls,
provenance and packaging. They do not benchmark the private optics engine.

The separate private v0.19.12 release reported **373 automated regression checks
passed**, including mocked voice providers and deterministic browser harnesses.
That is a software regression result, not a physical cloaking experiment, a live
microphone test or an independently reproduced public performance benchmark.

[Validation plan](docs/DRONE_LAB_VALIDATION.md) specifies paired baselines,
held-out disturbances, weather sweeps and convergence checks before making
quantitative suppression claims. No successful 100%-perturbation cloak is claimed.

## Collaborate / request private access

The **only offered route to the complete unpublished drone implementation is an
approved collaboration**. Start with [COLLABORATION.md](COLLABORATION.md), then
open a [collaboration request](https://github.com/csalnav2/obscura-metamaterial-observability-lab/issues/new?template=collaboration.yml).
Describe a concrete research question, contribution, deliverable and minimum
access requirement. A request does not automatically grant access.

Initial access can be a sanitized dataset, hosted interface or a specific module.
Full-source access requires explicit approval and agreed scope and terms. Do not
post credentials, private datasets or confidential proposals in a public issue.

## Public/private boundary and license

The repository's existing [LICENSE](LICENSE) is **Apache License 2.0**. This
revision retains that license for the public materials; collaboration approval
is not required to exercise the rights granted for those public files.

The unpublished drone engine is **not included in this public release**. Its
access policy applies to material not distributed here. A notice, a draft PR,
a hidden branch name or deletion after a push cannot keep public bytes private.
Use a separate private repository for the full implementation.

The earlier README's blanket “all rights reserved / not open source” wording
conflicted with the existing LICENSE; this revision removes that contradiction.
See [release boundaries](docs/PUBLIC_PRIVATE_BOUNDARY.md).

## References and attribution

See [the annotated bibliography](REFERENCES.md) and [BibTeX entries](references.bib)
for nine verified method references: SPSA (Spall, 1992/1998), transformation optics
(Pendry et al., 2006), QFI (Braunstein and Caves, 1994), atmospheric visibility
(Narasimhan and Nayar, 2002), PCA–SVD (Jolliffe and Cadima, 2016), quaternion
interpolation (Shoemake, 1985), Meta^n (Kim et al., 2026) and HyperSkill
(Xu et al., 2026). The two agent papers are cited as preprints and inspiration
for adapted components, not as reproduced benchmark results.

[Original OBSCURA plot mathematics](PLOTS_AND_MATH.md) and
[scientific scope](SCIENTIFIC_SCOPE.md) describe the earlier reduced model.
The references establish background methods; simulation-specific performance
requires its own experiment records and independent evaluation.

Copyright © 2026 Christopher Salnave. Public repository materials are distributed
under the existing Apache-2.0 license. See [CITATION.cff](CITATION.cff).
