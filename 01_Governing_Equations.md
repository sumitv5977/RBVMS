# Chapter 1 — Governing Equations of Incompressible Flow

---
<br>
$\boldsymbol{\rho} \left( \frac{\partial \boldsymbol{u}}{\partial t} + (\boldsymbol{u} \cdot \boldsymbol{\nabla})\boldsymbol{u} \right) = -\boldsymbol{\nabla} p + \boldsymbol{\nabla} \cdot \Big(2\mu \boldsymbol{\varepsilon}(\boldsymbol{u})\Big) + \boldsymbol{\rho} \boldsymbol{b}$
</br>

## 1. Introduction

The Residual-Based Variational Multiscale (RBVMS) method is derived from the governing equations of fluid mechanics. Therefore, the starting point is the strong form of the incompressible Navier–Stokes equations.

The governing equations arise from the fundamental conservation principles:

1. Conservation of mass
2. Conservation of linear momentum

For incompressible, Newtonian, isothermal flows, these equations provide the mathematical foundation for the variational formulation developed in later chapters.

---

# 2. Conservation of Mass

Consider an arbitrary control volume $\Omega$ with boundary $\partial\Omega$.

The conservation of mass states that the rate of change of mass inside the control volume equals the net mass flux through its boundary.

The integral form is:

$$\frac{d}{dt}\int_{\Omega}\rho\,d\Omega+\int_{\partial\Omega}\rho\mathbf{u}\cdot\mathbf{n}\,d\Gamma=0$$

where:

- $\rho$ is the density,
- $\mathbf{u}$ is the velocity field,
- $\mathbf{n}$ is the outward unit normal vector.

Using the divergence theorem:

$$\int_{\partial\Omega}\rho\mathbf{u}\cdot\mathbf{n}\,d\Gamma=\int_{\Omega}\nabla\cdot(\rho\mathbf{u})\,d\Omega$$

Therefore:

$$\int_{\Omega}\left(\frac{\partial\rho}{\partial t}+\nabla\cdot(\rho\mathbf{u})\right)d\Omega=0$$

Since the control volume is arbitrary:

**Continuity equation:**

$$\boxed{\frac{\partial\rho}{\partial t}+\nabla\cdot(\rho\mathbf{u})=0}$$

---

# 3. Incompressibility Constraint

For an incompressible fluid:

$$\rho=\text{constant}$$

Therefore:

$$\frac{\partial\rho}{\partial t}=0$$

and

$$\nabla\cdot(\rho\mathbf{u})=\rho\nabla\cdot\mathbf{u}$$

Since density is non-zero:

**Incompressibility constraint:**

$$\boxed{\nabla\cdot\mathbf{u}=0}$$

---

# 4. Conservation of Linear Momentum

Newton's second law applied to a fluid control volume gives:

$$\frac{d}{dt}\int_{\Omega}\rho\mathbf{u}\,d\Omega=\int_{\partial\Omega}\boldsymbol{\sigma}\mathbf{n}\,d\Gamma+\int_{\Omega}\rho\mathbf{b}\,d\Omega$$

where:

- $\boldsymbol{\sigma}$ is the Cauchy stress tensor,
- $\mathbf{b}$ is the body force per unit mass.

Using the divergence theorem:

$$\int_{\partial\Omega}\boldsymbol{\sigma}\mathbf{n}\,d\Gamma=\int_{\Omega}\nabla\cdot\boldsymbol{\sigma}\,d\Omega$$

Therefore:

$$\int_{\Omega}\left[\frac{\partial(\rho\mathbf{u})}{\partial t}+\nabla\cdot(\rho\mathbf{u}\otimes\mathbf{u})-\nabla\cdot\boldsymbol{\sigma}-\rho\mathbf{b}\right]d\Omega=0$$

Since the domain is arbitrary:

**Conservative momentum equation:**

$$\boxed{\frac{\partial(\rho\mathbf{u})}{\partial t}+\nabla\cdot(\rho\mathbf{u}\otimes\mathbf{u})=\nabla\cdot\boldsymbol{\sigma}+\rho\mathbf{b}}$$

---

# 5. Cauchy Stress Tensor

The stress tensor is decomposed into pressure and viscous components:

$$\boldsymbol{\sigma}=-p\mathbf{I}+\boldsymbol{\tau}$$

where:

- $p$ is pressure,
- $\mathbf{I}$ is the identity tensor,
- $\boldsymbol{\tau}$ is the viscous stress tensor.

---

# 6. Newtonian Constitutive Relation

