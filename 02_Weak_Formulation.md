# Chapter 2 — Weak Formulation of Incompressible Navier–Stokes Equations

---

# 1. Introduction

The governing equations introduced in Chapter 1 represent the **strong form** of the incompressible Navier–Stokes problem.

The strong form requires the velocity field and pressure field to possess sufficient smoothness for the derivatives to exist pointwise.

The finite element method does not directly solve the strong form. Instead, it seeks an equivalent **weak (variational) formulation**, where the governing equations are satisfied in an integral sense.

The weak formulation is the foundation for:

- Galerkin finite element methods,
- stabilized methods,
- Variational Multiscale (VMS) methods,
- Residual-Based VMS (RBVMS).

---

# 2. Strong Form of the Governing Equations

The incompressible Navier–Stokes equations are:

**Momentum equation:**

$$\rho\left(\frac{\partial\mathbf{u}}{\partial t}+(\mathbf{u}\cdot\nabla)\mathbf{u}\right)+\nabla p-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}))-\rho\mathbf{b}=0$$

**Continuity equation:**

$$\nabla\cdot\mathbf{u}=0$$

where:

- $\mathbf{u}$ is velocity,
- $p$ is pressure,
- $\rho$ is density,
- $\mu$ is dynamic viscosity,
- $\mathbf{b}$ is body force.

---

# 3. Weighted Residual Concept

The finite element method introduces an approximation of the unknown fields.

Instead of requiring:

$$R(\mathbf{u},p)=0$$

everywhere, we require that the residual is orthogonal to a set of weighting functions.

The general weighted residual statement is:

$$\int_{\Omega}wR\,d\Omega=0$$

where:

- $R$ is the residual,
- $w$ is a weighting function.

For vector-valued momentum equations, the weighting function is a vector field.

---

# 4. Definition of Trial and Test Spaces

The velocity field belongs to the space:

$$\mathbf{u}\in V$$

and the pressure field belongs to:

$$p\in Q$$

The corresponding test functions are:

$$\mathbf{v}\in V_0$$

and

$$q\in Q$$

where:

- $V$ is the velocity trial space,
- $V_0$ contains functions satisfying homogeneous velocity boundary conditions,
- $Q$ is the pressure space.

The velocity space is:

$$V=\{\mathbf{u}\in H^1(\Omega)^d:\mathbf{u}=\bar{\mathbf{u}}\text{ on }\Gamma_D\}$$

The test space is:

$$V_0=\{\mathbf{v}\in H^1(\Omega)^d:\mathbf{v}=0\text{ on }\Gamma_D\}$$

---

# 5. Momentum Equation Residual

The strong residual of the momentum equation is:

$$R_m=\rho\left(\frac{\partial\mathbf{u}}{\partial t}+(\mathbf{u}\cdot\nabla)\mathbf{u}\right)+\nabla p-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}))-\rho\mathbf{b}$$

The exact solution satisfies:

$$R_m=0$$

inside the domain.

The weighted residual statement becomes:

$$\int_{\Omega}\mathbf{v}\cdot R_m\,d\Omega=0$$

---

# 6. Expanding the Weighted Residual

Substituting the momentum residual:

$$\int_{\Omega}\mathbf{v}\cdot\left[\rho\frac{\partial\mathbf{u}}{\partial t}+\rho(\mathbf{u}\cdot\nabla)\mathbf{u}+\nabla p-\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}))-\rho\mathbf{b}\right]d\Omega=0$$

The terms are treated separately:

1. Transient term
2. Convective term
3. Pressure term
4. Diffusion term
5. Body force term

---

# 7. Integration by Parts

The purpose of integration by parts is to reduce the order of derivatives acting on the unknown fields.

The general identity is:

$$\int_{\Omega}\mathbf{v}\cdot\nabla\phi\,d\Omega=-\int_{\Omega}\phi\nabla\cdot\mathbf{v}\,d\Omega+\int_{\Gamma}\phi\mathbf{v}\cdot\mathbf{n}\,d\Gamma$$

---

# 8. Pressure Term

The pressure contribution is:

$$\int_{\Omega}\mathbf{v}\cdot\nabla p\,d\Omega$$

Applying integration by parts:

$$\int_{\Omega}\mathbf{v}\cdot\nabla p\,d\Omega=-\int_{\Omega}p\nabla\cdot\mathbf{v}\,d\Omega+\int_{\Gamma}p\mathbf{v}\cdot\mathbf{n}\,d\Gamma$$

For homogeneous velocity test functions on Dirichlet boundaries, the boundary contribution is handled through the natural traction condition.

The pressure term becomes:

$$-\int_{\Omega}p\nabla\cdot\mathbf{v}\,d\Omega$$

---

# 9. Viscous Term

The viscous contribution is:

