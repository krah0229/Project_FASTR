# AMME2000 Quiz 2 — Comprehensive Conceptual Study Guide
## Weeks 3–10: Everything You Need to Know (Conceptually)

---

# 1. INITIAL CONDITIONS & BOUNDARY CONDITIONS

## What Are They?

**Boundary Conditions (BCs)** constrain the solution at the *spatial edges* of the domain for *all time*. They represent physical constraints — a wall held at a fixed temperature, an insulated end, a clamped string.

**Initial Conditions (ICs)** describe the state of the system at *t = 0* across the *entire spatial domain*. They represent how the system starts — an initial temperature distribution, an initial displacement of a string.

## Types of Boundary Conditions

| Type | Mathematical Form | Physical Meaning | Example |
|------|-------------------|------------------|---------|
| **Dirichlet** | u(0,t) = value | Fixed value at boundary | End of wire held at 0°C |
| **Neumann** | ∂u/∂x(0,t) = value | Fixed flux/gradient at boundary | Insulated end (= 0) |
| **Mixed/Robin** | au + b(∂u/∂x) = value | Combination of value and flux | Convective cooling |

## Why Do BCs Matter So Much?

BCs determine the *form* of your solution:
- **Dirichlet BCs** (u = 0 at both ends) → solution is a **sine series** (because sin(0) = sin(nπ) = 0)
- **Neumann BCs** (∂u/∂x = 0 at both ends) → solution is a **cosine series** (because d/dx[cos] = -sin, and sin(0) = 0)

This is not a choice you make — it falls out of applying the BCs to the general separated solution.

## Homogeneous vs Non-Homogeneous BCs

- **Homogeneous**: BC = 0 (e.g., u(0,t) = 0). These are "nice" — separation of variables works directly.
- **Non-Homogeneous**: BC ≠ 0 (e.g., u(0,t) = 5). You must decompose: u = u_ss + w, where u_ss is the steady-state that satisfies the non-homogeneous BCs, and w satisfies homogeneous BCs with a modified IC.

## Key Conceptual Point

The IC tells you *what* to decompose into Fourier modes. The BCs tell you *which type* of Fourier modes (sine or cosine) and determine the eigenvalues. Together, they fully specify the solution.

---

# 2. SOLVING THE HEAT EQUATION — ANALYTICAL METHOD

## The Governing Equation

$$u_t = c^2 u_{xx}$$

This is a **parabolic** PDE (discriminant B² − 4AC = 0). It models diffusion: heat conduction, chemical diffusion, moisture transport. The parameter c² is the diffusivity.

## Physical Intuition

The heat equation says: "the rate of change of temperature at a point is proportional to the curvature of the temperature profile at that point." If a region is hotter than its neighbours (negative curvature), it cools. If cooler (positive curvature), it warms. Everything smooths out over time.

## The Solution Method: Separation of Variables

**Step 1 — Assume** u(x,t) = F(x)·G(t)

**Step 2 — Substitute** into PDE and separate: F·G' = c²·F''·G → G'/(c²G) = F''/F = −λ² (separation constant must be negative for bounded solutions)

**Step 3 — Solve two ODEs:**
- Spatial: F'' + λ²F = 0 → F = A·cos(λx) + B·sin(λx)
- Temporal: G' + c²λ²G = 0 → G = e^(−c²λ²t)

**Step 4 — Apply BCs** to determine λₙ = nπ/L and whether you keep sin or cos terms

**Step 5 — Superpose** all modes: u(x,t) = Σ Bₙ sin(nπx/L) e^(−(nπc/L)²t)

**Step 6 — Apply IC** to find the Fourier coefficients Bₙ

## Why Negative Separation Constant?

If the separation constant were positive (+λ²), the spatial solution would be exponentials (e^λx) which blow up. If zero, you get a linear solution which can't match general ICs. Only −λ² gives oscillating spatial solutions (sin/cos) that can represent arbitrary functions via Fourier series, with temporal solutions that decay (e^−...) as physically required.

---

## 2a. HOMOGENEOUS HEAT EQUATION

**Homogeneous** means the BCs are zero: u(0,t) = u(L,t) = 0.

The solution is straightforward:
$$u(x,t) = \sum_{n=1}^{\infty} B_n \sin\left(\frac{n\pi x}{L}\right) e^{-\left(\frac{n\pi c}{L}\right)^2 t}$$

where Bₙ comes from the Fourier sine series of the IC:
$$B_n = \frac{2}{L}\int_0^L f(x)\sin\left(\frac{n\pi x}{L}\right)dx$$

**Key behaviour**: Each mode decays at rate proportional to n². Higher modes (more wiggly) die out much faster. Eventually only the n=1 mode survives — the solution becomes a single smooth sine curve that slowly decays to zero.