For a Newtonian fluid, the viscous stress tensor is proportional to the strain-rate tensor.

The strain-rate tensor is:

$$\boldsymbol{\varepsilon}(\mathbf{u})=\frac{1}{2}(\nabla\mathbf{u}+\nabla\mathbf{u}^{T})$$

The constitutive equation is:

$$\boldsymbol{\tau}=2\mu\boldsymbol{\varepsilon}(\mathbf{u})$$

where $\mu$ is the dynamic viscosity.

Therefore:

$$\boxed{\boldsymbol{\sigma}=-p\mathbf{I}+2\mu\boldsymbol{\varepsilon}(\mathbf{u})}$$

---

# 7. Conservative Form of Navier–Stokes Equations

Substituting the constitutive relation into the momentum equation gives:

**Conservative Navier–Stokes momentum equation:**

$$\boxed{\frac{\partial(\rho\mathbf{u})}{\partial t}+\nabla\cdot(\rho\mathbf{u}\otimes\mathbf{u})=-\nabla p+\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}))+\rho\mathbf{b}}$$

Together with:

$$\boxed{\nabla\cdot\mathbf{u}=0}$$

these equations define incompressible flow.

---

# 8. Non-Conservative Form

The convective term can be expanded using:

$$\nabla\cdot(\mathbf{u}\otimes\mathbf{u})=(\mathbf{u}\cdot\nabla)\mathbf{u}+(\nabla\cdot\mathbf{u})\mathbf{u}$$

For incompressible flow:

$$\nabla\cdot\mathbf{u}=0$$

Therefore:

$$\nabla\cdot(\mathbf{u}\otimes\mathbf{u})=(\mathbf{u}\cdot\nabla)\mathbf{u}$$

The momentum equation becomes:

**Advective form of the momentum equation:**

$$\boxed{\rho\left(\frac{\partial\mathbf{u}}{\partial t}+(\mathbf{u}\cdot\nabla)\mathbf{u}\right)=-\nabla p+\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}))+\rho\mathbf{b}}$$

---

# 9. Conservative Versus Non-Conservative Form

At the continuous level, the conservative and non-conservative forms are equivalent when:

$$\nabla\cdot\mathbf{u}=0$$

is satisfied exactly.

However, finite element approximations generally satisfy:

$$\nabla\cdot\mathbf{u}_h\neq0$$

Therefore, after discretization, the two formulations may have different numerical properties:

- conservation behavior,
- kinetic-energy behavior,
- stability,
- nonlinear convergence.

This distinction becomes important in turbulence modeling and stabilized formulations such as RBVMS.

---

# 10. Boundary Conditions

The computational boundary is divided into:

$$\partial\Omega=\Gamma_D\cup\Gamma_N$$

where:

$$\Gamma_D\cap\Gamma_N=\emptyset$$

---

## 10.1 Dirichlet Boundary Condition

Velocity is prescribed:

$$\boxed{\mathbf{u}=\bar{\mathbf{u}}\quad\text{on }\Gamma_D}$$

Examples:

- inlet velocity,
- no-slip wall,
- moving boundary.

---

## 10.2 Neumann Boundary Condition

Traction is prescribed:

$$\boxed{\boldsymbol{\sigma}\mathbf{n}=\bar{\mathbf{t}}\quad\text{on }\Gamma_N}$$

---

# 11. Initial Condition

For transient problems:

$$\boxed{\mathbf{u}(\mathbf{x},0)=\mathbf{u}_0(\mathbf{x})}$$

---

# 12. Summary

The strong form of the incompressible Navier–Stokes equations consists of:

**Momentum equation:**

$$\boxed{\rho\left(\frac{\partial\mathbf{u}}{\partial t}+(\mathbf{u}\cdot\nabla)\mathbf{u}\right)=-\nabla p+\nabla\cdot(2\mu\boldsymbol{\varepsilon}(\mathbf{u}))+\rho\mathbf{b}}$$

**Continuity equation:**

$$\boxed{\nabla\cdot\mathbf{u}=0}$$

These equations provide the starting point for the weak formulation and the RBVMS derivation.

---

# References

1. R. Temam, *Navier–Stokes Equations: Theory and Numerical Analysis*.

2. P. G. Ciarlet, *The Finite Element Method for Elliptic Problems*.

3. T. J. R. Hughes, *The Finite Element Method: Linear Static and Dynamic Finite Element Analysis*.

4. R. Codina, "Stabilization of incompressibility and convection through orthogonal sub-scales in finite element methods."
