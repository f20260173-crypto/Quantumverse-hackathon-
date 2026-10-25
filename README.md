# QuantumVerse 4.0 – Track 1: Interaction-Free Bomb Tester

A Qiskit implementation of the Elitzur–Vaidman bomb tester, improved with angle tuning and a repeated-probe (quantum Zeno) version, plus a noise study.

## What it does
1. **Mach–Zehnder interferometer, no bomb:** `RX(θ)` then `RX(-θ)` on the path qubit. The output is deterministic (`00` on every shot).
2. **Bomb circuit:** a CNOT from the path qubit to a bomb qubit. Reports detection, explosion and inconclusive probabilities.
3. **Score improvement:** score = detection / explosion = cos²(θ/2) for one probe.
   - θ = π/2 gives 50 % (standard)
   - θ = π/3 gives 75 % (improved angle)
   - A Zeno version with N cycles of `RX(π/N)` pushes the ratio far higher.
4. **Noise study:** depolarizing gate errors and readout errors, tracking the score and the false-alarm rate.
5. **Resources:** qubits, depth, run time and parameter count.

## Qubit mapping
- Qubit 0: path (`|0>` = safe arm, `|1>` = bomb arm)
- Qubit 1: bomb (`1` = exploded)

## Theory (ideal, single probe)
| Angle θ | Detection | Explosion | Inconclusive | Score |
|---|---|---|---|---|
| π/2 | 0.250 | 0.500 | 0.250 | 50 % |
| π/3 | 0.1875 | 0.250 | 0.5625 | 75 % |
| π/6 | 0.0625 | 0.067 | 0.871 | 93 % |

Zeno version (N cycles of π/N): detection = cos^2N(π/2N), explosion = 1 − detection, inconclusive = 0.

## Simulated results
All experiments use 10,000 shots. The tables and plots are in the notebook outputs.

<!-- TODO: paste the Section 6 summary table from your run here -->

## How to run
Open `Track1_bomb_tester.ipynb` in Google Colab and choose Runtime → Run all, or locally:

```
pip install -r requirements.txt
jupyter notebook Track1_bomb_tester.ipynb
```

## Notes
- The bomb check in the Zeno circuit is a mid-circuit measurement and reset of the probe qubit, so it needs only 2 qubits.
- Barriers and `optimization_level=0` stop the transpiler from cancelling `RX(θ)` against `RX(-θ)`.
- Noise model: depolarizing error `p/10` on `RX`, `p` on `CNOT`, readout flip `p/2`.