---

## 2b. NON-HOMOGENEOUS (INHOMOGENEOUS) HEAT EQUATION

**Non-homogeneous** means BCs ≠ 0, e.g. u(0,t) = T₁, u(L,t) = T₂.

**The trick: Decompose** u(x,t) = u_ss(x) + w(x,t)

**Step 1** — Find the steady-state u_ss(x): Set u_t = 0, so c²u_xx = 0 → u_ss is linear: u_ss(x) = T₁ + (T₂ − T₁)x/L. This satisfies the non-homogeneous BCs.

**Step 2** — The transient w(x,t) = u − u_ss satisfies:
- Same PDE: w_t = c²w_xx
- **Homogeneous** BCs: w(0,t) = 0, w(L,t) = 0
- Modified IC: w(x,0) = f(x) − u_ss(x)

**Step 3** — Solve for w using the standard homogeneous method, then add u_ss back.

**Why this works**: The steady-state "absorbs" the non-zero BCs, leaving a problem we know how to solve.

---

# 3. FOURIER SERIES — THE ENGINE BEHIND ANALYTICAL SOLUTIONS

## Why Fourier Series?

Separation of variables produces individual modes (sin or cos functions). But the IC is generally not a single mode — it's an arbitrary function. Fourier series lets us decompose *any* (reasonable) function into a sum of these modes, so we can match the IC.

## Sine Series vs Cosine Series

| Sine Series | Cosine Series |
|---|---|
| Represents **odd** functions on [−L, L] | Represents **even** functions on [−L, L] |
| Used with **Dirichlet** BCs | Used with **Neumann** BCs |
| Only Bₙ coefficients | A₀ and Aₙ coefficients |
| Bₙ = (2/L)∫₀ᴸ f(x)sin(nπx/L)dx | A₀ = (1/L)∫₀ᴸ f(x)dx |
| | Aₙ = (2/L)∫₀ᴸ f(x)cos(nπx/L)dx |

## The Critical Conceptual Link

You do NOT choose sine vs cosine arbitrarily. Your BCs (from the physics) determine the form of your solution, which in turn dictates which Fourier series you must use to match the IC.

## Common Coefficient Results to Know

| Function | Sine coefficient Bₙ |
|---|---|
| Constant k | Bₙ = 2k/(nπ) for odd n, 0 for even n |
| Sawtooth kx/L | Bₙ = −2k·(−1)ⁿ/(nπ) = 2k·cos(nπ)/(nπ) |
| Already a mode sin(mπx/L) | Bₙ = 1 if n=m, 0 otherwise (orthogonality!) |

## Key Tricks

- cos(nπ) = (−1)ⁿ (alternates: −1, 1, −1, 1, ...)
- cos(nπ/2) cycles: 0, −1, 0, 1, 0, −1, 0, 1, ...
- sin(nπ) = 0 always
- sin(nπ/2) cycles: 1, 0, −1, 0, 1, 0, ...

---

# 4. DISCRETISATION IN NUMERICAL METHODS

## Why Go Numerical?

Analytical solutions (separation of variables + Fourier series) only work for simple geometries (rectangles), constant coefficients, and linear PDEs. Real-world problems have complex shapes, variable material properties, nonlinear effects. Numerical methods handle all of these.

## The Core Idea

Replace continuous derivatives with algebraic approximations at discrete grid points.

**Spatial discretisation**: Divide [0, L] into N intervals → grid points x₀, x₁, ..., xₙ with spacing Δx = L/N

**Temporal discretisation**: Divide time into steps → time levels t⁰, t¹, t², ... with spacing Δt

## Notation

u_i^n = the numerical approximation to u(xᵢ, tⁿ)
- Subscript i → spatial position
- Superscript n → time level

So u₃₂⁹ means "the value of u at the 32nd spatial grid point and the 9th time step."

## Finite Difference Approximations (from Taylor Series)

| Derivative | Approximation | Type | Order |
|---|---|---|---|
| ∂u/∂t | (uᵢⁿ⁺¹ − uᵢⁿ)/Δt | Forward in Time (FT) | O(Δt) |
| ∂u/∂t | (uᵢⁿ − uᵢⁿ⁻¹)/Δt | Backward in Time (BT) | O(Δt) |
| ∂u/∂t | (uᵢⁿ⁺¹ − uᵢⁿ⁻¹)/(2Δt) | Central in Time (CT) | O(Δt²) |
| ∂²u/∂x² | (uᵢ₋₁ⁿ − 2uᵢⁿ + uᵢ₊₁ⁿ)/Δx² | Central in Space (CS) | O(Δx²) |

## How These Are Derived

