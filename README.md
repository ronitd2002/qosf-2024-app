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

This tasks deals with the potrayal of increasing noise to the results. For these purposes of noise simulation I was required to build a adder circuit using the Draper algorithm. The drpaer algorithm achieves it using $|a,b\rangle \rightarrow^{Draper} |a,b+a\rangle$ using $|a,b\rangle \rightarrow^{QFT} |a,\phi(b)\rangle \rightarrow^{QFT} |a,\phi(a+b)\rangle \rightarrow^{IQFT} |a,a+b\rangle$

We use two three bit numbers like 3 ('b'='011') and 4 ('b' = '100') and sum it to get 7 which should be ('b'='111'). The scheme of **Draper adder** is to first turn b into $|\phi(b)\rangle$ and then use phase gates from a to b similar to the general qft scheme. 

Now to simulate noise I have used the `add_noise` function which randomly adds X, Y and Z gates into the circuit based on $\alpha$ probability for single qubit gates and CX, CY and CZ gates into the circuit based on $\beta$ probability. Till now we have the adder circuit and the noise function finished. I have used the basis gates given in the question for this transpilation routine. 

To potray the effect of noise I sweep alpha and beta for 5 values between [0.5, 1]. And store these circuits to show the count values for each.

To directly address the noise introduced by one-qubit and two-qubit gates in our QPU-transpiled circuit, we can employ both circuit optimization and error mitigation techniques that are compatible with the target QPU’s hardware topology. Our main aim is to minimize noise while preserving the statevector fidelity and ensuring accurate measurement outcomes.

#### 1. **Circuit Optimization During Transpilation**
   - Trivial methods - **Gate Reduction**: Qiskit provides optimization levels within the transpiler that minimize the gate count by reducing redundant gate sequences and simplifying the circuit structure. For instance, adjacent gates that cancel each other (like successive Pauli-X gates) or gate pairs that simplify to a lower-cost equivalent (e.g., combining rotations) can be merged, reducing noise.
   - More importantly - **Native Gate Decomposition**: By decomposing the circuit and good transpilation with proper optimization directly into the QPU’s native gate set, we bypass intermediate gates that would require further decomposition, reducing error from accumulated noise.

#### 2. **Error Mitigation Techniques**
   - **Zero-Noise Extrapolation (ZNE)**: This method simulates low-noise conditions by scaling gate errors in the circuit (like amplifying noise slightly) and then extrapolating the results back to estimate the zero-noise outcome. Practically, this is done by running the circuit at multiple noise levels and fitting the results to “zero” noise—particularly useful for both one-qubit and two-qubit errors.
   
#### 3. **Algorithmic Solutions for Real Hardware**
   - **Dynamical Decoupling (DD)**: This method helps tackle the issue of decoherence greatly. Applied at the level of the hardware, DD sequences mitigate noise for one-qubit gates using pulses.
   - **Qubit Mapping and Routing**: Intelligent qubit mapping, where the logical qubits are placed on physically “closer” qubits in the QPU, reduces the need for two-qubit gates across long distances, directly decreasing noise from two-qubit gate errors. This becomes even more effective when combined with the transpiler’s layout optimization, ensuring that the transpilation process respects hardware-specific connectivity.

## Effect of noise on results..

The counts around the correct answer progressively decreases with increase in the noise probabilities $\alpha$ & $\beta$.