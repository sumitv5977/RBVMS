# Chapter 8 — Residual-Based Variational Multiscale (RBVMS) Weak Formulation

---

# 1. Introduction

The previous chapters established:

1. The incompressible Navier–Stokes equations.
2. The weak Galerkin formulation.
3. The VMS decomposition:

$$\boxed{\mathbf{u}=\mathbf{u}_h+\mathbf{u}'}$$

4. The residual-based fine-scale approximation:

$$\boxed{\mathbf{u}'=-\tau_m\mathbf{R}_m(\mathbf{u}_h,p_h)}$$

The purpose of this chapter is to substitute the modeled unresolved scales into the coarse-scale weak formulation and derive the stabilized RBVMS formulation.

---

# 2. Coarse-Scale Variational Problem

The coarse-scale problem is obtained by selecting coarse-scale test functions:

$$\mathbf{v}_h\in V_h$$

The VMS weak form is:

$$B(\mathbf{u}_h+\mathbf{u}',p_h+p';\mathbf{v}_h,q_h)=F(\mathbf{v}_h)$$

The unresolved scales influence the resolved scales through additional terms.

---

# 3. Navier–Stokes Residuals

The momentum residual is:

$$\boxed{\mathbf{R}_m=\rho\frac{\partial\mathbf{u}_h}{\partial t}+\rho(\mathbf{u}_h\cdot\nabla)\mathbf{u}_h+\nabla p_h-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}_h))-\rho\mathbf{b}}$$

The continuity residual is:

$$\boxed{R_c=\nabla\cdot\mathbf{u}_h}$$

The residuals represent the amount by which the finite element solution fails to satisfy the strong equations.

---

# 4. Fine-Scale Substitution

The velocity fine scale is approximated as:

$$\mathbf{u}'=-\tau_m\mathbf{R}_m$$

The pressure fine scale is approximated as:

$$p'=-\tau_pR_c$$

Substituting these into the coarse-scale equation introduces additional residual-dependent terms.

---

# 5. Stabilized Momentum Equation

The stabilized weak form becomes:

$$B_G(\mathbf{u}_h,p_h;\mathbf{v}_h,q_h)+B_S(\mathbf{u}_h,p_h;\mathbf{v}_h,q_h)=F(\mathbf{v}_h)$$

where:

- $B_G$ is the standard Galerkin contribution,
- $B_S$ is the stabilization contribution.

---

# 6. Galerkin Contribution

The standard Galerkin term is:

$$\rho(\frac{\partial\mathbf{u}_h}{\partial t},\mathbf{v}_h)+c(\mathbf{u}_h;\mathbf{u}_h,\mathbf{v}_h)+a(\mathbf{u}_h,\mathbf{v}_h)-b(\mathbf{v}_h,p_h)+b(\mathbf{u}_h,q_h)$$

This is the unstabilized formulation.

---

# 7. Stabilization Contribution

The RBVMS stabilization term is:

$$\boxed{B_S=\sum_e(\tau_m\mathbf{R}_m,\mathcal{L}^*(\mathbf{v}_h))_{\Omega_e}}$$

where:

- $\Omega_e$ is the element domain,
- $\mathcal{L}^*$ is the adjoint operator.

---

# 8. Adjoint Operator

The adjoint operator contains contributions from:

## Time

$$-\rho\frac{\partial\mathbf{v}_h}{\partial t}$$

## Convection

$$-\rho(\mathbf{u}_h\cdot\nabla)\mathbf{v}_h$$

## Diffusion

$$-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{v}_h))$$

Therefore:

$$\boxed{\mathcal{L}^*(\mathbf{v}_h)=-\rho\frac{\partial\mathbf{v}_h}{\partial t}-\rho(\mathbf{u}_h\cdot\nabla)\mathbf{v}_h-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{v}_h))}$$

---

# 9. SUPG Contribution

The dominant part of the RBVMS stabilization comes from the convection operator.

Using:

$$\mathcal{L}^*(\mathbf{v}_h)\approx-\rho(\mathbf{u}_h\cdot\nabla)\mathbf{v}_h$$

gives:

