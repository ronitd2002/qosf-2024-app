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