Take the Taylor expansion of u(x+Δx,t) and u(x−Δx,t) about point (xᵢ,tⁿ), then combine them to isolate the derivative you want. The terms that don't cancel become the **truncation error**.

---

# 5. ACCURACY, CONSISTENCY & STABILITY — HEAT EQUATION NUMERICAL SOLUTIONS

## The Three Pillars

### Consistency
**Question**: Does the finite difference scheme approximate the correct PDE?

**Test**: Substitute Taylor expansions for every term in the scheme. After simplification, you should recover the original PDE plus higher-order remainder terms. If the remainder → 0 as Δx, Δt → 0, the scheme is **consistent**.

A scheme that isn't consistent is solving the wrong equation — no matter how fine your grid, you'll get the wrong answer.

### Stability
**Question**: Do numerical errors grow or shrink over time?

Every numerical scheme introduces small errors (rounding, truncation). If these errors amplify from one time step to the next, the solution will eventually blow up — the scheme is **unstable**.

A stable scheme keeps errors bounded. The tool to check this is **von Neumann stability analysis** (see Section 6).

### Convergence
**Question**: Does the numerical solution approach the true solution as Δx, Δt → 0?

**Lax Equivalence Theorem**: For a consistent scheme, **stability ⟺ convergence**. So if you've verified consistency and stability, convergence is guaranteed. This is why stability analysis is so important.

## Accuracy (Order of Accuracy)

The **order** tells you how fast the error decreases as you refine the grid:
- O(Δx²) means halving Δx reduces the spatial error by a factor of 4
- O(Δt) means halving Δt only reduces the temporal error by a factor of 2

FTCS for heat: O(Δt) + O(Δx²) — first-order in time, second-order in space.

## How to Find Truncation Error

1. Write out the numerical scheme
2. Replace every uᵢ±₁ⁿ±₁ with its Taylor expansion about (xᵢ, tⁿ)
3. Collect terms — the leading PDE terms should cancel, leaving the truncation error
4. The lowest-order remaining term determines the accuracy

---

# 6. VON NEUMANN STABILITY ANALYSIS — BASIC CONCEPTS

## The Core Idea

Assume the error at each grid point can be written as a Fourier mode:

$$\epsilon_i^n = G^n \cdot e^{i\beta m \cdot i}$$

where G is the **amplification factor** (how much the error grows per time step), β_m is a wavenumber, and i = √(−1).

Substitute this into the numerical scheme. Everything simplifies because e^(iβ(i±1)) = e^(iβi)·e^(±iβ), and the G^n factors out.

## The Stability Criterion

$$|G| \leq 1 \quad \text{for ALL wavenumbers } \beta$$

- |G| < 1: errors decay (good)
- |G| = 1: errors stay constant (acceptable)
- |G| > 1: errors grow (unstable — solution blows up)

## What "Conditionally" vs "Unconditionally" Stable Means

- **Conditionally stable**: |G| ≤ 1 only if some condition on Δt and Δx is met (e.g., σ ≤ ½ for FTCS heat)
- **Unconditionally stable**: |G| ≤ 1 for ANY Δt and Δx (e.g., BTCS heat)
- **Unconditionally unstable**: |G| > 1 always, no matter what Δt and Δx you choose (e.g., FTCS for advection)

## Practical Tip

When computing |G|², use |G|² = Re(G)² + Im(G)². If G = a + ib, then |G|² = a² + b². Use Euler's formula: e^(iθ) = cos(θ) + i·sin(θ).

---

# 7. FTCS — FORWARD TIME, CENTRAL SPACE (HEAT EQUATION)

## The Scheme

$$u_i^{n+1} = u_i^n + \sigma\left(u_{i-1}^n - 2u_i^n + u_{i+1}^n\right)$$

where σ = c²Δt/Δx² is the **mesh ratio** (sometimes called the diffusion number).

## The Stencil

To compute uᵢⁿ⁺¹, you need three values at time level n: uᵢ₋₁ⁿ, uᵢⁿ, uᵢ₊₁ⁿ.

```
    n+1:        ●          ← what we compute
                |
     n:    ●────●────●     ← what we need
          i-1   i   i+1
```

## Key Properties

| Property | Value |
|---|---|
| **Type** | Explicit (compute directly, no system to solve) |
| **Accuracy** | O(Δt) + O(Δx²) |
| **Consistency** | Yes |
| **Stability** | Conditionally stable: σ ≤ 1/2 |
| **Pros** | Simple, easy to implement, each time step is fast |
| **Cons** | Stability restriction can force very small Δt |

## Why σ ≤ 1/2?

