## OpenFHE

### Code Example
~~~ C++
	std::vector<int64_t> vectorOfInts1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12};
	// BFV, BGV
	Plaintext plaintext1 = cryptoContext->MakePackedPlaintext(vectorOfInts1);

	// CKKS
    Plaintext plaintext1 = cryptoContext->MakePackedPlaintext(vectorOfInts1);
~~~

### Related Classes

[Template Class CryptoContextImpl](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CryptoContextImpl.html) 
-> [Class PlaintextFactory](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1PlaintextFactory.html?highlight=plaintextfactory)
[Class PlaintextImpl](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1PlaintextImpl.html#exhale-class-classlbcrypto-1-1plaintextimpl)
-> [Class CKKSPackedEncoding](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CKKSPackedEncoding.html#class-ckkspackedencoding "Permalink to this headline")
-> [Class CoefPackedEncoding](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CoefPackedEncoding.html#class-coefpackedencoding "Permalink to this headline")
-> [Class PackedEncoding](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1PackedEncoding.html#class-packedencoding "Permalink to this headline")
-> [Class StringEncoding](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1StringEncoding.html#class-stringencoding "Permalink to this headline")
![digraph {
graph [bgcolor="#00000000"]
node [shape=rectangle style=filled fillcolor="#FFFFFF" font=Helvetica padding=2]
edge [color="#1414CE"]
"652" [label="lbcrypto::PlaintextImpl" tooltip="lbcrypto::PlaintextImpl"]
"654" [label="lbcrypto::CoefPackedEncoding" tooltip="lbcrypto::CoefPackedEncoding"]
"655" [label="lbcrypto::PackedEncoding" tooltip="lbcrypto::PackedEncoding"]
"656" [label="lbcrypto::StringEncoding" tooltip="lbcrypto::StringEncoding"]
"653" [label="lbcrypto::CKKSPackedEncoding" tooltip="lbcrypto::CKKSPackedEncoding"]
"654" -> "652" [dir=forward tooltip="public-inheritance"]
"655" -> "652" [dir=forward tooltip="public-inheritance"]
"656" -> "652" [dir=forward tooltip="public-inheritance"]
"653" -> "652" [dir=forward tooltip="public-inheritance"]
}](https://openfhe-development.readthedocs.io/en/latest/_images/graphviz-e33ffc8b32875e071067201d2724f5e9183b98ec.png)

### Details
[CKKS Packed Encoding (ckkspackedencoding.h)](https://github.com/openfheorg/openfhe-development/blob/main/src/pke/include/encoding/ckkspackedencoding.h)
- Describes the CKKS packing. Accepts a `std::vector<double>` or a `std::vector<std::complex>` (although the double is recommended) unlike the other schemes.
[Coef Packed Encoding (coefpackedencoding.h)](https://github.com/openfheorg/openfhe-development/blob/main/src/pke/include/encoding/coefpackedencoding.h)
- Packs integers into a vector of size `n`
- Element-wise ops: Supports only element-wise addition (`EvalAdd`), but not multiplication. Thus, typically only works well when no multiplications are necessary
- Scalar ops: multiplication supported
Note
`Coef Packed Encoding` is rarely used.
[Encoding Params (encodingparams.h)](https://github.com/openfheorg/openfhe-development/blob/main/src/pke/include/encoding/encodingparams.h)
- The object containing the parameters for encoding. These parameters are kept and continually reused (can be modified) during the encoding of new values
[Encodings (encodings.h)](https://github.com/openfheorg/openfhe-development/blob/main/src/pke/include/encoding/encodings.h)
- “import” file which can be used for a single `#include`
[Packed Encoding (packedencoding.h)](https://github.com/openfheorg/openfhe-development/blob/main/src/pke/include/encoding/packedencoding.h)
- Packs integers into a vector of size `n`
- Supports element-wise addition and multiplication via `EvalAdd` and `EvalMult` respectively
- Supports automorphisms (commonly known as rotations) via `EvalAtIndex`
    - Right Shift: positive index
    - Left Shift: negative index
    - Rotations work cyclically
Note
is almost always what you want to use (other than if you want to deal with floating numbers)
[Plaintext (plaintext.h)](https://github.com/openfheorg/openfhe-development/blob/main/src/pke/include/encoding/plaintext.h)
- The base plaintext implementation
[Plaintext Factory (plaintextfactory.h)](https://github.com/openfheorg/openfhe-development/blob/main/src/pke/include/encoding/plaintextfactory.h)
- Factory class that instantiates plaintexts
[String Encoding (stringencoding.h)](https://github.com/openfheorg/openfhe-development/blob/main/src/pke/include/encoding/stringencoding.h)
- Encodes strings

#### PlaintextImpl
PlaintextImpl is primarily intended to be used as a container and in conjunction with specific encodings which inherit from this class which depend on the application the plaintext is used with. It provides virtual methods for encoding and decoding of data. 

---

## SEAL

### Code Example
~~~ C++
	/* BFV/BGV */
	BatchEncoder batch_encoder(context);

    /*
    The total number of batching `slots' equals the poly_modulus_degree, N, and
    these slots are organized into 2-by-(N/2) matrices that can be encrypted and
    computed on. Each slot contains an integer modulo plain_modulus.
    */
    size_t slot_count = batch_encoder.slot_count();
    size_t row_size = slot_count / 2;
    /*
    The matrix plaintext is simply given to BatchEncoder as a flattened vector
    of numbers. The first `row_size' many numbers form the first row, and the
    rest form the second row. Here we create the following matrix:

        [ 0,  1,  2,  3,  0,  0, ...,  0 ]
        [ 4,  5,  6,  7,  0,  0, ...,  0 ]
    */
    vector<uint64_t> pod_matrix(slot_count, 0ULL);
    pod_matrix[0] = 0ULL;
    pod_matrix[1] = 1ULL;
    pod_matrix[2] = 2ULL;
    pod_matrix[3] = 3ULL;
    pod_matrix[row_size] = 4ULL;
    pod_matrix[row_size + 1] = 5ULL;
    pod_matrix[row_size + 2] = 6ULL;
    pod_matrix[row_size + 3] = 7ULL;
    /*
    First we use BatchEncoder to encode the matrix into a plaintext polynomial.
    */
    Plaintext plain_matrix;
    batch_encoder.encode(pod_matrix, plain_matrix);

    /* CKKS */
	CKKSEncoder encoder(context);
    size_t slot_count = encoder.slot_count();
    cout << "Number of slots: " << slot_count << endl;

    vector<double> input;
    input.reserve(slot_count);
    double curr_point = 0;
    double step_size = 1.0 / (static_cast<double>(slot_count) - 1);
    for (size_t i = 0; i < slot_count; i++)
    {
        input.push_back(curr_point);
        curr_point += step_size;
    }
    /*
    We create plaintexts for PI, 0.4, and 1 using an overload of CKKSEncoder::encode
    that encodes the given floating-point value to every slot in the vector.
    */
    Plaintext plain_coeff3, plain_coeff1, plain_coeff0;
    encoder.encode(3.14159265, scale, plain_coeff3);
    encoder.encode(0.4, scale, plain_coeff1);
    encoder.encode(1.0, scale, plain_coeff0);

    Plaintext x_plain;
    encoder.encode(input, scale, x_plain);
	...
	encoder.decode(plain_result, result);
~~~

### Related Classes
[seal::BatchEncoder](seal::BatchEncoder)
[seal::CKKSEncoder](https://maokami.github.io/SEAL/classseal_1_1_c_k_k_s_encoder.html)

### Details

#### BatchEncoder
Provides functionality for CRT batching. If the polynomial modulus degree is N, and the plaintext modulus is a prime number T such that T is congruent to 1 modulo 2N, then BatchEncoder allows the plaintext elements to be viewed as 2-by-(N/2) matrices of integers modulo T. Homomorphic operations performed on such encrypted matrices are applied coefficient (slot) wise, enabling powerful SIMD functionality for computations that are vectorizable. This functionality is often called "batching" in the homomorphic encryption literature.

Mathematical Background
    Mathematically speaking, if the polynomial modulus is X^N+1, N is a power of two, and plain_modulus is a prime number T such that 2N divides T-1, then integers modulo T contain a primitive 2N-th root of unity and the polynomial X^N+1 splits into n distinct linear factors as X^N+1 = (X-a_1)*...*(X-a_N) mod T, where the constants a_1, ..., a_N are all the distinct primitive 2N-th roots of unity in integers modulo T. The Chinese Remainder Theorem (CRT) states that the plaintext space Z_T\[X\]/(X^N+1) in this case is isomorphic (as an algebra) to the N-fold direct product of fields Z_T. The isomorphism is easy to compute explicitly in both directions, which is what this class does. Furthermore, the Galois group of the extension is (Z/2NZ)* ~= Z/2Z x Z/(N/2)Z whose action on the primitive roots of unity is easy to describe. Since the batching slots correspond 1-to-1 to the primitive roots of unity, applying Galois automorphisms on the plaintext act by permuting the slots. By applying generators of the two cyclic subgroups of the Galois group, we can effectively view the plaintext as a 2-by-(N/2) matrix, and enable cyclic row rotations, and column rotations (row swaps).

Valid Parameters
    Whether batching can be used depends on whether the plaintext modulus has been chosen appropriately. Thus, to construct a BatchEncoder the user must provide an instance of SEALContext such that its associated EncryptionParameterQualifiers object has the flags parameters_set and enable_batching set to true.

#### CKKSEncoder
Provides functionality for encoding vectors of complex or real numbers into plaintext polynomials to be encrypted and computed on using the CKKS scheme. If the polynomial modulus degree is N, then CKKSEncoder converts vectors of N/2 complex numbers into plaintext elements. Homomorphic operations performed on such encrypted vectors are applied coefficient (slot-)wise, enabling powerful SIMD functionality for computations that are vectorizable. This functionality is often called "batching" in the homomorphic encryption literature.

Mathematical Background
    Mathematically speaking, if the polynomial modulus is X^N+1, N is a power of two, the CKKSEncoder implements an approximation of the canonical embedding of the ring of integers Z\[X\]/(X^N+1) into C^(N/2), where C denotes the complex numbers. The Galois group of the extension is (Z/2NZ)* ~= Z/2Z x Z/(N/2) whose action on the primitive roots of unity modulo coeff_modulus is easy to describe. Since the batching slots correspond 1-to-1 to the primitive roots of unity, applying Galois automorphisms on the plaintext acts by permuting the slots. By applying generators of the two cyclic subgroups of the Galois group, we can effectively enable cyclic rotations and complex conjugations of the encrypted complex vectors. 

---
## HEaaN

### Code Example
~~~ C++
?
~~~

### Related Classes
[HEaaN::EnDecoder](https://heaan.it/docs/heaan/classHEaaN_1_1EnDecoder.html)

### Details

#### HEaaN::EnDecoder
Class containing functions dealing with message/plaintext encoding and decoding.