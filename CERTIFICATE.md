# Generate Keystore

### Generate Keystores
```shell
keytool -genkeypair -alias <service> -keyalg RSA -keysize 2048 -validity 3650 -dname "CN=<domain>" -keypass <password> -keystore ./keys/<keystore>.p12 -storetype PKCS12 -storepass <password>
```

### Export the public certificate
```shell
keytool -export -alias <service> -keystore ./keys/<keystore>.p12 -storepass <password> -file ./keys/<domain>.cer -rfc
```
