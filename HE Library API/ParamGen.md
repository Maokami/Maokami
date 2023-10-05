## OpenFHE

todo : UniformGenerator?

### Code Example
~~~ C++
    CCParams<CryptoContextBFVRNS/BGVRNS/CKKSRNS> parameters;
    parameters.SetPlaintextModulus(65537);
    parameters.SetMultiplicativeDepth(2);
    CCParams<CryptoContextCKKSRNS> parameters;

	// simple-ckks-bootstrapping.cpp
    // A. Specify main parameters
    /*  A1) Secret key distribution
    * The secret key distribution for CKKS should either be SPARSE_TERNARY or UNIFORM_TERNARY.
    * The SPARSE_TERNARY distribution was used in the original CKKS paper,
    * but in this example, we use UNIFORM_TERNARY because this is included in the homomorphic
    * encryption standard.
    */
    SecretKeyDist secretKeyDist = UNIFORM_TERNARY;
    parameters.SetSecretKeyDist(secretKeyDist);

    /*  A2) Desired security level based on FHE standards.
    * In this example, we use the "NotSet" option, so the example can run more quickly with
    * a smaller ring dimension. Note that this should be used only in
    * non-production environments, or by experts who understand the security
    * implications of their choices. In production-like environments, we recommend using
    * HEStd_128_classic, HEStd_192_classic, or HEStd_256_classic for 128-bit, 192-bit,
    * or 256-bit security, respectively. If you choose one of these as your security level,
    * you do not need to set the ring dimension.
    */
    parameters.SetSecurityLevel(HEStd_NotSet);
    parameters.SetRingDim(1 << 12);

    /*  A3) Scaling parameters.
    * By default, we set the modulus sizes and rescaling technique to the following values
    * to obtain a good precision and performance tradeoff. We recommend keeping the parameters
    * below unless you are an FHE expert.
    */
    ScalingTechnique rescaleTech = FLEXIBLEAUTO;
    usint dcrtBits               = 59;
    usint firstMod               = 60;

    parameters.SetScalingModSize(dcrtBits);
    parameters.SetScalingTechnique(rescaleTech);
    parameters.SetFirstModSize(firstMod);

    /*  A4) Multiplicative depth.
    * The goal of bootstrapping is to increase the number of available levels we have, or in other words,
    * to dynamically increase the multiplicative depth. However, the bootstrapping procedure itself
    * needs to consume a few levels to run. We compute the number of bootstrapping levels required
    * using GetBootstrapDepth, and add it to levelsAvailableAfterBootstrap to set our initial multiplicative
    * depth. We recommend using the input parameters below to get started.
    */
    std::vector<uint32_t> levelBudget = {4, 4};

    uint32_t levelsAvailableAfterBootstrap = 10;
    usint depth = levelsAvailableAfterBootstrap + FHECKKSRNS::GetBootstrapDepth(levelBudget, secretKeyDist);
    parameters.SetMultiplicativeDepth(depth);

~~~

