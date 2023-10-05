## OpenFHE

### Code Example
~~~ C++
	/* PKE */
    CryptoContext<DCRTPoly> cryptoContext = GenCryptoContext(parameters);
    // Enable features that you wish to use
    cryptoContext->Enable(PKE);
    cryptoContext->Enable(KEYSWITCH);
    cryptoContext->Enable(LEVELEDSHE);

	/* BinFHE */
	auto cc = BinFHEContext();

    // STD128 is the security level of 128 bits of security based on LWE Estimator
    // and HE standard. Other common options are TOY, MEDIUM, STD192, and STD256.
    // MEDIUM corresponds to the level of more than 100 bits for both quantum and
    // classical computer attacks.
    cc.GenerateBinFHEContext(STD128);
~~~

### Related Classes

[CryptoContext](https://openfhe-development.readthedocs.io/en/latest/api/typedef_namespacelbcrypto_1a6443ca2b0d18f9fef302de7962b61270.html#_CPPv4N8lbcrypto13CryptoContextE) = std::shared_ptr<[CryptoContextImpl](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CryptoContextImpl.html#_CPPv4I0EN8lbcrypto17CryptoContextImplE "lbcrypto::CryptoContextImpl")\<Element\>>

### Details

#### CryptoContextImpl
A CryptoContextImpl is the object used to access the OpenFHE library

**All OpenFHE functionality is accessed by way of an instance of a CryptoContextImpl; we say that various objects are “created in” a context, and can only be used in the context in which they were created**

All OpenFHE methods are accessed through CryptoContextImpl methods. Guards are implemented to make certain that only valid objects that have been created in the context are used

Contexts are created using GenCryptoContext(), and can be serialized and recovered from a serialization 

##### Functions
inline void Enable([PKESchemeFeature](https://openfhe-development.readthedocs.io/en/latest/api/enum_namespacelbcrypto_1a7c246b89a50fd0bdacc556c53c2042a3.html#_CPPv4N8lbcrypto16PKESchemeFeatureE "lbcrypto::PKESchemeFeature") feature)[](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CryptoContextImpl.html#_CPPv4N8lbcrypto17CryptoContextImpl6EnableE16PKESchemeFeature "Permalink to this definition")  
Enable a particular feature for use with this [CryptoContextImpl](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CryptoContextImpl.html#classlbcrypto_1_1CryptoContextImpl)
	feature – the feature that should be enabled

inline void Enable([usint](https://openfhe-development.readthedocs.io/en/latest/api/typedef_inttypes_8h_1ac95a0fa2550c285e957dc8834cf73e67.html#_CPPv45usint "usint") featureMask)[](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CryptoContextImpl.html#_CPPv4N8lbcrypto17CryptoContextImpl6EnableE5usint "Permalink to this definition")  
Enable several features at once
	featureMask – bitwise or of several PKESchemeFeatures

##### PKESchemeFeature
 + PKE
 + KEYSWITCH
 + PRE
 + LEVELEDSHE
 + ADVANCEDSHE
 + MULTIPARTY
 + FHE
 + SCHEMESWITCH


---
**TODO**

## SEAL

### Code Example
~~~ C++
    SEALContext context(parms);
    /*
    When parameters are used to create SEALContext, Microsoft SEAL will first
    validate those parameters. The parameters chosen here are valid.
    */
	context.parameter_error_message();
~~~

### Related Classes


### Details

These are Params in SEAL
+ scheme: scheme_type
+ poly_modulus_degree: std::size_t
+ coeff_modulus: const std::vector< Modulus > & 
+ plain_modulus: const Modulus &
+ random_generator: std::shared_ptr< UniformRandomGeneratorFactory >

#### EncryptionParameters
Represents user-customizable encryption scheme settings. The parameters (most importantly poly_modulus, coeff_modulus, plain_modulus) significantly affect the performance, capabilities, and security of the encryption scheme. Once an instance of EncryptionParameters is populated with appropriate parameters, it can be used to create an instance of the SEALContext class, which verifies the validity of the parameters, and performs necessary pre-computations.

Picking appropriate encryption parameters is essential to enable a particular application while balancing performance and security. Some encryption settings will not allow some inputs (e.g. attempting to encrypt a polynomial with more coefficients than poly_modulus or larger coefficients than plain_modulus) or, support the desired computations (with noise growing too fast due to too large plain_modulus and too small coeff_modulus).

parms_id
    The EncryptionParameters class maintains at all times a 256-bit hash of the currently set encryption parameters called the parms_id. This hash acts as a unique identifier of the encryption parameters and is used by all further objects created for these encryption parameters. The parms_id is not intended to be directly modified by the user but is used internally for pre-computation data lookup and input validity checks. In modulus switching the user can use the parms_id to keep track of the chain of encryption parameters. The parms_id is not exposed in the public API of EncryptionParameters, but can be accessed through the SEALContext::ContextData class once the SEALContext has been created.

Thread Safety
    In general, reading from EncryptionParameters is thread-safe, while mutating is not.

Warning
    Choosing inappropriate encryption parameters may lead to an encryption scheme that is not secure, does not perform well, and/or does not support the input and computation of the desired application. We highly recommend consulting an expert in RLWE-based encryption when selecting parameters, as this is where inexperienced users seem to most often make critical mistakes. 

#### EncryptionParameterQualifiers
Stores a set of attributes (qualifiers) of a set of encryption parameters. These parameters are mainly used internally in various parts of the library, e.g., to determine which algorithmic optimizations the current support. The qualifiers are automatically created by the SEALContext class, silently passed on to classes such as Encryptor, Evaluator, and Decryptor, and the only way to change them is by changing the encryption parameters themselves. In other words, a user will never have to create their own instance of this class, and in most cases never have to worry about it at all. 

#### Modulus
Represent an integer modulus of up to 61 bits. An instance of the Modulus class represents a non-negative integer modulus up to 61 bits. In particular, the encryption parameter plain_modulus, and the primes in coeff_modulus, are represented by instances of Modulus. The purpose of this class is to **perform and store the pre-computation required by Barrett reduction**.

Thread Safety
    In general, reading from Modulus is thread-safe as long as no other thread is concurrently mutating it.

See also
    EncryptionParameters for a description of the encryption parameters. 

#### CoeffModulus
This class contains static methods for creating a coefficient modulus easily. Note that while these functions take a sec_level_type argument, all security guarantees are lost if the output is used with encryption parameters with a mismatching value for the poly_modulus_degree.

The default value sec_level_type::tc128 provides a very high level of security and is the default security level enforced by Microsoft SEAL when constructing a SEALContext object. Normal users should not have to specify the security level explicitly anywhere.

#### PlainModulus
This class contains static methods for creating a plaintext modulus easily. 


---
## HEaaN

### Code Example
~~~ C++
Context context = makeContext(preset);
~~~

### Related Classes

### Details

These are Params in HEaaN

#### HEaaN::ParameterPreset
Class of Parameter presets.

The first alphabet denotes whether a parameter is full or somewhat homomorphic encryption. Somewhat homomorphic encryption parameters can use only a finite number of multiplications while full homomorphic encryption parameters can use infinitely many multiplications via bootstrapping. The second alphabet denotes the size of log2(N), where N denotes the dimension of the base ring. V(Venti), G(Grande), T(Tall), S(Short), D(Demi) represent log2(N) = 17, 16, 15, 14, 13, respectively. For somewhat parameters, a number that comes after these alphabets indicates total available multiplication number.