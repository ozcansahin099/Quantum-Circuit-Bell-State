# 🧠 Quantum Circuit - Bell States

## 1. Introduction
In quantum mechanics, entanglement allows two qubits to become strongly correlated. If you measure one, you immediately know the result of the other. For example, if you measure the first qubit and get |0⟩, the second qubit will also be |0⟩ — even if they are far apart.


## 2. Quantum Theory Behind Bell States
### What is a Bell state?

A Bell state is a maximally entangled state of two qubits. There are four possible Bell states, each representing a specific type of correlation between two qubits.
    
Example:
    
                 |Φ⁺⟩ = (1/√2) (|00⟩ + |11⟩)

This means the two qubits are strongly correlated. If you measure one, you immediately know the result of the other. For example, if you measure the first qubit in the |0⟩ state, the second qubit will also be in the |0⟩ state. This means the two qubits are correlated, and before the measurement, they exist in a superposition of both states. 


### Entanglement

Entanglement means that the state of one qubit cannot be described independently of the other. Measuring one qubit immediately determines the state of the other, no matter how far apart they are.

Example:
Measuring the first qubit as |0⟩ ensures the second is also |0⟩ — even if they are far apart.

### Superposition

A qubit can exist in a linear combination of |0⟩ and |1⟩:
                    
        |+⟩ = (1/√2) (|0⟩ + |1⟩)

This is called superposition, and it’s created by applying the Hadamard gate (H) to a qubit in state |0⟩.

### Ket notation

Multi-qubit states are represented using the tensor product of single-qubit states:

Basic single qubit states:

            |0⟩ = [1, 0]  
            |1⟩ = [0, 1]

Two-qubit state is written using tensor product:

            |00⟩ = |0⟩ ⊗ |0⟩ = [1, 0] ⊗ [1, 0] = [1, 0, 0, 0]

### The 4 Bell states

There are four Bell states:

            |Φ⁺⟩ = (1/√2)(|00⟩ + |11⟩)  
            |Φ⁻⟩ = (1/√2)(|00⟩ − |11⟩)  
            |Ψ⁺⟩ = (1/√2)(|01⟩ + |10⟩)  
            |Ψ⁻⟩ = (1/√2)(|01⟩ − |10⟩)

## 3. Mathematical Representation
- Tensor product: |ψ⟩ = |0⟩⊗|0⟩ = |00⟩

- Start with both qubits in |0⟩:


            |0⟩ = [1, 0]  
            |0⟩ ⊗ |0⟩ = [1, 0, 0, 0]

- Initial state is:  


            |ψ₀⟩ = |00⟩ = [1, 0, 0, 0]


- Hadamard matrix (H):


            (1/√2) *  [ [1, 1],  
                        [1,-1] ]

- Apply (H ⊗ I) to |0⟩:   


         H|0⟩ = (1/√2)(|0⟩ + |1⟩)
         
- We applied the Hadamard gate to the first qubit.


        |ψ₁⟩ = (1/√2)(|0⟩ + |1⟩) ⊗ |0⟩  
            = (1/√2)(|00⟩ + |10⟩)  
            = (1/√2) * [1, 0, 1, 0]
- CNOT matrix:


            [ [1, 0, 0, 0],  
             [0, 1, 0, 0],  
             [0, 0, 0, 1],  
             [0, 0, 1, 0] ]


- Apply CNOT to |ψ₁⟩:


    CNOT|ψ₁⟩ = (1/√2) *
        [ [1, 0, 0, 0],  
          [0, 1, 0, 0],  
          [0, 0, 0, 1],  
          [0, 0, 1, 0] ] * [1, 0, 1, 0]





- Final state calculation:


            |ψ₂⟩ = (1/√2)(|00⟩ + |11⟩)  
            = (1/√2) * [1, 0, 0, 1]


## 4. Circuit Design Logic
- Why Hadamard first?

We apply the Hadamard gate to the first qubit to create a superposition.

"Without the Hadamard gate, the CNOT would act on a definite |0⟩ state, and the system would remain unentangled."


        |0⟩ → (1/√2)(|0⟩ + |1⟩)

This superposition allows the system to explore both computational paths: |0⟩ and |1⟩.  
By doing this **before** the CNOT gate, we enable entanglement when the control qubit (qubit 0) is in a superposed state.
If we skip the Hadamard, CNOT would act only on |0⟩ — and no entanglement would occur.


- How CNOT causes entanglement?

The CNOT gate entangles the two qubits **only if the control qubit is in superposition**.

In our case, after the Hadamard gate, the first qubit is in the state:

            (1/√2)(|0⟩ + |1⟩)

Applying CNOT (control = qubit 0, target = qubit 1) gives:

            (1/√2)(|00⟩ + |11⟩)

CNOT flips the second qubit **only when the first qubit is |1⟩**, so it links the outcome of qubit 1 to qubit 0.

As a result, the two qubits become **entangled**:  
measuring one instantly determines the state of the other.


- Measurement and result probabilities

After applying the Hadamard and CNOT gates, the system is in the Bell state:

            |ψ⟩ = (1/√2)(|00⟩ + |11⟩)

When we measure both qubits in the computational basis, the probability of each outcome is:

- P(00) = 0.5  
- P(11) = 0.5  
- P(01) = 0.0  
- P(10) = 0.0

This means:
- There is a 50% chance to get |00⟩
- A 50% chance to get |11⟩
- You will **never** observe |01⟩ or |10⟩

These perfect correlations are the signature of quantum entanglement.

## 5. Implementation with Qiskit
- Import libraries

        !pip install qiskit
        !pip install qiskit qiskit-aer --upgrade --no-cache-dir --quiet
        !pip install pylatexenc --quiet

Qiskit is the main library used to build and simulate quantum circuits in this project.

            from qiskit import QuantumCircuit, transpile
            from qiskit_aer import AerSimulator # Updated import
            from qiskit.visualization import plot_histogram
            import matplotlib.pyplot as plt
        
- Create quantum circuit
We create a circuit with 2 quantum bits and 2 classical bits: 

            qc = QuantumCircuit(2, 2)  
- Add Hadamard and CNOT

            qc.h(0)
            qc.cx(0, 1)
            
- Measure and simulate

            qc.measure([0, 1], [0, 1])

## 6. Output and Analysis

### Plot the results
- Draw circuit


        display(qc.draw('mpl'))
        
![Circuit](https://i.imgur.com/AD3m5GB.png)


### Measurement result histogram


        backend = AerSimulator()
        compiled = transpile(qc, backend)
        job = backend.run(compiled, shots=1024)
        result = job.result()
        
 Result: {'11': 521, '00': 503}
        
### Histogram Visualization

        plot_histogram(result.get_counts()) # Use get_counts() without qc argument
        
![Result](https://i.imgur.com/9JUYdF4.png)  

## 7. Conclusion

### Key Takeaways


- How to create a Bell state using Hadamard and CNOT gates  
- The concept of quantum superposition and entanglement  
- How quantum circuits are represented mathematically  
- How to implement and simulate a Bell state using Qiskit  
- How measurement results reflect quantum entanglement

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1CvRQfi9aeKa1PI57KFSC8QrxNa70ohBR)

