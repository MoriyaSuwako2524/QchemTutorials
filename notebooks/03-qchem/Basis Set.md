# Basis Sets





## I. Basis Function

To solve the HF and KS-DFT equations, molecular orbitals are expanded as linear combinations of basis functions:

$$
\varphi_i(\mathbf{r}) = C_{1,i}\chi_1(\mathbf{r}) + C_{2,i}\chi_2(\mathbf{r}) + C_{3,i}\chi_3(\mathbf{r}) + \cdots + C_{N,i}\chi_N(\mathbf{r})
$$

- $\varphi_i(\mathbf{r})$: the *molecular orbital* $i$  
- $\chi_j(\mathbf{r})$: the *basis function* $j$ (often constructed from atomic orbitals)  
- $C_{j,i}$: the *expansion coefficient*, obtained from solving the electronic structure equations  
- $N$: the total number of basis functions used  

### 1.1 Concept of Basis Functions 

- The concept of basis functions is **mathematical**.  
- More basis functions → higher accuracy.  
- However, the essential question is: **how to choose a reasonable set of basis functions**.

When the basis set is sufficiently large, the calculation approaches the **Complete Basis Set (CBS) limit**, where the molecular orbitals are represented exactly.  

In practice, basis sets are always finite, so there is always a **basis set error** in addition to the inherent method error. The number of basis functions sets the number of molecular orbitals. However, not all orbitals are chemically relevant. Typically only the occupied orbitals and low-lying virtual orbitals have real chemical significance. If basis functions are chosen as atomic orbitals, the expansion is called the **Linear Combination of Atomic Orbitals (LCAO)**. In practice, most basis functions are **constructed functions**, not pure atomic orbitals.

---

### 1.2 Slater Type Orbital (STO)
- Resembles the real atomic orbitals in terms of radial behavior.  
- Historically used in some quantum chemistry programs (e.g., ADF), but not common because integrals involving STOs are more difficult to evaluate.  
- STOs require fewer functions to achieve similar accuracy compared to GTOs.

The general form of an STO is:

$$
\chi_{\zeta, l, m, n}^A(r, \theta, \phi) = N Y_{l,m}(\theta, \phi) \, r^{n-1} e^{-\zeta r}
$$

where  
- $r = |\mathbf{r} - \mathbf{R}_A|$: distance from electron to nucleus $A$  
- $Y_{l,m}(\theta,\phi)$: spherical harmonics  
- $N$: normalization constant  

---

### 1.3 Plane Wave
- Suitable for **periodic systems** such as solids.  
- Basis functions take the form:

$$
\chi_{\mathbf{k+G}}(\mathbf{r}) = \exp \left[ i (\mathbf{k} + \mathbf{G}) \cdot \mathbf{r} \right]
$$

- Requires the use of pseudopotentials to handle core electrons.  
- Consumes large computational resources, especially for molecules, making it less efficient for quantum chemistry.


---

### 1.4 Gaussian Type Function (GTF) or Gaussian Type Orbital (GTO)

- The most widely used basis functions in quantum chemistry.  
- Their Gaussian form makes it computationally efficient to evaluate electron integrals.

The explicit form of a Gaussian type function is:

$$
\phi_{\alpha, l, m, n}^A(\mathbf{r}) 
= N \, x^l \, y^m \, z^n \, e^{-\alpha (\mathbf{r} - \mathbf{R}_A)^2}
$$

where the normalization constant $N$ is

$$
N = \left( \frac{2\alpha}{\pi} \right)^{3/4}
\sqrt{ \frac{(8\alpha)^{\,l+m+n} \, l! \, m! \, n!}{(2l)! \, (2m)! \, (2n)!} }
$$

- $\alpha$: Gaussian exponent  
- $\mathbf{R}_A$: coordinates of the nucleus $A$ to which this Gaussian belongs  
- $x, y, z$: Cartesian components of $\mathbf{r} - \mathbf{R}_A$  
- $l, m, n$: integers defining the type of GTF  
- $u = l + m + n$: corresponds to the angular momentum of the GTF  

---

#### Examples of GTF Types

| $u$   | Type | $l$  | $m$  | $n$  |
| ----- | ---- | ---- | ---- | ---- |
| 0 (S) | S    | 0    | 0    | 0    |
| 1 (P) | X    | 1    | 0    | 0    |
| 1 (P) | Y    | 0    | 1    | 0    |
| 1 (P) | Z    | 0    | 0    | 1    |
| 2 (D) | XX   | 2    | 0    | 0    |
| 2 (D) | YY   | 0    | 2    | 0    |
| 2 (D) | ZZ   | 0    | 0    | 2    |
| 2 (D) | XY   | 1    | 1    | 0    |
| 2 (D) | XZ   | 1    | 0    | 1    |
| 2 (D) | YZ   | 0    | 1    | 1    |
| 3 (F) | XXX  | 3    | 0    | 0    |
| 3 (F) | YYY  | 0    | 3    | 0    |
| 3 (F) | ZZZ  | 0    | 0    | 3    |
| 3 (F) | XXY  | 2    | 1    | 0    |
| 3 (F) | XXZ  | 2    | 0    | 1    |
| 3 (F) | YYX  | 1    | 2    | 0    |
| 3 (F) | YYZ  | 0    | 2    | 1    |
| 3 (F) | ZZX  | 1    | 0    | 2    |
| 3 (F) | ZZY  | 0    | 1    | 2    |
| 3 (F) | XYZ  | 1    | 1    | 1    |

