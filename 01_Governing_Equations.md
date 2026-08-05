# Chapter 1 — Governing Equations

---

# Objective

This chapter introduces the governing equations of incompressible fluid flow that form the basis of the Residual-Based Variational Multiscale (RBVMS) method.

The presentation begins with the conservation laws of continuum mechanics and derives the incompressible Navier–Stokes equations in both conservative and non-conservative forms. The relationship between these formulations is established, along with the constitutive equation for a Newtonian fluid and the associated boundary conditions.

These equations constitute the **strong form** of the problem. All subsequent chapters derive the weak formulation, finite element discretization, and RBVMS stabilization directly from this mathematical model.

---

# 1. Conservation Laws

Fluid motion is governed by three fundamental conservation principles:

1. Conservation of mass
2. Conservation of linear momentum
3. Conservation of energy

For incompressible isothermal flows, only the first two are required.

---

# 2. Conservation of Mass

For an arbitrary control volume $\Omega$, conservation of mass is

$$\frac{d}{dt} \int_{\Omega} \rho\,d\Omega + \int_{\partial\Omega} \rho\mathbf{u}\cdot\mathbf{n}\,d\Gamma = 0. $$

Applying the divergence theorem,

$$
\frac{\partial\rho}{\partial t}
+
\nabla\cdot(\rho\mathbf{u})
=
0.
$$

This is the continuity equation.

## Constant Density

For incompressible flow,

$$
\rho=\text{constant},
$$

which gives

$$
\nabla\cdot\mathbf{u}=0.
$$

This states that the velocity field is divergence free.

---

# 3. Conservation of Linear Momentum

Newton's second law applied to a control volume gives

$$
\frac{d}{dt}
\int_{\Omega}
\rho\mathbf{u}\,d\Omega
=
\int_{\partial\Omega}
\boldsymbol{\sigma}\mathbf{n}\,d\Gamma
+
\int_{\Omega}
\mathbf{f}\,d\Omega.
$$

Applying the divergence theorem,

$$
\frac{\partial(\rho\mathbf{u})}{\partial t}
+
\nabla\cdot(\rho\mathbf{u}\otimes\mathbf{u})
=
\nabla\cdot\boldsymbol{\sigma}
+
\mathbf{f}.
$$

This is the conservative momentum equation.

---

# 4. Cauchy Stress Tensor

The stress tensor is decomposed into pressure and viscous stress,

$$
\boldsymbol{\sigma}
=
-p\mathbf{I}
+
\boldsymbol{\tau}.
$$

where

- $p$ is the thermodynamic pressure,
- $\mathbf{I}$ is the identity tensor,
- $\boldsymbol{\tau}$ is the viscous stress tensor.

---

# 5. Newtonian Constitutive Law

For a Newtonian fluid,

$$
\boldsymbol{\tau}
=
2\mu\boldsymbol{\varepsilon}(\mathbf{u}),
$$

where

$$
\boldsymbol{\varepsilon}(\mathbf{u})
=
\frac12
\left(
\nabla\mathbf{u}
+
\nabla\mathbf{u}^T
\right)
$$

is the strain-rate tensor.

Therefore,

$$
\boldsymbol{\sigma}
=
-p\mathbf{I}
+
2\mu\boldsymbol{\varepsilon}(\mathbf{u}).
$$

---

# 6. Conservative Navier–Stokes Equations

Substituting the constitutive relation into the momentum equation yields

$$
\frac{\partial(\rho\mathbf{u})}{\partial t}
+
\nabla\cdot(\rho\mathbf{u}\otimes\mathbf{u})
=
-\nabla p
+
\nabla\cdot
\left(
2\mu\boldsymbol{\varepsilon}(\mathbf{u})
\right)
+
\mathbf{f}.
$$

Together with

$$
\nabla\cdot\mathbf{u}=0,
$$

these form the incompressible Navier–Stokes equations in conservative form.

---

# 7. Non-Conservative (Advective) Form

Using the tensor identity

$$
\nabla\cdot(\mathbf{u}\otimes\mathbf{u})
=
(\mathbf{u}\cdot\nabla)\mathbf{u}
+
(\nabla\cdot\mathbf{u})\mathbf{u},
$$

and the incompressibility constraint

$$
\nabla\cdot\mathbf{u}=0,
$$

the momentum equation becomes

$$
\rho
\left(
\frac{\partial\mathbf{u}}{\partial t}
+
(\mathbf{u}\cdot\nabla)\mathbf{u}
\right)
=
-\nabla p
+
\nabla\cdot
\left(
2\mu\boldsymbol{\varepsilon}(\mathbf{u})
\right)
+
\mathbf{f}.
$$

This is commonly called the **advective** or **non-conservative** form.

---

# 8. Conservative vs. Non-Conservative Forms

For the continuous equations,

$$
\nabla\cdot\mathbf{u}=0,
$$

so both formulations are mathematically equivalent.

However, finite element approximations satisfy incompressibility only approximately,

$$
\nabla\cdot\mathbf{u}_h
\neq
0,
$$

which means the two forms are no longer identical at the discrete level.

This affects

- momentum conservation,
- kinetic-energy conservation,
- nonlinear linearization,
- stabilization,
- and numerical robustness.

Later chapters discuss why many RBVMS formulations use the advective form, while conservative and skew-symmetric formulations are often preferred for improved conservation properties.

---

# 9. Boundary Conditions

The computational boundary is partitioned into

$$
\partial\Omega
=
\Gamma_D
\cup
\Gamma_N,
$$

where

$$
\Gamma_D
\cap
\Gamma_N
=
\varnothing.
$$

## Dirichlet Boundary Conditions

Velocity is prescribed,

$$
\mathbf{u}
=
\bar{\mathbf{u}}
\qquad
\text{on }
\Gamma_D.
$$

Typical examples include

- inflow velocity,
- no-slip walls,
- moving walls.

## Neumann Boundary Conditions

Traction is prescribed,

$$
\boldsymbol{\sigma}\mathbf{n}
=
\bar{\mathbf{t}}
\qquad
\text{on }
\Gamma_N.
$$

Typical examples include

- outlet pressure,
- imposed surface traction,
- stress boundary conditions.

---

# 10. Initial Conditions

Transient simulations require

$$
\mathbf{u}(\mathbf{x},0)
=
\mathbf{u}_0(\mathbf{x}).
$$

Pressure is typically initialized consistently with the momentum equations.

---

# 11. Summary

The incompressible Navier–Stokes equations consist of

- continuity,
- momentum,
- constitutive law,
- boundary conditions,
- initial conditions.

These equations define the **strong form** of the problem.

All finite element formulations—including RBVMS—begin with these governing equations.

---

# C++ Mapping

| Mathematical Quantity | C++ Representation |
|-----------------------|--------------------|
| Velocity $\mathbf{u}$ | `Eigen::VectorXd velocity` |
| Pressure $p$ | `Eigen::VectorXd pressure` |
| Density $\rho$ | `Material::density()` |
| Dynamic viscosity $\mu$ | `Material::viscosity()` |
| Body force $\mathbf{f}$ | `BodyForce` |
| Stress tensor $\boldsymbol{\sigma}$ | `computeStress()` |

---

# Key Takeaways

- The Navier–Stokes equations are the starting point for RBVMS.
- Conservative and non-conservative forms are equivalent only for exactly divergence-free velocity fields.
- The finite element method begins from the strong form and derives a weak (variational) formulation.
- The next chapter introduces weighted residuals and the variational formulation that underpins both classical Galerkin FEM and RBVMS.

---

## Next Chapter

➡ **Chapter 2 — Variational Formulation**
