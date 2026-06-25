# Experimental FRF Measurement and Estimation Techniques

> A structured study note on **Frequency Response Function (FRF)** — covering measurement theory, signal formulation, modal analysis, and noise-robust estimation methods.

Prepared as part of undergraduate research activities at the **Applied Modeling and Simulation Lab (AMAS), Hankyong National University**, under the guidance of Prof. Un Chang Jeoung.

---

## 📌 Overview

The Frequency Response Function (FRF) describes the relationship between input and output of a dynamic system in the frequency domain. This repository summarizes the theoretical background and experimental methodology for FRF measurement, including practical considerations for noise handling and modal interpretation.

---

## 📚 Contents

1. [Measurement Types and Equipment](#1-measurement-types-and-equipment)
2. [Input Force Analysis](#2-input-force-analysis)
3. [Theoretical vs. Experimental FRF](#3-theoretical-vs-experimental-frf)
   - [Crosspower and Autopower Formulation](#31-crosspower-and-autopower-formulation)
   - [Derivation of Experimental FRF](#32-derivation-of-experimental-frf)
4. [Imaginary FRF and Mode Shape](#4-imaginary-frf-and-mode-shape)
   - [Example](#41-example)
5. [Coherence](#5-coherence)
6. [FRF Estimation Methods](#6-frf-estimation-methods)
   - [H1 Estimator](#61-h1-estimator)
   - [H2 Estimator](#62-h2-estimator)
   - [HV Estimator](#63-hv-estimator)
7. [Conclusion](#7-conclusion)

---

## 1. Measurement Types and Equipment

FRF can be measured across various system types depending on the nature of input and output signals:

| System Type | Input | Output |
|-------------|-------|--------|
| Mechanical | Force (N) | Acceleration (g), Velocity (m/s), Displacement (m) |
| Acoustical | Volume Acceleration (Q) | Sound Pressure (Pa) |
| Combined (Aero-acoustic) | Q or Force (N) | Pressure (Pa), Acceleration (g), etc. |
| Rotational Mechanical | Torque (Nm) | Rotational Displacement (°) |

**Key equipment:** Impact hammer, accelerometer, data acquisition system (DAQ)

---

## 2. Input Force Analysis

For reliable FRF measurement, the input excitation must cover the target frequency range uniformly.

- A **short, sharp impulse** produces a flat input spectrum across a wide frequency band
- The **impact hammer tip** (stiffness and mass) determines the effective frequency range:
  - Soft tip → low-frequency excitation
  - Hard tip → high-frequency excitation

> Achieving a flat input spectrum is essential — non-uniform excitation introduces bias into the measured FRF.

---

## 3. Theoretical vs. Experimental FRF

### Theoretical FRF

The theoretical FRF `H(ω)` is defined directly from the system's equation of motion:

```
H(ω) = X(ω) / F(ω)
```

Where `X(ω)` is the output (response) spectrum and `F(ω)` is the input (force) spectrum.

### Experimental FRF

In practice, signals contain noise. The experimental FRF is estimated using **Autopower** and **Crosspower** spectra to suppress the effect of noise:

```
H(ω) = S_xf(ω) / S_ff(ω)
```

Where:
- `S_xf(ω)` — Cross-power spectrum between input and output
- `S_ff(ω)` — Auto-power spectrum of the input

---

## 3.1 Crosspower and Autopower Formulation

| Term | Definition |
|------|-----------|
| Auto-power spectrum `S_ff` | `F*(ω) · F(ω)` — power of the input signal |
| Auto-power spectrum `S_xx` | `X*(ω) · X(ω)` — power of the output signal |
| Cross-power spectrum `S_xf` | `F*(ω) · X(ω)` — correlation between input and output |

> `*` denotes complex conjugate. These formulations are the foundation for all practical FRF estimators.

---

## 3.2 Derivation of Experimental FRF

Starting from the noise-free relationship `X(ω) = H(ω) · F(ω)`, multiplying both sides by `F*(ω)` and taking the expected value:

```
E[F*(ω) · X(ω)] = H(ω) · E[F*(ω) · F(ω)]
       S_xf(ω)  = H(ω) · S_ff(ω)

∴ H(ω) = S_xf(ω) / S_ff(ω)
```

This formulation averages out uncorrelated noise, providing a more reliable estimate than a single-shot measurement.

---

## 4. Imaginary FRF and Mode Shape

FRF is a **complex-valued function** containing both amplitude and phase information:

- **Real part**: related to stiffness and damping contributions
- **Imaginary part**: peaks at natural (resonant) frequencies

### Key observations at resonance:

| FRF Component | Behavior at Resonance |
|---------------|----------------------|
| Real part | Approaches zero |
| Imaginary part | Shows a peak (positive or negative) |

The **sign and magnitude** of the imaginary FRF peak at each measurement location directly reflects the **mode shape** of that resonant frequency.

---

## 4.1 Example

FRF measurements at multiple locations on a structure were compared at **532 Hz**:

| Location | Phase | Amplitude | Interpretation |
|----------|-------|-----------|----------------|
| 1, 13 | Same | Large | Move in the same direction |
| 3, 15 | Opposite | Large | Move in opposite directions |
| 7, 9 | — | Small | Near a nodal point (minimal displacement) |

> Even at the same frequency, phase and amplitude vary by location — this spatial variation defines the **mode shape**.

---

## 5. Coherence

Coherence `γ²(ω)` quantifies the linear causality between input and output:

```
γ²(ω) = |S_xf(ω)|² / (S_ff(ω) · S_xx(ω))
```

| Coherence Value | Meaning |
|-----------------|---------|
| ≈ 1.0 | Strong linear relationship — measurement is reliable |
| ≈ 0.0 | Dominated by noise or nonlinearity — measurement is unreliable |

**Important:** Coherence is only meaningful with **repeated (averaged) measurements**.
- Averaging stabilizes the FRF estimate
- Coherence then serves as a validation metric for measurement quality

---

## 6. FRF Estimation Methods

In real experiments, both input and output signals contain noise. Three estimators address this differently depending on noise source assumptions.

---

## 6.1 H1 Estimator

```
H1(ω) = S_xf(ω) / S_ff(ω)
```

- Assumes **noise is present only in the output**
- Minimizes the effect of output noise through cross-power averaging
- **Tends to underestimate** FRF magnitude when input noise is present
- Best suited when the input signal has **high SNR** (e.g., shaker excitation)

---

## 6.2 H2 Estimator

```
H2(ω) = S_xx(ω) / S_fx(ω)
```

- Assumes **noise is present only in the input**
- **Tends to overestimate** FRF magnitude when output noise is present
- Best suited when the output signal has **high SNR**

---

## 6.3 HV Estimator

```
HV(ω) = √(H1(ω) · H2(ω))
```

- Geometric mean of H1 and H2
- Assumes **noise exists in both input and output**
- Provides a balanced estimate between H1 and H2
- More robust under general noise conditions

### Estimator Comparison Summary

| Estimator | Noise Assumption | Tendency | Best Use Case |
|-----------|-----------------|----------|---------------|
| H1 | Noise in output only | Underestimates | High-quality input (shaker) |
| H2 | Noise in input only | Overestimates | High-quality output |
| HV | Noise in both | Balanced | General / unknown noise conditions |

---

## 7. Conclusion

Experimental FRF is an essential tool that bridges theoretical dynamics and real-world structural analysis:

- **Crosspower/Autopower** formulation enables stable FRF estimation even under noisy conditions
- **Imaginary FRF** allows intuitive identification of resonant frequencies and mode shapes
- **Coherence** provides a quantitative measure of measurement reliability
- **H1, H2, HV estimators** offer flexible strategies depending on the noise characteristics of the measurement environment

---

## 🛠️ Tools & References

| Item | Detail |
|------|--------|
| Study material | Lecture slides — AMAS Lab, Hankyong National University |
| Adviser | Prof. Un Chang Jeoung |
| Primary reference | Siemens Community: [What is a Frequency Response Function (FRF)?](https://community.sw.siemens.com/s/article/what-is-a-frequency-response-function-frf) |
| Related topic | Modal Analysis, Structural Dynamics, PHM |

---

## 📁 Repository Structure

```
experimental-frf-study/
│
├── README.md
└── slides/
    └── FRF_Measurement_and_Estimation.pptx
```

---

*Studied and documented as part of undergraduate research at AMAS Lab, Hankyong National University.*
