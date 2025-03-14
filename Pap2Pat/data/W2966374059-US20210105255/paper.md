# Introduction

Besides the recent development of network services and internet of things (IoT) devices, an efficient processing technology for encrypted big data is required. For this purpose, speed and security should be balanced reasonably. One of the well-known methods to achieve this is searchable encryption (SE) where one can search keywords on encrypted messages without obtaining more information than search results. SE has been studied for about 20 years, and it is expected as a possible solution applicable to cloud services [1,2]. In recent years, SE focused on IoT and big data has also been actively studied [3][4][5][6]. SE is provided between users and servers with the special function for encryption. There are several algorithms of SE [7][8][9][10][11], and one can calculate the distance between two ciphered texts without decryption, where the distance is equivalent to that between the original plain texts. However, in each case, the requirement of resource costs, for example, computational power, time, and memory size, is expensive.

In this paper, we propose a class of cryptosystems where the distance is calculated on the space of ciphered texts, the so-called verifiable encryption (VE). We also construct a fast and secure authentication algorithm based on the VE. The known authentication algorithms, viz., Fiat-Shamir [12] and Schnorr [13], are shown to be included in the class of VE. Moreover, we provide the implementation of one of the new authentication algorithms and demonstrate its performance.

# Digital Identity and Authentication

Digital information generated from personal information is the so-called digital identity, and it is utilized everywhere; e.g., internet shopping, unlocking smartphones, and transactions. The following materials are used as digital identity:

• appearance information (fingerprints, facial features, biometrics, etc.) • certificates published by credential service providers

• cards issued by trusted institutions (credit cards, debit cards, driving license cards, etc.) • personal memories (passwords and secret questions)

In many cases, the digital identity is stored on network storage, for example, a cloud server that belongs to the service provider. However, users do not pay much attention to security and its management, including advertisement use, because they ordinarily trust the service provider.

Security problems are based on two different factors. One is the identification method and the other is the cryptosystem. The identification problem arises from the registration step in authentication. For instance, the service provider queries the issuer as to whether the customer's information is authentic the first time. Queries are sent via an insecure network; then the authentication requires a cryptographic system, which is a public key agreement (PKA), and the trusted third party for the preparation of parameters such as a certificate authority (CA). The digital identity must be encrypted before sending, and the secret keys must also be distributed. Therefore, the process generates some time cost and consumes computational resources. In many cases, identification algorithms based on interactive proof are applied to authentication. One of the well-known authentication algorithms is based on zero-knowledge proof [12,13]. Because digital identity is static material for each service, it is not necessary to be queried every time. To unlock our smartphones, we just put our finger or look at the display for face recognition. Digital identity is stored in the local storage, and never sent via the network. There are two usages of digital identity:

These usage methods are used independently. However, the usage of digital identity has spread widely to several markets, for example, in entering a workplace, concert hall, checkout-free shopping store, and private residence without receptions. In these cases, the following third usage of digital identity is required:

• unlock local devices via network However, local devices do not always have enough computational power, and customers do not want to wait for several minutes to unlock them.

# Verifiable Encryption

The comparison of two encrypted data items is generally performed after decryption. Homomorphic encryption, for example, RSA encryption [7], Elgamal encryption [8], and fully homomorphic encryption [9], can perform numerical addition or multiplication in the space of cipher texts. A cryptosystem that is operable without decryption is expected to contribute to the ubiquitous network society. According to this viewpoint, we propose a class of cryptosystems, VE, which calculates the distance between two plain texts without decryption. In this section, we describe mathematical notations and VE. Moreover, we show cryptosystems belonging to VE.

## Cryptosystem and Authentication Algorithm

Let Z n be a finite field for a positive integer n. Let P, C, K be spaces of plain texts, cipher texts, and keys, respectively. A set E of encryptions and a set D of decryptions are given by

Usual well-known methods for authentication via the network are described by the interactive proof, which is a mathematical model of a common procedure for the exchange of messages between Alice and Bob. The process has two parts, viz., registration and verification:

Registration Distribute the secret information by public key distribution Verification Verify the information using an authentication algorithm.

Well-known algorithms are proposed by Fiat-Shamir [12] and Schnorr [13]. Both algorithms are based on zero-knowledge proof [14][15][16], and repeated until Bob becomes convinced. Let P be a space of plain texts, f , g : P → P be functions, and w ∈ P be an initial message. For a positive integer m, let ( f ↔ g) ≡ a 1 a 2 • • • a m be message streams holding

. . .

and the output after m rounds Out f ( f ↔ g)(w) ≡ f (w, a 1 , . . . , a m ) ∈ {0, 1}.

