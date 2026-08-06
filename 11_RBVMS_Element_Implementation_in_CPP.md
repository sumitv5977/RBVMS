# Chapter 11 — RBVMS Element Implementation in C++

---

# 1. Introduction

The mathematical RBVMS formulation introduces additional stabilization terms:

$$\boxed{B_{RBVMS}=B_G+B_{SUPG}+B_{PSPG}+B_{GD}}$$

At the finite element level, these terms must be converted into:

1. Element residual vector:

$$\mathbf{R}_e$$

2. Element Jacobian matrix:

$$\mathbf{J}_e$$

These are then assembled into the global system:

$$\boxed{\mathbf{R}=\sum_e\mathbf{R}_e}$$

$$\boxed{\mathbf{J}=\sum_e\mathbf{J}_e}$$

---

# 2. Element-Level Philosophy

A CFD finite element solver does not directly assemble the continuous equations.

Instead, every element performs:

1. Interpolation of variables.
2. Evaluation of gradients.
3. Computation of physical residuals.
4. Stabilization parameter calculation.
5. Residual and Jacobian contribution.

The overall process is:

$$\boxed{\text{Element physics}\rightarrow\text{Element assembly}\rightarrow\text{Global solver}}$$

---

# 3. Required Element Data

For RBVMS, each element requires:

## Geometry

- node coordinates,
- Jacobian matrix,
- determinant of Jacobian,
- shape functions,
- shape gradients.

---

## Material Properties

For incompressible flow:

$$\rho$$

and:

$$\mu$$

or:

$$\nu=\frac{\mu}{\rho}$$

---

## Flow Variables

At each integration point:

Velocity:

$$\mathbf{u}_h$$

Pressure:

$$p_h$$

Velocity gradient:

$$\nabla\mathbf{u}_h$$

Pressure gradient:

$$\nabla p_h$$

---

# 4. Element Residual Structure

The Navier–Stokes residual is:

$$\mathbf{R}_e=\mathbf{R}_{G}+\mathbf{R}_{RBVMS}$$

where:

$$\mathbf{R}_{G}$$

is the Galerkin contribution.

and:

$$\mathbf{R}_{RBVMS}$$

is the stabilization contribution.

---

# 5. Velocity Interpolation

The velocity field is approximated as:

$$\mathbf{u}_h=\sum_A N_A\mathbf{U}_A$$

The velocity gradient is:

$$\nabla\mathbf{u}_h=\sum_A\nabla N_A\mathbf{U}_A$$

In C++:

```cpp
Vector velocity =
    shapeFunctions.transpose()
    * nodalVelocity;


Matrix gradU =
    gradShapeFunctions
    * nodalVelocity;
```

---

# 6. Momentum Residual Evaluation

The momentum residual is:

$$\boxed{\mathbf{R}_m=
\rho\frac{\partial\mathbf{u}_h}{\partial t}
+\rho(\mathbf{u}_h\cdot\nabla)\mathbf{u}_h
+\nabla p_h
-\mu\nabla^2\mathbf{u}_h}
$$


The individual terms are:

## Transient term

$$\rho\frac{\partial\mathbf{u}_h}{\partial t}$$


## Convection term

$$\rho(\mathbf{u}_h\cdot\nabla)\mathbf{u}_h$$


## Pressure term

$$\nabla p_h$$


## Diffusion term

$$-\mu\nabla^2\mathbf{u}_h$$

---

# 7. Computing the Stabilization Tensor

At each Gauss point:

Compute velocity magnitude:

$$U=|\mathbf{u}_h|$$

Compute element size:

$$h_e$$

Compute viscosity:

$$\nu=\frac{\mu}{\rho}$$


Then:

$$\boxed{
\tau_m=
\left[
\left(\frac{C_t}{\Delta t}\right)^2
+
\left(\frac{U}{C_a h_e}\right)^2
+
\left(\frac{C_d\nu}{h_e^2}\right)^2
\right]^{-1/2}
}
$$


C++:

```cpp
double tau =
1.0 /
sqrt(
pow(Ct/dt,2)
+
pow(U/(Ca*h),2)
+
pow(Cd*nu/(h*h),2)
);
```

---

# 8. SUPG Residual Contribution

The SUPG term is:

$$
B_{SUPG}
=
\sum_e
(\tau_mR_m,
\rho(\mathbf{u}_h\cdot\nabla)v_h)
$$