### Related Classes
![digraph {
graph [bgcolor="#00000000"]
node [shape=rectangle style=filled fillcolor="#FFFFFF" font=Helvetica padding=2]
edge [color="#1414CE"]
"616" [label="lbcrypto::Params" tooltip="lbcrypto::Params"]
"617" [label="lbcrypto::CCParams< CryptoContextBFVRNS >" tooltip="lbcrypto::CCParams< CryptoContextBFVRNS >"]
"618" [label="lbcrypto::CCParams< CryptoContextBGVRNS >" tooltip="lbcrypto::CCParams< CryptoContextBGVRNS >"]
"619" [label="lbcrypto::CCParams< CryptoContextCKKSRNS >" tooltip="lbcrypto::CCParams< CryptoContextCKKSRNS >"]
"617" -> "616" [dir=forward tooltip="public-inheritance"]
"618" -> "616" [dir=forward tooltip="public-inheritance"]
"619" -> "616" [dir=forward tooltip="public-inheritance"]
}](https://openfhe-development.readthedocs.io/en/latest/_images/graphviz-98ba69bc1555a6504bf445bdbc732d390d4543ab.png)

[Class Params](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1Params.html)
Getter/Setter of Params
[API List](https://docs.google.com/spreadsheets/d/1HkEu2ASZjmVwJOm9ldZu2ARvfnS_d6pWKWJILEQEyhU/edit#gid=0)

### Details
These are Params in OpenFHE
+ SCHEME
+ PlaintextModulus: PlaintextModulus
+ DigitSize: usint
+ StandardDeviation: float
+ SecretKeyDist: SecretKeyDist
+ MaxRelinSkDeg: usint
+ PREMode: ProxyReEncryptionMode
+ MultipartyMode: MultipartyMode
+ ExecutionMode: ExecutionMode
+ DecryptionNoiseMode: DecryptionNoiseMode
+ NoiseEstimate: double
+ DesiredPrecision: double
+ StatisticalSecurity: uint32_t
+ NumAdversarialQueries: uint32_t
+ ThresholdNumOfParties: uint32_t
+ KeySwitchTechnique: KeySwitchTechnique
+ ScalingTechnique: ScalingTechnique
+ BatchSize: usint
+ FirstModSize: usint
+ NumLargeDigits: uint32_t
+ MultiplicativeDepth: usint
+ ScalingModSize: usint
+ SecurityLevel: SecurityLevel
+ RingDim: usint
+ EvalAddCount: usint
+ KeySwitchCount: usint
+ EncryptionTechnique: EncryptionTechnique
+ MultiplicationTechnique: MultiplicationTechnique
+ MultiHopModSize: usint
+ InteractiveBootCompressionLevel: COMPRESSION_LEVEL
---

## SEAL

### Code Example
~~~ C++
	/* BFV */
	EncryptionParameters parms(scheme_type::bfv);

    size_t poly_modulus_degree = 4096;
    parms.set_poly_modulus_degree(poly_modulus_degree);
	parms.set_coeff_modulus(CoeffModulus::BFVDefault(poly_modulus_degree));

    parms.set_plain_modulus(1024);

    /* BGV */
    EncryptionParameters parms(scheme_type::bgv);
    size_t poly_modulus_degree = 8192;
    parms.set_poly_modulus_degree(poly_modulus_degree);


    parms.set_coeff_modulus(CoeffModulus::BFVDefault(poly_modulus_degree));
    parms.set_plain_modulus(PlainModulus::Batching(poly_modulus_degree, 20));
~~~

### Related Classes
[seal::EncryptionParameters](https://maokami.github.io/SEAL/classseal_1_1_encryption_parameters.html)
[seal::EncryptionParameterQualifiers](https://maokami.github.io/SEAL/classseal_1_1_encryption_parameter_qualifiers.html)
[seal::Modulus](https://maokami.github.io/SEAL/classseal_1_1_modulus.html)
[seal::CoeffModulus](https://maokami.github.io/SEAL/classseal_1_1_coeff_modulus.html)
[seal::PlainModulus](https://maokami.github.io/SEAL/classseal_1_1_plain_modulus.html)
[seal::UniformRandomGenerator](https://maokami.github.io/SEAL/classseal_1_1_uniform_random_generator.html)
![UniformRandomGenerator](https://maokami.github.io/SEAL/classseal_1_1_uniform_random_generator.png)  
[seal::UniformRandomGeneratorFactory](https://maokami.github.io/SEAL/classseal_1_1_uniform_random_generator_factory.html)
![UniformRandomGeneratorFactory](https://maokami.github.io/SEAL/classseal_1_1_uniform_random_generator_factory.png)

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

#### UniformRandomGenerator
Provides the base class for a seeded uniform random number generator. Instances of this class are meant to be created by an instance of the factory class UniformRandomGeneratorFactory. This class is meant for users to sub-class to implement their own random number generators.

#### UniformRandomGeneratorFactory
Provides the base class for a factory instance that creates instances of UniformRandomGenerator. This class is meant for users to sub-class to implement their own random number generators. 

---
## HEaaN

### Code Example
~~~ C++
const ParameterPreset preset = ParameterPreset::FGb;
~~~

### Related Classes
[HEaaN::ParameterPreset](https://heaan.it/docs/heaan/namespaceHEaaN.html#a06722c6e93b68e99244d832def124d5e)

### Details

These are Params in HEaaN
+ FVb
+ FGa
+ FGb
+ FTa
+ FTb
+ ST19
+ ST14
+ ST11
+ ST8
+ ST7
+ SS7
+ SD3
+ CUSTOM
+ FVc
+ FX
+ FGd
+ SGd0


#### HEaaN::ParameterPreset
Class of Parameter presets.

The first alphabet denotes whether a parameter is full or somewhat homomorphic encryption. Somewhat homomorphic encryption parameters can use only a finite number of multiplications while full homomorphic encryption parameters can use infinitely many multiplications via bootstrapping. The second alphabet denotes the size of log2(N), where N denotes the dimension of the base ring. V(Venti), G(Grande), T(Tall), S(Short), D(Demi) represent log2(N) = 17, 16, 15, 14, 13, respectively. For somewhat parameters, a number that comes after these alphabets indicates total available multiplication number.