## Verifiable Encryption

Let V : P × P → R + (= [0, +∞)) be a metric between two texts. It should be noted that F(E k (p 1 ), E k (p 2 )) is encrypted; hence, one can send it via the network. This property helps to increase security against plain-text attack.

# Cryptosystems Belonging to the VE Class

Here, we review authentication algorithms of Fiat-Shamir [12] and Schnorr [13], and derive the inclusion relation between several cryptosystems and the VE class.

## Fiat-Shamir's Algorithm

Let p and q be two prime numbers such that p = q. The number n = pq is opened to the public. Let k A (= k B ) be secret information distributed at the registration step. In Fiat-Shamir's algorithm, Alice and Bob repeat the following steps for m rounds to prove Alice's authenticity:

Step 1 Alice chooses random number r ∈ Z n arbitrarily. Then Alice calculates x := r 2 mod n, and sends x to Bob.

Step 2 Bob chooses a random number e ∈ {0, 1} arbitrarily, and sends e to Alice.

Step 3 Alice calculates y := rk e A mod n by using secret information k A of Alice, and sends y to Bob.

Step 4 Bob calculates and checks that y 2 mod n is equal to xk 2e B mod n.

At step 4, if k A , possessed by Alice, is the same as the secret information k B possessed by Bob, the following equation holds:

Otherwise, the equation does not hold, and Bob rejects Alice. This algorithm contains two cryptosystems C 1 = (P, C, K, E , D) by Alice, C 2 = (P, C, K , E , D ) by Bob where

We show that the set (E , E ) can be called a VE under some conditions.

Theorem 1. If x = r 2 mod n and e = 1, then Fiat-Shamir's cryptosystem belongs to the class of VE.

Proof. Let p 1 , p 2 ∈ P be two plain texts, (r, x) ∈ K × K be two keys, and

Here we give a metric V as

We construct a map F : C × C → C and D : C → R + by the following:

Suppose x = r 2 mod n; then we have

In this case, for all p 1 and p

In the case of e = 1, D r,x (F(E r (p 1 ), E x (p 2 ))) = V(p 1 , p 2 ) for all p 1 , p 2 ∈ P with the keys (r, x) = (r, r 2 ) mod n. Therefore, Fiat-Shamir's algorithm belongs to the class of VE in the case of x = r 2 mod n and e = 1.

## Schnorr's Algorithm

Let p and q be two prime numbers holding q|(p -1) and g ∈ Z p with order q such that g = 1, where x|y means x is divisible by y. The numbers p, q, g are public information. Let k A (= k B ) be secret information that is registered. In Schnorr's algorithm, Alice proves to Bob that Alice owns k A , which is equal to k B possessed by Bob. Repeat the following steps for m rounds until Bob is satisfied:

Step 1 Alice chooses random number r ∈ Z q arbitrarily. Then Alice calculates a := g r mod p, and sends a to Bob.

Step 2 Bob chooses random number e ∈ {0, 1} arbitrarily, and sends e to Alice.

Step 3 Alice calculates x := rek A mod q by using secret information k A of Alice, and sends x to Bob.

Step 4 Bob confirms that a = g r mod p is equal to g x g ek B mod p.

If Alice has k A that is equal to Bob's k B at step 4, the following equation is obtained:

Let n = pq. Schnorr's algorithm has two cryptosystems C 1 = (P, C, K, E , D) by Alice and Proof. Let p 1 , p 2 ∈ P be two plain texts, (r, x) ∈ K × K be two keys, and

We compose a map F : C × C → C and D : C → R + as follows:

