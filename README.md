#  Grover's Algorithm for SAT Problems

This repository contains the implementation and theoretical analysis of **Grover's Algorithm**, demonstrating its application to solving unstructured search problems, specifically focusing on **Quantum Search** and the **3-SAT Problem**.

##  Overview of Grover's Algorithm

Grover's algorithm is a quantum algorithm designed for searching an unsorted database or an unstructured list of items more efficiently than classical algorithms.

* **Classical Approach:** The best a classical algorithm can do is to search through the list sequentially, requiring $O(N)$ queries in the worst case.
* **Grover's Algorithm:** It reduces the number of required queries to $O(\sqrt{N})$, providing a **quadratic speedup**.

---

##  Mathematical Foundations

The core of Grover's algorithm involves iteratively applying two main operators: the **Oracle** and the **Diffusion Operator**.

### 1. Initialization State $|s\rangle$

The initialization state $|s\rangle$ is the equal superposition state over all possible $n$-bit strings, achieved by applying the Hadamard gate to every qubit initialized to $|0\rangle$:

$$
|s\rangle = H^{\otimes n} |0\rangle^{\otimes n} = \frac{1}{\sqrt{N}} \sum_{x \in \{0,1\}^n} |x\rangle,
$$

where $N = 2^n$ is the total number of possible states. This state can also be represented in the basis of the marked state $|w\rangle$ and all other states $|w^\perp\rangle$:

$$
|s\rangle = \sqrt{\frac{N-1}{N}} |w^\perp\rangle + \sqrt{\frac{1}{N}} |w\rangle,
$$

### 2. Oracle Operator $U_w$

The Oracle $U_w$ marks the correct solution by flipping its sign (phase) while leaving others unchanged. This is achieved via a phase shift:

$$
U_w |x\rangle = (-1)^{f(x)} |x\rangle,
$$

where $f(x) = 1$ if $x$ is the marked state $|w\rangle$ and $0$ otherwise. In matrix form:

$$
U_w = I - 2 |w\rangle \langle w|.
$$

### 3. Grover's Diffusion Operator $U_s$

The diffusion operator $U_s$ amplifies the amplitude of the marked state while simultaneously decreasing the amplitudes of the other states. It inverts the state about the average amplitude.

$$
U_s = 2 |s\rangle \langle s| - I,
$$

Alternatively, using the Hadamard transformation:

$$
U_s = H^{\otimes n} \left( 2 |0\rangle^{\otimes n} \langle 0|^{\otimes n} - I \right) H^{\otimes n}.
$$

### Optimal Number of Iterations

The maximum probability of measuring the marked state is achieved after approximately $r$ iterations, where:

$$
r_{\text{max}} \approx \frac{\pi}{4} \sqrt{N}.
$$

For a search space of size $N$, this shows the **quadratic speedup** over classical $O(N)$ search.

---

##  Implementation: Quantum Search for 10 Marked Strings

### Problem Statement

We implemented Grover's Algorithm for efficient quantum search within a set of **10-bit strings**.
* **Search Space ($N$):** $|\mathcal{X}| = 2^{10} = 1024$.
* **Marked Strings ($M$):** 10 specific strings.
* **Objective:** To find the 10 marked strings with high probability using $O(\sqrt{N/M})$ oracle calls, significantly fewer than the classical $O(N)$ evaluations.

### Implementation Details

The quantum circuit is implemented using the **PennyLane** library. The circuit iteratively applies the Oracle and Diffusion operator to amplify the probability of the 10 marked strings.

The states with higher probabilities after the final measurement represent the marked strings, demonstrating that Grover's Algorithm successfully concentrates the probability on the correct solutions.

