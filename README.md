# Quantum Computer Programs

**Qubits**

Current computers manipulate individual bits as binary 0 and 1 states but Quantum computers leverage quantum mechanical phenomena to manipulate information, relying on quantum bits, or qubits.

**Quantum Registers**

A Quantum computer stores its data in quantum registers and they can store data in quantum superposition.

These examples of Quantum computing are reused with some modification from IBM's Qiskit https://github.com/Qiskit . Qiskit is an open-source framework for working with quantum computers at the level of circuits, pulses, and algorithms.

**Circuit**

quantum circuit usually starts with the qubits in the |0,…,0> state and evolve with results of operation.

**Gates**

A quantum gate is a basic quantum circuit operating on a small number of qubits. Number of qubits in the input and output of the gate must be equal

##Hammard Gate

gate rotates the states |0⟩ and |1⟩ to |+⟩ and |−⟩ .
It acts on a single qubit. It maps the basis state.

|0} to (|0}+|1}) / $latex \sqrt{2}  $ and |1} to (|0}-|1}) / $latex \sqrt{2}  $

This means that measurement will have equal probabilities to become 0 or 1 thereby creating a state of superimposition.

##Controlled (cX cY cZ) gates

controlled-X gate also called  controlled-NOT
acts on a pair of qubits, with one acting as ‘control’ and the other as ‘target.
whenever the control is in state |1⟩ 

##X, Y , Z Gates

rotating the qubit state around the x , y or z axis by the given angle.
On the Bloch sphere
- Rx gate - corresponds to rotating the qubit state around the x axis by the given angle.
- Ry Gate - corresponds to rotating the qubit state around the y axis by the given angle.
- Rz Gate - corresponds to rotating the qubit state around the z axis by the given angle.

##Pauli- X, Pauli-Y , Pauli-Z gates

Acts on a single qubit. It equates to a rotation around the X, Y and Z-axis  by  radians
- Pauli-X maps |0} to |1} and |1} to |0} . Thus also called bit flip
- Pauli-Y maps |0} to i|1} and |1} to -i|0} . Thus also called bit flip
- Pauli-Z matrix leaves the basis state |0} unchanged and maps |1} to -|1} . It is also called phase-flip

##U1 , U2 and U3 gates

- U1 - Equivalent to Rz.
- U2 - two parameters control two different rotations within the gate. Has a duration of one unit of gate time.
- U3 -three parameters allow the construction of any single qubit gate, Has a duration of one unit of gate time.

##SWAP Gate 

Swaps the state of 2 qubits 

##$latex \sqrt{2}  $ SWAP

performs half-way of a two-qubit swap

**Measurement**

Circuit representation of measurement. The two lines on the right hand side represents a classical bit, the single line on the left hand side represents a qubit.

The two lines on the right hand side represents a classical bit, the single line on the left hand side represents a qubit.

## Program 

Following program creates a quantum register Circuit to perform operation and a classical Register to store the information once a result has been obtained.

Some simple programming constructs examples:

Initialising with 2 qubits in the zero state
```
circuit = QuantumCircuit(2, 2)
```
or Quantum Circuit acting on a quantum register of three qubits where by default, each qubit in the register is initialized to |0⟩
```
circ = QuantumCircuit(3)
```

A Hadamard gate H on qubit 0, which puts it into the superposition state
```
circ.h(0)
```
Execute the circuit based on some simulator and give no of shots
```
job = execute(circuit, simulator, shots=100)
```

Lets try some programs 

### Program 1 : Flipping values of Qbits using X gate 
```
qr = QuantumRegister(2, name="q")
cr = ClassicalRegister(1, name="result")
circuit = QuantumCircuit(qr, cr)

# Inverse qr state , since it was initially 0 by default it becomes 1 
circuit.x(qr)

# measure qbit 0 
circuit.measure(qr[0], cr)
circuit.draw()

job = execute(circuit, backend=local_simulator)
job.result().get_counts()
```
output for flipped state of X is 1 with maximum probability
{'1': 1024}

### Programs 2 : Add a CX (CNOT) gate on control qubit 0 and target qubit 1

To demonstrate the CX gate which flips the value of target qubit only when control qubits state is 1 , we need following steps

step 1  - declare 2 Quantum registers ( target i1 and control i2)
and declare 1 classical register for result
```
i1 = QuantumRegister(1, name='input1')
i2 = QuantumRegister(1, name='input2')
cr = ClassicalRegister(1, name="result")
```

step 2 - First invert control state i1 , that is set i1 to 1 . Then apply controlled x on target i1 when i2 is control is 1
```
circuit = QuantumCircuit(i1, i2, cr)
circuit.x(i1) 
circuit.cx(i1, i2) 
```

Measure i2 and get count of probability
```
circuit.measure(i2, cr)
job = execute(circuit, backend=local_simulator)
job.result().get_counts()
```

Output
{'1': 1024}

### Program 3 : quantum circuit for Bells State

Transforms two qubits, both of which started off in the state
|0⟩, into the state
|00⟩ + |11⟩√2

Bell states - 50-50 chance of measuring 0 or 1 on any one of the 2 qubits.

Imports statements from Qiskit
```
import numpy as np
from qiskit import(  QuantumCircuit,  execute,  Aer)
from qiskit.visualization import plot_histogram
```
Program to add Gates to the circuit one-by-one to form the Bell state
|𝜓⟩=(|00⟩+|11⟩)/ √2

Use Aer's qasm_simulator
```
simulator = Aer.get_backend('qasm_simulator')
circuit = QuantumCircuit(2, 2)
```

Add a H gate on qubit 0 , making it go in superimposiion
```
circuit.h(0)
```

Add a CX (CNOT) gate on control qubit 0 and target qubit 1 making them go into state of entanglement
```
circuit.cx(0, 1)
```

Measure qbit 0 and 1 into classical 0 and 1
```
circuit.measure([0,1], [0,1])
```
Execute the circuit on the qasm simulator and grab results from the job
```
job = execute(circuit, simulator, shots=1000)
result = job.result()
```
Return counts for the occurrence of states 00 and 11 
```
counts = result.get_counts(circuit)
print("\nTotal count for 00 and 11 are:",counts)
```
Under ideal conditions ( simulator ) it may be equal 
Total count for 00 and 11 are: {'00': 500, '11': 500}

However with actual super computers , results will differ by a small margin 
Total count for 00 and 11 are: {'00': 553, '11': 447}

Draw the circuit
```
circuit.draw()
```