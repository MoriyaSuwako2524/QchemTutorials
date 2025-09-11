# Quantum Chemistry Theory Basic

> **Note:** This part is for a quick introduction to Quantum Chemistry. For a more comprehensive study, I recommend *Modern Quantum Chemistry* by Szabo.

Quantum chemistry calculations face the central challenge of solving the Schrödinger equation for many-body systems. Because the Schrödinger equation is too complex to be solved exactly, in practical numerical calculations various approximations are always introduced to reduce computational complexity and cost. Common approximations include:

- Born–Oppenheimer approximation
- Finite basis set approximation
- Single-electron approximation, electron correlation approximations
- Relativistic approximations
- Approximations in numerical accuracy of computation

In most cases, the accuracy of quantum chemistry calculations depends critically on two aspects: the theoretical method and the basis set.

## I. Born–Oppenheimer (BO) Approximation  

To rigorously solve the Schrödinger equation of a molecular system, both electrons and nuclei must be treated quantum mechanically, which makes the problem very complex:  

$$
\hat{H}_{\text{tot}} \, \Psi_{\text{tot}}(\mathbf{R}; \mathbf{r}) 
= E_{\text{tot}} \, \Psi_{\text{tot}}(\mathbf{R}; \mathbf{r})
$$

Since nuclear masses are much larger than electron masses, quantum effects of nuclei are much smaller than those of electrons. The BO approximation is one of the most important approximations in quantum chemistry. It separates nuclear and electronic motions, thereby reducing the complexity of solving the molecular Schrödinger equation:  

$$
\Psi_{\text{tot}}(\mathbf{R}; \mathbf{r}) 
= \Psi_{\text{nuc}}(\mathbf{R}) \, \Psi_{\text{ele}}^{(\mathbf{R})}(\mathbf{r})
$$

---

### 1.1 Electronic Schrödinger Equation  

Under the BO approximation, when solving the electronic wavefunction, the nuclear positions are regarded as fixed parameters. Thus, the BO approximation is also referred to as the fixed-nuclei approximation.  

For each set of nuclear coordinates $\mathbf{R}$, the corresponding electronic energy $E_{\text{ele}}(\mathbf{R})$ can be obtained by solving the electronic Schrödinger equation. This energy is called the potential energy or potential energy surface (PES) under the BO approximation:  

$$
\hat{H}_{\text{ele}}^{(\mathbf{R})} \Psi_{\text{ele}}^{(\mathbf{R})}(\mathbf{r}) 
= E_{\text{ele}}^{(\mathbf{R})} \Psi_{\text{ele}}^{(\mathbf{R})}(\mathbf{r})
$$

To describe nuclear motion, one must further solve the nuclear Schrödinger equation, which includes the nuclear kinetic energy operator and the interaction potential:  

$$
\left[ \hat{T}_{\text{nuc}} + E_{\text{pot}}(\mathbf{R}) \right] 
\Psi_{\text{nuc}}(\mathbf{R}) 
= E_{\text{tot}} \Psi_{\text{nuc}}(\mathbf{R})
$$

---

### 1.2 Beyond the BO Approximation  

For directly solving the nuclear + electronic system, using the BO approximation to obtain the nuclear wavefunction and $E_{\text{tot}}$, the following corrections may be introduced:  

1. **Non-adiabatic correction**: This introduces the non-adiabatic coupling matrix elements (NACME), which describe the coupling between different electronic states and nuclear motion.  
2. **Diagonal correction**: The diagonal part corresponding to NACME, used to correct the coupling between a single electronic state and nuclear motion; this contribution is usually small.  
3. **Mass polarization correction**: Arising from the coupling between the internal motion of the electrons and the center-of-mass motion of the nuclei.  

*For details, see* *Introduction to Computational Chemistry* (3rd edition), Section 3.1.  

---

### 1.3 Diagonal Born–Oppenheimer Correction (DBOC)  

When no strong intersection of two potential energy surfaces occurs, the BO approximation is often very reasonable. Errors in the electronic part of the calculation are mainly due to the neglect of nuclear motion. If further corrections are required, one can add the **Diagonal Born–Oppenheimer Correction (DBOC)** to the electronic energy:  

$$
\Delta E_{\text{DBOC}} = 
\sum_A^{\text{atoms}} 
- \frac{1}{2M_A} 
\left\langle \Psi_{\text{ele}} \middle| \nabla_A^2 \middle| \Psi_{\text{ele}} \right\rangle
$$

In molecular dynamics simulations involving nuclear motion, potential energy surfaces alone are not sufficient; one must also consider the coupling between electronic and nuclear motion. The **non-adiabatic coupling matrix elements (NACME)** then become particularly important. In such cases, although the BO approximation provides a useful starting point, the electronic Schrödinger equation must often be solved repeatedly with explicit inclusion of NACME terms.  



## II. Electron Correlation  

In practical quantum chemical calculations, the molecular Hamiltonian is written as:  