$$-\int_{\Omega}\mathbf{v}\cdot\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}))\,d\Omega$$

Using integration by parts:

$$\int_{\Omega}2\mu\boldsymbol{\varepsilon}(\mathbf{u}):\boldsymbol{\varepsilon}(\mathbf{v})\,d\Omega-\int_{\Gamma}\mathbf{v}\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u})\mathbf{n})\,d\Gamma$$

The volume contribution becomes:

$$\int_{\Omega}2\mu\boldsymbol{\varepsilon}(\mathbf{u}):\boldsymbol{\varepsilon}(\mathbf{v})\,d\Omega$$

---

# 10. Continuity Equation

The continuity equation is:

$$\nabla\cdot\mathbf{u}=0$$

Multiplying by the pressure test function $q$:

$$\int_{\Omega}q\nabla\cdot\mathbf{u}\,d\Omega=0$$

---

# 11. Weak Form of Momentum Equation

Combining all contributions gives:

$$\rho\int_{\Omega}\mathbf{v}\cdot\frac{\partial\mathbf{u}}{\partial t}d\Omega+\rho\int_{\Omega}\mathbf{v}\cdot(\mathbf{u}\cdot\nabla)\mathbf{u}d\Omega+\int_{\Omega}2\mu\boldsymbol{\varepsilon}(\mathbf{u}):\boldsymbol{\varepsilon}(\mathbf{v})d\Omega-\int_{\Omega}p\nabla\cdot\mathbf{v}d\Omega=\int_{\Omega}\rho\mathbf{b}\cdot\mathbf{v}d\Omega$$

---

# 12. Weak Form of Continuity Equation

The weak incompressibility constraint is:

$$\boxed{\int_{\Omega}q\nabla\cdot\mathbf{u}\,d\Omega=0}$$

---

# 13. Compact Variational Statement

The weak form of the incompressible Navier–Stokes equations is:

Find $(\mathbf{u},p)\in V\times Q$ such that for all $(\mathbf{v},q)\in V_0\times Q$:

$$\boxed{\rho(\frac{\partial\mathbf{u}}{\partial t},\mathbf{v})+c(\mathbf{u};\mathbf{u},\mathbf{v})+a(\mathbf{u},\mathbf{v})-b(\mathbf{v},p)+b(\mathbf{u},q)=F(\mathbf{v})}$$

where:

## Transient term

$$\rho(\frac{\partial\mathbf{u}}{\partial t},\mathbf{v})=\int_{\Omega}\rho\mathbf{v}\cdot\frac{\partial\mathbf{u}}{\partial t}d\Omega$$

## Convective term

$$c(\mathbf{u};\mathbf{u},\mathbf{v})=\int_{\Omega}\rho\mathbf{v}\cdot(\mathbf{u}\cdot\nabla)\mathbf{u}d\Omega$$

## Viscous term

$$a(\mathbf{u},\mathbf{v})=\int_{\Omega}2\mu\boldsymbol{\varepsilon}(\mathbf{u}):\boldsymbol{\varepsilon}(\mathbf{v})d\Omega$$

## Pressure-velocity coupling

$$b(\mathbf{v},p)=\int_{\Omega}p\nabla\cdot\mathbf{v}d\Omega$$

## Body force term

$$F(\mathbf{v})=\int_{\Omega}\rho\mathbf{b}\cdot\mathbf{v}d\Omega$$

---

# 14. Importance for RBVMS

The weak formulation introduces the central object used by Variational Multiscale methods:

$$\boxed{\text{Residual}= \text{Strong form evaluated using the approximate solution}}$$

RBVMS modifies this weak form by introducing unresolved scales:

$$\boxed{\mathbf{u}=\mathbf{u}_h+\mathbf{u}'}$$

where:

- $\mathbf{u}_h$ represents resolved finite element scales,
- $\mathbf{u}'$ represents unresolved subgrid scales.

The next chapter introduces the function spaces and mathematical framework required for this decomposition.

---

# Summary

The weak formulation is obtained by:

1. Writing the strong residual.
2. Multiplying by test functions.
3. Integrating over the domain.
4. Applying integration by parts.
5. Introducing trial and test spaces.

The resulting variational problem forms the basis of the Galerkin FEM and provides the starting point for the derivation of RBVMS stabilization.

---

# References

1. T. J. R. Hughes, *The Finite Element Method: Linear Static and Dynamic Finite Element Analysis*.

2. O. C. Zienkiewicz, R. L. Taylor, *The Finite Element Method*.

3. T. J. R. Hughes, "Multiscale phenomena: Green's functions, the Dirichlet-to-Neumann formulation, subgrid scale models, bubbles and the origin of stabilized methods."

4. R. Codina, "Stabilization of incompressibility and convection through orthogonal sub-scales in finite element methods."