Von Neumann analysis gives G = 1 − 2σ(1 − cos β). The worst case is β = π (highest frequency mode), giving G = 1 − 4σ. For |G| ≤ 1, we need −1 ≤ 1 − 4σ ≤ 1, which gives σ ≤ 1/2.

Physically: if σ > 1/2, you're "over-correcting" — taking too big a time step causes the solution to oscillate and blow up.

---

# 8. BTCS — BACKWARD TIME, CENTRAL SPACE (HEAT EQUATION)

## The Scheme

$$u_i^{n+1} - \sigma\left(u_{i-1}^{n+1} - 2u_i^{n+1} + u_{i+1}^{n+1}\right) = u_i^n$$

or equivalently:

$$-\sigma u_{i-1}^{n+1} + (1+2\sigma)u_i^{n+1} - \sigma u_{i+1}^{n+1} = u_i^n$$

## The Stencil

```
    n+1:   ●────●────●     ← three unknowns (coupled)
                |
     n:         ●          ← what we know
          i-1   i   i+1
```

## Key Properties

| Property | Value |
|---|---|
| **Type** | Implicit (must solve a tridiagonal system each time step) |
| **Accuracy** | O(Δt) + O(Δx²) — same as FTCS! |
| **Consistency** | Yes |
| **Stability** | **Unconditionally stable** (any σ) |
| **Pros** | No restriction on Δt — can take large time steps |
| **Cons** | Each step requires solving a linear system (Thomas algorithm) |

## Why Unconditionally Stable?

Von Neumann gives G = 1/(1 + 2σ(1 − cos β)). Since σ > 0 and (1 − cos β) ≥ 0, the denominator ≥ 1, so |G| ≤ 1 always. The implicit nature means future values are coupled, preventing over-correction.

## FTCS vs BTCS — The Trade-off

| | FTCS | BTCS |
|---|---|---|
| Ease per step | Very easy | Need Thomas algorithm |
| Step size restriction | σ ≤ 1/2 | None |
| Best for | Short-time, fine grids | Long-time, coarse grids |
| Same accuracy? | Yes — both O(Δt, Δx²) | Yes |

---

# 9. SOLVING THE WAVE EQUATION — ANALYTICAL METHOD

## The Governing Equation

$$u_{tt} = c^2 u_{xx}$$

This is a **hyperbolic** PDE (discriminant B² − 4AC > 0). It models wave propagation: vibrating strings, sound, electromagnetic waves. c is the wave speed.

## Key Difference from Heat Equation

The wave equation is **second order in time**, so it needs **two** initial conditions:
- u(x, 0) = f(x) — initial displacement
- u_t(x, 0) = g(x) — initial velocity

The heat equation only needs one IC (first order in time).

## Physical Intuition

The heat equation *diffuses* (smooths, decays). The wave equation *propagates* (information travels at speed c, energy is conserved). Modes don't decay — they oscillate forever.

## The Solution (Dirichlet BCs)

$$u(x,t) = \sum_{n=1}^{\infty}\left[A_n\cos(\omega_n t) + B_n\sin(\omega_n t)\right]\sin\left(\frac{n\pi x}{L}\right)$$

where ωₙ = nπc/L is the natural frequency of mode n.

- Aₙ comes from matching the initial displacement f(x): Aₙ = (2/L)∫₀ᴸ f(x)sin(nπx/L)dx
- Bₙ comes from matching the initial velocity g(x): Bₙ = (2/(ωₙL))∫₀ᴸ g(x)sin(nπx/L)dx

**If g(x) = 0** (released from rest): all Bₙ = 0, solution only has cos(ωₙt) terms.
**If f(x) = 0** (struck from equilibrium): all Aₙ = 0, solution only has sin(ωₙt) terms.

## Natural Frequencies and Modes

- Mode n has n half-wavelengths in the domain
- Frequency: fₙ = ωₙ/(2π) = nc/(2L)
- Fundamental frequency: f₁ = c/(2L) — determines the "pitch" for strings
- Higher modes are harmonics: f₂ = 2f₁, f₃ = 3f₁, etc.

---

# 10. CTCS — CENTRAL TIME, CENTRAL SPACE (WAVE EQUATION)

## The Scheme

$$u_i^{n+1} = 2u_i^n - u_i^{n-1} + \sigma^2\left(u_{i-1}^n - 2u_i^n + u_{i+1}^n\right)$$

where σ = cΔt/Δx is the **Courant number** (CFL number).

## The Stencil

```
    n+1:        ●          ← what we compute
                |
     n:    ●────●────●     ← spatial neighbours
                |
    n-1:        ●          ← previous time level
```

This needs **two** previous time levels — matching the fact that the wave equation is second order in time.

## The First Time Step Problem

