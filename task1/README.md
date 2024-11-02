# Explanation :

This task checks the time required for quantum operations upon increase in the number of computational qubits. For the same purpose we test our results for standard quantum circuit using pennylane's lightning qubit device.

Our main solution starts in the next bit where the "simulation" of these quantum operations are carried out using simple arithmetic with the help of numpy library. Here we use the idea of statevector simulation, where multi-qubit (n qubit) circuits and states are represented using $2^n$ sized vectors. Then the quantum operations are carried out using matrices of size $2^n \times 2^n$. The time required for these computations to make is then plotted and as the graph suggests the computation time increases monotonously with increase in the number of qubits as is expected. 

To reduce the load we explore another "classical" technique employing the idea of tensors and writing them as a tensor of shape `(2,2,...)` n times. once this is done using `np.tensordot` reduces the time for computation drastically as is also shown in the code. 

The BONUS section deals with sampling of these simulated quantum states mimicking real measurements using `np.random.choice` parameterized by the coefficient probability of each of these states and secondly the calculation of expectation value for arbitrary operators of suitable `shape`.