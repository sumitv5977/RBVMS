# Chapter 9 — Dynamic Variational Multiscale (DVMS) Formulation

---

# 1. Introduction

The Residual-Based Variational Multiscale (RBVMS) formulation introduced in the previous chapter uses an algebraic approximation for the unresolved scales:

$$\boxed{\mathbf{u}'=-\tau_m\mathbf{R}_m}$$

This is known as the **static subscale approximation**.

The assumption is that the unresolved scales instantaneously adjust to the resolved-scale residual.

However, turbulent flows contain time-dependent fluctuations.

The unresolved scales possess:

- inertia,
- memory,
- transient evolution.

Dynamic Variational Multiscale (DVMS) methods account for this temporal evolution by solving an equation for the unresolved scales.

---

# 2. Static Versus Dynamic Subscales

## Static RBVMS

The static approximation assumes:

$$\boxed{\frac{\partial\mathbf{u}'}{\partial t}=0}$$

Therefore:

$$\mathbf{u}'=-\tau_m\mathbf{R}_m$$

The fine scales respond immediately to the residual.

---

## Dynamic VMS

The dynamic formulation retains:

$$\boxed{\frac{\partial\mathbf{u}'}{\partial t}\neq0}$$

The fine scales satisfy their own evolution equation.

---

# 3. Fine-Scale Momentum Equation

The fine-scale momentum equation is obtained by projecting the Navier–Stokes equations onto the unresolved space.

The exact form is:

$$\rho\frac{\partial\mathbf{u}'}{\partial t}+\mathcal{L}(\mathbf{u}')=\mathbf{R}_m(\mathbf{u}_h,p_h)$$

where:

- $\mathbf{u}'$ is the unresolved velocity,
- $\mathcal{L}$ is the fine-scale operator,
- $\mathbf{R}_m$ is the resolved momentum residual.

---

# 4. Linearized Fine-Scale Operator

The fine-scale operator contains:

## Temporal contribution

$$\rho\frac{\partial\mathbf{u}'}{\partial t}$$

## Advection contribution

$$\rho(\mathbf{u}_h\cdot\nabla)\mathbf{u}'$$

## Diffusion contribution

$$-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}'))$$

Therefore:

$$\rho\frac{\partial\mathbf{u}'}{\partial t}+\rho(\mathbf{u}_h\cdot\nabla)\mathbf{u}'-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}'))=\mathbf{R}_m$$

---

# 5. Local Dynamic Approximation

Solving the full fine-scale PDE is computationally expensive.

A localized approximation is introduced:

$$\boxed{\rho\frac{d\mathbf{u}'}{dt}+\tau_m^{-1}\mathbf{u}'=\mathbf{R}_m}$$

Using the conventional sign convention:

$$\boxed{\frac{d\mathbf{u}'}{dt}+\tau_m^{-1}\mathbf{u}'=-\frac{1}{\rho}\mathbf{R}_m}$$

This is the dynamic subscale equation.

---

# 6. Interpretation of the Dynamic Equation

The dynamic fine-scale equation contains two competing mechanisms.

## Generation

The residual produces unresolved fluctuations:

$$-\frac{1}{\rho}\mathbf{R}_m$$

---

## Dissipation

The stabilization term removes unresolved energy:

$$\tau_m^{-1}\mathbf{u}'$$

---

The balance is:

$$\boxed{\text{Generation of fine scales}=\text{Dissipation of fine scales}}$$

---

# 7. Solution of the Dynamic Fine-Scale Equation

The dynamic equation:

$$\frac{d\mathbf{u}'}{dt}+\tau_m^{-1}\mathbf{u}'=-\frac{1}{\rho}\mathbf{R}_m$$

is a first-order ordinary differential equation.

The exact solution is:

$$\mathbf{u}'(t)=e^{-t/\tau_m}\mathbf{u}'(0)-\frac{1}{\rho}\int_0^te^{-(t-s)/\tau_m}\mathbf{R}_m(s)ds$$

This shows that the unresolved scales retain memory of previous residuals.

---

# 8. Relationship to Static RBVMS

If the transient term is neglected:

$$\frac{d\mathbf{u}'}{dt}=0$$

then:

$$\tau_m^{-1}\mathbf{u}'=-\frac{1}{\rho}\mathbf{R}_m$$

Therefore:

$$\boxed{\mathbf{u}'=-\tau_m\frac{1}{\rho}\mathbf{R}_m}$$

which recovers the static RBVMS model.

Thus:

$$\boxed{\text{Static RBVMS is a limiting case of Dynamic VMS}}$$

---

# 9. Dynamic VMS Stabilization Term

The coarse-scale equation now contains:

$$\mathbf{u}'$$

as a time-dependent quantity.

The stabilization contribution becomes:

$$\boxed{B_{DVMS}=(\mathbf{u}',\mathcal{L}^*(\mathbf{v}_h))}$$

where:

$$\mathbf{u}'$$

is obtained from the dynamic subscale equation.

---

# 10. Time Discretization of Dynamic Subscales

For implementation, the dynamic equation must be discretized.

Using backward Euler:

$$\frac{\mathbf{u}'^{n+1}-\mathbf{u}'^n}{\Delta t}+\tau_m^{-1}\mathbf{u}'^{n+1}=-\frac{1}{\rho}\mathbf{R}_m^{n+1}$$

Rearranging:

$$\boxed{\mathbf{u}'^{n+1}=\frac{\mathbf{u}'^n-\frac{\Delta t}{\rho}\mathbf{R}_m^{n+1}}{1+\frac{\Delta t}{\tau_m}}}$$

The unresolved scale now carries information from previous time steps.

---

# 11. Physical Meaning for Turbulence

Turbulent flows contain rapidly changing structures.

The dynamic formulation allows:

- delayed response,
- transient energy transfer,
- interaction between scales.

The unresolved scales behave as a local turbulent model.

The energy transfer mechanism becomes:

$$\boxed{\text{Large scales}\rightarrow\text{Unresolved scales}\rightarrow\text{Dissipation}}$$

---

# 12. Comparison Between RBVMS and DVMS

| Feature | RBVMS | Dynamic VMS |
|---|---|---|
| Fine scale | Algebraic | Time-dependent |
| Equation solved | None | ODE for $\mathbf{u}'$ |
| Memory effects | No | Yes |
| Computational cost | Lower | Higher |
| Turbulence modeling | Good | More accurate |
| Time-dependent flows | Limited | Natural |

---

# 13. Computational Implications

Dynamic VMS requires additional unknown storage:

For every element:

Store:

$$\mathbf{u}'_e$$

Update:

$$\mathbf{u}'_e^{n+1}$$

at every time step.

The computational sequence becomes:

1. Compute resolved residual:

$$\mathbf{R}_m$$

2. Update fine scale:

$$\mathbf{u}'^{n+1}$$

3. Add fine-scale contribution to residual.

4. Assemble Jacobian.

---

# 14. Application to Cylinder Flow

For flow over a circular cylinder:

- vortex shedding creates time-dependent structures,
- wake turbulence contains unresolved scales,
- static models may not capture transient interactions.

Dynamic VMS is especially suitable for:

- vortex shedding,
- transitional Reynolds numbers,
- turbulent wake prediction.

---

# Summary

Dynamic VMS extends RBVMS by solving an evolution equation for the unresolved scales.

The central equation is:

$$\boxed{\frac{d\mathbf{u}'}{dt}+\tau_m^{-1}\mathbf{u}'=-\frac{1}{\rho}\mathbf{R}_m}$$

Static RBVMS is recovered when:

$$\boxed{\frac{d\mathbf{u}'}{dt}=0}$$

Dynamic VMS provides:

- temporal memory,
- improved turbulence representation,
- better modeling of transient flows.

---

# References

1. T. J. R. Hughes, "Multiscale phenomena: Green's functions, the Dirichlet-to-Neumann formulation, subgrid scale models, bubbles and the origins of stabilized methods."

2. L. P. Franca and S. L. Frey, "Stabilized finite element methods: II. The incompressible Navier-Stokes equations."

3. Codina, R., "Comparison of some finite element methods for solving the diffusion-convection-reaction equation."