$$\boxed{B_{SUPG}=\sum_e(\tau_m\mathbf{R}_m,\rho(\mathbf{u}_h\cdot\nabla)\mathbf{v}_h)_{\Omega_e}}$$

This stabilizes convection-dominated flows.

---

# 10. PSPG Stabilization

Equal-order velocity-pressure interpolation violates the inf-sup condition.

The pressure stabilization term is obtained from the pressure residual:

$$R_c=\nabla\cdot\mathbf{u}_h$$

The PSPG contribution is:

$$\boxed{B_{PSPG}=\sum_e(\tau_m\mathbf{R}_m,\nabla q_h)_{\Omega_e}}$$

This provides pressure stabilization.

---

# 11. Grad-Div Stabilization

An additional incompressibility stabilization term is often introduced:

$$\boxed{B_{GD}=\sum_e(\gamma\nabla\cdot\mathbf{u}_h,\nabla\cdot\mathbf{v}_h)_{\Omega_e}}$$

where:

- $\gamma$ is the grad-div parameter.

This improves mass conservation.

---

# 12. Complete RBVMS Formulation

The complete stabilized weak form becomes:

$$\boxed{B_G+B_{SUPG}+B_{PSPG}+B_{GD}=F}$$

or explicitly:

$$\boxed{\rho(\frac{\partial\mathbf{u}_h}{\partial t},\mathbf{v}_h)+c(\mathbf{u}_h;\mathbf{u}_h,\mathbf{v}_h)+a(\mathbf{u}_h,\mathbf{v}_h)-b(\mathbf{v}_h,p_h)+b(\mathbf{u}_h,q_h)+B_{SUPG}+B_{PSPG}+B_{GD}=F(\mathbf{v}_h)}$$

---

# 13. Physical Interpretation

The RBVMS formulation introduces stabilization through the residual:

$$\mathbf{R}_m$$

The mechanism is:

$$\boxed{\text{Residual}\rightarrow\text{Fine scales}\rightarrow\text{Stabilization}}$$

Large residuals generate stronger corrections.

Small residuals produce negligible stabilization.

---

# 14. Difference Between Classical Stabilization and RBVMS

Classical stabilization methods:

- add artificial terms,
- modify the numerical operator.

RBVMS:

- derives stabilization from unresolved scales,
- preserves the original physics,
- models scale interactions.

The stabilization is therefore physics-based.

---

# 15. Final RBVMS Form Used in CFD

The final formulation can be summarized as:

$$\boxed{\text{Navier-Stokes Galerkin formulation}+\text{Residual-based multiscale stabilization}=0}$$

The stabilization depends on:

$$\boxed{\tau_m,\tau_p,\mathbf{R}_m,R_c}$$

which are evaluated locally at element integration points.

---

# 16. Connection to Implementation

For each element, a CFD finite element code performs:

1. Compute velocity and pressure fields.
2. Evaluate momentum residual:

$$\mathbf{R}_m$$

3. Compute stabilization tensor:

$$\tau_m$$

4. Add stabilization residual:

$$\tau_m\mathbf{R}_m$$

5. Compute consistent Jacobian for Newton iteration.

The mathematical formulation directly determines the element residual and tangent matrix.

---

# Summary

The RBVMS formulation is obtained by:

1. Decomposing the solution:

$$\mathbf{u}=\mathbf{u}_h+\mathbf{u}'$$

2. Modeling unresolved scales:

$$\mathbf{u}'=-\tau_m\mathbf{R}_m$$

3. Substituting into the coarse-scale problem.

The final stabilized weak form contains:

- Galerkin contribution,
- SUPG stabilization,
- PSPG stabilization,
- grad-div stabilization.

This formulation is the basis for modern finite element turbulence solvers.

---

# References

1. T. J. R. Hughes, G. R. Feijóo, L. Mazzei, J. Quincy, "The variational multiscale method — a paradigm for computational mechanics."

2. T. E. Tezduyar, "Stabilized finite element formulations for incompressible flow computations."

3. R. Codina, "Stabilization of incompressibility and convection through orthogonal sub-scales in finite element methods."
