# Building blocks for zkp

* Arithmetisation
    * R1CS(https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649)
    * PLONK(https://eprint.iacr.org/2019/953.pdf)
    * AIR(https://hackmd.io/@aztec-network/plonk-arithmetiization-air)
    * CCS(https://eprint.iacr.org/2023/552)
    * Lookup Singularity
        * Jolt(https://eprint.iacr.org/2023/1217)

* PIOPs(grand prod, permutation, etc)
    * Univariate PIOPs - plonk https://eprint.iacr.org/2019/953.pdf
    * Multivariate PIOPs - hyperplonk https://eprint.iacr.org/2022/1355
* PCS
    * Univariate
        * KZG https://dankradfeist.de/ethereum/2020/06/16/kate-polynomial-commitments.html
        * IPA https://eprint.iacr.org/2017/1066.pdf
        * FRI https://eccc.weizmann.ac.il/report/2017/134
        * Circle STARK https://eprint.iacr.org/2024/278
        * STIR https://eprint.kobi.one/2024/390
    * Multilinear
        * Dory https://eprint.iacr.org/2020/1274.pdf
        * Hyrax https://eprint.iacr.org/2017/1132.pdf
        * FRI-Binius https://eprint.iacr.org/2024/504
        * Zeromorph https://eprint.iacr.org/2023/917
        * Gemini https://eprint.iacr.org/2022/420.pdf
        * Brakedown & Orion https://eprint.iacr.org/2021/1043.pdf
    * Lattice Based
        * Ajtai Commitment [Ajt96]
        * SLAP https://eprint.iacr.org/2023/1469

* Implementations - https://github.com/awesomezkp/AwesomeZKP?tab=readme-ov-file#library--repo
    * Libsnark
    * Gnark
    * Bellman
    * Snarkjs
    * Halo2
    * Plonky3
    * Spartan
    * Jolt
