# Draper's QFT-Based Quantum Addition with Noise Analysis

This project implements a **Quantum Fourier Transform (QFT)-based addition circuit** using Draper's algorithm. It explores the impact of noise on quantum computations and fidelity.

## Features

1. **Quantum Addition Circuit**  
   - Implements QFT and its inverse (IQFT) to add two numbers using quantum states.
   - Measures the output in the computational basis to verify correctness.  
   Example:  
   \( |3\rangle + |4\rangle = |7\rangle \)

2. **Noise Simulation**  
   - Adds random Pauli noise to one- and two-qubit gates:
     - \(\alpha\): Single-qubit noise probability.
     - \(\beta\): Two-qubit noise probability.  
   - Function: `add_noise()` injects noise into a given quantum circuit.

3. **Gate Basis Transformation**  
   - Transforms circuits to the QPU-supported gate basis: {CX, ID, RZ, SX, X}.

4. **Analysis and Visualization**  
   - Compares ideal and noisy circuits using histograms.  
   - Examines fidelity trends against noise levels (\(\alpha, \beta\)).  
   - Fidelity degradation suggests sensitivity to noise in the quantum addition process.

## Visualization  

- **Histogram**: Compares counts from ideal and noisy simulations.  
- **Line Plot**: Tracks fidelity variations with increasing noise probabilities.

## Tools & Libraries  

- **Qiskit**: Circuit design, simulation, and noise modeling.  
- **Matplotlib**: Visualization of results.

## How to Use  

1. Run the provided examples with varying noise levels.
2. Analyze the fidelity and noise effects on quantum addition.