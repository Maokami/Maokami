## OpenFHE

### Code Example
~~~ C++
    auto ciphertextAdd12     = cryptoContext->EvalAdd(ciphertext1, ciphertext2);
    auto ciphertextMul12     = cryptoContext->EvalMult(ciphertext1, ciphertext2);
    auto ciphertextRot1 = cryptoContext->EvalRotate(ciphertext1, 1);
    auto ciphertextAfter = cryptoContext->EvalBootstrap(ciph);
~~~

### Related Classes

### Details

---

## SEAL

### Code Example
~~~ C++
    Ciphertext x_sq_plus_one;
    Plaintext plain_one("1");
    evaluator.add_plain_inplace(x_sq_plus_one, plain_one);

    Ciphertext x_squared;
    evaluator.square(x_encrypted, x_squared);
    evaluator.relinearize_inplace(x_squared, relin_keys);

	evaluator.rotate_rows_inplace(encrypted_matrix, 3, galois_keys);
    Plaintext plain_result;
~~~

### Related Classes
[seal::Evaluator](https://maokami.github.io/SEAL/classseal_1_1_evaluator.html)

### Details
#### Evaluator
Provides operations on ciphertexts. Due to the properties of the encryption scheme, the arithmetic operations pass through the encryption layer to the underlying plaintext, changing it according to the type of the operation. Since the plaintext elements are fundamentally polynomials in the polynomial quotient ring Z_T\[x\]/(X^N+1), where T is the plaintext modulus and X^N+1 is the polynomial modulus, this is the ring where the arithmetic operations will take place. BatchEncoder (batching) provider an alternative possibly more convenient view of the plaintext elements as 2-by-(N/2) matrices of integers modulo the plaintext modulus. In the batching view the arithmetic operations act on the matrices element-wise. Some of the operations only apply in the batching view, such as matrix row and column rotations. Other operations such as relinearization have no semantic meaning but are necessary for performance reasons.

Arithmetic Operations
    The core operations are arithmetic operations, in particular multiplication and addition of ciphertexts. In addition to these, we also provide negation, subtraction, squaring, exponentiation, and multiplication and addition of several ciphertexts for convenience. in many cases some of the inputs to a computation are plaintext elements rather than ciphertexts. For this we provide fast "plain" operations: plain addition, plain subtraction, and plain multiplication.

Relinearization
    One of the most important non-arithmetic operations is relinearization, which takes as input a ciphertext of size K+1 and relinearization keys (at least K-1 keys are needed), and changes the size of the ciphertext down to 2 (minimum size). For most use-cases only one relinearization key suffices, in which case relinearization should be performed after every multiplication. Homomorphic multiplication of ciphertexts of size K+1 and L+1 outputs a ciphertext of size K+L+1, and the computational cost of multiplication is proportional to K*L. Plain multiplication and addition operations of any type do not change the size. Relinearization requires relinearization keys to have been generated.

Rotations
    When batching is enabled, we provide operations for rotating the plaintext matrix rows cyclically left or right, and for rotating the columns (swapping the rows). Rotations require Galois keys to have been generated.

Other Operations
    We also provide operations for transforming ciphertexts to NTT form and back, and for transforming plaintext polynomials to NTT form. These can be used in a very fast plain multiplication variant, that assumes the inputs to be in NTT form. Since the NTT has to be done in any case in plain multiplication, this function can be used when e.g. one plaintext input is used in several plain multiplication, and transforming it several times would not make sense.

NTT form
    When using the BFV/BGV scheme (scheme_type::bfv/bgv), all plaintexts and ciphertexts should remain by default in the usual coefficient representation, i.e., not in NTT form. When using the CKKS scheme (scheme_type::ckks), all plaintexts and ciphertexts should remain by default in NTT form. We call these scheme-specific NTT states the "default NTT form". Some functions, such as add, work even if the inputs are not in the default state, but others, such as multiply, will throw an exception. The output of all evaluation functions will be in the same state as the input(s), with the exception of the transform_to_ntt and transform_from_ntt functions, which change the state. Ideally, unless these two functions are called, all other functions should "just work".
	
---
## HEaaN

### Code Example
~~~ C++
?
~~~

### Related Classes

### Details
#### HEaaN::HomEvaluator
These are Operations in HEaaN
 + add
 + conjugate
 + inverseRescale
 + killImag
 + leftRotate
 + leftRotateReduce
 + levelDown
 + levelDownOne
 + mult
 + multImagUnit
 + multInteger
 + multWithoutRescale
 + negate
 + relevel
 + relinearize
 + rescale
 + rightRotate
 + rightRotateReduce
 + rotSum
 + square
 + sub
 + tensor
