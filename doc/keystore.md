# Generate Keystore

### Generate Keystores

To generate a keystore for our API's and web manager, which is used to enable HTTPS, you can use the following command:

```shell
keytool -genkeypair -alias <service> -keyalg RSA -keysize 2048 -validity 3650 -dname "CN=<domain>" -keypass <password> -keystore ./cert/<keystore>.p12 -storetype PKCS12 -storepass <password>
```

This command generates a keystore with the following properties:

* \<service>: The alias for the service (e.g. device-api)
* \<domain>: The domain name for the service (e.g. deviceapi)
* \<password>: The password for the keystore and private key
* ./cert/\<keystore>.p12: The file path and name for the generated keystore

### Export the public certificate

Once you have generated a keystore, you can export the public certificate using the following command:

```shell
keytool -export -alias <service> -keystore ./cert/<keystore>.p12 -storepass <password> -file ./cert/<domain>.cer -rfc
```

This command exports the public certificate with the following properties:

* \<service>: The alias for the service (e.g. device-api)
* \<keystore>: The keystore file containing the private key and certificate (e.g. ./cert/deviceapi.p12)
* \<password>: The password for the keystore
* \<domain>: The domain name for the exported certificate file (e.g. deviceapi.cer)
* ./cert/\<domain>.cer: The file path and name for the exported public certificate

The exported public certificate is then used on devices to establish a secure connection to our API. Specifically, the certificate is installed on devices to enable them to trust our API's SSL/TLS connection, allowing for encrypted communication between the device and our API.

Note: Make sure to update your hosts file with an entry for the domain name (e.g. 127.0.0.1 deviceapi) to enable local development.

1. Open `hosts` file:

   * On Unix-like systems (Linux, macOS), this directory is typically `/etc/hosts`.
   * On Windows, this directory is typically `%SystemRoot%\System32\drivers\etc\hosts`.

2. Add the following lines to the `hosts` file:
    ```text
    127.0.0.1	deviceapi		# Hlæja Device API
    ```
