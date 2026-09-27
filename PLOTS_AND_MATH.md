# OBSCURA plots and mathematics, step by step

This guide follows the current v16.45 source. It distinguishes three things that are easy to blur together:

1. **Mounted graph components** in the active Dash layout.
2. **Precomputed media composites** shown in media mode.
3. **Additional figure builders** that exist in source but are not mounted in the current layout.

The distinction matters. A function existing in the file does not mean the user currently sees it.

---

# Part I. Plot inventory

## A. Mounted graph components: 11 total

| # | Graph | Source builder | What it answers |
|---:|---|---|---|
| 1 | Optimizer comparison | `optimizer_bank_figure` | Does the selected SPSA policy improve QFI, classical observability, concealment index, and throughput relative to the baseline? |
| 2 | Degradation lifecycle | `degradation_figure` or precomputed variant | Does optical exposure or environment damage the material, and what does healing cost? |
| 3 | Photon scene | `photon_scene_figure` or orthographic fallback | Where are the moving cube, focus cube, eye, source, and display packets? |
| 4 | Rank-3 tensor | `tensor_figure` or slice fallback | How do all 27 world-frame coupling coefficients evolve? |
| 5 | Photon topology | `topology_figure` | What is the one-scale mutual-kNN flag complex of the sampled packet cloud? |
| 6 | Cube pose | `pose_figure` | How does the cube translate and rotate? |
| 7 | Retinal projection | `retinal_projection_figure` | Where do display packets project onto the eye focal plane, and how blurred are they? |
| 8 | Eye focus | `focus_figure` | Is the modeled accommodation following the target and depth-of-field band? |
| 9 | Material trajectory | `material_figure` | What material coordinates, cloak quality, and signed-index counterfactual are active? |
| 10 | Dispersion and tensor diagnostics | `dispersion_figure` | How broad and anisotropic is the scattered cloud, and how strong is the tensor contraction? |
| 11 | Controller diagnostics | `controller_figure` | What are the objective, optimizer gradient and step, optical response, and noise budget? |

## B. Media-mode composites

In media mode, the large interactive live grids are hidden and synchronized precomputed media are displayed.

### Hero movie

The hero composite combines:

- Projected 3-D scene.
- Eye-plane projection.
- Decisive QFI trace.
- Threshold-normalized capture and shadow traces.
- Derived invisibility index and capture SNR.

### Diagnostics atlas

The 12 retained panels are:

1. Cube translation.
2. Smooth cube tilt.
3. Eye focus depth.
4. Accommodation and focus error.
5. Constitutive state.
6. Signed-index cloak comparison.
7. Active phase control.
8. Photon dispersion.
9. Rank-3 coupling.
10. Controller objective.
11. Rank-3 tensor slices.
12. Detector-noise budget.

### Topology movie

A synchronized movie renders the bounded 64-node packet flag complex with nodes, edges, triangular 2-simplices, and finite Betti numbers.

## C. Source-defined but not currently mounted

- `visibility_figure`
- `wave_audit_figure`
- `wavefront_figure`
- `wavelength_audit_figure`

These are scientifically useful and should be mounted later under an “Audit” tab rather than described as current visible panels.

---

# Part II. Plot-by-plot interpretation

## 1. Optimizer comparison

