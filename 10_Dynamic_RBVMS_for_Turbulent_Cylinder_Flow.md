# Chapter 10 — Dynamic RBVMS for Turbulent Cylinder Flow

---

# 1. Introduction

The flow over a circular cylinder is one of the most widely used benchmark problems in computational fluid dynamics.

It is important because it contains several fundamental flow phenomena:

- separation,
- vortex shedding,
- transition to turbulence,
- wake instability,
- turbulent energy transfer.

The problem is therefore an excellent test case for Variational Multiscale turbulence modeling.

The governing equations are the incompressible Navier–Stokes equations:

$$\boxed{\rho\frac{\partial\mathbf{u}}{\partial t}+\rho(\mathbf{u}\cdot\nabla)\mathbf{u}+\nabla p-\mu\nabla^2\mathbf{u}=0}$$

$$\boxed{\nabla\cdot\mathbf{u}=0}$$

---

# 2. Cylinder Flow Configuration

The computational domain consists of:

- inlet boundary,
- cylinder surface,
- outlet boundary,
- far-field boundaries.

Typical conditions:

## Inlet

Prescribed velocity:

$$\mathbf{u}=U_\infty$$

---

## Cylinder Wall

No-slip condition:

$$\mathbf{u}=0$$

---

## Outlet

Often:

$$p=0$$

or a convective outflow condition.

---

# 3. Reynolds Number

The dominant parameter is the Reynolds number:

$$\boxed{Re=\frac{U_\infty D}{\nu}}$$

where:

- $U_\infty$ is free-stream velocity,
- $D$ is cylinder diameter,
- $\nu$ is kinematic viscosity.

The flow regime depends strongly on $Re$.

---

# 4. Flow Regimes

## Low Reynolds Number

$$Re<47$$

The flow is steady.

No vortex shedding occurs.

---

## Intermediate Reynolds Number

$$47<Re<200$$

Periodic vortex shedding develops.

The wake contains:

- alternating vortices,
- unsteady pressure fluctuations.

This is the classical von Kármán vortex street.

---

## Transitional Regime

$$200<Re<1000$$

Three-dimensional instabilities develop.

The wake becomes increasingly complex.

---

## High Reynolds Number

$$Re>1000$$

The flow becomes turbulent.

A large range of scales appears.

---

# 5. Why Dynamic RBVMS is Needed

The turbulent wake contains:

Large scales:

- vortex structures,
- separation bubbles.

Intermediate scales:

- vortex stretching,
- instability mechanisms.

Small scales:

- dissipation range.

A finite element mesh resolves only part of this spectrum.

Therefore:

