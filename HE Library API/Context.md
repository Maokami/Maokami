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
[seal::SEALContext](https://maokami.github.io/SEAL/classseal_1_1_s_e_a_l_context.html)
[seal::SEALContext::ContextData](https://maokami.github.io/SEAL/classseal_1_1_s_e_a_l_context_1_1_context_data.html)

### Details
 + Validate Encryption Parameters
 + Create a chain of ContextData

#### SEALContext
Performs sanity checks (validation) and pre-computations for a given set of encryption parameters. While the EncryptionParameters class is intended to be a light-weight class to store the encryption parameters, the SEALContext class is a heavy-weight class that is constructed from a given set of encryption parameters. **It validates the parameters for correctness, evaluates their properties, and performs and stores the results of several costly pre-computations.**

After the user has set at least the poly_modulus, coeff_modulus, and plain_modulus parameters in a given EncryptionParameters instance, the parameters can be validated for correctness and functionality by constructing an instance of SEALContext. The constructor of SEALContext does all of its work automatically, and concludes by constructing and storing an instance of the EncryptionParameterQualifiers class, with its flags set according to the properties of the given parameters. If the created instance of EncryptionParameterQualifiers has the parameters_set flag set to true, the given parameter set has been deemed valid and is ready to be used. If the parameters were for some reason not appropriately set, the parameters_set flag will be false, and a new SEALContext will have to be created after the parameters are corrected.

// TODO : 아랫 문단 이해하기
By default, SEALContext creates a chain of SEALContext::ContextData instances. The first one in the chain corresponds to special encryption parameters that are reserved to be used by the various key classes (SecretKey, PublicKey, etc.). These are the exact same encryption parameters that are created by the user and passed to th constructor of SEALContext. The functions key_context_data() and key_parms_id() return the ContextData and the parms_id corresponding to these special parameters. The rest of the ContextData instances in the chain correspond to encryption parameters that are derived from the first encryption parameters by always removing the last one of the moduli in the coeff_modulus, until the resulting parameters are no longer valid, e.g., there are no more primes left. These derived encryption parameters are used by ciphertexts and plaintexts and their respective ContextData can be accessed through the get_context_data(parms_id_type) function. The functions first_context_data() and last_context_data() return the ContextData corresponding to the first and the last set of parameters in the "data" part of the chain, i.e., the second and the last element in the full chain. The chain itself is a doubly linked list, and is referred to as the modulus switching chain.

#### SEALContext::ContextData
Class to hold pre-computation data for a given set of encryption parameters.

---
## HEaaN

### Code Example
~~~ C++
Context context = makeContext(preset);
~~~

### Related Classes

### Details

No public informations