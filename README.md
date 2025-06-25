# Entanglement aided transpilation of star qubit problems

Star qubit problems: 
Those problems that have a qubit who has many control operations onto other qubits. It is the "star" of the show!

Examples:
- Hadamard test phase extraction
- Error correction (shor)
- Entanglement preparation

Given that the above examples are used as subroutines or main-parts of many algorithms, we think this problem is of importance and of interest to optimize.

The star qubit is defined as the one that is "most connected" to other qubits.

## Approach

Given that a single qubit is central, we want to make it reachable to the rest of the system.

Solution is to prepare an entangled state which represents this "star qubit".

Given a hardware topology, we can find how to minimally distribute entangled qubits, such that every other qubit is directly coupled to an entangled qubit.

This enables each qubit to have a direct control operation by the "star" qubit.

This distribution of entangled qubits given a topology can be solved via "minimum dominating set" algorithms.

Once the entangled qubits are set, then you map an input circuit with the "star qubit" structure.

## Focus

For simplicity, we focus on Hadamard test phase extraction, using a set of random c-phase gates.

We test it on a naive approach without entanglement, and with entanglement.

We compare the errors to show that entanglement yields still correct results.

We then transpile the circuits using all available qubits.

The naive circuit is transpiled naiveley.

The entangled circuit is transpiled using a custom pipeline which considers the connectivity of the hardware.

We find a 50% improvement on output transpilation.

## Further explorations

### Identifying optimal star qubit via hyperparameter optimization

Superconducting qubits vary in quality across a chip, with some pairs yielding lower two-qubit gate error rates than others. We optimize “star”-shaped circuits by scoring each physical qubit based on connectivity, local CX error rates, and proximity to other qubits—then selecting the best as the circuit’s hub. The weighting of these score components is tuned using Optuna to minimize total CX error after transpilation.

### Metropolis optimizer of transpilation pipeline

In this part, we implement a metropolis algorithm to find a quasi-optimal pass manager pipeline for a given Quantum Circuit and Backend architecture. We start by proposing a random combination of pass manager options, and we then vary one of steps (initialization, layout, routing, synthesis). We accept the change only if the circuit depth is reduced in comparisson with the past configuration, or with a small probability if it increases the depth, so that we avoid local minima. We repeat this process for a given number of steps, and return the best pipeline that was found.

### GHZ preparation via binary trees

Use Binary Tree of CNOT gates to Construct a GHZ state and integrate this structure into the circuit.GHZ States and integrate it in our circuit.