> **Access the code:** The Python implementation and full results are available in this [Colab Notebook](https://colab.research.google.com/drive/1QtZ3Pt3SqSADqeV-cqCsuxE1dMX8Q8vF?usp=sharing).

---

##  Implementation: Solving the 3-SAT Problem

Grover's Algorithm can be adapted to solve the **Constraint Satisfaction Problem (CSP)** known as 3-SAT.

### 3-SAT Formula

We focused on a 3-SAT formula with 3 variables ($x_1, x_2, x_3$). The formula is:

$$
(x_1 \lor x_2 \lor x_3) \land (\neg x_1 \lor x_2 \lor x_3) \land (\neg x_1 \lor \neg x_2 \lor \neg x_3) \land (\neg x_1 \lor \neg x_2 \lor x_3) \land (x_1 \lor x_2 \lor \neg x_3) \land (\neg x_1 \lor x_2 \lor \neg x_3)
$$

The goal is to find the assignment(s) of $x_1, x_2, x_3$ that make the entire formula **True**.

### Grover's Components for 3-SAT

1.  **Oracle:** The oracle is constructed as a matrix that checks every assignment. If an assignment satisfies the SAT formula, the corresponding diagonal entry in the oracle matrix is set to $-1$, marking it as a solution.
2.  **Diffusion Operator:** The standard Grover diffusion operator is used to amplify the probability amplitude of these marked (satisfying) states.

### Results and Verification

The implementation successfully found a solution to the 3-SAT problem.

| Metric | Value |
| :--- | :--- |
| **Found Solution** | `111` |
| **Probability** | $0.667$ |
| **All Valid Solutions** | `111, 101, 011` |

The algorithm's efficiency is highlighted by its fast execution time:

* **Oracle Creation Time:** 0.002 seconds
* **Solution Finding Time:** 0.036 seconds
* **Total Execution Time:** 0.038 seconds

Grover's algorithm provides a powerful quantum approach to solving classical hard search problems like 3-SAT, offering a significant speedup compared to classical brute force methods as the number of variables and clauses increases.

---

##  Conclusion

This project successfully implemented Grover's Algorithm using the PennyLane library to tackle the unstructured search problem, applying it to both a general 10-bit string search and a specific 3-SAT instance. The resulting quantum circuit demonstrates the ability to amplify the probability of the correct marked states, confirming the **quadratic quantum speedup** over classical alternatives.

##  References

1.  Grover’s Algorithm - PennyLaneAI. [Online]. Available: [https://pennylane.ai/qml/demos/tutorial\_grovers\_algorithm/](https://pennylane.ai/qml/demos/tutorial_grovers_algorithm/)
2.  Medium, “Explaining Grover’s Algorithm with a Colony of Ants: A Pedagogical Model for Making Quantum Technology Comprehensible,” Quantum Algorithms Untangled, [Online]. Available: [https://medium.com/quantum-untangled/grovers-algorithm-quantum-algorithms-untangled-bdf15c8ccfab](https://medium.com/quantum-untangled/grovers-algorithm-quantum-algorithms-untangled-bdf15c8ccfab)
3.  XanaduAI, “PennyLane Grover's Algorithm Tutorial,” GitHub, [Online]. Available: [https://github.com/XanaduAI/PennyLane-YouTube-Tutorials/blob/main/grovers\_algorithm.ipynb](https://github.com/XanaduAI/PennyLane-YouTube-Tutorials/blob/main/grovers_algorithm.ipynb)
4.  A fast quantum mechanical algorithm for database search. [Online]. Available: [https://www.clausiuspress.com/assets/default/article/2020/12/04/article\_1607101207.pdf](https://www.clausiuspress.com/assets/default/article/2020/12/04/article_1607101207.pdf)
5.  Grover’s Algorithm: Quantum Database Search. [Online]. Available: [https://arxiv.org/pdf/quant-ph/9605043](https://arxiv.org/pdf/quant-ph/9605043)
6.  Polynomial Quantum Speedups for Constraint Satisfaction Problems. [Online]. Available: [https://qibo.science/qibo/stable/code-examples/tutorials/grover3sat/README.html](https://qibo.science/qibo/stable/code-examples/tutorials/grover3sat/README.html)
7.  "Exactly 1-3-SAT Problem & Grover’s Algorithm,” LinkedIn, [Online]. Available: [https://www.linkedin.com/pulse/exactly-1-3-sat-problem-grovers-algorithm-breaking-rules-porfiris/](https://www.linkedin.com/pulse/exactly-1-3-sat-problem-grovers-algorithm-breaking-rules-porfiris/)
8.  “Exact 1-3-SAT Problem Grover’s Algorithm,” ScienceDirect, [Online]. Available: [https://pdf.sciencedirectassets.com/272574/1-s2.0-S0022000006X01782/1-s2.0-S0022000006001012/main.pdf](https://pdf.sciencedirectassets.com/272574/1-s2.0-S0022000006X01782/1-s2.0-S0022000006001012/main.pdf)
9.  Qibo, “Grover’s Algorithm for 3-SAT,” [Online]. Available: [https://qibo.science/qibo/stable/code-examples/tutorials/grover3sat/README.html](https://qibo.science/qibo/stable/code-examples/tutorials/grover3sat/README.html)
10. Classiq, “Grover’s Algorithm for 3-SAT,” [Online]. Available: [https://docs.classiq.io/latest/explore/algorithms/grover/3\_sat\_grover/3\_sat\_grover/](https://docs.classiq.io/latest/explore/algorithms/grover/3_sat_grover/3_sat_grover/)
11. Stanford, “Quantum Computation and Information,” CS269Q Projects 2019, [Online]. Available: [https://cs269q.stanford.edu/projects2019/Dalal\_Y.pdf](https://cs269q.stanford.edu/projects2019/Dalal_Y.pdf)
12. IEEE Xplore, “Optimized Grover’s Search for Constraint Satisfaction,” [Online]. Available: [https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9764017](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=9764017)
13. Qiskit, “Quantum Algorithms: Using Grover’s Algorithm,” [Online]. Available: [https://notebook.community/antoniomezzacapo/qiskit-tutorial/community/aqua/optimization/grover](https://notebook.community/antoniomezzacapo/qiskit-tutorial/community/aqua/optimization/grover)
14. K. C. Wang, H. G. L. Reuter, and M. J. L. O'Neill, “Quantum Algorithm Implementations for Beginners,” ACM, [Online]. Available: [https://dl.acm.org/doi/pdf/10.1145/3517340](https://dl.acm.org/doi/pdf/10.1145/3517340)
