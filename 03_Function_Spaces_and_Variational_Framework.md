# Chapter 3 — Function Spaces and Variational Framework

---

# 1. Introduction

The weak formulation derived in Chapter 2 requires the definition of appropriate spaces for the unknown fields and test functions.

The velocity and pressure fields cannot be arbitrary functions. They must belong to mathematical spaces that provide:

- sufficient smoothness,
- well-defined derivatives,
- meaningful integrals,
- stability of the variational problem.

The framework of **Sobolev spaces** provides the mathematical foundation for finite element methods.

---

# 2. Hilbert Spaces

A Hilbert space is a complete vector space equipped with an inner product.

For two functions $u$ and $v$, the inner product is denoted as:

$$ (u,v) $$

and defines the norm:

$$ \|u\|=\sqrt{(u,u)} $$

Completeness means that every Cauchy sequence in the space converges to a function that remains inside the space.

The weak formulation of partial differential equations is typically posed in Hilbert spaces.

---

# 3. The $L^2(\Omega)$ Space

The space $L^2(\Omega)$ contains square-integrable functions.

A function belongs to $L^2(\Omega)$ if:

$$\int_{\Omega}|u|^2d\Omega<\infty$$

The inner product is:

$$ (u,v)_{L^2(\Omega)}=\int_{\Omega}uv\,d\Omega $$

The associated norm is:

$$\|u\|_{L^2(\Omega)}=\left(\int_{\Omega}|u|^2d\Omega\right)^{1/2}$$

The $L^2$ space is commonly used for:

- source terms,
- pressure fields,
- residuals.

---

# 4. Sobolev Spaces

The finite element formulation involves derivatives of the solution. Therefore, we require spaces that include information about derivatives.

The Sobolev space $H^1(\Omega)$ is defined as:

$$H^1(\Omega)=\{u\in L^2(\Omega):\nabla u\in L^2(\Omega)\}$$

In words:

A function belongs to $H^1(\Omega)$ if both the function and its first derivatives are square-integrable.

The corresponding norm is:

$$\|u\|_{H^1(\Omega)}=\left(\int_{\Omega}u^2d\Omega+\int_{\Omega}|\nabla u|^2d\Omega\right)^{1/2}$$

---

# 5. Velocity Function Space

The velocity field requires first derivatives because of the viscous term.

Therefore, velocity belongs to:

$$\mathbf{u}\in H^1(\Omega)^d$$

where:

- $d=2$ for two-dimensional flow,
- $d=3$ for three-dimensional flow.

The velocity trial space is:

$$V=\{\mathbf{u}\in H^1(\Omega)^d:\mathbf{u}=\bar{\mathbf{u}}\text{ on }\Gamma_D\}$$

The corresponding test space is:

$$V_0=\{\mathbf{v}\in H^1(\Omega)^d:\mathbf{v}=0\text{ on }\Gamma_D\}$$

---

# 6. Pressure Function Space

The pressure appears through its gradient in the momentum equation.

The pressure space is generally:

$$p\in L^2(\Omega)$$

The pressure space is:

$$Q=\{p\in L^2(\Omega)\}$$

Unlike velocity, pressure does not require first derivatives in the weak formulation.

---

# 7. Variational Problem Statement

The incompressible Navier–Stokes problem can now be written as:

Find:

$$ (\mathbf{u},p)\in V\times Q $$

such that for every:

$$ (\mathbf{v},q)\in V_0\times Q $$

the following conditions are satisfied:

Momentum equation:

$$\rho(\frac{\partial\mathbf{u}}{\partial t},\mathbf{v})+c(\mathbf{u};\mathbf{u},\mathbf{v})+a(\mathbf{u},\mathbf{v})-b(\mathbf{v},p)=F(\mathbf{v})$$

Continuity equation:

$$b(\mathbf{u},q)=0$$

---

# 8. Velocity-Pressure Coupling

The pressure-velocity interaction is described by:

$$b(\mathbf{v},p)=\int_{\Omega}p\nabla\cdot\mathbf{v}d\Omega$$

and:

