## OpenFHE

### Code Example
~~~ C++
	auto ciphertext1 = cryptoContext->Encrypt(keyPair.publicKey, plaintext1);
	...
    Plaintext plaintext1;
    cc->Decrypt(sk, ciphertext1, &plaintext1);
~~~

### Related Classes
[Template Class CryptoContextImpl](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CryptoContextImpl.html) 
-> [Template Class SchemeBase](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1SchemeBase.html?highlight=evalmultkeygen) 
 -> [Template Class PKEBase](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1PKEBase.html?highlight=pkebase#template-class-pkebase "Permalink to this headline")
	 -> [Class PKERNS](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1PKERNS.html?highlight=pkebase#class-pkerns "Permalink to this headline")
		 -> [Class PKEBFVRNS](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1PKEBFVRNS.html#class-pkebfvrns "Permalink to this headline")
		 -> [Class PKEBGVRNS](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1PKEBGVRNS.html#exhale-class-classlbcrypto-1-1pkebgvrns)
		 -> [Class PKECKKSRNS](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1PKECKKSRNS.html#exhale-class-classlbcrypto-1-1pkeckksrns)
![digraph {
graph [bgcolor="#00000000"]
node [shape=rectangle style=filled fillcolor="#FFFFFF" font=Helvetica padding=2]
edge [color="#1414CE"]
"644" [label="lbcrypto::PKEBase< DCRTPoly >" tooltip="lbcrypto::PKEBase< DCRTPoly >"]
"647" [label="lbcrypto::PKECKKSRNS" tooltip="lbcrypto::PKECKKSRNS"]
"643" [label="lbcrypto::PKERNS" tooltip="lbcrypto::PKERNS"]
"645" [label="lbcrypto::PKEBFVRNS" tooltip="lbcrypto::PKEBFVRNS"]
"646" [label="lbcrypto::PKEBGVRNS" tooltip="lbcrypto::PKEBGVRNS"]
"647" -> "643" [dir=forward tooltip="public-inheritance"]
"643" -> "644" [dir=forward tooltip="public-inheritance"]
"645" -> "643" [dir=forward tooltip="public-inheritance"]
"646" -> "643" [dir=forward tooltip="public-inheritance"]
}](https://openfhe-development.readthedocs.io/en/latest/_images/graphviz-1729e76fed247965168f1f104f8a68a24d51c23c.png)
### Details
Note : 
	Encryption (PKERNS = PKEBGVRNS = PKECKKSRNS != PKEBFVRNS)
	Decryption (PKERNS != PKEBGVRNS != PKECKKSRNS != PKEBFVRNS)
 

---

## SEAL

### Code Example
~~~ C++
	Encryptor encryptor(context, public_key);
    Decryptor decryptor(context, secret_key);
	encryptor.encrypt(x_plain, x1_encrypted);
    decryptor.decrypt(encrypted_result, plain_result);
~~~

### Related Classes
[seal::Encryptor](https://maokami.github.io/SEAL/classseal_1_1_encryptor.html)
[seal::Decryptor](https://maokami.github.io/SEAL/classseal_1_1_decryptor.html)

### Details
#### Encryptor
Encrypts Plaintext objects into Ciphertext objects. Constructing an Encryptor requires a SEALContext with valid encryption parameters, the public key and/or the secret key. If an Encrytor is given a secret key, it supports symmetric-key encryption. If an Encryptor is given a public key, it supports asymmetric-key encryption.

Overloads
    For the encrypt function we provide two overloads concerning the memory pool used in allocations needed during the operation. In one overload the global memory pool is used for this purpose, and in another overload the user can supply a MemoryPoolHandle to to be used instead. This is to allow one single Encryptor to be used concurrently by several threads without running into thread contention in allocations taking place during operations. For example, one can share one single Encryptor across any number of threads, but in each thread call the encrypt function by giving it a thread-local MemoryPoolHandle to use. It is important for a developer to understand how this works to avoid unnecessary performance bottlenecks.

NTT form
    When using the BFV/BGV scheme (scheme_type::bfv/bgv), all plaintext and ciphertexts should remain by default in the usual coefficient representation, i.e. not in NTT form. When using the CKKS scheme (scheme_type::ckks), all plaintexts and ciphertexts should remain by default in NTT form. We call these scheme-specific NTT states the "default NTT form". Decryption requires the input ciphertexts to be in the default NTT form, and will throw an exception if this is not the case. 

#### Decryptor
Decrypts Ciphertext objects into Plaintext objects. Constructing a Decryptor requires a SEALContext with valid encryption parameters, and the secret key. The Decryptor is also used to compute the invariant noise budget in a given ciphertext.

Overloads
    For the decrypt function we provide two overloads concerning the memory pool used in allocations needed during the operation. In one overload the global memory pool is used for this purpose, and in another overload the user can supply a MemoryPoolHandle to be used instead. This is to allow one single Decryptor to be used concurrently by several threads without running into thread contention in allocations taking place during operations. For example, one can share one single Decryptor across any number of threads, but in each thread call the decrypt function by giving it a thread-local MemoryPoolHandle to use. It is important for a developer to understand how this works to avoid unnecessary performance bottlenecks.

NTT form
    When using the BFV scheme (scheme_type::bfv), all plaintext and ciphertexts should remain by default in the usual coefficient representation, i.e. not in NTT form. When using the CKKS scheme (scheme_type::ckks), all plaintexts and ciphertexts should remain by default in NTT form. We call these scheme-specific NTT states the "default NTT form". Decryption requires the input ciphertexts to be in the default NTT form, and will throw an exception if this is not the case. 


---
## HEaaN

### Code Example
~~~ C++
Encryptor enc(context);
Message msg(log_slots);
fillRandomComplex(msg);

Ciphertext ctxt(context);
enc.encrypt(msg, pack, ctxt);

Decryptor dec(context);
Message dmsg, dmsg_boot;
dec.decrypt(ctxt, sk, dmsg);
~~~

### Related Classes
[HEaaN::Encryptor](https://heaan.it/docs/heaan/classHEaaN_1_1Encryptor.html)
[HEaaN::Decryptor](https://heaan.it/docs/heaan/classHEaaN_1_1Decryptor.html)

### Details

#### HEaaN::Encryptor
Abstract entity for encrypting messages into ciphertexts. 

#### HEaaN::DeEncryptor
Abstract entity for decrypting ciphertexts.