---
layout: page
title: "Introduction to Computational 2D Materials Science"
permalink: /research/computational_2d_materials
---

# Introduction to Computational 2D Materials Science

*A beginner's guide for high school researchers to the physics, software, and tools used in computational materials research.*

---

## Table of Contents

1. [What Are 2D Materials and Why Do They Matter?](#1-what-are-2d-materials-and-why-do-they-matter)
2. [The Physics: How Atoms Stick to Surfaces](#2-the-physics-how-atoms-stick-to-surfaces)
3. [Density Functional Theory (DFT) — The Quantum Calculator](#3-density-functional-theory-dft--the-quantum-calculator)
4. [ABINIT — The Software That Solves Quantum Equations](#4-abinit--the-software-that-solves-quantum-equations)
5. [Machine Learning Meets Materials Science](#5-machine-learning-meets-materials-science)
6. [The Research Toolkit: Python, ASE, and Friends](#6-the-research-toolkit-python-ase-and-friends)
7. [Writing It Up: LaTeX for Scientific Papers](#7-writing-it-up-latex-for-scientific-papers)
8. [Getting Started: Your First Steps](#8-getting-started-your-first-steps)
9. [Glossary](#9-glossary)

---

## 1. What Are 2D Materials and Why Do They Matter?

### The Basics

Imagine peeling a single layer of atoms off a crystal — like pulling one sheet from a stack of paper. That single-atom-thick sheet is a **two-dimensional (2D) material**. The most famous example is **graphene**, a single layer of carbon atoms arranged in a honeycomb pattern, first isolated in 2004 (which won the Nobel Prize in Physics in 2010).

### Important 2D Materials

| Material | Formula | Structure | Key Properties |
|----------|---------|-----------|----------------|
| **Graphene** | C | Carbon honeycomb | Strongest known material, excellent electrical conductor |
| **Hexagonal Boron Nitride (hBN)** | BN | Alternating B and N atoms in honeycomb | Electrical insulator, atomically smooth, "white graphene" |
| **Molybdenum Disulfide (MoS₂)** | MoS₂ | Mo layer sandwiched between two S layers | Semiconductor, useful for transistors and catalysis |

### Why They Matter

2D materials are exciting because their properties change dramatically when you put things on their surface. A single metal atom sitting on graphene behaves very differently from the same atom in a bulk metal. This makes 2D materials promising for:

- **Biosensing**: Detecting diseases by how molecules bind to the surface
- **Catalysis**: Speeding up chemical reactions for clean energy
- **Environmental cleanup**: Capturing toxic heavy metals (Hg, Pb, Cd) from water
- **Electronics**: Building smaller, faster transistors

### MXenes — The New Frontier

Beyond the classic 2D materials, a newer family called **MXenes** (pronounced "max-eens") has emerged. These are 2D transition-metal carbides/nitrides with the formula Mₙ₊₁XₙTₓ, where T represents surface termination groups (–O, –OH, –F). MXenes are particularly interesting for biosensing because their surface chemistry can be tuned by controlling which termination groups are present.

---

## 2. The Physics: How Atoms Stick to Surfaces

### Adsorption Energy

When an atom or molecule approaches a 2D material surface, it can "stick" — this is called **adsorption**. The strength of this sticking is measured by the **adsorption energy** (or binding energy):

```
E_bind = E(surface + adsorbate) − E(surface alone) − E(adsorbate alone)
```

- **Negative** E_bind → the atom/molecule *wants* to stick (favorable)
- **More negative** → stronger binding
- **Positive** → the atom/molecule is repelled

### Adsorption Sites

On a 2D material, not every spot is equivalent. Common adsorption sites include:

- **Top site**: Directly above an atom
- **Bridge site**: Between two atoms
- **Hollow site**: At the center of a ring of atoms

Different metals prefer different sites, and finding the preferred site is a key research question.

### What Controls Binding?

Several factors determine how strongly something binds:

1. **Electronic structure**: How electrons are shared between the adsorbate and surface
2. **Geometry**: The distance and angle of approach
3. **Van der Waals forces**: Weak but important long-range attractions
4. **Charge transfer**: Electrons moving from one system to another

Understanding these factors requires solving the quantum mechanical equations that govern electron behavior — which brings us to DFT.

---

## 3. Density Functional Theory (DFT) — The Quantum Calculator

### The Problem

All material properties ultimately come from how electrons behave. The Schrödinger equation describes this behavior exactly, but solving it for more than a few electrons is impossibly expensive — the computational cost grows exponentially with the number of electrons.

A typical 2D material unit cell with an adsorbate has ~200 electrons. Solving the full Schrödinger equation for this system would take longer than the age of the universe.

### The DFT Solution

In 1964, Walter Kohn and Pierre Hohenberg proved a remarkable theorem: instead of tracking every electron individually, you only need to know the **electron density** — how many electrons are at each point in space. This reduces the problem from tracking N electrons in 3D (3N dimensions) to a single function in 3D.

The key equation in DFT is the **Kohn-Sham equation**, which turns the many-electron problem into a set of single-electron equations that can be solved on a computer.

### What DFT Gives You

From a DFT calculation, you can extract:

- **Total energy** of the system (in Hartree or eV)
- **Atomic forces** (which direction atoms want to move)
- **Electronic band structure** (how electrons are distributed in energy)
- **Charge density** (where electrons are concentrated)
- **Work function** (how easily electrons escape the surface)

### Key Concepts You'll Encounter

| Concept | What It Means | Analogy |
|---------|---------------|---------|
| **Exchange-correlation functional** | Approximation for electron-electron interactions (e.g., PBE, LDA) | The "recipe" for how electrons interact |
| **Pseudopotential / PAW** | Simplification of core electrons | Treating the atom's inner electrons as a fixed background |
| **Plane-wave basis set** | Mathematical representation of electron wavefunctions | Like expressing a sound wave as a sum of pure tones |
| **k-points** | Sampling points in reciprocal space | Like polling stations sampling voter preferences |
| **Energy cutoff (ecut)** | How many plane waves to include | Resolution of your calculation — higher = more accurate but slower |
| **SCF convergence** | Self-consistent field iteration | Iteratively refining the answer until it stops changing |

### Convergence — Getting Reliable Results

A critical skill in DFT is **convergence testing**: making sure your results don't change when you increase the accuracy settings. You must test:

1. **ecut** (energy cutoff): Increase until energy changes by < 1 meV/atom
2. **k-points**: Increase grid density until energy stabilizes
3. **Vacuum thickness**: For 2D materials, ensure periodic images don't interact (typically 15–20 Å)
4. **Supercell size**: Ensure adsorbates don't interact with their periodic images

---

## 4. ABINIT — The Software That Solves Quantum Equations

### What Is ABINIT?

**ABINIT** is an open-source software package that performs DFT calculations. It solves the Kohn-Sham equations using plane waves and pseudopotentials/PAW datasets. Other popular DFT codes include VASP, Quantum ESPRESSO, and SIESTA, but ABINIT is fully open-source and well-suited for academic research.

### How ABINIT Works

1. You write an **input file** (`.abi`) specifying:
   - Atomic positions and cell shape
   - Which pseudopotentials to use
   - Accuracy parameters (ecut, k-points, convergence criteria)
   - What type of calculation (single-point energy, relaxation, etc.)

2. ABINIT runs the calculation (can take minutes to days depending on system size)

3. It produces an **output file** (`.abo`) containing:
   - Total energy at each SCF iteration
   - Final converged energy
   - Forces on atoms
   - Other requested properties

### An Example ABINIT Input File

Here's a simplified input for a 2D material adsorption calculation:

```
# Accuracy settings
ecut 36.0          # Plane-wave cutoff in Hartree
pawecutdg 72.0     # PAW double-grid cutoff
ngkpt 2 2 1        # k-point grid (reduced along z for 2D)
nstep 200          # Maximum SCF iterations
toldfe 1e-06       # Energy convergence criterion (Hartree)

# System description
natom 28           # Number of atoms
ntypat 3           # Number of atom types
znucl 16 12 42     # Atomic numbers of each type
typat 1 1 1 ... 2 3 3 3 ...  # Which type each atom is

# Cell and coordinates
acell 17.91 15.51 37.79      # Cell dimensions (Bohr)
rprim
  1.0   0.0   0.0
  0.5   0.866 0.0            # Hexagonal lattice
  0.0   0.0   1.0
xred                          # Fractional coordinates
  0.111 0.222 0.578
  ...

# Pseudopotentials
pp_dirpath "$ABI_PSPDIR"
pseudos "PAW/S.GGA_PBE-JTH.xml, PAW/Mg.GGA_PBE-JTH.xml, PAW/Mo.GGA_PBE-JTH.xml"
```

### Running ABINIT with Docker

ABINIT can run inside a **Docker container** — a lightweight virtual environment that packages the software with all its dependencies:

```bash
docker run --rm \
    -v "$PWD":/workspace \
    -v "/path/to/pseudopotentials":/psp \
    -e ABI_PSPDIR=/psp \
    abinit \
    bash -c "cd /workspace && mpirun -np 16 abinit input.abi"
```

This runs ABINIT in parallel across 16 CPU cores using MPI (Message Passing Interface).

### Units in ABINIT

ABINIT uses **atomic units** internally:
- Energy: Hartree (1 Ha = 27.211 eV)
- Length: Bohr (1 Bohr = 0.529 Å)

You'll need to convert to more common units (eV and Å) for analysis and publication.

### Other DFT Software

| Software | License | Notes |
|----------|---------|-------|
| **ABINIT** | Open source | Plane-wave + PAW, good documentation |
| **Quantum ESPRESSO** | Open source | Very popular, large community |
| **VASP** | Commercial | Industry standard, requires license |
| **SIESTA** | Open source | Localized basis sets, efficient for large systems |
| **GPAW** | Open source | Integrates tightly with ASE |

---

## 5. Machine Learning Meets Materials Science

### The Problem with DFT Alone

DFT is accurate but slow. A single adsorption calculation on a 2D material can take hours. If you want to screen many metals, substrates, sites, and heights, the total compute time becomes weeks or months.

### Machine Learning Interatomic Potentials (MLIPs)

The idea: train a neural network on DFT data so it can predict energies and forces in milliseconds instead of hours. The network learns the relationship between atomic structure and energy from examples.

Key MLIP frameworks include:

| Framework | Developer | Training Data | Strengths |
|-----------|-----------|---------------|-----------|
| **FairChem** | Meta AI | OC20 (catalysis) | Large pre-trained models, ASE integration |
| **MACE** | Cambridge | Various | Equivariant architecture, high accuracy |
| **NequIP** | Harvard | Various | E(3)-equivariant, data-efficient |
| **CHGNet** | Berkeley | Materials Project | Charge-informed, broad coverage |

### The Domain Mismatch Problem

Pre-trained MLIPs are trained on specific datasets (e.g., metal surfaces for catalysis). When applied to a different domain (e.g., 2D materials), errors can be very large because the model has never seen similar structures during training.

### Transfer Learning

Instead of training a model from scratch (which requires millions of DFT calculations), **transfer learning** adapts a pre-trained model to a new domain using a small amount of new data:

1. Start with a pre-trained model (it already understands basic physics)
2. Run it on your new structures to get predictions
3. Train a small **correction model** that learns the systematic error:

```
E_corrected = E_pretrained + f_correction(E_pretrained)
```

This approach can dramatically reduce errors with only hundreds of training structures, because the pre-trained model already captures useful physical knowledge.

### Delta Learning

A more advanced strategy called **delta learning** models the *difference* between the pre-trained model and the true DFT result:

1. **Linear correction**: Learn per-element energy shifts
2. **Non-linear correction**: A neural network learns remaining patterns from structural features (composition, geometry, interactions)
3. **Ensemble**: Train multiple models and average for robustness

---

## 6. The Research Toolkit: Python, ASE, and Friends

### Python — The Glue Language

Python is the dominant language in computational materials science. You'll use it for:

- **Generating input files** for DFT codes
- **Parsing output files** to extract energies and structures
- **Data analysis** with NumPy and Pandas
- **Machine learning** with PyTorch and scikit-learn
- **Visualization** with Matplotlib

### ASE — Atomic Simulation Environment

**ASE** is a Python library for working with atoms. It provides a unified interface to many simulation codes:

```python
from ase import Atoms
from ase.build import mx2, graphene_nanoribbon
from ase.visualize import view

# Create a MoS2 monolayer
slab = mx2(formula='MoS2', kind='2H', a=3.16, thickness=3.17, vacuum=15)

# View the structure
view(slab)

# Attach a calculator and compute energy
slab.calc = my_calculator
energy = slab.get_potential_energy()
forces = slab.get_forces()
```

### Key Python Libraries

| Library | Purpose | Example Use |
|---------|---------|-------------|
| **NumPy** | Numerical arrays and math | Storing atomic positions, computing statistics |
| **Matplotlib** | Plotting | Parity plots, learning curves, energy landscapes |
| **Pandas** | Data tables | Organizing results by material/metal/site |
| **PyTorch** | Neural networks | Training ML models |
| **scikit-learn** | Classical ML | Ridge regression, cross-validation |
| **pickle** | Data serialization | Saving/loading datasets as `.pkl` files |
| **PyYAML** | Configuration files | Defining calculation parameters |

### A Typical Computational Workflow

```
Define system (material, adsorbate, sites)
    ↓
Generate DFT input files (Python scripts)
    ↓
Run DFT calculations (ABINIT / VASP / QE)
    ↓
Parse outputs → structured dataset (Python)
    ↓
Analyze: energies, structures, trends (NumPy, Pandas)
    ↓
Visualize: plots, figures (Matplotlib)
    ↓
(Optional) Train ML models (PyTorch)
    ↓
Write paper (LaTeX)
```

---

## 7. Writing It Up: LaTeX for Scientific Papers

### What Is LaTeX?

**LaTeX** (pronounced "lah-tech" or "lay-tech") is a typesetting system used for virtually all scientific papers in physics, chemistry, and materials science. Unlike Word, you write plain text with formatting commands, and LaTeX produces beautifully formatted PDFs.

### Why LaTeX?

- **Equations** look professional: `$E_{ads} = E_{slab+mol} - E_{slab} - E_{mol}$`
- **References** are managed automatically with BibTeX
- **Figures and tables** are numbered and cross-referenced
- **Journal templates** are available for most publishers

### A Minimal LaTeX Example

```latex
\documentclass{article}
\usepackage{graphicx}
\usepackage{booktabs}
\usepackage{amsmath}

\title{My First Computational Materials Paper}
\author{Your Name}

\begin{document}
\maketitle

\begin{abstract}
We study adsorption on 2D materials using density functional theory...
\end{abstract}

\section{Introduction}
Two-dimensional materials are promising for many applications.

\section{Methods}
Calculations were performed using DFT with the PBE functional.
The adsorption energy is defined as:
\begin{equation}
E_{\text{ads}} = E_{\text{slab+mol}} - E_{\text{slab}} - E_{\text{mol}}
\end{equation}

\section{Results}
The binding energies are shown in Table~\ref{tab:results}.

\begin{table}[htbp]
\centering
\caption{Binding energies (eV) for metal adsorption on graphene.}
\label{tab:results}
\begin{tabular}{@{}lcc@{}}
\toprule
Metal & Top Site & Hollow Site \\ \midrule
Mg    & $-0.52$  & $-0.38$ \\
Al    & $-1.15$  & $-0.97$ \\
\bottomrule
\end{tabular}
\end{table}

\begin{figure}[htbp]
\centering
\includegraphics[width=0.7\linewidth]{my_figure.pdf}
\caption{Predicted vs DFT energies.}
\label{fig:parity}
\end{figure}

\bibliographystyle{unsrt}
\bibliography{references}
\end{document}
```

### Tools for LaTeX

- **Overleaf** (overleaf.com): Online LaTeX editor — great for beginners and collaboration
- **TeXLive**: Local installation for offline work
- **latexmk**: Automated compilation (`latexmk -pdf paper.tex`)

### Generating Figures for Papers

Use Python to generate publication-quality figures:

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(5, 4))
ax.scatter(dft_energies, predicted_energies, alpha=0.6)
ax.plot([min_e, max_e], [min_e, max_e], 'k--', label='Ideal')
ax.set_xlabel('DFT Energy (eV)')
ax.set_ylabel('Predicted Energy (eV)')
ax.legend()
plt.savefig('my_figure.pdf', dpi=300, bbox_inches='tight')
```

Save as PDF for LaTeX — vector graphics scale perfectly at any size.

---

## 8. Getting Started: Your First Steps

### Prerequisites

- Basic Python programming (variables, loops, functions)
- High school chemistry (periodic table, chemical bonds)
- High school physics (energy, forces)
- Comfort with the command line (terminal)

### Step-by-Step Path

1. **Week 1–2**: Learn Python basics (if needed)
   - Free resource: [Python for Everybody](https://www.py4e.com/)

2. **Week 3–4**: Understand atomic structure and bonding
   - Read about crystal structures, unit cells, periodic boundary conditions
   - Install ASE and visualize simple structures

3. **Week 5–6**: Run your first DFT calculation
   - Install Docker and pull an ABINIT image
   - Run a simple bulk calculation (e.g., silicon)
   - Learn convergence testing (ecut, k-points)

4. **Week 7–8**: Study a 2D material
   - Build a graphene slab with vacuum
   - Place a metal atom on it
   - Compute the binding energy

5. **Week 9–10**: Analyze and visualize results
   - Parse DFT outputs with Python
   - Make plots with Matplotlib
   - Compare different metals/sites

6. **Week 11–12**: Write it up
   - Learn basic LaTeX on Overleaf
   - Write a short report with figures and tables

### Recommended Reading

- Sholl & Steckel, *Density Functional Theory: A Practical Introduction* (beginner-friendly textbook)
- The [ASE tutorials](https://wiki.fysik.dtu.dk/ase/tutorials/tutorials.html)
- The [ABINIT tutorials](https://docs.abinit.org/tutorial/)
- FairChem documentation at [fair-chem.github.io](https://fair-chem.github.io/)

---

## 9. Glossary

| Term | Definition |
|------|-----------|
| **2D material** | A material that is only one or a few atoms thick |
| **Adsorption** | When an atom or molecule sticks to a surface |
| **Binding energy** | How strongly an adsorbate is attached to a surface (negative = favorable) |
| **DFT** | Density Functional Theory — a quantum mechanical method for calculating material properties |
| **eV** | Electron volt — a unit of energy (1 eV = 1.602 × 10⁻¹⁹ J) |
| **Hartree** | Atomic unit of energy (1 Ha = 27.211 eV) |
| **Bohr** | Atomic unit of length (1 Bohr = 0.529 Å) |
| **PAW** | Projector Augmented Wave — a method for treating core electrons |
| **PBE** | Perdew-Burke-Ernzerhof — a common exchange-correlation functional |
| **SCF** | Self-Consistent Field — iterative procedure to solve DFT equations |
| **k-points** | Sampling points in the Brillouin zone (reciprocal space) |
| **Pseudopotential** | Approximation replacing core electrons with an effective potential |
| **MLIP** | Machine Learning Interatomic Potential |
| **Transfer learning** | Adapting a pre-trained model to a new domain |
| **ASE** | Atomic Simulation Environment — Python library for atomistic simulations |
| **MXene** | 2D transition-metal carbide/nitride (Mₙ₊₁XₙTₓ) |
| **Supercell** | Enlarged unit cell used to model isolated adsorbates |
| **Vacuum layer** | Empty space added above/below a 2D slab to prevent periodic image interaction |
| **Convergence** | Ensuring results don't change when accuracy parameters are increased |
| **Docker** | Container platform for running software in isolated environments |
| **MPI** | Message Passing Interface — protocol for parallel computing |
| **LaTeX** | Typesetting system for scientific documents |