$$
\hat{H}_{\text{ele}}^{(\mathbf{R})} 
= \sum_i \left[ -\frac{1}{2} \nabla_i^2 - \sum_A \frac{Z_A}{|\mathbf{r}_i - \mathbf{R}_A|} \right] 
+ \sum_{i>j} \frac{1}{r_{ij}} 
+ \sum_{A>B} \frac{Z_A Z_B}{|\mathbf{R}_A - \mathbf{R}_B|}
$$

where the terms correspond to **electron kinetic energy**, **electron–nucleus attraction**, **electron–electron repulsion**, and **nucleus–nucleus repulsion**. The first bracketed part is the **one-electron part**.  

---

### 2.1 Electron–Electron Interaction in Terms of Density  

The electron–electron interaction can also be expressed in terms of the electron density, which provides a more intuitive physical picture:  

$$
E_{ee} = \left\langle \Psi \middle| \sum_{i>j} \frac{1}{r_{ij}} \middle| \Psi \right\rangle 
= \iint \frac{\rho(\mathbf{r}_1, \mathbf{r}_2)}{r_{12}} \, d\mathbf{r}_1 d\mathbf{r}_2
$$

The two-particle density can be decomposed as:  

$$
\rho(\mathbf{r}_1, \mathbf{r}_2) = 
[\rho^{\alpha\alpha}(\mathbf{r}_1, \mathbf{r}_2) + \rho^{\beta\beta}(\mathbf{r}_1, \mathbf{r}_2)] 
+ [\rho^{\alpha\beta}(\mathbf{r}_1, \mathbf{r}_2) + \rho^{\beta\alpha}(\mathbf{r}_1, \mathbf{r}_2)]
= \rho^{\uparrow\uparrow}(\mathbf{r}_1, \mathbf{r}_2) + \rho^{\uparrow\downarrow}(\mathbf{r}_1, \mathbf{r}_2)
$$

- **Same-spin pair density**  
  $$
  \rho^{\uparrow\uparrow}(\mathbf{r}_1, \mathbf{r}_2) 
  = \Gamma^{\uparrow\uparrow}(\mathbf{r}_1, \mathbf{r}_2) 
  = \rho^{\uparrow}(\mathbf{r}_1) \rho^{\uparrow}(\mathbf{r}_2) + \Gamma_X^{\uparrow\uparrow}(\mathbf{r}_1, \mathbf{r}_2)
  $$
- **Opposite-spin pair density**  
  $$
  \rho^{\uparrow\downarrow}(\mathbf{r}_1, \mathbf{r}_2) 
  = \Gamma^{\uparrow\downarrow}(\mathbf{r}_1, \mathbf{r}_2) 
  = \rho^{\uparrow}(\mathbf{r}_1) \rho^{\downarrow}(\mathbf{r}_2)
  $$

---

### 2.2 Exchange and Coulomb Correlation  

- **Exchange correlation ($X$)**: Arises from the Pauli exclusion principle (antisymmetry of the electronic wavefunction). This reduces the probability of finding two same-spin electrons close to each other. The corresponding energy lowering is the **exchange energy $E_X$**.  

- **Coulomb correlation ($C$)**: Due to Coulomb repulsion, electrons tend to avoid each other even beyond exchange effects, lowering the probability of electron–electron co-location. This further reduces the system energy, corresponding to the **correlation energy $E_C$**.  

Thus, electron correlation effects beyond exchange are essential to capture long-range Coulomb avoidance. Approximations neglecting this (e.g., Hartree–Fock) miss correlation energy, which must be included for quantitative accuracy.  

---

### Example: $H_2$  

Consider the hydrogen molecule:  

- **Correlation hole**: If one electron is known to be at a certain position, the probability of finding another electron nearby is reduced due to exchange and correlation effects.  
- **Fermi hole**: Reflects the Pauli principle; the minimum value is 0, showing that the probability of finding two same-spin electrons at the same position is zero.  
- **Coulomb hole**: Arises from Coulomb repulsion; the distribution shifts slightly to avoid close encounters even for opposite-spin electrons.  

Graphically, the **total hole** can be seen as a combination of the **Fermi hole** and **Coulomb hole**, shifting electron density to reduce repulsion.  



## III. Classification of Common Theoretical Methods  

- **Semi-empirical methods** (e.g., Semi-empirical, DFTB, GFN-xTB)  
- **Density functional theory (DFT)**  
- **Hartree–Fock (HF)**  
- **Perturbation theory**  
- **Coupled cluster (CC)**  
- **Configuration interaction (CI)**  
- **Multiconfigurational self-consistent field (MCSCF)**  
- **Multireference methods** (e.g., MRCI, CASPT2)  
- **Molecular mechanics (MM, non-quantum chemistry)**  

Among them, **ab initio** methods are those derived directly from first principles without empirical parameters. In practice, computational chemistry often balances accuracy and computational cost.  

DFT is currently the most widely used method because it achieves a balance between efficiency and accuracy. Although not as rigorous as wavefunction-based *post-HF* methods, DFT is still a mainstream tool.  

### 