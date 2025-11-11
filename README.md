## CSE_326 - Analysis and Design of Algorithms
## Course Instructor: Dr. Walid Gomaa
## ALG-S2023-04

# CSE 326 - Primality Testing and Prime Number Analysis

**Algorithms Course Project**  
*Submitted to: Dr. Walid Gomaa*

## Authors
- Ahmad Mongy (120200033)
- Ahmed Abdelkader (120200028)
- Mahmoud Akrm (120200045)
- Mohammed Ayman (120200081)
- Mohanned Ahmed (120200113)
- Peter Fayez (120200073)
- Yasser Ossama (120190122)
- Ziad Hesham (120200078)

---

## Abstract

This repository presents a comprehensive study of primality testing algorithms with a focus on the **Miller-Rabin probabilistic test** and its deterministic variants. The project implements efficient primality testing for integers up to 10¹⁰ and conducts extensive statistical analysis of prime number distributions and special prime properties including Twin Primes, Mersenne Primes, Sophie-Germain primes, and Pythagorean primes. Through empirical analysis using 10⁵ sampling points, we validate theoretical predictions from the Prime Number Theorem and explore the density distributions of various prime classifications.

## Table of Contents
1. [Introduction](#introduction)
2. [Theoretical Background](#theoretical-background)
3. [Implemented Algorithms](#implemented-algorithms)
4. [Prime Properties Analysis](#prime-properties-analysis)
5. [Experimental Results](#experimental-results)
6. [Installation and Usage](#installation-and-usage)
7. [References](#references)

---

## Introduction

Prime numbers have fascinated mathematicians for centuries and play a crucial role in modern cryptography. However, determining whether a given number is prime or composite remains a computationally challenging problem, particularly for large numbers. This project explores the evolution of primality testing algorithms from ancient methods to cutting-edge techniques, with emphasis on practical implementation and statistical analysis.

### Motivation

While deterministic algorithms like Trial Division and the Sieve of Eratosthenes are conceptually simple, they become impractical for large numbers as running time grows exponentially. More sophisticated approaches, such as the **Miller-Rabin algorithm** and the **AKS algorithm**, have been developed to overcome these limitations. This research focuses on the Miller-Rabin algorithm due to its practical efficiency and wide adoption in cryptographic applications.

---

## Theoretical Background

### Prime Number Theorem

The Prime Number Theorem provides an asymptotic estimate for the distribution of prime numbers. The prime counting function π(n), representing the number of primes less than or equal to n, can be approximated by:

```
π(n) ≈ n / ln(n)
```

More precisely:

```
lim(x→∞) [π(x) / (x/log(x))] = 1
```

This theorem underpins our statistical analysis and density calculations throughout the project.

### Fermat's Little Theorem

For a prime number p and coprime integer a:

```
a^(p-1) ≡ 1 (mod p)
```

This forms the basis of the Fermat Primality Test. While powerful, this test has limitations with Carmichael numbers—composite numbers that satisfy the equation for all bases coprime to n.

---

## Implemented Algorithms

### 1. Deterministic Algorithms (Comparison Baseline)

#### Naive Approach
- **Complexity:** O(n)
- **Method:** Tests divisibility by all integers from 2 to n-1
- **Limitation:** Impractical for large numbers

#### Trial Division
- **Complexity:** O(√n)
- **Method:** Tests divisibility only up to √n
- **Limitation:** Exponential in terms of binary representation

#### Sieve of Eratosthenes
- **Complexity:** O(n log(log n))
- **Application:** Primarily for generating all primes ≤ n
- **Method:** Iteratively marks multiples of each prime as composite

#### AKS (Agrawal-Kayal-Saxena) Test
- **Complexity:** O(d¹²) where d is the number of digits
- **Features:** First deterministic polynomial-time algorithm
- **Status:** Galactic algorithm—not used in practice due to large constants

### 2. Probabilistic Algorithms (Primary Focus)

#### Fermat Primality Test
```python
def Fermat_Test(n, k):
    if n == 1 or n == 4:
        return False
    elif n == 2 or n == 3:
        return True
    else:
        for i in range(k):
            a = randint(2, n-2)
            if pow(a, n-1, n) != 1:
                return False
        return True
```

- **Complexity:** O(k log n)
- **Advantage:** Fast filtering for key generation (e.g., RSA)
- **Limitation:** Can be fooled by Carmichael numbers

#### Miller-Rabin Primality Test (Primary Implementation)

The Miller-Rabin test extends Fermat's approach by factoring n-1 as 2^s · d where d is odd, then checking:

```
a^d ≡ 1 (mod n)  OR  a^(2^r · d) ≡ -1 (mod n) for some 0 ≤ r ≤ s-1
```

**Deterministic Variant:**  
For testing numbers up to:
- **3 × 10⁹:** Check bases {2, 3, 5, 7}
- **10¹⁹:** Check first 12 prime bases {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37}

This deterministic approach was used for our large-scale analysis up to 10¹⁰.

---

## Prime Properties Analysis

### 1. Twin Primes
**Definition:** Pairs of primes (p, p+2) with prime gap of 2  
**Examples:** (3,5), (5,7), (11,13), (17,19), (29,31), (41,43)

**Open Conjecture:** The Twin Prime Conjecture remains unproven—whether infinitely many twin prime pairs exist.

**Recent Progress:** Zhang (2014) proved the existence of infinitely many primes differing by some x < 7×10⁶, later reduced to 246.

### 2. Mersenne Primes
**Definition:** Primes of the form Mp = 2^p - 1

**Properties:**
- If p is composite, then 2^p - 1 is composite
- Efficient verification via Lucas-Lehmer Test
- Connection to perfect numbers: If Mp is prime, then (2^p - 1) · 2^(p-1) is a perfect number

```python
def Lucas_Lehmer(x):
    s = 4
    M = pow(2, x) - 1
    for i in range(x - 2):
        s = (s * s - 2) % M
    return s == 0
```

### 3. Sophie-Germain Primes
**Definition:** Prime p where 2p + 1 is also prime  
**Applications:** Cryptography and pseudo-random number generation

### 4. Pythagorean Primes
**Definition:** Primes of the form p = 4x + 1  
**Property:** Can be expressed as the sum of two squares and appear as hypotenuses in Pythagorean triples

---

## Experimental Results

### Methodology
- **Range:** Analyzed integers up to 10¹⁰
- **Sampling:** Constant interval of 10⁵ (yielding 10⁵ data points)
- **Regression:** Least-square fitting using SciPy optimization
- **Algorithm:** Deterministic Miller-Rabin with proven base sets

### Key Findings

#### 1. Prime Density Distribution
Our empirical analysis confirms the Prime Number Theorem:

- Fitted function: `a · (x/ln(x)) + b`
- As n increases, the approximation converges to x/ln(x)
- Prime density approaches 1/ln(x) asymptotically

**Observation:** Higher number ranges show increasingly accurate agreement with theoretical predictions.

#### 2. Twin Prime Density
Analysis reveals consistent patterns in twin prime distribution, though density decreases as n increases, supporting the rarity of twin primes in higher ranges.

#### 3. Mersenne Prime Rarity
Mersenne primes exhibit extreme rarity, with density dropping significantly compared to general primes, confirming their special nature.

#### 4. Sophie-Germain and Pythagorean Prime Distributions
Both classes show predictable density patterns, with Sophie-Germain primes being notably rare due to their dual-prime requirement.

### Statistical Validation
All density analyses include:
- Comparative plots across different magnitude ranges (10¹ to 10¹⁰)
- Ratio analysis relative to overall prime density
- Asymptotic behavior verification

---

## Installation and Usage

### Prerequisites
- Python 3.x
- Required libraries:
  ```bash
  pip install numpy scipy matplotlib
  ```

### Running Primality Tests

```python
from miller_rabin import deterministic_miller_rabin

# Test a single number
n = 982451653
is_prime = deterministic_miller_rabin(n)
print(f"{n} is {'prime' if is_prime else 'composite'}")

# Generate primes in a range
primes = [p for p in range(1, 10000) if deterministic_miller_rabin(p)]
```

### Running Statistical Analysis

```python
from analysis import analyze_prime_density, analyze_twin_primes

# Analyze prime distribution up to 10^8
analyze_prime_density(max_n=10**8, sample_interval=10**5)

# Analyze twin primes
analyze_twin_primes(max_n=10**8, sample_interval=10**5)
```

### Performance Benchmarks

| Algorithm | Numbers Tested | Time Complexity | Practical Limit |
|-----------|----------------|-----------------|-----------------|
| Trial Division | 10⁶ | O(√n) | ~10⁹ |
| Fermat Test (k=20) | 10⁹ | O(k log n) | 10¹⁵+ |
| Miller-Rabin (deterministic) | 10¹⁰ | O(k log n) | 10¹⁹ |

---

## Research Contributions

1. **Empirical validation** of the Prime Number Theorem across 10 orders of magnitude
2. **Comprehensive density analysis** of four special prime classes
3. **Efficient implementation** of deterministic Miller-Rabin for large-scale testing
4. **Statistical framework** for analyzing prime distributions using least-square regression
5. **Performance comparison** between deterministic and probabilistic approaches

---

## Future Work

- Extension to primality testing beyond 10¹⁹ using advanced witnesses
- Implementation of elliptic curve primality proving (ECPP)
- Analysis of prime gaps and their distribution
- Investigation of computational efficiency improvements using parallel processing
- Study of prime number applications in modern cryptographic protocols

---

## References

1. McKee, M. (2013). "First proof that prime numbers pair up into infinity." *Nature*. https://doi.org/10.1038/nature.2013.12989

2. Zhang, Y. (2014). "Bounded gaps between primes." *Annals of Mathematics*, 179(3). https://doi.org/10.4007/annals.2014.179.3.7

3. Lehmer, D. H. (1927). "Tests for primality by the converse of Fermat's theorem." *Bull. Amer. Math. Soc.*, 33(3), 327-340.

4. Jaeschke, G. (1993). "On strong pseudoprimes to several bases." *Mathematics of Computation*. https://doi.org/10.1090/S0025-5718-1993-1192971-8

5. SciPy Documentation: curve_fit. https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.curve_fit.html

---

**Course:** CSE 326 - Algorithms  
**Academic Year:** 2020-2021

![Logo](assets/logo.png)


