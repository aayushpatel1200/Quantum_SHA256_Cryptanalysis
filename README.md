# Quantum Cryptanalysis: SHA-256 vs Grover's Algorithm

A research notebook exploring whether quantum computers can crack SHA-256. Short answer: not really, and this project shows exactly why - with real circuits, real benchmarks, and actual numbers to back it up.

Built using Qiskit and run on both ideal simulators and IBM's fake hardware backends to model real-world noise.

---

## What This Is About

SHA-256 is used everywhere - Bitcoin, TLS, password hashing. The fear is that quantum computers will eventually break it. Grover's algorithm is the quantum attack that people worry about most, because it can search an unsorted space in √N steps instead of N.

This project actually implements that attack, runs it at small scales where it's feasible, and then extrapolates to show what it would take to threaten SHA-256 at full 256-bit scale. Spoiler: the numbers are astronomical, even with a quantum computer.

---

## Key Findings

- Grover's algorithm does give a real quadratic speedup over classical brute-force search
- At small bit sizes (3–6 bits), the quantum circuit finds preimages with high probability
- Scaling to 256 bits would require ~2¹²⁸ operations on a fault-tolerant quantum computer - completely infeasible with any foreseeable hardware
- Noise on real (or simulated real) quantum hardware degrades success probability significantly - ideal conditions are far from what you'd actually get
- SHA-256 is post-quantum safe for the foreseeable future
#### # You can find the results & analysis indepth in the Quantum_Project_Report.pdf
---

## What's Inside the Notebook

| Section | What it covers |
|---|---|
| 1 - Setup | Package installation and imports |
| 2 - Hash Functions | SHA-256 explained, truncated versions for experiments |
| 3 - Grover's Algorithm | Oracle + diffusion operator, full implementation |
| 4 - Classical vs Quantum | Side-by-side search comparison across bit sizes |
| 5 - Preimage Attack | Actual quantum attack on a truncated SHA-256 |
| 6 - Iteration Analysis | How over/under-iterating Grover's affects success rate |
| 7 - Noise Analysis | Ideal simulator vs custom noise model vs FakeManila backend |

---

## Requirements

```bash
pip install qiskit qiskit-aer qiskit-ibm-runtime matplotlib numpy pylatexenc
```

Or just run the first cell in the notebook - it installs everything automatically.

---

## Running It

1. Open the notebook in Jupyter or VS Code
2. Run all cells from top to bottom
3. The notebook is self-contained - no external data files needed

```bash
jupyter notebook quantum_sha256_project.ipynb
```

---

## How Grover's Algorithm Works (Quick Version)

1. Start with all possible inputs in equal superposition
2. **Oracle** - flips the phase of states that match the target hash
3. **Diffusion** - amplifies the flipped states, suppresses everything else
4. Repeat oracle + diffusion roughly √N times
5. Measure - the right answer comes out with high probability

At 3 qubits (8 states), this works in ~2 iterations. At 256 bits (2²⁵⁶ states), you'd need ~2¹²⁸ iterations. That's not a practical attack.

---

## Noise Modeling

Section 7 compares three scenarios:

- **Ideal simulator** - perfect gates, no errors, best-case results
- **Custom noise model** - configurable depolarizing and readout errors
- **FakeManila** - Qiskit's simulation of a real IBM quantum device, with calibrated noise from actual hardware

The noise analysis shows how quickly quantum advantage disappears as circuits get deeper and gate errors compound.

---

## Authors

Aayush Patel & Dhwani Patel  
CPSC 4110 - Quantum Algorithms  
University of Lethbridge, December 2025
