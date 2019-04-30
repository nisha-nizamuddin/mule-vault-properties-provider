# Vault Properties Provider

### Global Config
Add a Vault Properties Provider Config global element to your application. Specify the Vault URL and properties to log in.

##### Basic Configuration
Attributes:

*   vaultToken - Token to use for authentication
*   kvVersion - Version of the KV engine being used

```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:basic vaultToken="s.uo18rIGCFexkcxOOJET97EPA" kvVersion="1" />
</vault-properties-provider:config>
```

##### SSL Configuration with Token Authentication (PEM)
Attributes:
basic element attributes:

*   vaultToken - Token to use for authentication
*   kvVersion - Version of the KV engine being used

ssl element attributes:

*   pemFile - path to PEM file for vault server SSL
*   verifySSL - true to validate certificates

```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:basic vaultToken="s.uo18rIGCFexkcxOOJET97EPA" kvVersion="2"/>
  <vault-properties-provider:ssl pemFile="ssl/my.pem" verifySSL="true" />
</vault-properties-provider:config>
```

##### SSL Configuration with Token Authentication (KeyStore)
Attributes:
basic element attributes:

*   vaultToken - Token to use for authentication

ssl element attributes:

*   useTlsAuth - false to use Token Authentication
*   verifySSL - true to validate certificats
*   trustStorePath - path to Java trust store (JKS)

```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:basic vaultToken="s.uo18rIGCFexkcxOOJET97EPA" kvVersion="2"/>
  <vault-properties-provider:ssl verifySSL="true" trustStorePath="/tmp/trustStore.jks" />
</vault-properties-provider:config>
```

##### SSL Configuration with TLS Authentication (JKS)
Attributes:

*   kvVersion - version of the secrets engine to use (1 or 2)
*   verifySSL - true to validate certificate
*   trustStorePath - path to Java trust store (JKS)
*   keyStorePath - path to Java key store (JKS)
*   keyStorePassword - password for the key store


```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:basic kvVersion="2" />
  <vault-properties-provider:ssl verifySSL="true" trustStorePath="/tmp/trustStore.jks" />
  <vault-properties-provider:tls keyStorePath="/tmp/keystore.jks" keyStorePassword="***" />
</vault-properties-provider:config>
```

##### SSL Configuration with TLS Authentication (PEM)
Attributes:

*   kvVersion - version of the secrets engine to use (1 or 2)
*   verifySSL - true to validate certificate
*   pemFile - path to PEM file for vault server SSL
*   clientPemFile - An X.509 client certificate, for use with Vault's TLS Certificate auth backend
*   clientKeyPemFile - An RSA private key, for use with Vault's TLS Certificate auth backend

```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:basic kvVersion="2" />
  <vault-properties-provider:ssl verifySSL="true" pemFile="ssl/vault.pem" />
  <vault-properties-provider:tls clientPemFile="ssl/my_vault.pem" clientKeyPemFile="ssl/my_vault_key.pem" />
</vault-properties-provider:config>
```

##### IAM Configuration
Attributes:

*   iamAwsAuthMount - the Vault mount for AWS authentication
*   iamVaultRole - the Vault role to login as
*   iamUrl - Most likely https://sts.amazonaws.com/
*   iamReqBody - The IAM request body, most likely: Action=GetCallerIdentity&Version=2011-06-15
*   iamReqHeaders - IAM request headers

```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:iam iamAwsAuthMount="aws" iamVaultRole="test-role" iamUrl="https://sts.amazonaws.com/" iamReqBody="Action=GetCallerIdentity&Version=2011-06-15" iamReqHeaders="" />
</vault-properties-provider:config>
```

##### EC2 Configuration with Instance Metadata Authentication
Attributes:

*   ec2AwsAuthMount - the Vault mount for AWS authentication
*   ec2VaultRole - the Vault role to login as
*   useInstanceMetadata - true to login with instance metadata (PKCS7 is looked up on the host) 

```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:ec2 ec2AwsAuthMount="aws" ec2VaultRole="test-role" useInstanceMetadata="true" />
</vault-properties-provider:config>
```

##### EC2 Configuration with PKCS7 Authentication
Attributes:

*   ec2AwsAuthMount - the Vault mount for AWS authentication
*   ec2VaultRole - the Vault role to login as
*   pkcs7 - PKCS7 value with all \n characters removed
*   useInstanceMetadata - false to login with pkcs7 property