At n=0, we need values at n=−1, which don't exist. Use the initial velocity condition:
$$u_i^{-1} \approx u_i^0 - \Delta t \cdot g(x_i)$$
(from backward FD of u_t at t=0). If g(x) = 0 (zero initial velocity), then u_i^{-1} = u_i^0, and the first step simplifies to:
$$u_i^1 = u_i^0 + \frac{\sigma^2}{2}(u_{i-1}^0 - 2u_i^0 + u_{i+1}^0)$$

Note the factor of 1/2 — this is specific to the first step only.

## Key Properties

| Property | Value |
|---|---|
| **Type** | Explicit |
| **Accuracy** | O(Δt²) + O(Δx²) — second order in BOTH |
| **Stability** | Conditionally stable: σ ≤ 1 (CFL condition) |

## The CFL Condition: σ = cΔt/Δx ≤ 1

Physical meaning: Information travels at speed c. In one time step Δt, it covers distance cΔt. The grid spacing is Δx. If cΔt > Δx (σ > 1), information travels further than one grid cell per step — the numerical scheme can't "keep up" with the physics, and instability results.

**Special case σ = 1**: The numerical solution is EXACT (no truncation error). This is a remarkable property unique to CTCS for the wave equation.

---

# 11. ACCURACY, CONSISTENCY & STABILITY — WAVE EQUATION

## Comparison with Heat Equation Schemes

| Property | FTCS Heat | BTCS Heat | CTCS Wave |
|---|---|---|---|
| Accuracy (time) | O(Δt) | O(Δt) | O(Δt²) |
| Accuracy (space) | O(Δx²) | O(Δx²) | O(Δx²) |
| Stability condition | σ ≤ 1/2 | None | σ ≤ 1 |
| σ definition | c²Δt/Δx² | c²Δt/Δx² | cΔt/Δx |
| Explicit/Implicit | Explicit | Implicit | Explicit |

Note the different definitions of σ! For heat: σ = c²Δt/Δx² (diffusion number). For wave: σ = cΔt/Δx (Courant number). Don't mix these up.

## Why CTCS is Better Than FTCS for Waves

FTCS applied to the wave equation is **unconditionally unstable** (the FTCS advection problem from Week 6 Q5). You need central differencing in time because the wave equation is second-order in time — a first-order time discretisation introduces artificial dissipation or growth.

---

# 12. SOLVING THE LAPLACE & POISSON EQUATIONS — ANALYTICAL METHOD

## The Governing Equations

- **Laplace**: ∇²u = u_xx + u_yy = 0 (no sources)
- **Poisson**: ∇²u = f(x,y) (with sources)

These are **elliptic** PDEs (discriminant B² − 4AC < 0). They model steady-state phenomena: temperature distribution when ∂T/∂t = 0, electrostatic potential, steady fluid flow.

## Key Conceptual Difference from Heat/Wave

There is **no time variable**. The Laplace equation is purely spatial — the solution is determined entirely by boundary conditions (no initial conditions needed). Physically, the system has reached equilibrium.

## Solution by Separation of Variables

Assume u(x,y) = F(x)·G(y). Substitution gives F''/F = −G''/G = constant.

**Critical choice**: Which direction gets the sin/cos (oscillating) solution?
→ The direction where you have **homogeneous** BCs gets the sin/cos. The other direction gets sinh/cosh (or exponentials).

**Example**: If u(x,0) = u(x,b) = 0 (homogeneous in y), then G(y) = sin(nπy/b) and F(x) involves sinh and cosh.

## The Role of sinh

For the Laplace equation, you get sinh/cosh instead of exponential decay (heat) or oscillation (wave). The general solution in the non-oscillating direction looks like:

$$F(x) = C_1\sinh\left(\frac{n\pi x}{b}\right) + C_2\cosh\left(\frac{n\pi x}{b}\right)$$

To satisfy a homogeneous BC at one end, you choose the appropriate combination. For example, if F(0) = 0, then C₂ = 0 and F(x) = C₁·sinh(nπx/b). If F needs to vanish as x → ∞, use F(x) = C₁·e^(−nπx/b).

## Non-Homogeneous BCs on Multiple Sides: Superposition

If more than one boundary has a non-zero condition, decompose into sub-problems:

u = u₁ + u₂ + u₃ + u₄

where each sub-problem has a non-zero BC on one side only and zero on the other three. Solve each separately and add.

---

# 13. NUMERICAL LAPLACE SOLUTION — CENTRAL SPACE (CS) IN BOTH DIRECTIONS

## The Scheme (5-Point Stencil)

Apply central differences in both x and y:

$$\frac{u_{i-1,j} - 2u_{i,j} + u_{i+1,j}}{\Delta x^2} + \frac{u_{i,j-1} - 2u_{i,j} + u_{i,j+1}}{\Delta y^2} = 0$$