In practical quantum chemistry calculations, basis sets are almost always built from **contracted Gaussian type functions (CGTOs)**.  
Each contracted function is formed as a linear combination of several **primitive Gaussian type functions (PGTOs)**:
$$
\chi(\mathbf{r}) = \sum_i d_i \, \phi_i(\mathbf{r})
$$

Here, $\phi_i(\mathbf{r})$ represents a primitive GTF and $d_i$ is the contraction coefficient.  
The number of primitives included in such a combination is referred to as the **degree of contraction**.  

If a basis function is made from a single primitive GTF, it is called an **uncontracted function**.

---

Both contracted and uncontracted functions are used in modern quantum chemistry.  
If one only uses uncontracted GTFs, the number of primitives required for a reasonably accurate description becomes extremely large, making the computation prohibitively expensive.  
By contracting several primitives into a single basis function, one can mimic the behavior of Slater type orbitals near the nucleus with relatively few functions, while still leaving flexibility to describe the chemically important valence region using additional (often less contracted) functions.

It is important to note that contraction coefficients are **fixed** once the basis set is defined. During the SCF procedure, these coefficients remain unchanged. What evolves through the SCF iterations are the **molecular orbital expansion coefficients**, which are optimized until convergence.

---

### 1.5 Shells

In basis set terminology, a **shell** refers to a group of Gaussian type functions (GTFs) that share the same angular momentum but differ in their Cartesian form.  
For Cartesian GTFs, the shell structures are:

- **S shell:** S  
- **P shell:** X, Y, Z  
- **D shell:** XX, YY, ZZ, XY, XZ, YZ  
- **F shell:** XXX, YYY, ZZZ, XXY, XXZ, YYX, YYZ, ZZX, ZZY, XYZ  
- **G shell:** XXXX, YYYY, ZZZZ, XXXY, XXXZ, YYYX, YYYZ, ZZZX, ZZZY, XXYY, XXZZ, YYZZ, XXYZ, YYXZ, ZZXY  

…and so on for higher angular momenta.  

---

In practice, a contracted basis function is formed by contracting multiple primitive GTFs.  
Thus, a basis set shell can also be constructed by contracting a set of **primitive shells**.  

Within a given shell, all functions share the same contraction coefficients and Gaussian exponents.  
Therefore, in real basis set definitions, shells are often specified as a unit rather than listing each function separately.

## II. Basis Sets

There are many different types of basis sets, such as **6-31G\***, **def2-TZVP**, and **cc-pVTZ**.  

Each basis set specifies, for every atom, the types of shells to be used, along with the contraction coefficients and Gaussian exponents. The parameters of a basis set can be determined in several ways. For example, one can fit Slater-type orbitals (STOs), or optimize against certain atomic or molecular properties such as ionization energies, electron affinities, or atomic excitation energies. Basis sets can also be constructed by using special schemes like **even-tempered exponents**.  

Different basis sets vary in reliability, size, and computational cost. A well-designed basis set balances accuracy with efficiency: it should be large enough to provide good results, but not so large that the computation becomes prohibitively expensive. It is worth emphasizing that you cannot compare the energies in different basis sets.
For problems requiring high accuracy, one often needs to employ systematically improvable basis sets that converge toward the complete basis set limit.

#### Example: Basis Set Definition for Carbon in Gaussian Format

The table below shows how a basis set is defined for carbon in the Gaussian program format.

- **Shell type (S, P, …)**  
- **Contraction degree** (number of primitive GTFs combined)  
- **GTF exponent** (α values)  
- **Contraction coefficients** (d values)

Example:

```
 S 3 1.00
 	0.4464510E+03 0.197880E-01
 	0.6746260E+02 0.145340E+00
 	0.1506700E+02 0.185290E+00

 S 3 1.00
 	0.6746260E+02 -0.999100E-02
 	0.1506700E+02 0.285190E+00
 	0.2999100E+01 0.566280E+00

 S 1 1.00
 	0.2999100E+01 1.000000E+00

 P 2 1.00
 	0.4367700E+01 0.116710E+00
 	0.8698200E+00 0.475580E+00

 P 1 1.00
	0.1883900E+00 1.000000E+00
```



### 2.1 Minimal Basis Set