```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:ec2 ec2AwsAuthMount="aws" ec2VaultRole="test-role" useInstanceMetadata="false" pkcs7="MIICiTCCAfICCQD6m7oRw0uXOjANBgkqhkiG9w0BAQUFADCBiDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAwDgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDASBgNVBAsTC0lBTSBDb25zb2xlMRIwEAYDVQQDEwlUZXN0Q2lsYWMxHzAdBgkqhkiG9w0BCQEWEG5vb25lQGFtYXpvbi5jb20wHhcNMTEwNDI1MjA0NTIxWhcNMTIwNDI0MjA0NTIxWjCBiDELMAkGA1UEBhMCVVMxCzAJBgNVBAgTAldBMRAwDgYDVQQHEwdTZWF0dGxlMQ8wDQYDVQQKEwZBbWF6b24xFDASBgNVBAsTC0lBTSBDb25zb2xlMRIwEAYDVQQDEwlUZXN0Q2lsYWMxHzAdBgkqhkiG9w0BCQEWEG5vb25lQGFtYXpvbi5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAMaK0dn+a4GmWIWJ21uUSfwfEvySWtC2XADZ4nB+BLYgVIk60CpiwsZ3G93vUEIO3IyNoH/f0wYK8m9TrDHudUZg3qX4waLG5M43q7Wgc/MbQITxOUSQv7c7ugFFDzQGBzZswY6786m86gpEIbb3OhjZnzcvQAaRHhdlQWIMm2nrAgMBAAEwDQYJKoZIhvcNAQEFBQADgYEAtCu4nUhVVxYUntneD9+h8Mg9q6q+auNKyExzyLwaxlAoo7TJHidbtS4J5iNmZgXL0FkbFFBjvSfpJIlJ00zbhNYS5f6GuoEDmFJl0ZxBHjJnyp378OD8uTs7fLvjx79LjSTbNYiytVbZPQUQ5Yaxu2jXnimvw3rrszlaEXAMPLE"/>
</vault-properties-provider:config>
```

##### EC2 Configuration with Identity Document Authentication
Attributes:

*   ec2AwsAuthMount - the Vault mount for AWS authentication
*   ec2VaultRole - the Vault role to login as
*   identityDoc - Base64-encoded EC2 instance identity document
*   identityDocSignature - Base64-encoded SHA256 RSA signature of the instance identity document
*   useInstanceMetadata - false to login with Identity Document Authentication

```xml
<vault-properties-provider:config name="config" vaultUrl="http://localhost:8200">
  <vault-properties-provider:ec2 ec2AwsAuthMount="aws" ec2VaultRole="test-role" useInstanceMetadata="false" identityDoc="eyAiZGV2cGF5UHJvZHVjdENvZGVzIiA6IG51bGwsICJtYXJrZXRwbGFjZVByb2R1Y3RDb2RlcyIgOiBbICIxYWJjMmRlZmdoaWprbG0zbm9wcXJzNHR1IiBdLCAiYXZhaWxhYmlsaXR5Wm9uZSIgOiAidXMtd2VzdC0yYiIsICJwcml2YXRlSXAiIDogIjEwLjE1OC4xMTIuODQiLCAidmVyc2lvbiIgOiAiMjAxNy0wOS0zMCIsICJpbnN0YW5jZUlkIiA6ICJpLTEyMzQ1Njc4OTBhYmNkZWYwIiwgImJpbGxpbmdQcm9kdWN0cyIgOiBudWxsLCAiaW5zdGFuY2VUeXBlIiA6ICJ0Mi5taWNybyIsICJhY2NvdW50SWQiIDogIjEyMzQ1Njc4OTAxMiIsICJpbWFnZUlkIiA6ICJhbWktNWZiOGM4MzUiLCAicGVuZGluZ1RpbWUiIDogIjIwMTYtMTEtMTlUMTY6MzI6MTFaIiwgImFyY2hpdGVjdHVyZSIgOiAieDg2XzY0IiwgImtlcm5lbElkIiA6IG51bGwsICJyYW1kaXNrSWQiIDogbnVsbCwgInJlZ2lvbiIgOiAidXMtd2VzdC0yIn0=" identityDocSignature="dExamplesjNQhhJan7pORLpLSr7lJEF4V2DhKGlyoYVBoUYrY9njyBCmhEayaGrhtS/AWY+LPxlVSQURF5n0gwPNCuO6ICT0fNrm5IH7w9ydyaexamplejJw8XvWPxbuRkcN0TAA1p4RtCAqm4ms=x2oALjWSCBExample="/>
</vault-properties-provider:config>
```

### Referencing Values
To reference a value, use this format:

```
${vault::<secret_engine>/<path_to_secret>.<key_name>}
```

By default, version 2 of the KV engine is used.

## Example
To retrieve a value from the Key/Value secrets engine exposed as "secret", stored at "test/mule-sample", with a key of "name", use this format:

```
${vault::secret/test/mule-sample.name}
```

### Publishing to a Private Exchange

To publish to a private exchange, some updates are necessary in the `pom.xml` file and your Maven `settings.xml`.

Update the `groupId` to the organization ID used by your organization on the Anypoint platform.

In addition, update the `url` in the `distributionManagement` section of the pom to the following, replacing `${orgId}` with your Organization ID:
```
https://maven.anypoint.mulesoft.com/api/v1/organizations/${orgID}/maven
```

Add a `server` for the exchange repository in your Maven `settings.xml` file with the username and password to use for AnyPoint Exchange. 

After it is published in the exchange, the dependency in a project would change to look like this:

```xml
<dependency>
    <groupId>${orgId}</groupId>
    <artifactId>vault-connector</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <classifier>mule-plugin</classifier>
</dependency>
```