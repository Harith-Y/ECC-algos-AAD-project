# ECDLP Solvers

Implementations of 5 algorithms for solving the Elliptic Curve Discrete Logarithm Problem.

## Algorithms

1. **Brute Force** - O(n)
2. **Baby-Step Giant-Step** - O(√n)
3. **Pohlig-Hellman** - O(∑√qᵢ) for smooth orders
4. **Pollard Rho** - O(√n) probabilistic
5. **Las Vegas** - Polynomial probabilistic

## Quick Start

```bash
# Run individual algorithm
python3 BruteForce/main.py input/testcase_1.txt
python3 BabyStep/main.py input/testcase_1.txt
python3 PohligHellman/main.py input/testcase_1.txt

# Compare all algorithms
python3 compare_algorithms.py # bruteforce + bsgs + pohlig (works properly till now)
```

## Input Format

```
p           # Prime modulus
a b         # Curve coefficients y² = x³ + ax + b
Gx Gy       # Base point G
n           # Order of G
Qx Qy       # Target point Q
```

Output: Secret `d` where Q = d·G

## Bonus: Partial Key Leakage

Side-channel attack simulations showing how leaked information breaks ECDLP:

```bash
# Deterministic algorithms (work well)
python3 BruteForce/bonus.py         # LSB leaks: 6,000x speedup
python3 BabyStep/bonus.py           # MSB leaks: 200x speedup
python3 PohligHellman/bonus.py      # Residue leaks

# Quick comparison
python3 compare_bonus_quick.py
```

**Bonus Results:**
- BruteForce: 16-bit LSB leak → 65,536x search space reduction
- BSGS: 16-bit MSB leak → Reduces √n parameter dramatically
- Pohlig-Hellman: Residue leaks eliminate CRT subproblems

## Project Structure

```
utils/                  # Shared utilities
  └── bonus_utils.py    # Leak simulation
BruteForce/
  ├── main.py           # Standard
  └── bonus.py          # With LSB leaks
BabyStep/
  ├── main.py
  └── bonus.py          # With MSB leaks
PohligHellman/
  ├── main.py
  └── bonus.py          # With residues
PollardRho/main.py
LasVegas/main.py
compare_algorithms.py
compare_bonus_quick.py
```

## Status

**Done:**
- ✅ All 5 core algorithms working
- ✅ Pohlig-Hellman with CRT
- ✅ 5 test cases (40-bit primes)
- ✅ Bonus for BruteForce, BSGS, Pohlig-Hellman
- ✅ Comparison scripts

**Not Practical:**
- ⚠️ Pollard Rho bonus (probabilistic, timeouts)
- ⚠️ Las Vegas bonus (unreliable, slow)

These are implemented but not usable for demos.

## Performance

| Algorithm | Complexity | Best For |
|-----------|-----------|----------|
| Brute Force | O(n) | n < 10⁶ |
| BSGS | O(√n) | n < 10¹² |
| Pohlig-Hellman | O(∑√qᵢ) | Smooth n |

**With Leaks:**
- Brute + 16-bit LSB: **6,000x faster**
- BSGS + 16-bit MSB: **200x faster**
- Pohlig + residues: **1.1-5x faster**
