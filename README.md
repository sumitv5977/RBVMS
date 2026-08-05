# Residual-Based Variational Multiscale (RBVMS) Methods

*A graduate-level set of notes on the mathematical formulation, finite element implementation, and C++ development of Residual-Based Variational Multiscale (RBVMS) methods for incompressible flow.*

---

## Overview

These notes document the complete derivation and implementation of the **Residual-Based Variational Multiscale (RBVMS)** method for incompressible Navier–Stokes equations using the Finite Element Method (FEM).

The primary objective is to bridge the gap between:

- mathematical derivation,
- finite element formulation,
- numerical implementation,
- and modern C++ software design.

Unlike many texts that stop at the variational formulation, these notes continue all the way to:

- element residual evaluation,
- consistent tangent matrix (Jacobian),
- Newton–Raphson iteration,
- stabilization parameter derivation,
- dynamic VMS,
- and practical software implementation.

The long-term goal is to serve as the theoretical foundation for a research-grade FEM framework capable of solving:

- incompressible laminar flow,
- turbulent flow,
- flow over a circular cylinder,
- adaptive mesh refinement (AMR),
- and eventually large-scale parallel simulations.

---

# Repository Structure

```
RBVMS/

README.md

01_Governing_Equations.md

02_Variational_Formulation.md

03_Function_Spaces.md

04_VMS_Decomposition.md

05_Fine_Scale_Equation.md

06_Greens_Function_and_Tau.md

07_RBVMS_Weak_Form.md

08_Finite_Element_Discretization.md

09_Newton_Linearization.md

10_Dynamic_VMS.md

11_Dynamic_RBVMS_Implementation.md

12_Conservative_vs_Nonconservative.md

13_AMR_and_RBVMS.md

14_DG_vs_RBVMS.md

References.md
```

---

# Learning Roadmap

The chapters are organized in the same sequence that one would naturally derive the method.

## Part I — Governing Equations

- Navier–Stokes equations
- Conservative form
- Non-conservative form
- Stress tensor
- Boundary conditions

---

## Part II — Variational Formulation

- Sobolev spaces
- Weak formulation
- Integration by parts
- Boundary integrals
- Galerkin finite element method

---

## Part III — Variational Multiscale Method

- Scale decomposition
- Coarse scales
- Fine scales
- Projection operators
- Fine-scale equations
- Green's function approximation

---

## Part IV — Residual-Based VMS

- Residual modeling
- Stabilization tensor
- SUPG
- PSPG
- LSIC
- Stabilized weak form

---

## Part V — Finite Element Implementation

- Shape functions
- Isoparametric mapping
- Numerical integration
- Element residual
- Element Jacobian
- Global assembly
- Newton iteration

---

## Part VI — Dynamic VMS

- Dynamic fine scales
- Evolution equation
- Time discretization
- Dynamic residual model
- Storage of unresolved scales
- Implementation details

---

## Part VII — Advanced Topics

- Conservative vs non-conservative formulations
- Adaptive mesh refinement
- Discontinuous Galerkin methods
- Parallel implementation
- Future research directions

---

# Intended Audience

These notes are intended for:

- graduate students,
- CFD researchers,
- computational mechanics researchers,
- scientific software developers,
- finite element practitioners.

A basic familiarity with:

- calculus,
- linear algebra,
- partial differential equations,
- finite element methods,

is assumed.

---

# Software Perspective

Throughout these notes, every mathematical derivation is connected to its eventual implementation in C++.

Typical mappings include:

| Mathematics | Software |
|--------------|----------|
| Weak form | Element residual routine |
| Jacobian | Element tangent matrix |
| Stabilization tensor | `computeTau()` |
| Fine-scale model | `computeResidual()` |
| Newton linearization | Global nonlinear solver |
| Dynamic VMS | Gauss-point state variables |

---

# Software Architecture

The implementation follows a modular finite element framework.

```
Mesh
 │
 ▼
Element
 │
 ▼
Physics
 │
 ▼
Residual Evaluation
 │
 ▼
Jacobian Assembly
 │
 ▼
Linear Solver
 │
 ▼
Newton Iteration
```

Later chapters describe where RBVMS enters this workflow.

---

# Current Status

The notes are organized as an evolving document.

Each chapter is self-contained and can be read independently.

The repository is expected to grow with future topics including:

- compressible flow,
- discontinuous Galerkin FEM,
- quadtree adaptive mesh refinement,
- GPU acceleration,
- PETSc/Trilinos integration,
- and large-scale parallel CFD.

---

# References

The derivations are primarily based on the foundational work of:

- Hughes
- Franca
- Mallet
- Bazilevs
- Codina
- Brooks
- Tezduyar

A detailed bibliography is provided in **References.md**.

---

# Next Chapter

➡ **01_Governing_Equations.md**
