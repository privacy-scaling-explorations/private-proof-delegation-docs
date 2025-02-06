# FHE Schemes

> This is a WIP.

Some materials we use for below notes:
> [Summary of FHE Slides](https://drive.google.com/file/d/1NYeUNnmQooY7Ty53nYuSNo9PhSGgkc15/view) [1]
> [Summary of FHE Blog](https://www.jeremykun.com/2024/05/04/fhe-overview/) [2]
> [FHE survey paper 2022](https://eprint.iacr.org/2022/1602.pdf) [3]
> [Intro to FHE Video](https://www.youtube.com/watch?v=y3Mu3GSRoTA) [4]
> [CHIMERA](https://eprint.iacr.org/2018/758.pdf) [5]

## Preliminaries on Mumber/Polynomial Representation

Number can be presented in "whole", radix, rns, or tower form:
- "whole" when the max value is n while the domain is [0,n-1], examples include (native) prime field modulo arithmetic
- radix is in the form of $x = \sum_0^{l-1} x_i \cdot b^i$ where $x_i \in [0, b-1]$ and $b$ is the base, decimal or binary representation are examples of radix form
    > radix form is more general than the "whole" where we can say that $n$ is the base and we only count $i$ to $0$
    > we can do additions in parallel but we have to accumulate the carries
    > we can do comparison fast by going from most significant "bit" to least significant "bit"
- rns, (residue number system, see more at https://hackmd.io/@namncc/Hki_tclq6), is a more general form where we have different bases as long as they are co-prime $x_i = \sum_0^{l-1} r_i \cdot p_i$ (the $p_i$' s co-prime with each other)
    > if all $p_i$' s are different then we have advantage when doing additions in parallel as there is no carry
    > we have to convert to radix form or "whole" for comparison
- tower form, TBD

Nevertheless, when we have bases (radix or rns) we can do SIMD (packing).

Polynomial can be presented in coefficient or evaluation form:
- coefficient: $f(X) = \sum_0^{d-1} a_i \cdot X^i$
- evaluation: $\{x_i, f(X_i)\}_0^{d-1}$

Coefficient of polynomials are numbers represented in forms above;
Polynomial also has bases ($f(X) = q(X) \cdot z(X)$): 
- $z(X) = X^{l-1} + 1$ (used in (R)LWE [2])
- $z(X) = \prod_0^{l-1} (X - x_i)$

When dealing with polynomial we rely on FFT (NTT) for fast computation (as opposed to radix or rns parallelization of numbers computation).

## Generations of FHE schemes

1st, 2nd, 3rd ([1]):
![Screenshot 2025-02-06 at 09.38.20](https://hackmd.io/_uploads/H1lhejbF1l.png)

4th ([1]):
![Screenshot 2025-02-06 at 09.40.07](https://hackmd.io/_uploads/S1pMWoWtkx.png)

> 4th is not directly usable because it is on approximate numbers, however its idea should be useful for tweaks (later schemes)

## Preliminaries on Encapsulation and Noise Growth Management

Encapsulation of numbers (e.g. encryption) can carry noises, e.g. an encapsulation of 16 "bits" (think in radix or rns form) can have actual value of upto 4 bits and noises upto 12 bits, and we can always recover the actual value if the noises do not exceed that 12 bits threshold.
> noises increase "a bit" when adding two encapsulations, in our example if noise is 11 bits and adding two of that we still have only 12 bits and still good, but not beyond that
> noises increase a lot when multiplying an encapsulation with a number, e.g. multiply an encapsulation with 8 bit noise and a 4 bit number still get good encapsulation of 12 bits noise
> noises increase a lot lot when multiplying two encapsulations, e.g. multiply two encapsulations of 4 bits value and 2 bits noise is safe with 12 bits noise afterwards, but not beyond that

Its all about noise growth management when handling encapsulation.
If we trivially make something like 4 bits value with 1.000.000 bits of noise we can have a lot of multiplication depth but this is expansive in terms of both storage and computation.

### Noise Growth Management Strategy

#### Relinearization (a.k.a Key Switching, 2nd + 3rd gen FHE) [2]

![Screenshot 2025-02-06 at 10.00.17](https://hackmd.io/_uploads/H1F0HsZt1e.png)
> quite circular and we need to keep the "degree" of the relinearization polynomial low below the noise threshold

#### Modulus Switching (2nd + 3rd gen FHE) [4]
![Screenshot 2025-02-06 at 10.28.46](https://hackmd.io/_uploads/r1OF2jZtJg.png)

#### Rescaling (4th gen FHE) [2]
![Screenshot 2025-02-06 at 09.40.07](https://hackmd.io/_uploads/S1pMWoWtkx.png)

## Important Schemes

### 1st gen FHE [3]
![Screenshot 2025-02-06 at 10.08.24](https://hackmd.io/_uploads/ByfCwobF1x.png)

### 2nd gen FHE [3]
![Screenshot 2025-02-06 at 10.09.30](https://hackmd.io/_uploads/rJ6wOoWKJg.png)

### 3rd gen FHE [3]
![Screenshot 2025-02-06 at 10.13.59](https://hackmd.io/_uploads/B1XMtibFyx.png)

### 4th gen FHE [3]
![Screenshot 2025-02-06 at 10.15.38](https://hackmd.io/_uploads/r1SdKs-tye.png)

### Comparison [3]

> 3rd gen is faster but requires more communication (ciphertext size) --> we encrypt (and pack) with 2nd gen for communication and then switch (and unpack) to 3rd gen for computation!

![Screenshot 2025-02-06 at 10.17.24](https://hackmd.io/_uploads/ByPl9jbFyl.png)
![Screenshot 2025-02-06 at 10.18.19](https://hackmd.io/_uploads/rJCZ5iWFke.png)

> For non-binary we use 2nd gen and binary we use 3rd gen

## A bit more details on important Schemes

### BGV/BFV, non-binary, can be fixed point, i.e. Relinearization (Key Switching) + Dimension-Modulus Reduction (Modulus Switching)

#### BGV

Scalar in $\mathbb{Z}_q$ (ring prime $q$), vector size $n$, error $e < q/2$ from a distribution $\chi$

- Enc: $\mathbf{c} = (\mathbf{a}, b = \mathbf{a}\cdot\mathbf{s} + 2e + m)$
- Dec: $m = (b - \mathbf{a}\cdot\mathbf{s}) \mbox{ mod } 2$

Relinearization

#### FV

### CKKS, approx., i.e. Rescaling

### TFHE, binary, short, i.e. Programmable Boostrapping


## More recent advances

### GBFV

## Important concepts (whiteboxing the schemes)

### Key Switching
No noise reset but can switch to a new (can be smaller) key

### Modulus Switching
Can reset noise and switch to a smaller modulus

### Packing/Unpacking
Pack and unpack, think about sending a packed ciphertext (not possible to do mult) that can be unpacked (using some big key) into several ciphertexts (that can do mult)

### Amortization
Doing things in a batch so that it cost less in average (caution: may not parallelizable)

### Rotation (Automorphism)

### Programmable Bootstrapping
Beyond multiplication, basically a lookup table

### Functional Bootstrapping
This is similar to PB?, think about doing a specific hash like SHA256?

### Scheme Switching
CHIMERA on LWE/RLWE schemes, think about switching from non-binary to binary

### Caution
All convenient things may require a BIG one time setup (but may be it is fine we do that and we are free next time)

## A bit more complex: Arithmetic Simulation

A whole number modulo p can fit "natively" into an FHE ciphertext prime p base but can be expansive depending on p.

A whole number modulo p can fit "non-natively" into an FHE ciphertext composite n in rns form $n = \prod_0^{l-1} p_i$ resulting in many ciphertexts, cheaper in computation but has to do modulo reduction on p and this is expansive.

A byte array can fit "natively" into a binary ciphertext.