At element level:

```cpp
elementResidual +=
tau *
Rm *
streamlineOperator;
```

---

# 9. PSPG Contribution

The PSPG stabilization term is:

$$
B_{PSPG}
=
(\tau_mR_m,\nabla q_h)
$$


This stabilizes pressure:

```cpp
pressureResidual +=
tau *
Rm *
pressureGradient;
```

---

# 10. Dynamic VMS Addition

For Dynamic VMS, each element stores:

$$\boxed{\mathbf{u}'_e}$$

Example:

```cpp
class DynamicSubscale
{

Eigen::VectorXd uPrime;

};
```

The update equation is:

$$\boxed{\mathbf{u}'^{n+1} = \frac{\mathbf{u}'^n -
\frac{\Delta t}{\rho}\mathbf{R}_m
}
{
1+\frac{\Delta t}{\tau_m}
}
}
$$


C++:

```cpp
uPrime =
(
uPrime
-
dt * Rm / rho
)
/
(
1.0 + dt/tau
);
```

---

# 11. Element Class Design

A possible architecture:

```cpp
class RBVMSElement
{

public:

    Matrix stiffness();

    Vector residual();

    Matrix jacobian();


private:

    MeshElement geometry;

    Material material;

    DynamicSubscale subscale;

};
```

---

# 12. Residual Assembly Algorithm

The element routine:

```cpp
Vector RBVMSElement::residual()
{

Vector Re;


for(GaussPoint gp : integrationPoints)
{

    computeShapeFunctions();

    computeVelocity();

    computeGradient();

    computeMomentumResidual();


    computeTau();


    addGalerkinTerm();


    addSUPGTerm();


    addPSPGTerm();


    updateDynamicSubscale();


}


return Re;

}
```

---

# 13. Jacobian Requirement

For nonlinear Navier–Stokes:

$$
\mathbf{J}
=
\frac{\partial\mathbf{R}}
{\partial\mathbf{U}}
$$


The Newton iteration is:

$$
\boxed{
\mathbf{J}\Delta\mathbf{U}
=
-\mathbf{R}
}
$$


The RBVMS Jacobian contains derivatives of:

- convection,
- stabilization,
- residual,
- dynamic subscale.

---

# 14. Integration With Existing FEM Framework

For a solver architecture:

```
Mesh
 |
Element
 |
ElementPhysics
 |
RBVMSNavierStokesElement
 |
GlobalAssembler
 |
LinearSolver
 |
NewtonSolver
```

RBVMS belongs inside the physics element.

---

# 15. Relation to Heat Solver Architecture

Your existing heat solver structure:

```
ElementPhysics
     |
Triangle3Heat
     |
GlobalAssembler
```

can be extended:

```
ElementPhysics
       |
       +-- HeatElement
       |
       +-- NavierStokesElement
                  |
                  +-- RBVMS
                  |
                  +-- DynamicSubscale
```

The mesh, DOF manager, sparse assembly, and solver infrastructure can remain unchanged.

---

# 16. Important Implementation Decisions

Before coding RBVMS:

1. Decide velocity-pressure formulation:

Examples:

- Taylor-Hood:
$$P_2-P_1$$

- Equal order:
$$P_1-P_1$$
with PSPG stabilization.

---

2. Decide time integration:

Examples:

- backward Euler,
- BDF2,
- generalized-alpha.

---

3. Decide nonlinear solver:

Usually:

Newton-Raphson:

$$J\Delta U=-R$$

---

# Summary

The RBVMS implementation requires:

1. Element residual evaluation.
2. Stabilization tensor calculation.
3. Residual-based stabilization.
4. Dynamic subscale storage for DVMS.
5. Consistent Jacobian assembly.

The key computational loop is:

$$
\boxed{
\text{Compute residual}
\rightarrow
\text{Compute }\tau
\rightarrow
\text{Update fine scales}
\rightarrow
\text{Assemble}
}
$$

This is the point where the theoretical RBVMS formulation becomes a CFD solver.

---

# References

1. Hughes, T. J. R., Feijóo, G. R., Mazzei, L., Quincy, J. Y., "The variational multiscale method — a paradigm for computational mechanics."

2. Bazilevs, Y., Calo, V. M., Cottrell, J. A., Hughes, T. J. R., "Variational multiscale modeling of turbulence."

3. Tezduyar, T. E., "Stabilized finite element formulations for incompressible flow computations."