**Source:** [`optimizer_bank_figure`](../obscura.py#L39646-L40140)

### What is plotted

- Decisive QFI for the baseline and selected optimizer.
- Maximum threshold-normalized classical observability:

  \[
  O_C(t)=\max\!\left(
  \frac{C_{\max}(t)}{C_{\mathrm{lim}}},
  \frac{S_{\max}(t)}{S_{\mathrm{lim}}}
  \right).
  \]

- Derived invisibility index.
- Throughput.

The QFI shown for decision purposes is the conservative maximum of measured QFI and spectral worst-case QFI.

### How to read it

- QFI below its dotted line is a quantum-channel pass.
- Classical observability below 1 is a joint capture-shadow pass.
- High invisibility index is favorable, but it is a derived score and never replaces the raw gates.
- Throughput must remain high enough that concealment is not obtained by destroying the probe.

### Important display rule

The red traces use a strictly causal two-pole display filter. Raw optimizer-bank JSON and CSV values are unchanged. Smoothed visual curves are presentation, not evidence.

### Claim boundary

This is a paired simulation comparison, not proof that MSGA is universally superior to SPSA.

---

## 2. Degradation and lifecycle

**Source:** [`degradation_figure`](../obscura.py#L21292-L21892)

### Four rows

1. **Committed damage:** oxidation state \(D\), with a configured reference.
2. **Spectral loss:** detector-weighted scattering and absorption with limits.
3. **Exposure and healing:** source, intercepted, absorbed, nonlinear, and healing powers.
4. **Damage kinetics:** net damage rate and accumulated damage dose.

### Core kinetics

The state follows a bounded formation-healing model:

\[
\frac{dD}{dt}=k_f(1-D)-k_hD,
\]

where

\[
k_f=k_{\mathrm{photo}}+k_{\mathrm{environment}}.
\]

With rates held over a solver step, the code uses the exact update:

\[
D_{n+1}=D_\infty+(D_n-D_\infty)
e^{-(k_f+k_h)\Delta t},
\qquad
D_\infty=\frac{k_f}{k_f+k_h}.
\]

Photo-oxidation scales with absorbed irradiance:

\[
k_{\mathrm{photo}}\propto
\left(\frac{I_{\mathrm{abs}}}{I_{\mathrm{ref}}}\right)^p.
\]

Healing power is quadratic in the bounded healing command:

\[
P_h=P_{h,\max}u_h^2.
\]

### How to read it

A visually excellent low-observability state is not useful if damage climbs rapidly or healing energy explodes. This plot makes the control problem multi-objective rather than purely cosmetic.

### Claim boundary

The lifecycle rates are accelerated configurable surrogates, not measured lifetime predictions.

---

## 3. Photon scene

**Source:** [`photon_scene_figure`](../obscura.py#L19654-L20017)

### Visual elements

- Outer programmable cloak cube.
- One solid inscribed focus cube.
- Downstream eye.
- Nine geometric observer-coverage vectors.
- Incident, internal, transmitted, and scattered display packets.
- Directional emitter when a laser source is selected.
- Optional eye focal plane and frustum.

### What is simulated versus visualized

The packet cloud is a Monte Carlo visualization and detector surrogate. Packet speed is deliberately slowed. The nine eye-to-cube lines are coverage vectors, not literal photon worldlines. Teal packets are post-exit transmitted energy, not reflection.

### Signed-index interface rule

For unit incident direction \(\mathbf i\), interface normal \(\mathbf n\), and signed refractive indices \(n_1,n_2\), the code isolates the incident tangent:

\[
\mathbf t_i=\mathbf i+\cos\theta_i\,\mathbf n,
\qquad
\cos\theta_i=-\mathbf i\cdot\mathbf n.
\]

It then applies

\[
\mathbf t_t=\frac{n_1}{n_2}\mathbf t_i,
\qquad
k=1-\|\mathbf t_t\|^2.
\]

If \(k\ge 0\),

\[
\mathbf d_t=\mathbf t_t-\sqrt{k}\,\mathbf n.
\]

If \(k<0\), the ray is reflected. Negative \(n_2\) reverses the tangential component and selects the negative-refraction branch.

### Claim boundary

This is a signed-ray surrogate, not a Maxwell boundary-value solution.

---

## 4. Rank-3 tensor lattice or slices

**Sources:** [`tensor_figure`](../obscura.py#L20020-L20098), [`tensor_slices_fallback_figure`](../obscura.py#L20394-L20432)

### What is plotted

All 27 coefficients of the world-frame effective coupling tensor \(T_{ijk}\), either as a 3-D lattice or as the three \(k=x,y,z\) heatmap slices.

### Rotation law

The body-frame tensor is rotated with the cube:

\[
T^{\mathrm{world}}_{ijk}
=R_{ia}R_{jb}R_{kc}T^{\mathrm{body}}_{abc}.
\]

The displayed Frobenius norm is

\[
\|T\|_F=\sqrt{\sum_{ijk}T_{ijk}^2}.
\]

A directional contraction used elsewhere is

\[
\mathbf v_T=T:\mathbf d\otimes\mathbf d,
\qquad
(v_T)_i=T_{ijk}d_jd_k.
\]

### How to read it

Tensor sign and magnitude represent the effective model's anisotropic coupling, not a measured third-order susceptibility tensor.

---

## 5. Photon flag-complex topology

**Source:** [`photon_flag_complex`](../obscura.py#L19089-L19355) and [`topology_figure`](../obscura.py#L19358-L19538)

### Construction

- At most 64 packet nodes.
- Four nearest neighbors per node.
- Edge only when neighborhood is mutual and distance is at most \(\epsilon\).
- Default \(\epsilon=0.28\) times the cube side.
- Maximum 128 edges and 128 triangles.
- Triangles are filled 3-cliques, or 2-simplices.

### Betti numbers

Connected components give

\[
\beta_0=\text{number of connected components}.
\]

For the finite flag complex,

\[
\beta_1=|E|-|V|+\beta_0-
\operatorname{rank}_{\mathbb F_2}(\partial_2).
\]

### How to read it

- \(\beta_0\) counts disconnected packet groups.
- \(\beta_1\) counts independent one-dimensional loops after filled triangles are removed from the cycle count.

### Claim boundary

These are exact Betti numbers for one finite, capped, one-scale complex. They are not persistent homology estimates or continuum topological invariants.

---

## 6. Cube pose

**Source:** [`pose_figure`](../obscura.py#L23177-L23200)

### What is plotted

- Translation \(x,y,z\) in metres.
- Roll, pitch, and yaw in degrees.

### Exact Ornstein-Uhlenbeck target update

For a target process with mean \(\mu\), relaxation time \(\tau\), diffusion scale \(\sigma\), and normal sample \(\xi\):

\[
X_{n+1}=\mu+e^{-\Delta t/\tau}(X_n-\mu)
+\sigma\sqrt{\frac{\tau}{2}\left(1-e^{-2\Delta t/\tau}\right)}\,\xi.
\]

### Exact critically damped follower

The follower solves

\[
\ddot x+2\omega\dot x+\omega^2(x-x_\star)=0
\]

under a held target. Let \(y=x-x_\star\) and \(c=v+\omega y\). Then

\[
x_{n+1}=x_\star+(y+c\Delta t)e^{-\omega\Delta t},
\]

\[
v_{n+1}=(v-\omega c\Delta t)e^{-\omega\Delta t}.
\]

This removes the explicit-Euler oscillation that previously appeared at the coarse optical cadence.

### Quaternion orientation

The body-frame angular increment is converted to a unit quaternion and multiplied into the current orientation. The result is renormalized, and the corresponding rotation matrix is used for geometry and tensor rotation.

---

## 7. Retinal projection

**Source:** [`retinal_projection_figure`](../obscura.py#L20434-L20544)

### What is plotted

Packet positions projected onto the eye's dynamic focal plane. Marker size and opacity depend on the blur radius relative to the accepted circle of confusion.

### Thin-lens relation

With object distance \(u\), retina distance \(v\), and focal length \(f\):

\[
\frac{1}{f}=\frac{1}{u}+\frac{1}{v},
\qquad
f=\left(\frac{1}{u}+\frac{1}{v}\right)^{-1}.
\]

### Defocus blur

For pupil radius \(a\), focus distance \(s\), object distance \(u\), and retina distance \(v\), the modeled retinal blur radius is

\[
r_b=a v\left|\frac{1}{s}-\frac{1}{u}\right|.
\]

### How to read it

A compact projection with small blur can still be detectable. The plot is geometric context, while the capture ratio and noise model determine the classical detection gate.

---

## 8. Eye focus and accommodation

**Source:** [`focus_figure`](../obscura.py#L20547-L20631)

### What is plotted

- Selected-target distance.
- Accommodated focus distance.
- Near and far depth-of-field limits.
- Accommodation demand in dioptres.
- Signed focus error.

### Depth-of-field criterion

The accepted blur radius \(c\) defines reciprocal-distance tolerance

\[
\delta=\frac{c}{av}.
\]

Then

\[
u_{\mathrm{near}}=\frac{1}{1/s+\delta},
\qquad
u_{\mathrm{far}}=\frac{1}{1/s-\delta},
\]

with the far value capped by the configured maximum distance when the denominator approaches zero.

### Terminology note

The plotted “accommodation [D]” is \(1/s\), best interpreted as object vergence or accommodation demand associated with the focus distance. It is not a complete biomechanical model of total ocular lens power.

---

## 9. Material trajectory

**Source:** [`material_figure`](../obscura.py#L23203-L23277)

### Three conceptual rows

1. Programmed and effective constitutive coordinates.
2. Signed-index cloak quality, residual scatter, and positive-index counterfactual.
3. Active cancellation, phase bias, and scatter-width controls.

### Normalized controller coordinates

The optimizer works in \(x_i\in[-1,1]\), mapped to physical bounds by

\[
\theta_i=\theta_{i,\min}
+\frac{x_i+1}{2}(\theta_{i,\max}-\theta_{i,\min}).
\]

### Cloak quality

The reduced transformation-optics quality score is a geometric product:

\[
Q_c=w_{-n}
M_n^{0.34}M_a^{0.24}M_\phi^{0.16}
M_Z^{0.18}M_L^{0.08},
\]

where the factors encode negative-index branch weight, index match, anisotropy match, phase match, impedance match, and low loss.

Residual scatter has the form

\[
\sigma_{\mathrm{res}}=
\sigma_{\mathrm{raw}}
(1-s_cQ_c)G_{\mathrm{size}},
\]

with a configured ideal suppression factor \(s_c\) and electrical-size factor \(G_{\mathrm{size}}\).

### Positive-index counterfactual

The source holds all other controls fixed and replaces the effective index with \(+|n|\). The difference

\[
\Delta\sigma=\sigma_{+n}-\sigma_{-n}
\]

exposes the sign-specific contribution inside this model. It is disabled as a fabricated comparison for named passive platforms.

### Material models

- Ideal active broadband benchmark.
- Generic anisotropic Drude-like effective medium.
- Finite-temperature graphene Kubo sheet mapped to an effective film.
- GO/rGO endpoint mixture.
- Uniaxial hBN TO-LO phonon model.
- ITO Drude ENZ film.

Named-material constants are illustrative and require sample-specific calibration.

---

## 10. Photon dispersion and tensor diagnostics

**Source:** [`dispersion_figure`](../obscura.py#L23280-L23302)

### What is plotted

- RMS radius of scattered packets.
- Square roots of the three covariance eigenvalues.
- Tensor Frobenius norm.
- Directional tensor-contraction norm.

### Covariance geometry

For scattered positions \(\mathbf r_p\) with centroid \(\bar{\mathbf r}\):

\[
\Sigma=\frac{1}{N}\sum_p
(\mathbf r_p-\bar{\mathbf r})
(\mathbf r_p-\bar{\mathbf r})^T.
\]

The RMS radius is related to the trace:

\[
r_{\mathrm{RMS}}=\sqrt{\operatorname{tr}\Sigma}.
\]

The principal radii are \(\sqrt{\lambda_1},\sqrt{\lambda_2},\sqrt{\lambda_3}\).

### How to read it

Large anisotropy in the covariance spectrum means scattering is stretched along preferred directions. It does not by itself diagnose invisibility.

---

## 11. Controller and detector-noise diagnostics

**Source:** [`controller_figure`](../obscura.py#L23305-L23377)

### What is plotted

- Objective \(J\).
- Invisibility index.
- Optimizer gradient norm.
- Accepted step norm.
- Residual scatter, positive-index counterfactual, cloak quality, throughput, and shadow.
- Shot, thermal, residual quantum, and total count-noise scales.

### Noise model

For expected signal \(N_s\):

\[
\sigma_{\mathrm{shot}}=\sqrt{F N_s},
\]

with Fano factor \(F\). Thermal background includes dark count, a fixed background, and a signal-dependent term. A residual quantum-noise floor scales with phase and dephasing contrasts. Independent standard deviations combine as

\[
\sigma_{\mathrm{tot}}=
\sqrt{\sigma_{\mathrm{shot}}^2+
      \sigma_{\mathrm{thermal}}^2+
      \sigma_{\mathrm{quantum}}^2}.
\]

The significance-aware capture estimate subtracts

\[
N_{\mathrm{sig,lim}}=z\sigma_{\mathrm{tot}}
\]

from the noisy background-subtracted signal before normalizing by the bare reference count. The default \(z\) is 3.

### How to read it

A shrinking objective with a growing gradient norm can indicate unstable optimization. A low capture ratio with rising shadow or falling throughput is not a successful cloak.

---

# Part III. Additional audit figures in source

## 12. Standalone dual-visibility atlas

**Source:** [`visibility_figure`](../obscura.py#L20634-L20958)

### Row 1: decisive QFI

\[
F_{Q,\mathrm{decisive}}(t)=
\max\left(F_{Q,\mathrm{measured}}(t),
          F_{Q,\mathrm{spectral\ max}}(t)\right).
\]

The pass line is the raw configured QFI threshold. The axis is nonnegative and linear.

### Row 2: exactly two classical traces

\[
\widetilde C(t)=
\frac{\max(C_{\mathrm{measured}},C_{\mathrm{spectral\ max}})}
     {C_{\mathrm{lim}}},
\]

\[
\widetilde S(t)=
\frac{\max(S_{\mathrm{measured}},S_{\mathrm{spectral\ max}})}
     {S_{\mathrm{lim}}}.
\]

The common boundary is 1.

### Row 3: derived index and SNR

Define

\[
q=F_Q/F_{Q,\mathrm{lim}},\quad
c=C/C_{\mathrm{lim}},\quad
s=S/S_{\mathrm{lim}},\quad
\nu=\mathrm{SNR}.
\]

The source computes severity

\[
\chi=\frac{1}{2}\sqrt{q^2+c^2+s^2+\nu^2}
\]

and invisibility index

\[
I=\frac{1}{1+\chi}.
\]

This index is monotone and bounded, but it is a dashboard composite, not a new physical observable.

---

## 13. Wave audit atlas

**Source:** [`wave_audit_figure`](../obscura.py#L22295-L22416)

### Row 1: field and polarization

- Field NMSE.
- Phase RMSE.
- Polarization leakage.

### Row 2: spatial modes and OAM

- Modal crosstalk.
- OAM infidelity.
- OAM resolved closure.

### Row 3: quantum distinguishability

- Selected-probe trace distance.
- Sampled-probe maximum trace distance.
- Normalized-Choi distance.

For the default all-sky incoherent source, a single coherent field, global phase, modal state, and OAM state are not defined. The panel correctly labels those metrics not applicable unless a directional paraxial probe is selected.

---

## 14. Wavefront or angular audit

**Source:** [`wavefront_figure`](../obscura.py#L22419-L22866)

### Directional coherent branch

Displays six detector-plane maps, including free and cloak intensity, relative phase, and cross-polarized intensity.

### Incoherent all-sky branch

Displays an angular observability surrogate over arrival longitude and latitude. It explicitly does not invent a global complex phase for an incoherent intensity ensemble.

### Claim boundary

The field grid resolves a slowly varying paraxial envelope, not the optical carrier and not a full vector Maxwell field.

---

## 15. Rolling wavelength audit

**Source:** [`wavelength_audit_figure`](../obscura.py#L22869-L23174)

### Heatmap 1: observability severity

\[
\chi_O(t,\lambda)=\max\left(
\frac{F_Q}{F_{Q,\mathrm{lim}}},
\frac{C}{C_{\mathrm{lim}}},
\frac{S}{S_{\mathrm{lim}}}
\right).
\]

### Heatmap 2: detector-loss severity

\[
\chi_L(t,\lambda)=\max\left(
\frac{\text{scattering}}{\text{scattering}_{\mathrm{lim}}},
\frac{\text{absorption}}{\text{absorption}_{\mathrm{lim}}}
\right).
\]

Values at or below 1 pass the corresponding sampled node.

---

# Part IV. Core mathematical pipeline

## 1. State and units

The simulation uses SI units for position, time, and angles. The default native optical/control step is 0.40 s. The browser may render causally densified traces at higher visual cadence without changing raw solver samples.

## 2. Spectral quadrature

Compact optical bands use endpoint-inclusive Legendre-Gauss-Lobatto nodes. Wide far-IR and microwave intervals use trapezoidal integration in log-frequency. Weights are normalized, so increasing the number of nodes does not multiply source power.

A robust scalar is generally blended between weighted mean and maximum:

\[
y_{\mathrm{robust}}=
\bar y+w_{\max}(y_{\max}-\bar y).
\]

Importantly, nonlinear penalties are evaluated at each node before aggregation, preventing a narrow failure from disappearing inside an average.

## 3. Structured beams

### Gaussian TEM00

\[
I(r,z)=\frac{2}{\pi w(z)^2}
\exp\left[-\frac{2r^2}{w(z)^2}\right].
\]

### Laguerre-Gaussian

The source uses the normalized radial factor

\[
I_{p\ell}\propto
\left(\frac{2r^2}{w^2}\right)^{|\ell|}
\left[L_p^{|\ell|}\!\left(\frac{2r^2}{w^2}\right)\right]^2
e^{-2r^2/w^2}.
\]

The phase includes \(e^{i\ell\varphi}\), so the target OAM is \(\ell\hbar\) per photon in the model.

### Hermite-Gaussian

\[
E_{mn}\propto
\psi_m\!\left(\sqrt{2}y/w\right)
\psi_n\!\left(\sqrt{2}z/w\right).
\]

### Finite Bessel-Gaussian

\[
E_{\mathrm{BG0}}(r)=
N J_0(ur/w)e^{-r^2/w^2}.
\]

The Gaussian envelope makes the mode finite-energy. It is not an ideal infinite Bessel beam.

## 4. Coherence model

The Gaussian-Schell coherence kernel is represented through the identity

\[
e^{-\|\Delta\mathbf r\|^2/(2\xi^2)}
=\mathbb E_{\mathbf k}
\left[e^{i\mathbf k\cdot\Delta\mathbf r}\right],
\qquad
\mathbf k\sim\mathcal N(0,I/\xi^2),
\]

approximated by a finite positive Gauss-Hermite quadrature. Retained coherent modes are propagated separately and combined incoherently.

## 5. Energy partition

Every sampled material response checks

\[
R+A+S+T=1.
\]

Here \(R\) is reflection, \(A\) absorption, \(S\) diffuse scattering, and \(T\) forward transmission. This is one of the most important anti-cheat constraints in the project.

## 6. Capture model

The anisotropic Henyey-Greenstein phase density is

\[
p(\mu;g)=
\frac{1-g^2}
{4\pi(1+g^2-2g\mu)^{3/2}}.
\]

The capture ratio is approximately

\[
C=\sigma_{\mathrm{res}}
\frac{p(\mu;g)}{p(\mu;g_{\mathrm{bare}})}
\left(1+0.24\|T:\mathbf d\otimes\mathbf d\|\right).
\]

Bare pupil capture probability is

\[
p_{\mathrm{bare}}=
\eta_{\mathrm{retina}}
\frac{A_{\mathrm{pupil}}}{4\pi r^2}
f_{\mathrm{intercept}}.
\]

Expected detected signal is

\[
N_s=N_{\mathrm{source}}p_{\mathrm{bare}}C.
\]

## 7. Reduced quantum state

The channel uses a vacuum or erasure state plus two retained optical modes:

\[
\rho(\phi)=
\begin{pmatrix}
1-\eta & 0 & 0\\
0 & \eta/2 & \eta c\\
0 & \eta c^* & \eta/2
\end{pmatrix},
\]

with

\[
\eta(\phi)=
\operatorname{sigmoid}
\left(\operatorname{logit}\eta_0-\ell\phi\right),
\]

\[
V(\phi)=V_0e^{-d\phi^2},
\qquad
c(\phi)=\frac{V(\phi)}{2}e^{-i\kappa\phi}.
\]

The parameters are derived from throughput, loss contrast, coherence visibility, dephasing contrast, and phase coupling.

## 8. Quantum Fisher information

With \(\rho=\sum_i\lambda_i|i\rangle\langle i|\),

\[
F_Q=\sum_{i,j}
\frac{2|\langle i|\partial_\phi\rho|j\rangle|^2}
     {\lambda_i+\lambda_j},
\]

omitting terms with a numerically zero denominator. The derivative is either centered finite difference

\[
\partial_\phi\rho\approx
\frac{\rho(\phi+h)-\rho(\phi-h)}{2h}
\]

or JAX automatic differentiation.

For unresolved mixtures of genuinely distinct wavelength or source channels, density matrices are mixed first and QFI is calculated from the mixture. Identical angular nodes do not receive an artificial \(1/\sqrt N\) penalty.

## 9. Sampled vector-wave comparison

Let \(\mathbf E_f\) be the free-space reference and \(\mathbf E_c\) the cloak output.

Field overlap fidelity:

\[
F_E=\frac{|\langle\mathbf E_f,\mathbf E_c\rangle|^2}
{\|\mathbf E_f\|^2\|\mathbf E_c\|^2}.
\]

Field NMSE:

\[
\mathrm{NMSE}_E=
\frac{\|\mathbf E_c-\mathbf E_f\|^2}
     {\|\mathbf E_f\|^2}.
\]

Phase RMSE is weighted only where free and cloak fields have common optical support, avoiding meaningless phase at zero amplitude.

The polarization coherency matrix is normalized by detected power. For input Jones vector \(\mathbf e\),

\[
F_P=\mathbf e^\dagger J\mathbf e,
\qquad
L_P=1-F_P.
\]

Modal powers are squared projections onto a finite orthonormal basis. OAM is estimated through radial-complete azimuthal Fourier decomposition plus an unresolved bin.

## 10. Single-probe quantum distinguishability

Let \(\eta_f\) and \(\eta_c\) be free and cloak detected powers, and let \(\mu\) be the magnitude of normalized field overlap. The finite chosen-state fidelity is

\[
F_\rho=\left[
\sqrt{(1-\eta_f)(1-\eta_c)}+
\sqrt{\eta_f\eta_c}\,\mu
\right]^2.
\]

The corresponding trace distance used by the source is

\[
D=\frac{1}{2}\left[
|\eta_c-\eta_f|+
\sqrt{(\eta_c+\eta_f)^2-4\eta_c\eta_f\mu^2}
\right].
\]

Equal-prior Helstrom error is

\[
P_e=\frac{1-D}{2}.
\]

A retained finite Galerkin channel also produces normalized-Choi distance, sampled-probe distance, and finite-dimensional lower and upper bounds on diamond distance. These are truncated-channel diagnostics, not a continuous-mode diamond norm.

## 11. Two-photon loss

For a directional pulsed source, the reduced exponent is

\[
\Gamma_2=
\beta L_{\mathrm{eff}}
\frac{\langle I^2\rangle}{\langle I\rangle},
\]

and nonlinear absorptance is

\[
A_2=1-e^{-\Gamma_2},
\]

capped by configuration. The term is disabled for all-sky incoherent and microwave cases. Its coefficient remains illustrative unless calibrated.

## 12. SPSA

Gain schedules:

\[
a_k=\frac{a}{(A+k)^\alpha},
\qquad
c_k=\frac{c}{k^\gamma}.
\]

Bernoulli perturbation:

\[
\Delta_i\in\{-1,+1\}.
\]

Two-evaluation gradient estimate:

\[
\widehat{\mathbf g}_k=
\frac{J(\mathbf x_k+c_k\Delta_k)-
      J(\mathbf x_k-c_k\Delta_k)}{2c_k}
\Delta_k.
\]

The gradient and coordinate step are clipped, and the result is projected back to \([-1,1]^d\). Momentum, RMSProp, and Adam variants transform the same SPSA estimate.

## 13. MSGA guard

MSGA evaluates four candidates under the same frozen disturbance and lifecycle context:

- Hold current state.
- Adam-style proposal.
- Positive SPSA probe.
- Negative SPSA probe.

It accepts the candidate with minimum cost. Candidate damage projections are side-effect-free; only the realized accepted command commits one lifecycle update.

Its recovery score combines:

- QFI public-threshold barrier.
- Capture and shadow public barriers.
- Earlier classical headroom barriers.
- QFI tail risk.
- Absorption guard.
- Throughput guard.

## 14. Base objective

Normalized observables are

\[
q=F_Q/F_{Q,\mathrm{lim}},\qquad
c=C/C_{\mathrm{lim}},\qquad
s=S/S_{\mathrm{lim}}.
\]

Each receives a smooth low-level penalty plus a strong quadratic penalty above one. The base objective also includes active-power cost, slew, passive mismatch, cloak-quality deficit, Fresnel reflection, and optional material bias.

Additional families include:

- Peak, CVaR, area, joint, duration, rise, and throughput risk.
- Field, phase, polarization, modal, OAM, and distinguishability penalties.
- Detector scattering and absorption penalties.
- Spectral-hump penalties.
- Damage state, rate, dose, and healing energy.

## 15. Sampled robust gate

A sampled state passes only when all of the following are true:

\[
\max_\lambda F_Q(\lambda)\le F_{Q,\mathrm{lim}},
\]

\[
\max_\lambda C(\lambda)\le C_{\mathrm{lim}},
\qquad
\max_\lambda S(\lambda)\le S_{\mathrm{lim}},
\]

\[
\max_\lambda D_{\mathrm{scatter}}(\lambda)
\le D_{\mathrm{scatter,lim}},
\]

\[
\max_\lambda A(\lambda)\le A_{\mathrm{lim}},
\]

plus material-band validity and every applicable wave/channel gate.

The runtime's dual-below state additionally requires measured channels to pass and the sampled-robust flag to be true.

---

# Part V. Recommended interpretation workflow

For every run, read the dashboard in this order:

1. **Check source power and assessment availability.** A dark interval cannot establish detector concealment.
2. **Check decisive QFI.** Use raw measured and spectral maximum, not a smoothed display trace alone.
3. **Check both classical traces.** Capture and shadow must each be below their own limits.
4. **Check throughput and absorption.** Reject black-absorber solutions.
5. **Check sampled wave/channel metrics when applicable.** Do not treat N/A as a pass.
6. **Check lifecycle.** A temporary state obtained through rapidly increasing damage is not robust control.
7. **Check optimizer evidence.** Use paired seeds and raw exports.
8. **Check numerical convergence.** Repeat with smaller time step and denser spectral or wave grids.
9. **State the claim with the exact modeled scope.** Say “dual-observability suppression,” not “invisibility achieved.”
