oAuth2-sample
=============

oAuth2 sample: auth-server, resource server and client.

Here you can see 3 WARs: the authentication server, a resource server, and a client app.

Works over spring-security-oauth version M6 (milestone-6).
later on i will upgrade to a release-candidate.



Deploy all 3 WARs on a servlet container, e.g. tomcat.

Browse http://localhost:8080/oauth2-client/hello

The client needs a login by itsealf : admin/admin (future release will avoid this thing).

Then it will try to call the resource-server url http://localhost:8080/oauth2-resource-server/welcome

This will redirect to oauth2.0 authentication server.

Login to authentication-server, 
- currently it is from mem: demo@watchdox.com/demo
in future releases it will read from a DB.

Then the client should access the resource server using the access-token, and print a message.


common-crypto
=============
oAuth identity-provider and the authentication-flows JAR use cryptography in order to encrypt the data:
- oAuth encrypts the access-token (and the user's password)
- authentication-flows encrypts the user's password, and the links that are sent to the user's email, upon 
user's registration and "forget password" flows.
There is a utility JAR, called "common-crypto" that makes life easier. You can find it also in this project,
and also it is available in Maven repository:

```xml
<dependency>
  <groupId>com.ohadr</groupId>
  <artifactId>common-crypto</artifactId>
  <version>1.1.3</version>
</dependency>
```

Note the version - make sure you use the latest.

auth-common
=============
common code for authentication.  You can find it also in this project,
and also it is available in Maven repository:

```xml
<dependency>
  <groupId>com.ohadr</groupId>
  <artifactId>auth-common</artifactId>
  <version>1.1.3</version>
</dependency>
```

Note the version - make sure you use the latest.

KeyStore things to know:
========================
1. a keystore shall be created, both for SSL and for signing the tokens.
2. its alias and password should be updated in the prop file as well as in the tomcat's server.xml
3. algorithm should be DSA (because in the access-token signature my code expects it to be "SHA1withDSA"
4. if you want to work with "localhost", you should make the name "localhost": 
5. http://stackoverflow.com/questions/6908948/java-sun-security-provider-certpath-suncertpathbuilderexception-unable-to-find/12146838#12146838

creating a token using Java's keytool:
keytool.exe -genkeypair -alias <alias> -keypass <key-password> -keyalg DSA -keystore <file-name> -storepass <ks-password> -storetype JCEKS -v


Java Encryption:
================
Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding");  
SecretKeySpec secretKey = new SecretKeySpec(key, "AES");
cipher.init(Cipher.ENCRYPT_MODE, secretKey);
String encryptedString = Base64.encodeBase64String(cipher.doFinal(strToEncrypt.getBytes()));
return encryptedString;

http://techie-experience.blogspot.co.il/2012/10/encryption-and-decryption-using-aes.html
http://docs.oracle.com/javase/7/docs/api/javax/crypto/Cipher.html#init(int, java.security.Key)


HTML forms:
onSubmit vs action


Why "Secret Question" mechanism is a Bad Thing
-------------------------
The logic of "Secret Question" escapes me. Since the dawn of computer security we have been telling people, "DON'T make a password that is information about yourself that a hacker could discover or guess, like the name of your high school, or your favorite color. A hacker might be able to look up the name of your high school, or even if they don't know you or know anything about you, if you still live near where you went to school they might get it by tryinging local schools until they hit it. There are a small number of likely favorite colors so a hacker could guess that. Etc. Instead, a password should be a meaningless combination of letters, digits, and punctuation." But now we also tell them, "But! If you have a difficult time remembering that meaningless combination of letters, digits, and punctuation, no problem! Take some information about yourself that you can easily remember -- like the name of your high school, or your favorite color -- and you can use that as the answer to a 'security question', that is, as an alternative password."

Indeed, security questions make it even easier for the hacker than if you just chose a bad password to begin with. At least if you just used a piece of personal information for your password, a hacker wouldn't necessarily know what piece of personal information you used. Did you use the name of your dog? Your birth date? Your favorite ice cream flavor? He'd have to try all of them. But with security questions, we tell the hacker exactly what piece of personal information you used as a password!

Instead of using security questions, why don't we just say, "In case you forget your password, it is displayed on the bottom of the screen. If you're trying to hack in to someone else's account, you are absolutely forbidden from scrolling down." It would be only slightly less secure.
[{source]](http://stackoverflow.com/questions/2734367/implement-password-recovery-best-practice)

[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/OhadR/oauth2-sample/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

