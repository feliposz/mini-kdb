# Swagger

## Swagger Codegen

### Problem: Can't run Swagger Codegen tool because of local developer HTTPS certificate

```
mkdir swagger_codegen_aspnetcore
\PATH\TO\JRE\bin\java -jar swagger-codegen-cli.jar generate -i https://localhost:7261/swagger/v1/swagger.json -l aspnetcore -o swagger_codegen_aspnetcore
```

Error:

```
javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

### Solution: Import certificate

Export Developer HTTPS certificate:

```
dotnet dev-certs https -ep \PATH\TO\CERTIFICATE\https-development.crt
```

Import developer HTTPS certificate in JRE using keytool. Run as Administrator:

```
\PATH\TO\JRE\bin\keytool -import -trustcacerts -alias SERVER -file \PATH\TO\CERTIFICATE\https-development.crt -keystore \PATH\TO\JRE\lib\security\cacerts
```

Default password is "changeit".

### Problem: Can't run Swagger Codegen tool because of version mismatch

```
java.lang.RuntimeException: missing swagger input or config!\
```

Check swagger-codegen version:

```
\PATH\TO\JRE\bin\java -jar swagger-codegen-cli.jar version
2.4.26
```

Version from `swagger.json`:

```json
  "swagger": "3.xxx"
```
  
### Solution

1) Upgrade swagger-codegen-cli

-- or --

2) Make dotnet program output version 2:

Change `app.UseSwagger()` to:

```
app.UseSwagger(options => options.SerializeAsV2 = true);
```
