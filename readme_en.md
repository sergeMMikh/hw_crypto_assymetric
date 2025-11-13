# Homework for Lesson “3.2. Asymmetric Cryptosystems”
## Sergey Mikhalev

## Task: LibreSSL & OpenSSL

In this task, we will learn how to encrypt data using a public key and decrypt it using a private key.

The first thing we must do is generate a key. For this, we need to choose an algorithm, key size, and passphrase.

<details>
<summary>Steps to Complete the Task</summary>

#### Step 1. Create a private key with a length of 2048 bits:

```shell script
openssl genpkey -algorithm RSA -pkeyopt rsa_keygen_bits:2048 -aes256 -out private.key
```

Where [`genpkey`](https://www.openssl.org/docs/manmaster/man1/openssl-genpkey.html) is the subcommand responsible for generating keys.

Enter the passphrase twice (note that characters will not be displayed for security reasons).

Open the `private.key` file in any text editor (or view it in the terminal with `cat private.key`) and verify that it looks similar to this:

```text
-----BEGIN ENCRYPTED PRIVATE KEY-----
...
LOTS OF CHARACTERS
...
-----END ENCRYPTED PRIVATE KEY-----
```

#### Step 2. Generate a public key based on the private key:

```shell script
openssl pkey -pubout -in private.key -out public.key
```

Where [`pkey`](https://www.openssl.org/docs/man1.0.2/man1/pkey.html) is the subcommand responsible for key operations.

Open the `public.key` file (or view it using `cat public.key`) and verify that it looks like this:

```text
-----BEGIN PUBLIC KEY-----
...
LOTS OF CHARACTERS
...
-----END PUBLIC KEY-----
```

#### Step 3. Create a file `message.txt` containing your last name.

#### Step 4. Encrypt the message using the public key:

```shell script
openssl pkeyutl -encrypt -pubin -inkey public.key -in message.txt -out cypher.txt
```

Where [`pkeyutl`](https://www.openssl.org/docs/manmaster/man1/openssl-pkeyutl.html) is the subcommand used for low-level public key operations (including encryption and decryption).

Open `cypher.txt` in any text editor and ensure it is unreadable.

#### Step 5. Verify that the file `cypher.txt` can be decrypted:

```shell script
openssl pkeyutl -decrypt -inkey private.key -in cypher.txt -out decrypted.txt
```
</details>

#### Expected Result

Submit the following:
1. Public and private keys (`public.key` and `private.key`)
2. Passphrase (as a string)
3. Encrypted file (`cypher.txt`)

---

## Solution

<details>
<summary>Execution Details</summary>

Checking the OpenSSL version:

![openssl version](images/image.png)

Generating private and public keys:

![ssh keys](images/image-2.png)

While encrypting the text file, an error occurred: *the input data is larger than the RSA key allows with the chosen padding scheme*. The text had to be shortened.

![text](images/image-1.png)

Decrypting the file:

![decrypted](images/image-3.png)

</details>

Result:
 * [`public.key`](Task1/public.key)
 * [`private.key`](Task1/private.key)
 * The passphrase is included in the comment to the solution
 * Encrypted file: [`cypher.txt`](Task1/cypher.txt)

-----

## Task: GnuPG — Message Signing

In this task, we will learn how to sign and verify documents using [GnuPG](https://gnupg.org), a popular implementation of [OpenPGP](https://www.ietf.org/rfc/rfc4880.txt). It is widely used in many Open Source projects for signing software packages.

![](images/gnupg.png)

We will work with Russian cryptography later (since trial periods for Russian products are time-limited and we will need them again in several future courses).

#### Expected Result

Submit the following:
1. Public key (from step 5)
1. Message file and its signature (from step 3)

---

## Solution

<details>
<summary>Execution Details</summary>

Checking the GnuPG version:

![GnuPG version](images/image-4.png)

### Step 1. Generate the keypair

```bash
$ gpg --full-generate-key
gpg (GnuPG) 2.4.4; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (3072) 2048
Requested keysize is 2048 bits
Please specify how long the key should be valid.
         0 = key does not expire
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

Real name: Sergey
Email address: s.m.mih@mail.ru
Comment:
You selected this USER-ID:
    "Sergey <s.m.mih@mail.ru>"
```

### Step 2. View Keys

![keys view](images/image-5.png)

### Step 3. Sign the message

![message](images/image-6.png)

### Step 4. Signature verification (false message)

![bad message](images/image-7.png)

### Step 5. Export the public key

![key export](images/image-8.png)

</details>

Result:
* [Public key](Task_2/public.gpg) (from step 5)
* [Message file](Task_2/message.txt) and [its signature](Task_2/message.sig) (from step 3)

