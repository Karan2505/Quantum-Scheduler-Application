# TV Advertisement Scheduling Using Quantum Optimization

## Overview
This project leverages quantum computing techniques, specifically the **Quantum Approximate Optimization Algorithm (QAOA)**, to optimize TV advertisement scheduling. The goal is to allocate advertisements to time slots efficiently while ensuring that similar advertisements do not appear in the same break. The optimization is performed using **Amazon Braket**, a cloud-based quantum computing platform.

## Solution Approach

### 1. Encoding Advertisements
Each advertisement slot during a commercial break is assigned a quantum state (or qubit). The state of the qubit represents whether an ad is scheduled (`|1\rangle`) or not (`|0\rangle`).

### 2. Constraint Implementation
To ensure that similar advertisements do not appear in a single break, a **penalty function** is introduced in the quantum Hamiltonian. If two similar ads are scheduled in the same break, the energy of the system (cost function) increases.

### 3. Optimization using QAOA
QAOA is employed to find the lowest energy state of the system, representing the optimal advertisement schedule. The quantum algorithm iterates over different configurations to minimize the cost function.

### 4. Measurement
Once the quantum algorithm finds the optimal state, the qubits are measured to extract the final advertisement scheduling.

---

## Running on Amazon Braket
To execute the quantum optimization for advertisement scheduling, we utilize **Amazon Braket**. Below are the steps to set up the environment and run the code.

### 1. Environment Setup
Ensure you have the **Amazon Braket Python SDK** installed in your environment.
```bash
pip install amazon-braket-sdk
```

### 2. Import Necessary Libraries
```python
import numpy as np
from braket.circuits import Circuit
from braket.devices import LocalSimulator
from braket.aws import AwsDevice, AwsSession
from braket.ocean_plugin import BraketSampler, BraketDWaveSampler
```

### 3. Define the Problem
- Determine the number of qubits required.
- Formulate the objective function to minimize the total number of slots used while enforcing the constraint of avoiding product-type conflicts within the same slot.

### 4. Create a Quantum Circuit
Use the `braket.circuits.Circuit` class to construct the quantum circuit and define gates and operations that encode the problem constraints.
```python
circuit = Circuit()
circuit.h(range(num_qubits))  # Apply Hadamard gates to create a superposition
# Add additional gates for encoding the constraints
```

### 5. Execute the Quantum Circuit
#### Using a Local Simulator
```python
device = LocalSimulator()
```

#### Using an Actual Quantum Device (AWS)
```python
aws_session = AwsSession()
device = AwsDevice("your_device_arn", aws_session)
```

Run the circuit:
```python
task = device.run(circuit, shots=1000)
```

### 6. Retrieve and Analyze Results
```python
result = task.result()
print(result.measurement_counts)
```

### 7. Iteration and Optimization
- The objective function is optimized iteratively to find the best configuration.
- The QAOA approach ensures that the final schedule minimizes conflicts while effectively utilizing advertisement slots.

---

## Conclusion
This project presents an innovative approach to **TV advertisement scheduling** using **quantum optimization**. By leveraging QAOA and Amazon Braket, the algorithm efficiently allocates ads while adhering to predefined constraints, demonstrating the power of quantum computing in solving combinatorial optimization problems.

## References
- [Amazon Braket Documentation](https://docs.aws.amazon.com/braket/)
- [Quantum Approximate Optimization Algorithm (QAOA)](https://arxiv.org/abs/1411.4028)


