# Chapter 6 — Fine-Scale Equation Derivation for Incompressible Flow

---

# 1. Introduction

The Variational Multiscale formulation introduced in the previous chapter decomposes the solution into resolved and unresolved components:

$$\boxed{\mathbf{u}=\mathbf{u}_h+\mathbf{u}'}$$

and:

$$\boxed{p=p_h+p'}$$

where:

- $\mathbf{u}_h$ represents the resolved velocity scales,
- $\mathbf{u}'$ represents unresolved velocity scales,
- $p_h$ represents the resolved pressure,
- $p'$ represents unresolved pressure scales.

The purpose of this chapter is to derive the governing equation for the unresolved scales.

---

# 2. Strong Form of Navier–Stokes Equations

The incompressible Navier–Stokes equations are:

**Momentum equation:**

$$\rho\left(\frac{\partial\mathbf{u}}{\partial t}+(\mathbf{u}\cdot\nabla)\mathbf{u}\right)+\nabla p-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}))-\rho\mathbf{b}=0$$

**Continuity equation:**

$$\nabla\cdot\mathbf{u}=0$$

These equations can be written compactly as:

$$\mathcal{L}(\mathbf{u},p)=0$$

---

# 3. VMS Decomposition

The velocity and pressure fields are decomposed:

$$\boxed{\mathbf{u}=\mathbf{u}_h+\mathbf{u}'}$$

$$\boxed{p=p_h+p'}$$

Substituting into the momentum equation:

$$\rho\left(\frac{\partial(\mathbf{u}_h+\mathbf{u}')}{\partial t}+((\mathbf{u}_h+\mathbf{u}')\cdot\nabla)(\mathbf{u}_h+\mathbf{u}')\right)+\nabla(p_h+p')-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}_h+\mathbf{u}'))-\rho\mathbf{b}=0$$

---

# 4. Separation of Resolved and Unresolved Contributions

The momentum equation can be separated into:

Resolved contribution:

$$\mathcal{L}(\mathbf{u}_h,p_h)$$

and fine-scale contribution:

$$\mathcal{L}(\mathbf{u}',p')$$

Therefore:

$$\mathcal{L}(\mathbf{u}_h,p_h)+\mathcal{L}(\mathbf{u}',p')+\mathcal{N}(\mathbf{u}_h,\mathbf{u}')=0$$

where:

- $\mathcal{L}$ represents linear operators,
- $\mathcal{N}$ represents nonlinear coupling terms.

---

# 5. Definition of the Resolved-Scale Residual

The resolved solution does not satisfy the strong form exactly.

The momentum residual is defined as:

$$\boxed{\mathbf{R}_m(\mathbf{u}_h,p_h)=\rho\frac{\partial\mathbf{u}_h}{\partial t}+\rho(\mathbf{u}_h\cdot\nabla)\mathbf{u}_h+\nabla p_h-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}_h))-\rho\mathbf{b}}$$

If the finite element solution were exact:

$$\mathbf{R}_m=0$$

However, for a finite-dimensional approximation:

$$\mathbf{R}_m\neq0$$

---

# 6. Continuity Residual

The incompressibility constraint also produces a residual:

$$\boxed{R_c(\mathbf{u}_h)=\nabla\cdot\mathbf{u}_h}$$

The exact solution satisfies:

$$R_c=0$$

but the discrete solution may not satisfy incompressibility exactly.

---

# 7. Fine-Scale Problem

The fine-scale problem is obtained by projecting the governing equations onto the unresolved space.

For the velocity fine scales:

$$\mathbf{v}'\in V'$$

The fine-scale momentum equation is:

$$\boxed{\mathcal{L}(\mathbf{u}',p')=\mathbf{R}_m(\mathbf{u}_h,p_h)}$$

The unresolved scales are therefore driven by the residual of the resolved solution.

---

# 8. Exact Fine-Scale Solution

Formally, the fine-scale velocity can be written using the inverse operator:

$$\mathbf{u}'=\mathcal{L}^{-1}\mathbf{R}_m$$

where:

$$\mathcal{L}^{-1}$$

represents the inverse of the fine-scale differential operator.

This expression is exact but not computationally practical.

---

# 9. Localization of the Fine-Scale Operator

The exact inverse operator is replaced by a local approximation:

$$\mathcal{L}^{-1}\approx\tau$$

where:

- $\tau$ is the stabilization tensor,
- it represents unresolved scale behavior.

Therefore:

$$\boxed{\mathbf{u}'\approx\tau\mathbf{R}_m}$$

Using the conventional sign definition:

$$\boxed{\mathbf{u}'\approx-\tau\mathbf{R}_m}$$

---

# 10. Fine-Scale Pressure Approximation

The pressure fine scale is treated similarly:

$$p'\approx-\tau_p R_c$$

where:

- $\tau_p$ is the pressure stabilization parameter,
- $R_c$ is the continuity residual.

Therefore:

$$\boxed{p'\approx-\tau_p\nabla\cdot\mathbf{u}_h}$$

---

# 11. Residual-Based VMS Approximation

The unresolved scales are therefore approximated as:

**Velocity fine scale:**

$$\boxed{\mathbf{u}'\approx-\tau_m\mathbf{R}_m(\mathbf{u}_h,p_h)}$$

**Pressure fine scale:**

$$\boxed{p'\approx-\tau_pR_c(\mathbf{u}_h)}$$

where:

- $\tau_m$ is the momentum stabilization tensor,
- $\tau_p$ is the pressure stabilization parameter.

---

# 12. Physical Interpretation

The residual-based model states:

If the resolved finite element solution produces a large residual:

$$|\mathbf{R}_m|\uparrow$$

then unresolved fluctuations increase:

$$|\mathbf{u}'|\uparrow$$

If the resolved solution satisfies the governing equations:

$$\mathbf{R}_m\rightarrow0$$

then:

$$\mathbf{u}'\rightarrow0$$

Therefore:

$$\boxed{\text{Residual generates unresolved scales}}$$

---

# 13. Connection to Stabilization

Substituting the fine-scale approximation back into the coarse-scale equation produces additional stabilization terms.

The resulting formulation contains:

- SUPG stabilization,
- PSPG stabilization,
- grad-div stabilization,
- turbulence modeling contributions.

The stabilization terms are not artificially added.

They arise naturally from eliminating unresolved scales.

---

# 14. Role of the Stabilization Tensor

The stabilization tensor:

$$\tau$$

contains information about unresolved physics.

It depends on:

- element size,
- velocity magnitude,
- viscosity,
- time step,
- polynomial order.

A general form is:

$$\boxed{\tau=\tau(h,\mathbf{u},\nu,\Delta t)}$$

The derivation of practical expressions for $\tau$ is the next major step.

---

# Summary

The fine-scale derivation leads to the central RBVMS relationship:

$$\boxed{\mathbf{u}'\approx-\tau\mathbf{R}_m(\mathbf{u}_h,p_h)}$$

The unresolved scales are not explicitly solved.

Instead, their effect is modeled through the stabilization tensor.

The remaining task is to derive $\tau$ from the characteristic time scales of:

1. temporal evolution,
2. convection,
3. diffusion,
4. reaction or compressibility effects.

---

# References

1. T. J. R. Hughes, G. R. Feijóo, L. Mazzei, J. Quincy, "The variational multiscale method — a paradigm for computational mechanics."

2. R. Codina, "Stabilization of incompressibility and convection through orthogonal sub-scales in finite element methods."

3. T. E. Tezduyar, "Stabilized finite element formulations for incompressible flow computations."
