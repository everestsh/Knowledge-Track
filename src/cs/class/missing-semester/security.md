## Security and Cryptography

- Entropy

  - [Entropy](https://en.wikipedia.org/wiki/Entropy_(information_theory)) is a measure of randomness.

- Hash functions

  - A [cryptographic hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function) maps data of arbitrary size to a fixed size, and has some special properties.
  - `hash(value: array<byte>) -> vector<byte, N>  (for some fixed N)`
  - Applications:
    - Git, for content-addressed storage. 
    - A short summary of the contents of a file.
    - [Commitment schemes](https://en.wikipedia.org/wiki/Commitment_scheme). 

- Key derivation functions

  - Applications:
    - Producing keys from passphrases for use in other cryptographic algorithms
    - Storing login credentials.

- Symmetric cryptography

  - ```
    keygen() -> key  (this function is randomized)
    
    encrypt(plaintext: array<byte>, key) -> array<byte>  (the ciphertext)
    decrypt(ciphertext: array<byte>, key) -> array<byte>  (the plaintext)
    ```

  - Applications:
    - Encrypting files for storage in an untrusted cloud service.

- Asymmetric cryptography

  - ```
    keygen() -> (public key, private key)  (this function is randomized)
    
    encrypt(plaintext: array<byte>, public key) -> array<byte>  (the ciphertext)
    decrypt(ciphertext: array<byte>, private key) -> array<byte>  (the plaintext)
    
    sign(message: array<byte>, private key) -> array<byte>  (the signature)
    verify(message: array<byte>, signature: array<byte>, public key) -> bool  (whether or not the signature is valid)
    ```

  - Applications

    - [PGP email encryption](https://en.wikipedia.org/wiki/Pretty_Good_Privacy).
    - Private messaging. 
    - Signing software.

  - Key Distribution

- Case Studies
  - Password managers
    -  [KeePassXC](https://keepassxc.org/), [pass](https://www.passwordstore.org/), and [1Password](https://1password.com/)
  - Two-factor authentication (2FA)
  - Full disk encryption
  - Private messaging
    -  [Signal](https://signal.org/) or [Keybase](https://keybase.io/)
  - SSH
    - `ssh-keygen`
    - stored in the `.ssh/authorized_keys` file
- Resouces
  - [Cryptographic Right Answers](https://latacora.micro.blog/2018/04/03/cryptographic-right-answers.html): answers “what crypto should I use for X?” for many common X.

