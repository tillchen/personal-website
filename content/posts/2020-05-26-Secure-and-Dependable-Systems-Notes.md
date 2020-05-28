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
  * [Pretty Good Privacy](#pretty-good-privacy)
  * [Transport Layer Security](#transport-layer-security)
  * [Secure Shell](#secure-shell)
* [Information Hiding and Privacy](#information-hiding-and-privacy)
  * [Steganography and Watermarks](#steganography-and-watermarks)
  * [Covert Channels](#covert-channels)
  * [Anonymization Terminology](#anonymization-terminology)
  * [Mixes and Onion Routing](#mixes-and-onion-routing)
* [System Security](#system-security)
  * [Trusted Computing](#trusted-computing)
  * [Authentication](#authentication)
  * [Authorization](#authorization)
  * [Auditing](#auditing)
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

### Pretty Good Privacy

1. PGP key management:
    * Key rings: one key ring for public keys and one key ring for private keys.
    * Keys are identified by fingerprints.
    * Key gen utilizes various sources of random info (/dev/random) and symmetric encryption algorithms to generate good key material.
    * Web of trust: avoid centralized certification authorities.

### Transport Layer Security

1. TLS, formerly known as SSL (Secure Socket Layer) was made to enable e-commerce.

2. TPS uses X.509 certificates to authenticate servers and clients.

3. TLS protocols:
    * Handshake Protocol: authenticates, negotiates cryptographic modes and parameters, and establishes shared keying material.
    * Alert Protocol: communicates alters like closure alters and error alters.
    * Record Protocol: uses the parameters established by the handshake protocol to protect traffic between the communicating peers.
       * Record Protocol is the lowest internal layer and carries the handshake + alert protocol messages + app data.

4. Record Protocol:
    * Fragments the data
    * Optionally compresses the data
    * Adds MAC
    * Encrypts and transmits the result

5. Handshake Protocol:
    * Agree on algorithms, exchange random numbers, and check for session resumption
    * Agree on a premaster secret (parameters)
    * Exchange certificates and cryptographic info to authenticate
    * Gen a master secret
    * Provide security parameters to the record layer
    * Allow client and server to verify that the peer has calculated the same security parameters and the handshake completed without being tampered.
    * A full TLS 1.2 handshake requires two round-trips.
    * Full handshakes are expensive. A seesion resumption requires only one round-trip.
    * TLS 1.3 supports a 0-rtt (zero round-trip) mode.

6. Change Cipher Spec Protocol: signals transitions in ciphering strategies. (No longer in TLS 1.3)

7. Alert Protocol: signals exceptions (warnings, errors) occured during the processing of TLS protocol messages.
    * Used to properly close a TLS connection by exchanging close_notify alert messages.
    * In order to scale servers, it's the best if the clients init the TCP connection teardown and end up in TIME_WAIT.

### Secure Shell

1. SSH provides a secure connection through which user authentication and several inner protocols can be run.

2. SSH Protocol layers:
    * Transport Layer Protocol: server authentication, confidentiality, and integrity with perfect forward secrecy.
    * User Authentication Protocol: can auth by user's keys and also other means like passwords.
    * Connection Protocol: multiplexes the encrypted data stream into several logical channels.

3. SSH keys, passwords, and passphrases:
    * Host key:
      * Every server must have a public/private host key pair
      * Used for server authentication
      * Identified by their fingerprint
    * User key:
      * Every user must have their own public/private key pairs, optionally used to authenticate users.
    * User password: remote accounts may use passwords to authenticate users.
    * Passphrase: user's private key may be protected by a passphrase.
    * In reality, users often blindly accept the host key offered at the first connection time.

4. TCP forwarding: tunnel unencrypted traffic through an encrypted SSH connection,
    * `ssh -f joe@example.com -L 2000:example.com:25 -N` creates a tunnel connecting a client to port 25 on `example.com` and provides a listening endpoint on Joe's localhost on port 2000. If a program connects to the lcoal port 2000, it's talking to `example.com` using port 25.

5. X11 forwarding: a special application of TCP forwarding allowing X11 clients on remote machines to access the local X11 server (managing the display and the keyboard/mouse) (run graphical apps remotely)

6. Connection sharing: new SSH connections hook as new channel into an existing SSH connection, reducing session startup times.

7. IP tunneling: tunnel IP packets over an SSH connection by inserting tunnel interfaces into kernels and by configuring IP forwarding.
    * Essentially a simple VPN over which you can securely send arbitrary IP traffic.

8. SSH agent: maintains client credentials during a login session so that credentials can be reused by different SSH invocations without user interaction.

9. SSh agent forwarding: an SSH server emulates an SSh agent and forwards requests to the SSH agent, creating a chain of SSH agent delegations.
    * It enables users to access a remote system and from there to access further systems, always accessing the local SSH agent. SSH connections can go over multiple hops in a very convenient way without having to store any user keys on intermediate systems.

10. OpenSSh privilege separation: two processes (special and normal privileges).

## Information Hiding and Privacy

### Steganography and Watermarks

1. Information hiding: conceal the very existence of some kind of info for some specific purpose. Often used together with encryption.

2. Steganography: the embedding of some info (hidden-text) within digital media (cover-text) so that the resulting digital media (stego-text) looks unchanged (imperceptible) to a human/machine.
    * Unused or redundant bits.
    * Robust steganographic methods may survive some typical modifications of stego-texts (cropping/recoding or images).
    * Media types of large size usually make it easier to hide info.

3. Watermarking: the hidden info itself is not important; the watermark says something about the cover-text.
    * Steganography: the cover-text is not important; the hidden text is the valuable info and is independent of cover-text.
    * Digital watermarks are widely used for copyright protection and source tracking purposes.

4. Classification of steganographic algorithms:
    * Fragile vs robust (survive modifications)
    * Blind vs semi-blind (blind needs the original cover-text for detection/extraction)
    * Pure vs symmetric (secret key) vs asymmetric (public key) (pure needs no key) (asymmetric needs a secret key for embedding and a public key for extraction)

5. LSB-based image steganography: changes the least-significant bits are difficult for humans to see (three 8-bit color values)
    * Use a key to select some LSBs to embed info
    * Encode the info multiple times to achieve robustness against noise.
    * Problems:
      * Existence of hidden info may be revealed if the statistical properties of LSBs change.
      * Fragile against noise such as compression, resizing, cropping, rotating, or additive white Gaussian noise.

6. DCT-based image steganography: image formats like JPEG use discrete cosine transforms (DCT) to encode image data. The manipulation happens in the frequency domain instead of the spatial domain, which reduces visual attacks.
    * Replace the LSBs of some of the DCT coefficients.
    * Use a key to select some DCT coefficients.
    * Problem:
      * Existence of hidden info may be revealed if the statistical properties of the DCT coefficients are changed.
      * The risk may be reduced by using a pseudo-random number generator to select coefficients.

### Covert Channels

1. Covert channels represent unforeseen communication methods that break security policies. They hid the fact that communication takes place.

2. Cover channels embed info in:
    * header fields of protocol data units (protocol messages)
    * the timing of protocol data units (inter-arrival messages)

3. Cover channel patterns:
    * Size modulation: size of a head field or protocol message
    * Sequence: the sequence of head fields
    * Add redundancy: new space in a header field or a message
    * PDU (protocol data unit) corruption/loss: generates corrupted or unitizes packet loss
    * Random value: in a head field containing a random value
    * Value modulation: selects one of the values in a header
    * Reserved/unused: into a reserved or unused header field
    * Inter-arrival time: alters timing intervals
    * Rate: alters the data rate
    * Protocol message order: synthetic protocol message order
    * Re-transmission: retransmits sent or received messages.

### Anonymization Terminology

1. Anonymity: the attacker can't sufficiently identify the subject within a set of subjects, the anonymity set.
    * Larger anonymity set & more evenly distributed -> stronger anonymity
    * Robustness: hwo stable the quantity of anonymity is against changes

2. Unlinkability
    * Unlinkability of items of interest (IOIs), the attacker can't sufficiently distinguish whether these IOIs are related.
    * Sender anonymity means each message is unlinkable.

3. Undetectability: can't sufficiently distinguish whether an IOI exists

4. Unobservability:
    * undetectability of the IOI against all subjects uninvolved.
    * anonymity of the subjects involved in the IOI against others involved.
    * Sender unobservability: undetectable whether any sender within the unobservability set sends.
    * Relaitonship unobservability: undetectable whether anything is sent out of a set of could-be senders to a set of could-be recipients.

5. Relationships:
    * unobservability $\Rightarrow$ anonymity
    * sender/recipient anonymity/unobservability $\Rightarrow$ send/recipient anonymity/unobservability

6. Pseudonymity: the use of pseudonyms as identifiers.
    * Pseudonym: the identifier other than the real names.
    * A public key certificate bides the public key to another pseudonym. In case the pseudonym is the real name, it's called an identity certificate.

7. Identifiability: sufficiently identify the subject within the identifiability set.
    * Identity: any subset of attribute values which sufficiently identifies the person.

8. Identity management: managing various partial identities.
    * A partial identity is a subset of attribute values of a complete identity (the union of all attribute values of all identities)
    * A pseudonym might be an identifier for a partial identity.
    * It's privacy-enhancing if it sufficiently preserves unlinkability

### Mixes and Onion Routing

1. Mix networks:
    * It uses special proxies called mixes.
    * The mixes can filter, collect, recode, and reorder messages to hid conversations.
      * Removal or duplicate messages
      * Collection to create an ideally large anonymity set.
      * Recoding so that incoming and outgoing messages can't be linked.
      * Reordering so that the order can't be used to link.
      * Padding so that the message sizes do not reveal info to link.

2. Onion routing:
    * A message is sent via an overlay network of intermediate routers called a circuit.
    * A message is cryptographically wrapped multiple itmes that every router unwraps one layer and learns to which router the message needs to be forwarded
    * No node in the circuit can tell whether the node before is the originator or another intermediary.
    * Only the final node (exit node) can determine its own location in the chain.
    * Choose routes difficult to observe. Can provide real-time services.

3. Tor:
    * Every Tor router has a long-term identity key (sign TLS cert) and a short-term onion key (decrypt to set up circuits and ephemeral keys).
    * Crucial to use end-to-end encryption to protect against compromised exit node.

## System Security

1. Lamspson model: subject authenticates guard, which auhtorizes object. Audit trail audits guard.

2. Isolation: design and deployment.

### Trusted Computing

1. Trusted computing base: the set of hard and software components crititcial to achvieve the systems' security properties.
    * When other parts are attacked, the device will not misbehave.
    * Small to veify the correctness.
    * Tamper-resistant.
    * Spcial hardware components.

2. It's hard to design software that can be trusted.

3. Goals:
    * Isolation: separate essentail from general
    * Attestation: prove a component is in a certain state
    * Sealing: wrapping of code and data that can only be unwrapped under certain cicumstances.
    * Code confidentiality: sensitive code and static data can't be obtained by untrusted.
    * Side-channel resistence: unstructed can't deduce info about the internal state of a trusted component.
    * Memory protection: protects the integrity and authenticity of data.

4. The first candidates of functions to place into trusted hardware are cryptographic algorithms and key gen and storage.

5. Attestation may be local or remote.

6. Trusted platform module (TPM) is a dedicated microcontroller to secure hardware through integrated cryptographic operations and key storage.
    * RNG, Endorsement Key (EK), Attestation Identity Keys (AIKs)

7. Trusted and rich execution envrionment:
    * Trusted Execution Environment (TEE): secure
    * Rich Execution Environment (REE): non-secure
    * REE resources are acccessbile from TEE

8. TrustZone Cortex-A/M (ARM)
    * The non-secure (NS) bit conveys whether the processor works in secure or normal mode.
    * To perform a context switch, it transits through a monitor mode, which saves and restores the state.
    * Cotex-M repalces the monitor mode with a faster mechanism to call secure code via multiple secure fucntion entry points.
    * Cortex-A is for resoruce rich systems like mobile phones.
    * Cortex-M is for limited systems like embedded.

9. Secuity Guard Extension (SGE, Intel)
    * Protected parts - enclaves.
    * Non-enclave code can't access enclave code.
    * The content of enclaves is loaded when the enclaves are created.
    * Creation and deletion of enclaves is using the highest privilege. Entering and leaving is using the lowest privilege.

### Authentication

1. Authentication: verify that it ahs a certain attribute value.
    * Identificaiotn step: presenting the claimed attribute value (user identifier)
    * Verification step: presetning or genreating authentication info (e.g. a value singed with a private key) as evidence to prove the binding.

2. Authentication factors:
    * Knowledge: passowrd, PIN
    * Possesion: mobile phone, token
    * Static biometrics: fingerpirnt
    * Dynamic biometrics: voice, signature, typing rhythm

3. Password authentication: store H(s||p), s is the salt that ensures that the same passwords don't havve the same hash value.

4. Challegne-response authentication: require correct auth info to be provided in reposnse to a challenge.
    * Password is a special case.

5. One-time password authentication:
    * The user computes $q = H^n(s||p)$
    * The server verifies: $H(q) = H(H^n(s||p)) = H^{n+1}(s||p)$ and checks if it matches. If it does, set k = 1 and decrement n. If n becomes 0, a new ini must be performed.

### Authorization

1. Lampson's access control matrix: greate in theory by difficult in practice (huge.)
    * Subjects are row headings and objects are column headings. (S X O)

2. Access control list: the column of the matrix. Given an object, have a list of subjects X rights.
    * Example: Unix inode.

3. Capabliteies: the row of the matrix. Given a subject, have a list of obejcts X rights.
    * Example: Unix file descriptor.

4. ACL and capabilties are theorically equivalent.

5. Discreiotnary vs mandatory access control:
    * Deiscriotnary: subjects can define (Unix filesystem permissions)
    * Mandatory: system controls

### Auditing

1. Auditing: keep a log (audit trail) of decisions. For debugging and forensics.

## References

* <https://cnds.jacobs-university.de/courses/sads-2020/>
