# CNOT-Phase Circuits with Minimal CNOT Count

A project for the **Fault-Tolerant Quantum Computing** course at **EPFL**, exploring the synthesis of quantum circuits composed of CNOT gates and single-qubit Z-basis rotations with a focus on reducing the number of CNOT gates.

This project implements algorithms from existing research: **Gray-Synth**, introduced by **Matthew Amy, Parsiad Azimzadeh, and Michele Mosca** [[1]](#ref-1), and the **Patel–Markov–Hayes algorithm** for synthesizing linear reversible circuits [[2]](#ref-2). My work consists of implementing these methods in Python and Qiskit, combining them into a circuit-construction workflow, and illustrating their operation through worked examples and a presentation. The underlying algorithms and theoretical results are due to the authors cited below.

## Overview

CNOT-phase circuits can be described by a phase function and a reversible binary linear transformation. The phase function is expressed as a weighted sum of input parities, where each parity is an XOR of selected input bits.

The synthesis approach constructs a **parity network**: a CNOT circuit in which each required parity appears on a wire at some point. The corresponding phase rotations can then be inserted when those parities become available. A final linear correction restores the desired computational-basis transformation.

Although the project title refers to minimal CNOT count, **Gray-Synth is a heuristic**: it aims to produce circuits with low CNOT counts and does not guarantee a globally minimal circuit for arbitrary inputs.

## Implementation

The notebook contains:

- **Gray-Synth (`gray_synth`)** — an implementation based on Algorithm 1 of [[1]](#ref-1), using a stack to process subsets of parity vectors and tracking the binary linear transformation after each CNOT.
- **Linear reversible synthesis (`lwr_cnot_synth`, `cnot_synth`)** — an implementation of the approach in [[2]](#ref-2), using binary row operations to decompose an invertible binary matrix into CNOT gates. This is also used to undo the parity network's final linear transformation.
- **Circuit assembly (`build_cnot_phase_circuit`)** — combines the parity network with `RZ` rotations and the final linear-transformation stages.
- **Unitary verification (`equal_up_to_global_phase`)** — compares circuit operators while allowing for an overall global phase.

## Examples

### CCZ gate

The notebook constructs a three-qubit CCZ circuit from its seven parity terms. The construction uses four CNOTs in the parity network and three CNOTs for uncomputation, together with seven Z-basis rotations.

The saved notebook output confirms equivalence to Qiskit's reference CCZ gate up to global phase, with a maximum matrix-entry discrepancy of approximately `2.8e-16` in the comparison.

### Four-qubit phase circuit

A second example constructs the four-qubit diagonal circuit specified in **Example 4.2 of [[1]](#ref-1)**, using its six parity terms and phase coefficients.

## Files

| File | Contents |
| --- | --- |
| `implementation.ipynb` | Python/Qiskit implementations, circuit diagrams, and worked examples. |
| `presentation.pdf` | Theoretical background, algorithm walkthrough, and step-by-step CCZ construction. |

## Running the notebook

Install the notebook and circuit-drawing dependencies:

```bash
python -m pip install numpy qiskit matplotlib pylatexenc jupyter
jupyter notebook implementation.ipynb
```

Run the cells in order to define the synthesis functions and generate the examples.

## Scope and current limitation

This is an educational implementation using explicitly supplied parity vectors and phase coefficients. It does not implement the full benchmark pipeline or the search over equivalent phase functions discussed in [[1]](#ref-1).

## References and attribution

<a id="ref-1"></a>

1. **Matthew Amy, Parsiad Azimzadeh, and Michele Mosca.** *On the controlled-NOT complexity of controlled-NOT–phase circuits.* Quantum Science and Technology, vol. 4, no. 1, Art. no. 015002, 2018. doi: 10.1088/2058-9565/aad8ca. [arXiv:1712.01859](https://doi.org/10.48550/arXiv.1712.01859).

<a id="ref-2"></a>

2. **Ketan N. Patel, Igor L. Markov, and John P. Hayes.** *Efficient Synthesis of Linear Reversible Circuits.* 2003. [arXiv:quant-ph/0302002](https://arxiv.org/abs/quant-ph/0302002).
