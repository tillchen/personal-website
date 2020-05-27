---
title: "Secure and Dependable Systems Notes"
date: 2020-05-26T16:11:13+02:00
draft: false
description: "Course notes for Secure and Dependable Systems at Jacobs University Bremen"
tags: ["Computer Science"]
---

* [Introduction](#introduction)
  * [Recent Computing Disasters](#recent-computing-disasters)
  * [Dependability Concepts and Terminology](#dependability-concepts-and-terminology)
  * [Dependability Metrics](#dependability-metrics)
* [Software Engineering Aspects](#software-engineering-aspects)
  * [General Aspects](#general-aspects)
  * [Software Testing](#software-testing)
  * [Software Specification](#software-specification)
  * [Software Verification](#software-verification)
* [Software Vulnerabilities and Exploits](#software-vulnerabilities-and-exploits)
  * [Control Flow Exploits](#control-flow-exploits)
* [Cryptography](#cryptography)
  * [Cryptography Primer](#cryptography-primer)
  * [Symmetric Encryption Algorithms and Block Ciphers](#symmetric-encryption-algorithms-and-block-ciphers)
  * [Asymmetric Encryption Algorithms](#asymmetric-encryption-algorithms)
  * [Cryptographic Hash Functions](#cryptographic-hash-functions)
  * [Digital Signatures and Certificates](#digital-signatures-and-certificates)
  * [Key Exchange Schemes](#key-exchange-schemes)
* [Secure Communication Protocols](#secure-communication-protocols)
* [Information Hiding and Privacy](#information-hiding-and-privacy)
* [System Security](#system-security)
* [References](#references)

## Introduction

This is the course notes for Secure and Dependable Systems by Dr. Jürgen Schönwälder at Jacobs University Bremen.

### Recent Computing Disasters

1. Main memory and CPU memory caches: when the data need is in the main memory but not in the caches, the CPU has to wait quite a while.

2. Side channel attack: an attach where info is gained fro the physical implementation of a computer system (e.g., timing, power consumption, radiation) rather than weaknesses in an implemented algorithm itself.

3. Speculative execution: in a situation where a CPU would have to wait for slow memory, simply guess a value and continue execution speculatively.

4. Reading arbitrary memory

### Dependability Concepts and Terminology

1. System, environment and system boundary:
    * System: an entity that interacts with other entities (e.g. OS kernel)
    * Environment: the other systems. (e.g. hardware)
    * System boundary: the common frontier between the system and its environment. (e.g. the set of OS calls)
    * Systems almost never exist in isolation.

2. Components and state:
    * Components: the structure of a system is composed of a set of components, where each component is another system. The recursion stops when a component is considered atomic.
    * Total state: the set of the following states: computation, communication, stored info, interconnection, and physical condition.

3. Function and behavior:
    * Function: what the system is intended to do, which is described by the function specification.
    * Behavior: what the system does to implement its function and is described by a sequence of states.

4. Service and correct service:
    * Service: the behavior perceived by users; a user is another system that receives service from the service provider.
    * Correct service: the service that implements the system function.

5. Failure, error, and fault: (Threats)
    * Failure: service failure, when the delivered service deviates from correct service.
    * Error: the part of the total state that may lead to subsequent failure.
    * Fault: the adjudged or hypothesized cause of an error. A fault is active when it produces an error, otherwise it's dormant.
    * Fault -> Error -> Failure.

6. Dependability:
    * The ability of a system to deliver service than can justifiably be trusted.
    * The ability to avoid service failures that are more frequent and more severe than is acceptable.

7. Dependability attributes:
    * Availability: readiness to deliver
    * Reliability: continuity
    * Safety: no catastrophic consequences
    * Integrity: no improper alterations
    * Maintainability: undergo modifications
    * Confidentiality: no disclosure of info

8. Security - confidentiality, integrity, and availability.

9. Means:
    * Prevention: preventing the occurence or introduction of faults.
    * Tolerance: avoiding service failures in the presence of faults.
    * Removal: reducing the number and severity of faults.
    * Forecasting: estimating the present number, the future incidence, and the likely consequences of faults.

### Dependability Metrics

1. Reliability and MTTF/MTBF/MTTR:
    * Reliability: the probability that the system is delivering correct service in the time interval.
    * MTTF (Mean Time To Failure): non repairable.
    * MTBF (Mean Time Between Failure): repairable.
    * MTTR (Mean Time To Repair).

2. Availability: the probability that the system is delivering correct service at time t.
    * For a repairable system, A = MTBF/(MTBF + MTTR).
    * 5 nine availability means 99.999%.

3. Safety: the probability that the system is delivering correct service or has failed in the manner that does not cause harm in the interval.
    * Mean Time To Catastrophic Failure.

## Software Engineering Aspects

### General Aspects

1. Defensive programming: requires the preconditions to be checked when a function is called

### Software Testing

1. Unit and regression testing:
    * Regression testing: testing of an entire program to ensure that a modified version of a program still handles all input correctly that an older version did.
    * A bug reported by a customer is primarily a weakness of the regression test suite.

2. Test coverages:
    * Function coverage
    * Statement coverage
    * Branch coverage
    * Predicate coverage (condition coverage): Boolean sub-expression both true and false

3. Mutation testing:
    * Mutation testing involves modifying a program in small ways.
    * It evaluates the effectiveness of a test suite.
    * The source code is modified algorithmically by applying mutation operations in order to produce mutants.
    * A mutant is killed by the test suite if tests fail for the mutant. Mutants that are not killed indicate that the test suite is incomplete.
    * The mutation score is the number of mutants killed normalized by the number of mutants.

4. Fuzzing or fuzz testing feeds invalid, unexpected, or simply random data into computer programs.

5. Fault injection inject faults by:
    * modifying source code (very similar to mutation testing) or
    * injecting faults at runtime (often via modified library calls)
    * It's highly effective to test whether software deals with rare failure situations, e.g. the injection of system calls failures that usually work.

### Software Specification

1. Formal specification: uses a formal (mathematical) notation to provide a precise definition of what a program should do.

2. Formal verification uses logical rules to mathematically prove that a program satisfies a formal specification.

3. Hoare triple: {P}C{Q} (precondition, program, postcondition)

4. Partial correctness and total correctness:
    * Partially correct: the results satisfy the postcondition Q
    * Totally correct: partially correct + always terminates [P]C[Q]

5. Notations:
    * V: variable
    * E: expression
    * S: statement (either true or false)
    * C: command
    * x,y: auxiliary variables

6. Conditionals and while loop:
    * IF S THEN $C_1$ ELSE $C_2$ FI
    * WHILE S DO C OD

### Software Verification

1. $\vdash S$ means S has a proof.

2. $\frac{\vdash S_1, ..., \vdash S_n}{\vdash S}$ means S may be deduced from the above.

3. Precondition strengthening: (it's stronger when the set is smaller)

    $$\frac{\vdash P \rightarrow P', \vdash \\{P'\\} C \\{Q\\} }{\vdash \\{P\\} C \\{Q\\} }$$

4. Postcondition weakening: (it's weaker when the set is bigger)

    $$\frac{\vdash \\{P'\\} C \\{Q'\\}, \vdash Q' \rightarrow Q, }{\vdash \\{P\\} C \\{Q\\} }$$

5. Weakest precondition: the largest set of states for which C terminates.
    * Weakest liberal precondition: (doesn't have to terminate)

6. Strongest postcondition: the smallest set of states.

7. Assignment axiom: $\vdash \\{P[E/V]\\} V := E \\{P\\}$ (replace V with E in P) (backwards)

8. Conditional rule:

    $$\frac{\vdash\\{P \wedge S \\}C_1 \\{Q\\}, \vdash\\{P \wedge \neg S \\}C_2 \\{Q\\}}{\vdash \\{P\\} \text{ IF S THEN }  C_1 ELSE \\ C_2 \\ FI \\{Q\\} }$$

9. While rule: (P is an invariant)

    $$\frac{\vdash\\{P \wedge S \\}C \\{P\\}}{\vdash \\{P\\} \text{ WHILE S DO C OD } \\{ P \wedge \neg S \\} }$$

10. Annotations are required:
    * before each command in a sequence where it's not an assignment
    * after the keyword Do in a WHILE command (loop invariant)

11. Generation of verification conditions (VCs)
    * Assignment ($\\{P\\} V := E \\{Q\\}$): $P \rightarrow Q[E/V]$
    * Conditions: VCs generated by it
    * Sequences: VC generated by it
    * While loops ({P} WHILE S DO {R} C OD {Q}):
      * $P \rightarrow R$ and $R \wedge \neg S \rightarrow Q$
      * VCs generated by $\\{R \wedge S\\} C \\{R\\}$

12. For total correctness, we need to modify the VCs for WHILE:
    * Show that a non-negative integer, a variant, decreases on each iteration.
    * Add $R \wedge S \rightarrow E \geq 0$
    * Add VCs generated by $\vdash[P \wedge S \wedge E = n]C [P \wedge (E < n)]$

13. While for total correctness:

    $$\frac{\vdash[P \wedge S \wedge E = n]C [P \wedge (E < n)], \vdash P \wedge S \rightarrow E \geq 0}{\vdash [P] \text{ WHILE S DO C OD } [ P \wedge \neg S ] }$$

14. Partial correctness and termination implies total correctness. Total correctness implies partial correctness and termination.

## Software Vulnerabilities and Exploits

1. Malware:
    * Virus: replicates by modifying other programs
    * Worm: replicates itself to spread to other computers
    * Trojan horse: misleads user of its true intent
    * Ransomware: blocks access until a ransom is paid
    * Spyware: gathers info

2. Social engineering: the psychological manipulation of people into performing actions or divulging confidential information.
    * Phishing
    * Impersonation
    * USB drop

3. Backdoor: a method of bypassing normal authentication systems to gain access. They might be created by malicious developers, tools such as compilers, or other malware.

4. Rootkit: a collection of software, typically malicious, designed to enable access that's not otherwise allowed and often masks its existence.

5. Advanced persistent threat: a stealthy computer network threat actor which gains unauthorized access and remains undetected for an extended period.

### Control Flow Exploits

1. Registers:
    * ebp (rbp): base pointer
    * esp (rsp): stack pointer
    * eip (rip): instruction pointer

2. Shellcode: a small piece of code used as the payload in the exploitation. (It's called shellcode because it typically starts a command shell.)

3. Stack buffer overflow: a program writes to a memory address on the call stack outside of the intended data structure, which is usually a fixed-length buffer.
    * Stack smashing: inject executable code into the program.

4. Return-oriented programming: allows an attach to execute code in the presence of security defenses. An attacher gains control of the call stack to hijack the control flow and executes carefully chosen machine instruction sequences that are already present in the memory.

5. Format string attack: format strings can be used to crash a program or to execute harmful code.

## Cryptography

### Cryptography Primer

1. Terminology:
    * Cryptology: cryptography (secret writing) + cryptanalysis (break ciphers)
    * Ciper: an algorithm for encryption and decryption
    * Key: some secret info used as a parameter of a cipher and customizes the encryption algorithm.
    * The security rests on the secrecy of the keys not the algorithms.

2. Crptosystem: $D_k(E_k(m)) = m$

3. Cryptographic hash function:
    * Efficient to compute for arbitrary input.
    * Given a hash value h, difficult to find an input m such that h = H(m) (preimage resistance).
    * Given an input m, difficult to find another input $m' \neq m$ such that H(m) = H(m') (2nd-preimage resistance).
    * Difficult to find two different inputs such that H(m) = H(m') (collision resistance).

4. Digital signatures are used to prove the authenticity and integrity of a message.
    * Authentication: verify the identity
    * Non-repudiation: cant deny that it sent the message
    * Integrity: verify it's not tampered with

### Symmetric Encryption Algorithms and Block Ciphers

1. Substitution cipher: easier to attach via frequency analysis
    * Monoaphlaetic: bijection on the set of symbols of an alphabet.
    * Polyalphabetic: multiple bijections, i.e. a collection of monoalphabeic ciphers.

2. Permutation (transposition) cipher: maps a plaintext $m_0, ... ,m_{l-1}$ to $m_{\tau(0)}, ... , m_{\tau(l-1)}$ where $\tau$ is a bijection of the positions in the message.

3. Product cipher: combines two or more ciphers
    * Multiple substitution cipher gives another substitution cipher -> little value.
    * Multiple permutation cipher gives another permutation cipher -> little value.
    * Substitution + permutation -> harder to break.

4. Chosen plaintext attack and chosen ciphertext attack:
    * Choose arbitrary cleartext/ciphertext and feed them into E/D to obtain the corresponding ciphertext/cleartext.

5. Polynomial and negligible functions:
    * Polynomial: $f \in O(p)$ for some polynomial p
    * Super-polynomial: $f \notin O(p)$ for every polynomial p
    * Negligible: $f \in O(1/|p|)$ for every polynomial p
      * A security scheme is secure if the probability of security failure is negligible in terms of the cryptographic key length n.

6. Polynomial time and probabilistic algorithms:
    * Polynomial time: worst-case time complexity is a polynomial function
    * Probabilistic algorithm: may return different results when called multiple times for the same input.
    * Probabilistic polynomial algorithm.

7. One-way function: f can be computed by a polynomial time algorithm, but any polynomial time randomized algorithm F that attempts to compute a pseudo-inverse for f succeeds with negligible probability.
    * They are super-polynomial hard to invert.

8. Security of ciphers:
    * Pick two plaintexts $m_0$ and $m_1$ and randomly receives either $E(m_0)$ or $E(m_1)$.
    * Secure if we can't distinguish between the two situations with a probability that's non-negligibly better than 1/2.

9. Block cipher: a cipher that operates on fixed-length groups of bits called a block.
    * The last block may need to be padded.
    * Electronic codebook (ECB): simply slices the input into a sequence of blocks that are encrypted in isolation.
      * Parallelizable for encryption and decryption; Random access; Lack of diffusion (doesn't hide data pattern)
    * Cipher block chaining (CBC): feeds the ciphertext of the previous block to the subsequent block.
      * Parallelizable for decryption but not for encryption; Random access.
    * Output feedback (OFB): the encryption and decryption work exactly the same.
      * No parallelization nor random access.
    * Counter (CTR): improves OFB (sequential and doesn't support random access).
      * Parallelizable for both encryption and decryption; Random access.

10. Substitution-permutation network: a block cipher whose bijections arise as products of substitution and permutation ciphers.
    * Substitution step (S-box)
    * Permutation step (P-box)
    * Key step (xor)

11. Advanced encryption standard (AES):
    * Characteristics:
      * Overall blocksize: 128 bits
      * Number of parallel S-boxes: 16
      * Bitsize of an S-box: 8
      * 10 rounds with 128 bit keys; 12 with 192; 14 with 256

### Asymmetric Encryption Algorithms

1. Encrypt using the receiver's public key. Decrypt using the receiver's private key.

2. When signing, encrypt using the sender's private key. Decrypt using the sender's public key.

3. Rivest-Shamir-Adleman (RSA):
    * Key gen:
      * Gen two large prime numbers p and q of roughly the same length
      * Compute n = pq and $\phi (n) = (p-1)(q-1)$
      * Choose e satisfying $1 < e < \phi (n)$ and $gcd(e, \phi (n)) = 1$
      * Compute d satisfying $1 < d < \phi (n)$ and $ed \\ mod \\ \phi(n)= 1$
      * Public key: (n,e); private key: (n,d)
    * Encryption: compute $c_i = m_{i}^e \\ mod \\ n$ for all $m_i$
    * Decryption: compute $m_i = c_{i}^d \\ mod \\ n$ for all $c_i$
    * It's computationally intensive and hence used only on small cleartexts.

4. Elliptic curve cryptography (ECC):
    * Elliptic curve: $E = \\{(x,y)|y^2 = x^3 + ax + b\\} \cup \\{\infty \\}$

### Cryptographic Hash Functions

1. Purposes:
    * Integrity verification
    * Authentication
    * Fingerprints for digital signatures
    * Adjustable proof of work mechanisms

2. MD (Message Digest); SHA (Secure Hash Algorithm).

3. Merkle-Damgard construction: the message is padded and postfixed with a length value.

4. Hashed message authentication code (HMAC): a type of message authentication code (MAC) involving a cryptographic hash function and secret cryptographic key.
    * HMAC can be used to verify both data integrity and authenticity.
    * HMAC doesn't encrypt the message.
    * The message must be send with the HMAC hash. The receiver will hash it again with the key and match the hash.
    * Computation: $HMAC_H(k,m) = H((k' \bigoplus opad) || H((k' \bigoplus ipad) || m))$
      * k' is derived by padding k with 0s or hashing k.
      * opad is the outer padding: 0x5c5c5c5c...5c
      * ipad is the inner padding: 0x36363636 ... 36
      * xor and concatenation

5. Authenticated encryption with associated data:
    * It's often necessary to combine encryption with authentication.
    * MAC protects the data against modifications.
    * Encrypt-then-Mac (EtM): $E_k(M) || H_k(E_k(M))$
    * Encrypt-and-Mac (EaM): $E_k(M) || H_k(M)$
    * Mac-then-Encrypt (MtE): $E_k(M || H_k(M))$
    * EtM is preferred since it protects against chosen ciphertext attacks and avoids ay confidentiality issues from the MAC of the cleartext.

### Digital Signatures and Certificates

1. Direct signature:
    * Signer: $S = E_{k^{-1}}(m)$
    * Verifier: $D_k(S) \stackrel{?}{=} m$

2. Indirect signature of a hash: (faster and more common, but it requires the signature to be sent with the document)
    * Signer: $S = E_{k^{-1}}(H(m))$
    * Verifier: $D_k(S) \stackrel{?}{=} H(m)$

3. Public key certificate: an electronic document to prove the ownership of a public key. It contains:
    * Info about the public key.
    * Info about the identity of the owner (subject).
    * The digital signature of an entity that has verified the certificate's content (issuer).

4. Public key infrastructure (PKI): a set of roles, policies, and procedures.
    * A central element is the certificate authority (CA)
    * CAs are hierarchically organized. A root CS may delegate some work to trusted secondary CAs.
    * A key function of a CA is to verify the identity of the subject.

### Key Exchange Schemes

1. Key exchange (key establishment) is any method by which cryptographic keys are exchanged between two parties.
    * Ephemeral key: it's generated for each execution of a key establishment process.
    * Ephemeral keys provide perfect forward secrecy: the ephemeral keys will not be compromised even if long-term keys are compromised in the future.
    * Keys are easier to break the more they are used.

2. Diffie-Hellman Key Exchange:
    * Define a prime number p and a primitive root g with g < p.
    * A randomly picks $x_A \in Z_p$ and computes $y_A = g^{x_a} \\ mod \\ p$ and sends it to B.
    * B randomly picks $x_B \in Z_p$ and computes $y_B = g^{x_B} \\ mod \\ p$ and sends it to A.
    * A computes: $K_{AB} = y_{B}^{x_A} \\ mod \\ p = g^{x_Ax_B} \\ mod \\ P$
    * B computes: $K_{AB} = y_{A}^{x_B} \\ mod \\ p = = g^{x_Ax_B} \\ mod \\ P$
    * g is a primitive root of $Z_p = \\{0,...,p-1\\}$ if $g^1 \\ mod \\ p$, ..., $g^{p-1} \\ mod \\ p$ produces 1, ..., p-1 in any permutation.
    * It suffers from Man in the Middle.

3. Needham-Schroeder Protocol and Kerberos Protocol.
    * The former uses 5 messages
    * The latter uses 4 messages. It uses time stamps to address the flaw of the former.

4. BAN (Burrows-Abadi-Needham) logic: a first attempt to provide formalism for authentication protocol analysis.

## Secure Communication Protocols

## Information Hiding and Privacy

## System Security

## References

* <https://cnds.jacobs-university.de/courses/sads-2020/>
