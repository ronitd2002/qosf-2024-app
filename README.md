# QOSF 2024 fall cohort selection tasks

I have tried to solve Task 1 and Task 2. Please consider **Task 1** for my official submission.

---
# Task 1 : 

## Explanation :

This task checks the time required for quantum operations upon increase in the number of computational qubits. For the same purpose I test our results for standard quantum circuit using pennylane's lightning qubit device. this is just for an initial test and has no purpose with the tasks. 

### Sub task 1:
Our main solution starts in the next bit where the "simulation" of these quantum operations are carried out using simple arithmetic with the help of numpy library. The vector size increases as O($2^n$) and the computation time for matrix dot product must also follow a similar trend. Here I use the idea of statevector simulation, where multi-qubit (n qubit) circuits and states are represented using $2^n$ sized vectors obtained using the kronecker product of computational basis `[1,0]` denoting $|0\rangle$. Then the quantum operations are carried out using matrices of size $2^n \times 2^n$. The time required for these computations to make is then plotted and as the graph suggests the computation time increases monotonously with increase in the number of qubits as is expected. The trend here resembles that of an exponential graph. I have tested this circuit with several # of qubits but it seems like operator actions become almost impossible beyond 12 qubits. Hence I am keeping that as an upper limit. 

The technique here is that $|000\rangle$ = `[1,0,0,0,0,0,0,0]` if you operate it with X(0) it will become $|100\rangle$ = `[0,0,0,0,1,0,0,0]`. The init state is acquired using `np.kron()` looped "`n_qubit`" times. Same goes for X & H gates. But the CNOT Gate is not so trivial. So initially I use qiskit's circuit to operator functionality using `Operator.from_circuit(QuantumCircuit)` method after sequentially operating the CNOT gate as I wish for value based accuracy. But I thought it could perform better and since our goal is to check the computational load I relaxed the result accuracy a little. Here's what I did : I calculated the kronecker product of CNOT n_qubits//2 times and took the direct sum of an identity matrix with cnot in case of odd num_qubit. This will provide us an operator of appropriate dimensions and I can subsequently calculate the growth rate of runtime with respect to increasing qubits.

### Sub task 2:
Next I use an advanced simulation method by using tensors which is also another "classical" technique. The first idea is to writing the tensors as a tensor of shape `(2,2,...)` n times. We treat gates similarly too. We do not need to change single qubit gates as their shape is already `((2,2))`, but we do need to `reshape` 2 qubit gates since the dimensions they have is $2^2 \times 2^2$ i.e $4 \times 4$. Here I would have to play around with the axes since they would allow us to calculate the correct states. For single qubit gates, I use `np.tensordot` for the target axis in the state and the 1st axis in the single qubit operator (because there are only two axes). Then I rearrange the qubits by using `np.transpose` and setting the last qubit in the "target" position and using this list as the transpose method, finally in order to retreieve the correct state. Similarly for double qubit gates we follow the same routine by applying `tensordot` as `state = np.tensordot(state, gate, axes=([control, target], [0, 1]))`. We then as we did before by bringing the second last qubit in the control position and the last qubit in the target position. We also must pop out the last two entries since then there would be a shape mismatch otherwise. After that we transpose the state in the fashion dictated by the aforementioned `axes` list that we have created.

### BONUS
The BONUS section deals with sampling of these simulated quantum states mimicking real measurements using `np.random.choice` parameterized by the probability ($coefficient^2$) of each of these states and then sampling the same for 100 shots. Secondly the calculation of expectation value is also pretty simple as all we have to do for any arbitrary operator of appropriate shape is : $\langle \psi | O | \psi \rangle =$ `np.dot(np.conjugate(statevec), np.dot(O, statevec))`.


**For each section I have validated my results against traditional pennylane results to show the accuracy of my final product states for each implementation i.e NAIVE matrix as well as ADVANCED tensor.**

---
# Task 2

## Explanation and requirements for completion

# Draper's QFT-Based Quantum Addition with Noise Simulation

This research code implements **Draper’s QFT-based addition algorithm** and explores the impact of quantum noise on computational fidelity. It provides insights into the behavior of quantum circuits under realistic noise conditions.

## Key Components

### 1. Draper’s QFT-Based Addition  
The code constructs a quantum circuit to add two integers using quantum states:
- **QFT**: Encodes integer addition in the frequency domain.
- **IQFT**: Transforms results back to the computational basis.
- The circuit uses quantum registers to represent inputs and the sum.  
Example:  
( |3 $\rangle$ + |4 $\rangle$ = |7 $\rangle$ )

### 2. Noise Injection in Quantum Circuits  
Noise is modeled as random Pauli operators applied probabilistically after quantum gates:
- **( $\alpha$ )**: Probability of applying noise after single-qubit gates.
- **( $\beta$ )**: Probability of applying noise after two-qubit gates.  
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

# Questions :

### 1. How does noise affect the results?

Noise in quantum computations distorts the intended operations, leading to deviations in statevector fidelity and inaccurate measurement outcomes. Each gate, especially two-qubit gates, introduces errors that accumulate throughout the circuit, amplifying the impact of noise on the final results. Mitigating noise is essential to maintain reliable computations and accurate results.

For our purposes the above analyses answer the question.

### 2. Is there a way to decrease noise?

1. To address noise in QPU-transpiled circuits, employ circuit optimization and error mitigation techniques tailored to the hardware's topology. Key strategies include gate reduction, native gate decomposition directly translate circuits into the QPU's native gate set to reduce redundant operations, and methods like **Zero-Noise Extrapolation (ZNE)** to estimate low-noise results, preserving fidelity and improving measurement accuracy.  

---

2. **Dynamical Decoupling (DD)** reduces decoherence at the hardware level, and intelligent qubit mapping minimizes two-qubit gate errors by optimizing qubit placement on the QPU. Intelligent qubit mapping places logical qubits on physically closer QPU qubits, reducing long-distance two-qubit gates that amplify noise. Combined with layout optimization during transpilation, this approach ensures that the circuit respects hardware-specific connectivity, decreasing two-qubit gate errors and enhancing overall circuit performance.  

### 3. How does the number of gates affect the amount of noise? 

The number of gates directly correlates with noise levels, as each gate introduces errors that accumulate throughout the circuit. Two-qubit gates are particularly error-prone, so circuits with many such gates experience higher noise. Optimizing gate sequences and minimizing gate count are crucial for reducing overall noise in quantum computations.