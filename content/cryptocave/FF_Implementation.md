---
title: Implementing Finite Fields 
draft: true 
katex: true 
---

# Motivation 

implementing algebraic settings is incredibly important for cryptography. in particular, it is important to ensure fast operations in these settings.
getting familiar with these operations and what algorithms are available for them in the context of digital architectures is useful. this is my first
attempt at implementing a finite field. i'm hoping to keep notes here that document what i learn along the way. 

# incremental notes 

## Reduction Algorithms 

Modular multiplication can be done in the standard way. If we wish to compute $C = AB \pmod{N}$, then we can first multiply $A$ and $B$ then perform
division in order to obtain $P = NQ + C$, where $C, Q \in \mathbb{Z}\_{< N}$. 

**A Bit of History** 
1. Knuth: $O(n^{2})$ multiplications and $O(n)$ divisions, but divisions are expensive  
2. Observe that modern computer architectures internally store and compute on data in binary format using fixed word size $r = 2^{w}$. This means that
in practice
    - All arithmetic operations are computed implicity modulo $2^{w}$ for free 
    - Divisions and multiplications by powers of 2 are computed via shifts 
3. Barret: uses precomputed scaled variant of modulus' reciprocal to get cheap divisions $\longrightarrow$ Barret multiplication gets $O(n^{2})$
   multiplications. 
4. **Montgomery Multiplication**: 
    - Preferred choice in cryptographic applications. 
    - Most efficient method when generic modulus is used.
    - Has advantages when guarding against certain types of cryptographic attacks 
    - Generalization of Hensel's odd division for 2-adic numbers 

**To Include** 
1. Barrett Reduction 
2. Montgomery Reduction 
3. Crandall Reduction 
4. Solinas Reduction 

## Polynomials 

1. Horner's Method: optimal algorithm for evaluating polynomials. For a degree $n$ univariate polynomial $p(X)$, evaluation takes 
    - $n$ additions and $n$ multiplications or $n$ fused multiply-adds 
    - Storage requirement is $n \cdot \lceil \log_{2}(x)\rceil$ 
2. Preprocessing: Multi-point evaluation  
