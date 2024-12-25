# Draper's QFT-Based Quantum Addition with Noise Simulation

This research code implements **Draper’s QFT-based addition algorithm** and explores the impact of quantum noise on computational fidelity. It provides insights into the behavior of quantum circuits under realistic noise conditions.

## Key Components

### 1. Draper’s QFT-Based Addition  
The code constructs a quantum circuit to add two integers using quantum states:
- **QFT**: Encodes integer addition in the frequency domain.
- **IQFT**: Transforms results back to the computational basis.
- The circuit uses quantum registers to represent inputs and the sum.  
Example:  
\( |3\rangle + |4\rangle = |7\rangle \).

### 2. Noise Injection in Quantum Circuits  
Noise is modeled as random Pauli operators applied probabilistically after quantum gates:
- **\(\alpha\)**: Probability of applying noise after single-qubit gates.
- **\(\beta\)**: Probability of applying noise after two-qubit gates.  
Function `add_noise()` applies these noise models, creating a noisy version of the circuit.

### 3. Gate Basis Transformation  
To ensure compatibility with realistic quantum processing units (QPUs), circuits are transformed into the gate basis: {CX, ID, RZ, SX, X}.

### 4. Fidelity Analysis  
Fidelity quantifies the similarity between the ideal and noisy circuit outputs:
- **Noise Levels**: Fidelity is analyzed for varying noise probabilities (\(\alpha, \beta\)).
- **Findings**: Fidelity generally decreases with increased noise, but results can vary based on gate usage and noise configuration.

## Visualizations  
- **Histograms**: Show the output counts for ideal and noisy simulations.  
- **Line Plot**: Depict fidelity trends across different noise levels.

This code offers a framework for analyzing the robustness of quantum algorithms in noisy environments, providing critical insights for quantum error mitigation research.