$$\boxed{\mathbf{u}=\mathbf{u}_h+\mathbf{u}'}$$

where:

- $\mathbf{u}_h$ contains resolved vortex structures,
- $\mathbf{u}'$ models unresolved turbulence.

---

# 6. Dynamic Fine-Scale Equation

The unresolved velocity satisfies:

$$\boxed{\frac{d\mathbf{u}'}{dt}+\tau_m^{-1}\mathbf{u}'=-\frac{1}{\rho}\mathbf{R}_m}$$

where:

$$\mathbf{R}_m=\rho\frac{\partial\mathbf{u}_h}{\partial t}+\rho(\mathbf{u}_h\cdot\nabla)\mathbf{u}_h+\nabla p_h-\mu\nabla^2\mathbf{u}_h$$

---

# 7. Physical Interpretation in Cylinder Wake

When a vortex forms:

1. The resolved velocity field changes rapidly.

2. The momentum residual increases:

$$|\mathbf{R}_m|\uparrow$$

3. Fine scales are generated:

$$|\mathbf{u}'|\uparrow$$

4. Fine scales dissipate energy through the stabilization mechanism.

The energy pathway becomes:

$$\boxed{\text{Mean flow}\rightarrow\text{Large vortices}\rightarrow\text{Unresolved turbulence}\rightarrow\text{Dissipation}}$$

---

# 8. Dynamic VMS Compared with LES

Traditional LES introduces a filtered equation:

$$\bar{\mathbf{u}}$$

and models the subgrid stress:

$$\tau_{ij}=\overline{u_i u_j}-\bar{u_i}\bar{u_j}$$

Dynamic VMS does not explicitly introduce filtering.

Instead:

$$\boxed{\text{Unresolved scales arise from the variational decomposition}}$$

The finite element mesh defines the separation between resolved and unresolved scales.

---

# 9. Dynamic Subscale Update

At each time step:

$$t^n\rightarrow t^{n+1}$$

the algorithm performs:

## Step 1

Compute resolved velocity:

$$\mathbf{u}_h^{n+1}$$

---

## Step 2

Compute residual:

$$\mathbf{R}_m^{n+1}$$

---

## Step 3

Update unresolved scale:

$$\boxed{\mathbf{u}'^{n+1}=\frac{\mathbf{u}'^n-\frac{\Delta t}{\rho}\mathbf{R}_m^{n+1}}{1+\frac{\Delta t}{\tau_m}}}$$

---

## Step 4

Add fine-scale contribution:

$$B_{DVMS}=(\mathbf{u}',\mathcal{L}^*(\mathbf{v}_h))$$

---

# 10. Element-Level Implementation

For every finite element:

The element stores:

```cpp
VectorXd fineScaleVelocity;
```

Example:

```cpp
class DynamicSubscale
{
public:

    Eigen::VectorXd uPrime;

    void update(
        const Eigen::VectorXd& residual,
        double dt,
        double tau,
        double rho)
    {
        uPrime =
        (uPrime - dt * residual / rho)
        /
        (1.0 + dt / tau);
    }
};
```

The fine scale becomes an element-level internal variable.

---

# 11. Residual Assembly

The standard Galerkin residual:

$$R_G$$

is:

$$R_G=R_{mass}+R_{conv}+R_{diff}+R_{pressure}$$

Dynamic RBVMS adds:

$$R_{RBVMS}=R_G+R_{subscale}$$

where:

$$R_{subscale}=(\mathbf{u}',\mathcal{L}^*(v_h))$$

---

# 12. Jacobian Contribution

For Newton iterations:

$$\mathbf{J}\Delta U=-R$$

the tangent matrix requires:

$$\frac{\partial R}{\partial U}$$

Dynamic RBVMS introduces additional derivatives:

$$\frac{\partial\mathbf{u}'}{\partial U}$$

because:

$$\mathbf{u}'=\mathbf{u}'(R_m(U))$$

This makes the implementation more involved.

---

# 13. Computational Cost

Compared with static RBVMS:

Additional requirements:

| Component | Static RBVMS | Dynamic RBVMS |
|-|-|-|
| Residual evaluation | Yes | Yes |
| Stabilization tensor | Yes | Yes |
| Fine-scale storage | No | Yes |
| Fine-scale update | No | Yes |
| Additional Jacobian terms | Limited | More complex |

The increase in effort is moderate, not a complete solver redesign.

---

# 14. Application Workflow for Cylinder Simulation

A practical solver sequence:

```
Initialize mesh

Initialize velocity and pressure

Initialize fine scales uPrime = 0

For each timestep:

    Solve nonlinear Navier-Stokes

        Assemble element residual

            Compute:
                velocity gradient
                pressure gradient
                momentum residual

            Compute:
                tau_m

            Update:
                dynamic subscale

            Add:
                RBVMS stabilization

        Newton iteration

    Output:
        velocity field
        pressure field
        vorticity
        forces
```

---

# 15. Quantities of Interest

For cylinder flow, important outputs are:

## Drag coefficient

$$\boxed{C_D=\frac{F_D}{\frac12\rho U_\infty^2D}}$$

---

## Lift coefficient

$$\boxed{C_L=\frac{F_L}{\frac12\rho U_\infty^2D}}$$

---

## Strouhal number

$$\boxed{St=\frac{fD}{U_\infty}}$$

where:

- $f$ is vortex shedding frequency.

---

# 16. Validation

A Dynamic RBVMS cylinder solver should reproduce:

- vortex shedding frequency,
- mean drag coefficient,
- lift oscillations,
- wake velocity profile.

Comparison is usually performed against:

- experimental data,
- benchmark DNS results,
- high-resolution simulations.

---

# Summary

Dynamic RBVMS extends RBVMS by allowing unresolved scales to evolve.

The key equation is:

$$\boxed{\frac{d\mathbf{u}'}{dt}+\tau_m^{-1}\mathbf{u}'=-\frac{1}{\rho}\mathbf{R}_m}$$

For cylinder flow:

- resolved scales capture vortex dynamics,
- unresolved scales model turbulence,
- dynamic subscales provide temporal memory.

Implementation requires:

1. element-level storage of $\mathbf{u}'$,
2. dynamic update at every timestep,
3. stabilization residual contribution,
4. consistent Jacobian treatment.

---

# References

1. Hughes, T. J. R., Feijóo, G. R., Mazzei, L., Quincy, J. Y., "The variational multiscale method — a paradigm for computational mechanics."

2. Bazilevs, Y., Calo, V. M., Cottrell, J. A., Hughes, T. J. R., "Variational multiscale modeling of turbulence."

3. Tezduyar, T. E., "Stabilized finite element formulations for incompressible flow computations."