If Δx = Δy:

$$u_{i,j} = \frac{1}{4}\left(u_{i-1,j} + u_{i+1,j} + u_{i,j-1} + u_{i,j+1}\right)$$

## Physical Meaning

Each interior point is the **average of its four neighbours**. This is the discrete version of the maximum principle for harmonic functions (solutions to Laplace's equation can't have interior extrema).

## Solution Methods

Since there's no time stepping, you solve a large system of linear equations simultaneously. Common approaches:
- **Direct**: Assemble a matrix equation and solve (Gaussian elimination)
- **Iterative**: Start with a guess and repeatedly apply the averaging formula until convergence (Jacobi or Gauss-Seidel iteration)

## Key Properties

| Property | Value |
|---|---|
| Accuracy | O(Δx²) + O(Δy²) |
| Stencil | 5-point (2D) |
| Method type | System of linear equations (no time marching) |
| Always stable? | Yes — it's a direct or iterative solve, not a marching scheme |

---

# 14. DIFFERENT GOVERNING EQUATIONS & SOLUTION METHODS — THE BIG PICTURE

## PDE Classification Summary

| PDE | Type | Discriminant | Physical Behaviour | Time? |
|---|---|---|---|---|
| u_t = c²u_xx | Parabolic | B²−4AC = 0 | Diffusion, smoothing, decay | Yes (1st order) |
| u_tt = c²u_xx | Hyperbolic | B²−4AC > 0 | Wave propagation, oscillation | Yes (2nd order) |
| u_xx + u_yy = 0 | Elliptic | B²−4AC < 0 | Steady-state, equilibrium | No |

## How to Classify: Au_xx + Bu_xy + Cu_yy + ... = 0

Compute D = B² − 4AC:
- D > 0 → Hyperbolic
- D = 0 → Parabolic
- D < 0 → Elliptic

## Master Summary of Solution Methods

### Analytical Solutions

| Equation | Domain | BCs | Method | Solution Form |
|---|---|---|---|---|
| Heat (1D) | [0, L] | Dirichlet | Sep. of variables + Fourier sine | Σ Bₙ sin(nπx/L) e^(−λₙ²t) |
| Heat (1D) | [0, L] | Neumann | Sep. of variables + Fourier cosine | A₀ + Σ Aₙ cos(nπx/L) e^(−λₙ²t) |
| Heat (1D) | [0, L] | Non-homogeneous | Steady-state decomposition | u_ss(x) + transient |
| Heat (1D) | (−∞, ∞) | None | Fourier integral | Error function solution |
| Heat (1D) | [0, ∞) | Time-varying BC | Laplace transform | erfc solution |
| Wave (1D) | [0, L] | Dirichlet | Sep. of variables + Fourier sine | Σ (Aₙcos + Bₙsin)(ωₙt)·sin(nπx/L) |
| Wave (1D) | [0, ∞) | Time-varying BC | Laplace transform | Shifted solution via 2nd shift thm |
| Laplace (2D) | Rectangle | Mixed | Sep. of variables + superposition | Σ with sinh/cosh × sin/cos |

### Numerical Solutions

| Equation | Scheme | Stencil | Type | Stability |
|---|---|---|---|---|
| Heat | FTCS | 3-point + forward | Explicit | σ ≤ 1/2 |
| Heat | BTCS | 3-point + backward | Implicit | Unconditional |
| Wave | CTCS | 3-point + two time levels | Explicit | σ ≤ 1 (CFL) |
| Laplace | 5-point CS | 5-point (2D) | System solve | Always OK |

## Why Different Methods for Different Domains?

| Domain Type | Why? | Method |
|---|---|---|
| Finite [0,L] | Boundaries exist, function is periodic-ish | Fourier series |
| Infinite (−∞,∞) | No boundaries, no periodicity | Fourier integral |
| Semi-infinite [0,∞) | One boundary, time-varying | Laplace transform |

---

# 15. FOURIER INTEGRALS → FOURIER TRANSFORM & LAPLACE TRANSFORM

## From Fourier Series to Fourier Integral

On a finite domain [−L, L], a function is represented as a **Fourier series** (discrete frequencies nπ/L).

As L → ∞ (infinite domain), the discrete frequencies become continuous, and the sum becomes an **integral** — the Fourier integral:

$$f(x) = \frac{1}{\pi}\int_0^{\infty}\left[\int_{-\infty}^{\infty}f(v)\cos(p(v-x))\,dv\right]dp$$

This is the natural tool for infinite-domain problems where there are no boundaries to quantise the frequencies.

## Condition for Using Fourier Integrals

