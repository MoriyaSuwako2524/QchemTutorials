# Single-Point (SP) Calculation

A **single-point (SP)** calculation evaluates properties (most commonly the **electronic energy**) of a system at a **fixed geometry**. It’s the simplest job type in computational chemistry and a building block for scans, optimizations, and higher-level methods.

### Where the geometry comes from

- Manually built in a GUI (e.g., IQmol)
- From a prior **geometry optimization**
- From **experiment** (X-ray, neutron, cryo-EM models)
- From **supplementary information** (SI) in papers
- **Automatically generated** by other programs or scripts (ASE, RDKit, Amber, etc.)

### What Q-Chem does in a default SP run

1. Print license banner and echo the input.
2. Initialize the molecular structure and basis set.
3. Build an **initial guess** for the wavefunction and evaluate its energy.
4. Run **self-consistent field (SCF)** iterations until converged.
5. If requested, perform **additional models** (DFT options, TDDFT roots, post-HF correlation, etc.).
6. Analyze the wavefunction: orbital energies (HOMO–LUMO), charges, multipole moments, and other requested properties.
7. Write a summary and timings, then exit.
