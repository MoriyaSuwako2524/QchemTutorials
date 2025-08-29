# Basic knowledge of Qchem

**Q-Chem** is a general-purpose electronic-structure package maintained and distributed by Q-Chem, Inc., headquartered in Pleasanton, California (USA). Founded in 1993, it is developed by a broad community of contributors. Within the CCATS group, Q-Chem serves as the primary platform for method development and production calculations.

## 1.**Q-Chem overview**

Q-Chem is a general-purpose electronic-structure package offering broad coverage from HF/DFT to advanced correlated and multireference methods. As of **August 23, 2025**, the latest release is **Q-Chem 6.3**. 

**Core capabilities (selected)**

1. **Ground-state SCF (HF/ROHF/UHF):** analytic gradients/Hessians, robust optimizers, and property analysis. 
2. **Density Functional Theory:** semilocal → double-hybrid functionals; energies, geometry optimizations, and vibrational analyses; TDDFT including spin-flip variants for challenging excitations.
3. **Post-HF correlation:** MP2 family (incl. OO/RI/dual-basis, MP3), coupled-cluster (CCSD, CCSD(T)) and a wide EOM-CC suite (EE/SF/IP/EA), plus ADC methods. 
4. **Strong correlation & active-space options:** CASSCF/CAS-CI, RAS-CI/RAS-SF, CCVB/PP, and spin-flip DFT. 
5. **Excited states (broad toolbox):** CIS, CIS(D), TDDFT/SF-TDDFT, ΔSCF via MOM/SGM, NOCI/NOCIS, EOM-CC, ADC; support for core-level excitations (CVS). 
6. **Embedding & extended systems:** stand-alone **QM/MM**, CHARMM interface, **QM/EFP**, and modern dielectric/embedding models. 
7. **PES exploration & optimization:** constrained and unconstrained optimizations (minima/TS), IRC paths, and relaxed scans. 

**Performance & parallelism**
Q-Chem supports **OpenMP** threading, **MPI** for distributed-memory runs. Usually one should run with $2^n$ numbers of cores. I ran some benchmark of the parallel jobs:





## 2. Installation/Compile of Qchem