A **minimal basis set** assigns only **one basis function** to each atomic orbital (AO). This makes the basis very compact, but also too small to fully describe core and valence orbitals in real molecules. Therefore, minimal basis sets are rarely used in modern quantum chemistry. The most well-known minimal basis sets are the **STO-nG** series introduced by Pople. In these, each Slater-type orbital (STO) is approximated by a linear combination of **n Gaussian type functions (GTFs)**. The notation "nG" means that each STO is replaced by n Gaussians.

The most common example is **STO-3G**. STOs resemble real atomic orbitals, but integrals with STOs are difficult to compute. In contrast, integrals involving GTFs can be evaluated efficiently.  However, a single Gaussian does not reproduce the radial behavior of an STO very well, especially in the region close to the nucleus where the cusp is important. Therefore, a linear combination of several GTFs is used to approximate one STO. This is the principle behind the STO-nG minimal basis sets.

The figure below shows the radial behavior of a 1s STO compared to its Gaussian approximations:

- **SLATER:** true Slater-type orbital  
- **STO-1G:** single Gaussian approximation  
- **STO-2G:** contraction of two Gaussians  
- **STO-3G:** contraction of three Gaussians  

<img src="D:\calculate\github\QchemTutorials\notebooks\03-qchem\pngs\STO.png" alt="STO" style="zoom:25%;" />

### 2.2 Extended Basis Sets

Compared with minimal basis sets, **extended basis sets** use more than one basis function to represent each atomic orbital.  

If each valence orbital is described by *n* basis functions, the basis set is called an **n-zeta basis set**.  
A widely used example is the **Pople 6-31G series**, which is a **double-zeta (DZ) basis set**.

---

#### Example: 6-31G

- The **core orbitals** are represented by one contracted basis function formed from six primitive Gaussians.  
- Each **valence orbital** is described by two basis functions:  
  - One contracted function composed of three primitives.  
  - One uncontracted single primitive.  

Thus, 6-31G is a **double-zeta (DZ)** basis set.  
An extended version, **6-311G**, uses one contracted function of three primitives plus two uncontracted functions for each valence orbital, making it a **triple-zeta (TZ)** basis set.

In the 6-31G and 6-311G families, the **core orbitals** are represented by a single contracted function.  
The contraction degree is very high, but since core orbitals are relatively rigid, their shape and energy hardly change in molecules. Therefore, it is sufficient to describe them with a single contracted function. In contrast, **valence orbitals** are much more flexible. They change significantly during chemical bonding, and their shapes may be distorted in molecular environments. Thus, multiple basis functions are needed for valence orbitals. For example, in bonding and anti-bonding situations, splitting the valence description allows the basis set to capture such variations more accurately.

Minimal basis sets fail to capture the flexibility of valence orbitals, leading to poor accuracy. Extended basis sets, with split-valence functions, provide a much more reliable description of molecular electronic structure.

---

### 2.3 Polarization Functions

**Polarization functions** are additional basis functions with angular momentum higher than that of the highest occupied atomic orbital.  
For example:  
- For hydrogen, $p$ functions are polarization functions.  
- For oxygen, $d$ functions are polarization functions.  
- For iron (Fe), $f$ functions are polarization functions.  

Polarization functions are usually **uncontracted**.

Adding polarization functions allows the basis set to describe distortions of atomic orbitals in molecular environments. This is essential for obtaining accurate molecular properties, especially for atoms that are important in bonding. In Hartree–Fock and correlated methods, polarization functions greatly improve the flexibility of the wavefunction. They allow the basis set to reproduce features such as **bond polarization** and **cusp behavior** near nuclei. Without polarization functions, calculations systematically underestimate bond dipoles and fail to capture angular distortions.



### 2.4 Diffuse Functions

**Diffuse functions** are basis functions with very small exponents (close to zero).  
They extend very far into space, and are usually uncontracted.

---

#### When diffuse functions are useful
- Calculation of **dipole moments** and **higher multipole moments**  
- Description of **anions** and **Rydberg excited states**  
- Calculation of **vertical ionization energies**, **electron affinities**, and **excitation energies**  
- Description of **weak intermolecular interactions** (van der Waals, hydrogen bonding)  
- Calculation of **Raman** and **ROA intensities**  
- More accurate **reaction energies** and potential energy surfaces  

---

#### When diffuse functions may not help
- For systems with no strong polar character, diffuse functions do not necessarily improve results.  
- Adding diffuse functions increases computational cost and sometimes only brings marginal improvements.  

The **3-zeta basis set** family (e.g., triple-zeta) often already includes diffuse functions in its design.  The **4-zeta basis set** family almost always requires diffuse functions to be added explicitly. 

---

#### Problems Introduced by Diffuse Functions

- **Significant increase in computational cost**  
- SCF convergence becomes much more difficult  
- Some wavefunction analysis methods may fail (e.g., **Mayer bond order**, **Mulliken population analysis**)  
- May **destroy chemical interpretability** of virtual orbitals (loss of clear valence character)  
- Sometimes produce **unstable eigenvalues** in calculations  
- Can lead to **reduced symmetry** in wavefunctions and molecular structures due to numerical issues  