$$b(\mathbf{u},q)=\int_{\Omega}q\nabla\cdot\mathbf{u}d\Omega$$

The pressure acts as a Lagrange multiplier enforcing incompressibility.

The incompressibility constraint does not determine pressure directly. Instead, pressure adjusts itself so that:

$$\nabla\cdot\mathbf{u}=0$$

is satisfied.

---

# 9. Finite Element Subspaces

The continuous spaces are infinite dimensional.

The finite element method introduces finite-dimensional approximations:

$$V_h\subset V$$

and:

$$Q_h\subset Q$$

The approximate solution is:

$$\mathbf{u}_h\in V_h$$

and:

$$p_h\in Q_h$$

The finite element problem becomes:

Find:

$$ (\mathbf{u}_h,p_h)\in V_h\times Q_h $$

such that:

$$\rho(\frac{\partial\mathbf{u}_h}{\partial t},\mathbf{v}_h)+c(\mathbf{u}_h;\mathbf{u}_h,\mathbf{v}_h)+a(\mathbf{u}_h,\mathbf{v}_h)-b(\mathbf{v}_h,p_h)+b(\mathbf{u}_h,q_h)=F(\mathbf{v}_h)$$

for all:

$$ (\mathbf{v}_h,q_h)\in V_h\times Q_h $$

---

# 10. The Inf-Sup Condition

The incompressible Navier–Stokes equations contain a saddle-point structure because velocity and pressure are coupled.

The velocity-pressure pair must satisfy the Ladyzhenskaya–Babuška–Brezzi (LBB) condition.

The inf-sup condition is:

$$\boxed{\inf_{q_h\in Q_h}\sup_{\mathbf{v}_h\in V_h}\frac{b(\mathbf{v}_h,q_h)}{\|\mathbf{v}_h\|_{V}\|q_h\|_{Q}}\geq\beta>0}$$

where $\beta$ is a constant independent of mesh size.

---

# 11. Importance of the Inf-Sup Condition

If the finite element spaces violate the inf-sup condition:

- pressure oscillations may occur,
- the discrete system becomes unstable,
- the pressure solution may become nonphysical.

A classical example is equal-order interpolation:

$$\mathbf{u}_h\in P_1$$

and

$$p_h\in P_1$$

which generally violates the inf-sup condition.

---

# 12. Stabilization Motivation

The need to satisfy the inf-sup condition motivates stabilization methods.

Examples include:

- Taylor–Hood elements,
- bubble enrichment,
- pressure stabilization,
- PSPG stabilization,
- Variational Multiscale methods.

RBVMS provides stabilization by modeling the unresolved scales rather than modifying the interpolation spaces.

The central idea is:

$$\boxed{\text{Unresolved scales provide the stabilization required for stable computation}}$$

---

# 13. Connection to Variational Multiscale Methods

The function space is decomposed into resolved and unresolved components:

$$\boxed{V=V_h\oplus V'}$$

where:

- $V_h$ is the resolved finite element space,
- $V'$ is the unresolved fine-scale space.

The solution is decomposed as:

$$\boxed{\mathbf{u}=\mathbf{u}_h+\mathbf{u}'}$$

This decomposition is the foundation of the Variational Multiscale method.

---

# 14. Summary

The mathematical framework required for RBVMS consists of:

1. Hilbert spaces.
2. Sobolev spaces.
3. Velocity and pressure spaces.
4. Finite element subspaces.
5. Inf-sup stability condition.
6. Resolution of unresolved scales.

The failure of classical Galerkin FEM for convection-dominated flows and incompressible flow motivates the introduction of variational multiscale concepts.

---

# References

1. R. A. Adams and J. J. F. Fournier, *Sobolev Spaces*.

2. P. G. Ciarlet, *The Finite Element Method for Elliptic Problems*.

3. F. Brezzi and M. Fortin, *Mixed and Hybrid Finite Element Methods*.

4. T. J. R. Hughes, "Multiscale phenomena: Green's functions, the Dirichlet-to-Neumann formulation, subgrid scale models, bubbles and the origins of stabilized methods."
