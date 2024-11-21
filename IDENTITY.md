# Generate Private and Public RSA Key

OpenSSL Project is dedicated to providing a simple installation of OpenSSL for Microsoft Windows.
[Download](https://slproweb.com/products/Win32OpenSSL.html)

Generate an RSA private key, of size 2048, and output it to a file named `identity_private_key.pem` in to `./keys`

```shell
openssl genrsa -out identity_private_key.pem 2048
```

Extract the public key from `identity_private_key.pem` from `./keys`, and output it to a file named `identity_public_key.pem` in to `./keys`

```shell
openssl rsa -in identity_private_key.pem -pubout -out identity_public_key.pem
```