The function must be **absolutely integrable**: ∫|f(x)|dx < ∞. Physically, there must be a finite amount of "stuff" (heat, concentration, etc.). A function that's constant everywhere violates this, but tricks exist (see Week 9 Q3).

## Fourier Transform

The **Fourier transform** is the continuous-frequency version of Fourier coefficients:

$$\hat{f}(p) = \int_{-\infty}^{\infty}f(x)e^{-ipx}dx \quad \text{(forward)}$$

$$f(x) = \frac{1}{2\pi}\int_{-\infty}^{\infty}\hat{f}(p)e^{ipx}dp \quad \text{(inverse)}$$

It decomposes a function into its continuous frequency content.

## Laplace Transform

The **Laplace transform** converts functions of time to functions of a complex variable s:

$$\mathcal{L}\{f(t)\} = F(s) = \int_0^{\infty}f(t)e^{-st}dt$$

**Why it's useful for PDEs**: It converts time derivatives into algebraic expressions:
- L{f'(t)} = sF(s) − f(0)
- L{f''(t)} = s²F(s) − sf(0) − f'(0)

This eliminates the time variable, reducing a PDE to an ODE which is easier to solve.

## Fourier Transform vs Laplace Transform — When to Use Which

| | Fourier Transform/Integral | Laplace Transform |
|---|---|---|
| **Eliminates** | Spatial variable (infinite domain) | Time variable |
| **Domain** | (−∞, ∞) in space | [0, ∞) in time |
| **Used for** | No spatial boundaries | Semi-infinite spatial domains with time-varying BCs |
| **Returns** | erfc/erf solutions via Fourier integral | Shifted functions via 2nd shift theorem, or erfc |

---

# 16. FOURIER INTEGRAL SOLUTION OF THE HEAT EQUATION

## The Setup

Heat equation on an infinite domain (−∞ < x < ∞): u_t = c²u_xx, with IC u(x,0) = f(x) and no boundary conditions.

## The General Solution

Starting from the Fourier integral form of the solution (given on data sheet):

$$T(x,t) = \frac{1}{\pi}\int_0^{\infty}\left[\int_{-\infty}^{\infty}f(v)\cos(p(v-x))\,e^{-c^2p^2t}\,dv\right]dp$$

After switching order of integration and evaluating the p-integral via a Gaussian:

$$T(x,t) = \frac{1}{2c\sqrt{\pi t}}\int_{-\infty}^{\infty}f(v)\,e^{-\frac{(x-v)^2}{4c^2t}}\,dv$$

This is the **fundamental solution** — it convolves the IC with a Gaussian kernel that spreads over time.

## Solving for Specific ICs

For a "boxcar" IC (f = T₀ on [a, b], 0 elsewhere):

1. Substitute f(v) = T₀, change limits to [a, b]
2. Make the substitution: s = (v − x)/(2c√t)
3. The integral becomes an error function

**Result**:
$$T(x,t) = \frac{T_0}{2}\left[\text{erf}\left(\frac{b-x}{2c\sqrt{t}}\right) - \text{erf}\left(\frac{a-x}{2c\sqrt{t}}\right)\right]$$

## The Error Function

$$\text{erf}(z) = \frac{2}{\sqrt{\pi}}\int_0^z e^{-s^2}ds$$

Properties: erf(0) = 0, erf(∞) = 1, erf(−z) = −erf(z) (odd function).

**erfc(z) = 1 − erf(z)** is the complementary error function.

## Physical Interpretation

The Gaussian kernel is a "spreading" function — it smears the initial distribution over time. Sharp features in the IC get smoothed out. The width of the Gaussian grows as √t, so diffusion slows down over time (the concentration gradient decreases).

---

# 17. FORWARD & INVERSE LAPLACE TRANSFORMS

## The Forward Transform

$$F(s) = \mathcal{L}\{f(t)\} = \int_0^{\infty}f(t)e^{-st}dt$$

In practice, you use the table — here are the key entries:

| f(t) | F(s) |
|---|---|
| 1 | 1/s |
| t | 1/s² |
| tⁿ | n!/s^(n+1) |
| e^(at) | 1/(s−a) |
| sin(ωt) | ω/(s²+ω²) |
| cos(ωt) | s/(s²+ω²) |
| sinh(at) | a/(s²−a²) |
| cosh(at) | s/(s²−a²) |
| e^(at)sin(ωt) | ω/((s−a)²+ω²) |
| e^(at)cos(ωt) | (s−a)/((s−a)²+ω²) |

## Linearity

L{af(t) + bg(t)} = aF(s) + bG(s). So you can break up sums and pull out constants.

## The s-Shift Property

L{e^(at)f(t)} = F(s−a). This means: multiplying by e^(at) in the time domain shifts s → s−a in the Laplace domain.

---

# 18. COMPUTING INVERSE LAPLACE TRANSFORMS

## For Wave Equation Problems: Second Shift Theorem

$$\mathcal{L}^{-1}\left\{e^{-as}F(s)\right\} = f(t-a)\cdot H(t-a)$$

where H is the Heaviside step function and a = x/c.

**Interpretation**: The e^(−xs/c) factor represents a **time delay** of x/c — the time for a wave travelling at speed c to reach position x.

### Recipe:
1. Identify the e^(−xs/c) factor and set it aside
2. Find L⁻¹{F(s)} = f(t) using the table
3. Replace t with (t − x/c) and multiply by H(t − x/c)

### Key Pattern:

| F(s) part (without exponential) | f(t) | Full inverse (with delay x/c) |
|---|---|---|
| 1/s | 1 | H(t − x/c) |
| 1/(s+a) | e^(−at) | e^(−a(t−x/c))·H(t − x/c) |
| 1/s² | t | (t − x/c)·H(t − x/c) |
| 1/(s²+1) | sin(t) | sin(t − x/c)·H(t − x/c) |
| s/(s²+1) | cos(t) | cos(t − x/c)·H(t − x/c) |
| s/(s²−1) | cosh(t) | cosh(t − x/c)·H(t − x/c) |

### Partial Fractions

If F(s) doesn't match a table entry directly, decompose first:
- 1/(s(s+1)) = 1/s − 1/(s+1)
- s/(s²−1) = (1/2)·1/(s−1) + (1/2)·1/(s+1) → cosh(t)

## For Heat Equation Problems: erfc Entry

$$\mathcal{L}^{-1}\left\{\frac{e^{-a\sqrt{s}}}{s}\right\} = \text{erfc}\left(\frac{a}{2\sqrt{t}}\right)$$

where a = x/c.

### Critical Distinction

| Exponent contains | Method | Physical context |
|---|---|---|
| e^(−xs/c) (linear in s) | Second shift theorem | **Wave** equation — sharp wave front at t = x/c |
| e^(−x√s/c) (square root of s) | erfc table entry | **Heat** equation — smooth diffusive front |

This is probably the most important pattern recognition in Week 10. If you see s in the exponent → wave/shift. If you see √s → heat/erfc.

---

# QUICK REFERENCE: EXAM PATTERN RECOGNITION

## "What method do I use?"

```
Given a PDE problem
│
├─ Is the domain finite [0, L]?
│   ├─ Parabolic (heat)?
│   │   ├─ Homogeneous BCs → Separation of variables + Fourier series
│   │   └─ Non-homogeneous BCs → u_ss + transient decomposition
│   ├─ Hyperbolic (wave)?
│   │   └─ Separation of variables + Fourier series (need 2 ICs)
│   └─ Elliptic (Laplace)?
│       └─ Separation of variables + superposition for non-zero BCs
│
├─ Is the domain infinite (−∞, ∞)?
│   └─ Fourier integral → error function solution
│
├─ Is the domain semi-infinite [0, ∞)?
│   └─ Laplace transform in time
│       ├─ Wave equation → second shift theorem → H(t−x/c) solution
│       └─ Heat equation → erfc(x/(2c√t)) solution
│
└─ Numerical?
    ├─ Heat → FTCS (explicit, σ≤½) or BTCS (implicit, unconditional)
    ├─ Wave → CTCS (explicit, σ≤1)
    └─ Laplace → 5-point stencil, solve system
```

---

# COMMON CONCEPTUAL EXAM TRAPS

1. **σ definitions differ**: Heat: σ = c²Δt/Δx². Wave: σ = cΔt/Δx. Laplace: no σ (no time).

2. **Sine vs Cosine**: Dirichlet BCs → sine. Neumann BCs → cosine. Don't choose — the BCs dictate.

3. **Number of ICs**: Heat needs 1 (first-order in t). Wave needs 2 (second-order in t). Laplace needs 0 (no time).

4. **Stability ≠ Accuracy**: A scheme can be stable but inaccurate, or accurate but unstable.

5. **Convergence = Consistency + Stability** (Lax Equivalence Theorem).

6. **exp(−xs/c) vs exp(−x√s/c)**: Linear s → wave, shift theorem. Square root √s → heat, erfc.

7. **FTCS for advection is unconditionally unstable** — even though FTCS for diffusion is conditionally stable.

8. **A₀ exists for cosine series but not sine series**: Don't forget the constant term.

9. **First time step in CTCS**: Modified formula with factor of 1/2 when initial velocity = 0.

10. **Non-homogeneous BCs**: You CANNOT use separation of variables directly. Must decompose first.
