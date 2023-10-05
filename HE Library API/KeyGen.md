## OpenFHE

### Code Example
~~~ C++
	/* PKE */
	auto keyPair = cryptoContext->KeyGen();
    // Generate the relinearization key
    cryptoContext->EvalMultKeyGen(keyPair.secretKey);
    // Generate the rotation evaluation keys
    cryptoContext->EvalRotateKeyGen(keyPair.secretKey, {1, 2, -1, -2});
    cryptoContext->EvalBootstrapKeyGen(keyPair.secretKey, numSlots);

	/* BinFHE */
	auto cc = BinFHEContext();

    // STD128 is the security level of 128 bits of security based on LWE Estimator
    // and HE standard. Other common options are TOY, MEDIUM, STD192, and STD256.
    // MEDIUM corresponds to the level of more than 100 bits for both quantum and
    // classical computer attacks.
    cc.GenerateBinFHEContext(STD128);
~~~

### Related Classes
[Template Class KeyPair](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1KeyPair.html#template-parameter-order)

[Template Class CryptoContextImpl](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1CryptoContextImpl.html) 
-> [Template Class SchemeBase](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1SchemeBase.html?highlight=evalmultkeygen) 
	-> [Template Class LeveledSHEBase](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1LeveledSHEBase.html?highlight=evalmultkeygen) 
	-> [Class SWITCHCKKSRNS](https://openfhe-development.readthedocs.io/en/latest/api/classlbcrypto_1_1SWITCHCKKSRNS.html?highlight=SWITCHCKKSRNS#class-switchckksrns "Permalink to this headline")
  
 
### Details
These are KeyGen in OpenFHE
 + EvalAtIndexKeyGen
 + EvalAutomorphismKeyGen
 + EvalCKKStoFHEWKeyGen : SWITCHCKKSRNS
 + EvalFHEWtoCKKSKeyGen : SWITCHCKKSRNS
 + EvalMultKeyGen
 + EvalMultKeysGen
 + EvalRotateKeyGen = EvalRotateKeyGen
 + EvalSchemeSwitchingKeyGen : SWITCHCKKSRNS
 + EvalSumColsKeyGen
 + EvalSumKeyGen
 + EvalSumRowsKeyGen
 + KeyGen
 + KeySwitchGen
 + MultiEvalAtIndexKeyGen
 + MultiEvalAutomorphismKeyGen
 + MultiEvalSumKeyGen
 + MultiKeySwitchGen
 + MultipartyKeyGen
 + ReKeyGen
 + SparseKeyGen


---

## SEAL

### Code Example
~~~ C++
	KeyGenerator keygen(context);
	SecretKey secret_key = keygen.secret_key();
	PublicKey public_key;
	keygen.create_public_key(public_key);
	RelinKeys relin_keys;
	keygen.create_relin_keys(relin_keys);
~~~

### Related Classes
[seal::KeyGenerator](https://maokami.github.io/SEAL/classseal_1_1_key_generator.html)

### Details
These are Keys in SEAL
 + SecretKey
 + PublicKey
 + RelinKeys
 + GaloisKeys

---
## HEaaN

### Code Example
~~~ C++
	SecretKey sk(context);
	KeyPack pack(context);
	KeyGenerator keygen(context, sk, pack);
	const u64 log_slots = getLogFullSlots(context);
	keygen.genEncKey();
	keygen.genConjKey();
	keygen.genMultKey();
	keygen.genRotKeysForBootstrap(log_slots);
~~~

### Related Classes

### Details
These are KeyGen in HEaaN
 + genEncryptionKey
 + genMultiplicationKey
 + genConjugationKey
 + genLeftRotationKey
 + genRightRotationKey
 + genRotationKeyBundle
 + genSparseSecretEncapsulationKey
 + genCommonKeys
 + genRotKeysForBootstrap