In the case of e = 0, for all p 1 and p 2 , D r,x (F(c 1 ,

In this case, D r,x (F(E r,x (p 1 ), E x (p 2 ))) is equal to the metric V(p 1 , p 2 ) for all p 1 , p 2 ∈ P. Therefore, if the key x = rep 1 mod n and e = 1, then Schnorr's cryptosystem belongs to the class of VE.

## One-Time Pad

A one-time pad cryptosystem C is given as follows:

Note that P(k) is the appearance probability of 0 and 1 in all k and ⊕ means a bitwise exclusive or.

Theorem 3. The one-time pad cryptosystem belongs to the class of VE.

Proof. Let C 1 = (P, C, K, E , D) and C 2 = (P, C, K , E , D ) be two cryptosystems of the one-time pad. Let p 1 , p 2 ∈ P be two plain texts, (k, k ) ∈ K × K be two keys, and

Here we give a metric V as

and construct a map F : C × C → C and D : C → R + as follows:

For all p 1 , p 2 ∈ P, there exist keys k, k , and the metric V(p 1 , p 2 ) can be achieved. Therefore, the one-time pad cryptosystem belongs to the class of VE.

It should be noted that it can be configured by only one cryptosystem C in the case of a one-time pad.

## Block Cipher Modes of Operation

A block cipher is the algorithm that encrypts a plain text of fixed length such as 64 bits or 128 bits, called a block, and returns ciphered text of the same length as that of the plain text. A commonly used block cipher is the advanced encryption standard (AES) [17]. The block cipher consists of encryption function E k and decryption function D k for each block.

If a plain text is longer than the block length, the following modes, called block cipher modes of operation, are applied: Electric code block (ECB) mode divides a plain text into blocks and encrypts in each block. Cipher block chaining (CBC) mode calculates the plain text block and preceding ciphered text block by XOR, and then the output is encrypted. Output feedback (OFB) mode is used as a pseudorandom number generator that outputs an encrypted initial vector. Cipher feedback (CFB) mode calculates the XOR of the modified preceding ciphered text blocks and plain text block.

Remark 1. The ECB, CBC, and CFB modes do not obviously belong to the class of VE.

Let C 1 = (P, C, K, E , D) and C 2 = (P, C, K , E , D ) be two cryptosystems of the block cipher. Let p 1 , p 2 ∈ P be two plain texts, (k, k ) ∈ K × K be two keys, and

where E k and E k are the same encryption function. Because of the property of the block cipher, any operation except identity mapping is not allowed to be applied to the cipher text in order to decrypt correctly. By the definition of VE, the operation F : C × C → C is not an identity mapping. The block cipher does not allow any calculations in cipher text space. Intuitively, there is no F and D to achieve the VE conditions.

On the other hand, the OFB mode performs encryption as follows:

1. generate a key stream z of length n (a)

where IV ∈ P is an initialization vector. As can be seen from the above, only the IV is processed with a block cipher; namely, the OFB mode appears to be a pseudo random number generator.

# Remark 2.

As the OFB mode is applicable to the one-time pad, it belongs to the class of VE.

# New Authentication Algorithm Based on VE

Authentication refers to an act by which Bob gives permission to Alice. With the development of information technology, authentication scenes have spread widely. Table 1 lists the three types of authentication explained in Section 2 that we focus on. Authentication via network is the strictest scheme that protects valuable information, for example, electronic transactions. The second one is for unlocking small devices such as smartphones. Because of usability and frequency of use, the scheme is simple and directed for users. The third one, unlocking local devices via the network, is related to the management of groupware where the user management is operated through the network. Therefore, it should be secure against eavesdroppers and directed for users. The requirements of these schemes, which are a situation of the network, computational power, and convenience of registration and verification, are different from each other. In this section, we discuss these requirements precisely.

Meanwhile, a digital identity in authentication is denoted by information, which is sent to Bob during registration, and presented by Alice during verification. Because personal information is frequently used as the digital identity, it must be protected with suitable secure technologies and embedded into these authentications. In this section, we also present a new secure and no-key-distribution scheme for the third authentication type to unlock local devices via the network.

## Authentication via Network

The first authentication type is observed in the information transmission where the information must be protected, for example, electronic commerce, transaction, and private information transmission. Figure 1 shows the example of an implicit network structure where the user device and server are connected via the Internet. Cipher communication such as SSL/TLS is the most general scheme in authentication via the network. First, however, Bob confirms the genuineness of Alice's digital identity before authentication because of the possibility of impersonation. This process can provide interactive proof, for example, Fiat-Shamir's algorithm and Schnorr's algorithm described in Section 4, for which an expensive CA is required to prepare public parameters; then, an additional cost for security is expected. 

## Unlocking Local Devices

As unlocking local devices is one of the closest authentications, which is approved more than approximately 50 times per day, the faster the speed of the unlocking process, the better (see Figure 2). To increase usability and reduce computational time, authentication just compares the registered digital identity and preserves one for verification without any encryption. Implicitly, the connection between the user and device is supposed to be safe. We usually post our passphrases, pin codes, fingerprints, or biometrics to the device as it is. Because the digital identity is used only to unlock the device, it is not sent somewhere via the network. Authentication via the network pays a significant cost to protect itself, and unlocking the device does not take account of protection. The risk of leaking is obviously higher if the user loses the device or it is hacked. Essentially, it does not follow the general security policy to handle the digital identity. 

## Unlocking Local Devices via the Network

With the spread of IoT devices, a ubiquitous network society has steadily been realized [18]. Groupware, smart doors, and smart devices associated with multiple users surround us. In order to achieve the network service accompanying the devices, they require activation managed through the network (see Figure 3). Here, we call this activation unlocking local devices via the network. The devices that are distributed by service providers sometimes belong to them, not the users. In Figure 3, it is shown that unlocking local devices via the network is required in an environment where multiple users login on one device. The verification process of unlocking local devices via the network is performed as follows:

The user sends the digital identity to a device. The device queries a server via the network, and the server sends back responses. The device compares the digital identity with the one from the server.

In step 2, the query associated with the accompanying service indicates the user account. The device can be accessed by anyone because it is not occupied by the individual. As the device is also freely accessible to an eavesdropper, the digital identity must be encrypted and separated from the key.

In Section 3.2, the main purpose of unlocking local devices is to protect the data rather than digital identity. Because the lock prevents the leaking of information in the case of theft, users pay additional cost for it as insurance. The faster process of unlocking the local device via the network is a favorable circumstance for the service provider. The deterioration of usability leads to deterioration in the quality of service. Needless to say, neither the user nor the service provider desires the illegal use of the service. Therefore, the algorithm that maintains security with low cost is ideal for both the user and service provider.

## Algorithm

Here, we construct an authentication algorithm based on the VE for unlocking local devices via the network. Let Alice be the person to be authenticated, and Bob be the authenticator. Taking implementation into account, we assume that the server is an untrusted party. Let C 1 = (P, C, K, E , D) and C 2 = (P, C, K , E , D ) be two cryptosystems where the set (E , E ) is a verifiable encryption. Let p 1 , p 2 ∈ P be two plain texts, (k, k ) ∈ K × K be two keys, and

# Registration step

Step 1 Alice sends p 1 to Bob.

Step 2 Bob generates k, and calculates E k (p 1 ) = c 1 .

Step 3 Bob sends c 1 to the server S.

# Verification step

Step 1 Alice sends p 2 to Bob.

Step 2 Bob generates k , and calculates E k (p 2 ) = c 2 .

Step 3 Bob sends c 2 to S.

Step 4 Server calculates F(c 1 , c 2 ) = c d , and sends c d to Bob.

Step 5 Bob calculates D k,k (F(c 1 , c 2 )), and checks the result. This algorithm does not require key distribution nor any exchange of messages as in interactive proofs. This algorithm is deterministic, while the authentication using interactive proof is probabilistic.

# Implementation

In this section, we show an implementation using the one-time pad. Let S be a computation server, A a user, and D the user's device. We assume that D is the trusted device to generate and manage keys, and the server S belongs to an untrusted party.

Without loss of generality, 128-bit plain texts and random numbers are employed. Although many pseudo random number generators are available, here we use the QP-DYN, which is based on non-commutative algebra and ergodic theory, because of its performance [19,20].

## Algorithm

Here, we show the implementation using the one-time pad. Let C 1 = (P, C, K, E , D) be a cryptosystem of the one-time pad, where a set (E , E ) is a verifiable encryption. Let p 1 , p 2 ∈ P be two plain texts, (k, k ) ∈ K × K be two keys, and

be two cipher texts.

# Registration Step

Step 1 A sends A's digital identity p 1 to D.

Step 2 D generates the key k that is kept only with D. Then D computes E k (p 1 ) = c 1 , and sends the encoded digital identity c 1 to S.

Step 3 S stores c 1 in the database.

# Verification Step

Step 1 A sends A's digital identity p 2 to D.

Step 2 D generates the one-time key k for the only one-time authentication. This one-time key k is never used after finishing this authentication flow.

and sends the encoded digital identity c 2 to S.

Step 3 S computes the encoded distance c d between c 1 and c 2 using map F such that

and sends encoded distance c d to D.

Step 4 D decodes the encoded distance c d with the keys k and k to obtain the distance V(p 1 , p 2 ), where

If p 1 ⊕ p 2 ≤ s, then D returns "OK" to A, otherwise "NG", where s is the threshold arbitrary chosen by the verifier. Even the case that the plain texts are flexible elements, for example, biometrics the verification step with s can recognize them.

## Environment

The experimental environment is shown in Table 2. 

## Registered Information

The registered information is shown in Table 3. Let p 1 be plain text to be registered, k 1 be a secret key for encryption p 1 , and c 1 be a cipher text that encrypts p 1 . 

## Experiment

In what follows, we show the implementation that is changing the content of the plain text. Suppose the encrypted result c d i equals c 1 ⊕ c i+1 , where i = 1, 2, 3, 4. Moreover, we present necessary times for encryption, verification, and decryption, in the case of the plain text length being changed to 128 bits, 256 bits, 512 bits, 1024 bits, 2048 bits, 4096 bits and 8192 bits, respectively.

# Authentication of same sequences 1 (p 1 = p 2 )

We show the first experiment in Table 4; the information to be authenticated is equal to the registered information. Let p 2 be plain text to be authenticated, k be a key that is a one-time key for encryption p 2 , and c 2 be a cipher text that encrypts p 2 . Authentication of same sequences 2 (p 1 = p 3 )

We show the second experiment, where the information to be authenticated equals the registered information, in Table 5. This is almost the same as the first experiment; however, the encrypted information c 3 is different from the first experiment. Authentication of different sequences (p 1 = p 4 )

We show the third experiment, where the information to be authenticated is different from the information registered, in Table 6. These are different sequences, and the hamming distance is 60. 

# Authentication of different sequences (p eve = 0)

We show the fourth experiment, where the information to be authenticated is all 0, in Table 7. These are different sequences and the Hamming distance is 59. In this experiment, a chosen plain-text attack is assumed. As can be seen from the above experimental results, this cipher text c eve is equal to k 5 ; that is, there is a possibility that the key k 5 is known to an eavesdropper.

The key generation time is calculated from the environment and the cryptosystem used, hence it is not discussed. Table 8 and Figures 456shows the speed in each process for different plain text lengths. In this algorithm, the speed of each process has almost the same speed even if the plain text length is changed. 

# Length of Plaintext

Encryption Verification Decryption 128 bits 0.00095367 ms 0.00095367 ms 0.00095367 ms 256 bits 0.00095367 ms 0.00095367 ms 0.00095367 ms 512 bits 0.00095367 ms 0.00095367 ms 0.00095367 ms 1024 bits 0.00095367 ms 0.00095367 ms 0.00095367 ms 2048 bits 0.00095367 ms 0.00095367 ms 0.00095367 ms 4096 bits 0.00095367 ms 0.00095367 ms 0.00095367 ms 8192 bits 0.00095367 ms 0.00095367 ms 0.00095367 ms   

# Attacks

In this section, the impersonation attack and plain text attack are discussed. As in Section 6, let S be the server, A an user, and D the device. We assume that A trusts D, and S belongs to untrusted party. Let Eve be an eavesdropper.

## Impersonation (Man-in-the-Middle-Attack)

Let p 1 be a plain text to be registered, k 1 a key for encryption p 1 , and c 1 = E k 1 (p 1 ) a cipher text. Here, we assume that the Eve's goal is to obtain the plain text p 1 . The crucial impersonation is to obtain p 1 directly. In this case Eve must have full access permission for the device. If Eve has the backdoor of a device which can send the key k 1 to her, she can combine k 1 to the cipher text c 1 , then recover p 1 . Therefore, full access control and backdoor are crucial attacks. The authentication is totally weak against the impersonation. The user realizes these kinds of attacks only after the authentication which does not activate any services. The service provider must pay attention to protect the device and install some security software to detect unauthorized applications and backdoor.

## Plain Text Attack

The Table 7 in Section 6.4 shows the result of the plain text attack where p eve = 0. Although the cipher text c 5 is equal to the key k 5 , that is k 5 is stolen, the algorithm is kept secure because k 5 is a one-time key. If attacker holds k 5 , she can computes

According to the above equation, the plain text p 1 and the key k 1 can not be recover from c d 4 . The decryption result d 4 equals to the registered plaintext p 1 as follows:

However, only the device D knows the result d 4 insides, does not send to anywhere. Therefore, the plain text attack for the verification step is not valid.

# Conclusions

In this paper, we defined verifiable encryption, which is given by a class of the cryptosystem. The class VE = (E , E ) consists of two cryptosystems and two cipher text spaces. It was shown that the special cases of Fiat-Shamir and Schnorr, and one-time pad (also any stream ciphers) are included in the class of VE. We also constructed an authentication algorithm, which incorporates speed and security without PKA and SSL/TLS, with the one-time pad. The velocity of the protocol is less than 0.003 ms with 8192 bits of key length. If digital identity, such as biometric information, is employed, it is necessary to set a threshold value and calculate the Hamming distance. Moreover, we discussed on the possible attack such as impersonation and plain text attack. The authentication is essentially weak against the impersonation as well as unlocking smartphone, however, strong against plain text attack.

# Acknowledgments:

We would like to thank Editage for English language editing.

# Conflicts of Interest:

The authors declare no conflict of interest.

