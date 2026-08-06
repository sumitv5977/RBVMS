# Chapter 7 — Derivation of the Stabilization Tensor $\tau$

---

# 1. Introduction

The Residual-Based Variational Multiscale method introduces unresolved scales through the approximation:

$$\boxed{\mathbf{u}'\approx-\tau_m\mathbf{R}_m(\mathbf{u}_h,p_h)}$$

where:

- $\mathbf{u}'$ is the unresolved velocity scale,
- $\mathbf{R}_m$ is the resolved momentum residual,
- $\tau_m$ is the momentum stabilization tensor.

The formal expression:

$$\mathcal{L}^{-1}\approx\tau_m$$

requires a practical approximation.

The stabilization tensor must represent the characteristic response time of the unresolved scales.

---

# 2. Fine-Scale Operator

The fine-scale momentum equation is:

$$\mathcal{L}(\mathbf{u}')=\mathbf{R}_m$$

The differential operator contains several physical mechanisms:

1. Temporal evolution
2. Advection
3. Diffusion
4. Reaction or compressibility effects

A simplified linearized operator can be written as:

$$\mathcal{L}=\frac{\partial}{\partial t}+\mathbf{a}\cdot\nabla-\nu\nabla^2$$

where:

- $\mathbf{a}$ is the characteristic convection velocity,
- $\nu$ is kinematic viscosity.

---

# 3. Characteristic Time-Scale Concept

The stabilization parameter represents the time available for unresolved scales to develop.

Therefore:

$$\tau\approx\text{characteristic unresolved time scale}$$

Each physical mechanism contributes a limiting time scale.

The combined stabilization parameter is obtained by combining these contributions.

---

# 4. Temporal Contribution

Consider only the transient operator:

$$\frac{\partial\mathbf{u}'}{\partial t}$$

The characteristic time scale is related to the time step:

$$\tau_t\approx\Delta t$$

A more general form introduces a constant:

$$\boxed{\tau_t=\frac{\Delta t}{C_t}}$$

where:

- $C_t$ is a method-dependent constant.

---

# 5. Advective Contribution

Consider the convection operator:

$$\mathbf{a}\cdot\nabla\mathbf{u}'$$

The characteristic convection time is:

$$\tau_a=\frac{h}{|\mathbf{a}|}$$

where:

- $h$ is the element length scale,
- $|\mathbf{a}|$ is the characteristic velocity magnitude.

The corresponding inverse time scale is:

$$\tau_a^{-1}=\frac{|\mathbf{a}|}{h}$$

---

# 6. Diffusive Contribution

Consider the diffusion operator:

$$-\nu\nabla^2\mathbf{u}'$$

The diffusion time scale is:

$$\tau_d=\frac{h^2}{\nu}$$

The inverse diffusion time scale is:

$$\tau_d^{-1}=\frac{\nu}{h^2}$$

---

# 7. Combining Time Scales

The contributions are combined through the inverse time scales:

$$\tau^{-2}=\tau_t^{-2}+\tau_a^{-2}+\tau_d^{-2}$$

Therefore:

$$\boxed{\tau=\left(\tau_t^{-2}+\tau_a^{-2}+\tau_d^{-2}\right)^{-1/2}}$$

This form ensures that the smallest physical time scale dominates.

---

# 8. Practical RBVMS Stabilization Parameter

Substituting the individual contributions:

$$\tau=\left(\left(\frac{C_t}{\Delta t}\right)^2+\left(\frac{|\mathbf{u}_h|}{C_a h}\right)^2+\left(\frac{C_d\nu}{h^2}\right)^2\right)^{-1/2}$$

where:

- $C_t$ is the transient constant,
- $C_a$ is the advective constant,
- $C_d$ is the diffusive constant.

The commonly used expression is:

$$\boxed{\tau_m=\left[\left(\frac{C_t}{\Delta t}\right)^2+\left(\frac{|\mathbf{u}_h|}{C_a h}\right)^2+\left(\frac{C_d\nu}{h^2}\right)^2\right]^{-1/2}}$$

---

# 9. Element Length Scale

The element length scale $h$ depends on the finite element geometry.

For isotropic elements:

$$h\approx\sqrt{\text{element area}}$$

For anisotropic elements, the metric tensor formulation is preferred:

$$h=\frac{2}{\sqrt{\mathbf{u}^T\mathbf{G}\mathbf{u}}}$$

where:

- $\mathbf{G}$ is the element metric tensor.

---

# 10. Tensor Form of Stabilization

For anisotropic meshes, $\tau$ becomes a tensor:

$$\boxed{\boldsymbol{\tau}_m=\left(\frac{C_t^2}{\Delta t^2}\mathbf{I}+\mathbf{u}_h^T\mathbf{G}\mathbf{u}_h\mathbf{I}+C_d^2\nu^2\mathbf{G}:\mathbf{G}\right)^{-1/2}}$$

This formulation accounts for:

- stretched elements,
- boundary layers,
- directional resolution.

---

# 11. Pressure Stabilization Parameter

The pressure stabilization parameter is usually related to the momentum stabilization:

$$\boxed{\tau_p=\frac{h^2}{C_p\tau_m}}$$

where:

- $C_p$ is a stabilization constant.

This parameter controls pressure oscillations caused by violation of the inf-sup condition.

---

# 12. Dimensionless Interpretation

The relative importance of convection and diffusion is described by the Reynolds number:

$$Re=\frac{Uh}{\nu}$$

For:

$$Re\ll1$$

diffusion dominates.

For:

$$Re\gg1$$

convection dominates.

The stabilization tensor automatically adjusts between these regimes.

---

# 13. Limiting Cases

## Steady diffusion-dominated flow

If:

$$\frac{\nu}{h^2}\gg\frac{U}{h}$$

then:

$$\tau_m\approx\frac{h^2}{C_d\nu}$$

---

## High Reynolds number flow

If:

$$\frac{U}{h}\gg\frac{\nu}{h^2}$$

then:

$$\tau_m\approx\frac{h}{C_aU}$$

---

## Transient dominated flow

If:

$$\frac{1}{\Delta t}\gg\frac{U}{h},\frac{\nu}{h^2}$$

then:

$$\tau_m\approx\frac{\Delta t}{C_t}$$

---

# 14. Role in RBVMS Formulation

The fine-scale approximation becomes:

$$\boxed{\mathbf{u}'=-\tau_m\mathbf{R}_m}$$

Substituting into the coarse-scale weak form generates stabilization terms.

The resulting formulation contains additional residual-based contributions:

$$\boxed{\text{Galerkin terms}+\text{RBVMS stabilization terms}=0}$$

---

# 15. Connection to C++ Implementation

In a finite element implementation, $\tau_m$ is evaluated at integration points.

The computational sequence is:

1. Compute velocity gradient:

$$\nabla\mathbf{u}_h$$

2. Compute element length scale:

$$h$$

3. Compute Reynolds number:

$$Re=\frac{Uh}{\nu}$$

4. Evaluate stabilization tensor:

$$\tau_m$$

5. Compute residual:

$$\mathbf{R}_m$$

6. Add stabilization contribution to element residual and Jacobian.

---

# Summary

The stabilization tensor is derived by interpreting the unresolved scale operator as a characteristic time-scale operator.

The practical RBVMS form is:

$$\boxed{\tau_m=\left[\left(\frac{C_t}{\Delta t}\right)^2+\left(\frac{|\mathbf{u}_h|}{C_a h}\right)^2+\left(\frac{C_d\nu}{h^2}\right)^2\right]^{-1/2}}$$

It combines:

- temporal effects,
- convection,
- diffusion.

This tensor is the central parameter that transforms the theoretical VMS decomposition into a practical turbulence modeling framework.

---

# References

1. T. J. R. Hughes, G. R. Feijóo, L. Mazzei, J. Quincy, "The variational multiscale method — a paradigm for computational mechanics."

2. T. E. Tezduyar, "Stabilized finite element formulations for incompressible flow computations."

3. R. Codina, "Stabilization of incompressibility and convection through orthogonal sub-scales in finite element methods."
