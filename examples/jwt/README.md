# jsr375-extensions JWT example 


## Setup

* Add the dependency

You need to add the Maven dependency to include the code for the JWT extension for jsr375

````
    <dependency>
        <groupId>be.atbash.ee.security.jsr375</groupId>
        <artifactId>soteria-jwt</artifactId>
        <version>0.9</version>
    </dependency>
````

* Create the file with the RSA keys

Have a look at the class **be.atbash.ee.security.soteria.jwt.cli.RSAKeysGenerator** which contains the code to generate 3 keys. It outputs them in a format which is readable by _com.nimbusds.jose.jwk.JWKSet_ contained in the dependency _nimbus-jose-jwt_.

I have put them in the classpath.

* Implement the **be.atbash.ee.security.soteria.jwt.JWTTokenHandler**.

It is responsible for loading the RSA keys and verifying if the JWT is valid (Signing and time restriction for example)
The implementation is also responsible for retrieving the _user name_ and the _groups_ from the JWT payload and supply them to the system. They are used to construct the required principal and role info.

* Test the endpoint

The JAX-RS endpoints are now protected and a valid JWT token needs to be supplied with the _Authorization_ header. Something like this

````
  Authorization:Bearer eyJraWQiOiI3NGYyNDYxOS1kNzIwLTQ1YWQtOTk2Yy0zNWNkNzNiNDdmZmQiLCJ0eXAiOiJKV1QiLCJhbGciOiJSUzUxMiJ9.eyJzdWIiOiJTb3RlcmlhIFJJIiwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbInVzZXIiLCJtYW5hZ2VyIl19LCJleHAiOjE1MTAzNDgxMTMsImlhdCI6MTUxMDM0ODA4M30.Bk37fPLgymuLZfLq_hdxt94PDRwNAkPkoajegaLLMrsuCCeKEb5DRXcpT9kUyrwEzSFamg_19Y7IT-0utDw4yd_rEzq7lIG8UFc8qnoYi9692Es_sqzwu64x0dM1ODOAHYEPFhIXPo2-nxquYyayMJI5PN4WlTPrRgoFCkY6saxJGzAGlfQIqH3ozaMvEJn1GK3uj1_zglv2HHK1t8lliazRkmRI-p1k9A_HCnnpChb9czpUP_wXRN4HxbADldS5sM7lgsFeLZDB4oewAh9sTXiseH7HnbPfmQKF18vb9Vejc9XzIGf0CrSOf6yVTyYDChYhI1eB5z6Wv33ofFgbPg
````

Such tokens can be generated by the **be.atbash.ee.security.soteria.jwt.cli.TokenGenerator** be aware that these tokens in the current implementation of the Generator are only 30 seconds valid.

The endpoint can be found at _http://localhost:8080/soteria-jws/data/hello_ or equivalent.

## Issues

Seems that I have issues with JAX-RS endpoints on my GlassFish 5.0 installation.

Therefore I tested with WildFly 10.1 and have created the maven profile _EE7_ for this. And I had to specify the JVM flag _-noverify_ because the extension is compiled against the Java EE 8 artifact.
 