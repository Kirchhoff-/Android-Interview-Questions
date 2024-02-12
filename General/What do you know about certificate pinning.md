# Certificate pinning 
Certificate pinning is a security mechanism used in the context of authenticating client-server connections, particularly in the context of secure communication over HTTPS (Hypertext Transfer Protocol Secure) or other TLS (Transport Layer Security) protocols. Its primary purpose is to enhance the security of the connection by mitigating the risk of man-in-the-middle (MITM) attacks and ensuring that the client only communicates with a trusted server.<sup>[1](https://www.ssl.com/blogs/what-is-certificate-pinning/#:~:text=Certificate%20pinning%20is,a%20trusted%20server.)</sup>

## [What Is Man-in-the-Middle Attack?](https://www.sapphire.net/security/certificate-pinning/#:~:text=What%20Is%20Man%2Din%2Dthe%2DMiddle%20Attack%3F)
Take the case of a mobile application whereby the user calls what they think is a safe server. The man-in-the-middle intercepts the message and relays it to a different server. Plus, they receive the server’s public key and pass on its distinct public key to the caller. The hacker can communicate with both the client and the server, but the client and the server cannot communicate with each other. Since they can communicate with the two, the hacker can intercept any data being passed between the client and the server and modify it back and forth.

## [How Certificate Pinning Works](https://www.sapphire.net/security/certificate-pinning/#:~:text=How%20Certificate%20Pinning%20Works)
This is a straightforward process whereby the host or service’s certificate is associated with the pre-designated public key that obeys [x.509 cryptography standards](https://www.techopedia.com/definition/4172/x509). Therefore, if an API or client wants to make a secure connection, the server already has the pinned certificate hence a secure connection is established.

Before a client and server connect and exchange data, a figurative handshake ensures that the client communicates with the right server. There are up to five steps during the figurative handshake. These include:
- The client starts a handshake with the server and specifies a Transport Layer Security version (TLS);
- The server responds with the public key and certificate;
- The client then verifies the server’s public key or certificate and then sends back a shared key based on the server’s public key;
- The server now confirms receipt of the shared key;
- Data starts flowing between the client and server, and the data shared is encrypted using the new shared public key hash.

## [Types of SSL Pinning](https://www.indusface.com/learning/what-is-ssl-pinning-a-quick-walk-through/#:~:text=Types%20of%20SSL%20Pinning)
**Certificate SSL pinning**. SSL certificate pinnning technique involves hard-coding the SSL certificate of a server into the client’s application. The client then compares the server’s certificate with the hard-coded certificate and only allows communication if they match. This ensures the client only communicates with the intended server, not an imposter server with a fake certificate.

This security measure pins the identity of trustworthy certificates on apps and blocks unknown documents from suspicious servers.

With this technique, you can pin an SSL certificate host – a list of trustful certificates to your application during development and compare the server certificates against the list during runtime.

**Public key pinning**. Public key pinning involves hard-coding the public key of the server’s SSL certificate instead of the entire certificate. The client then checks that the server presents a certificate containing the same public key that was hard-coded into the client’s application. This method ensures that even if the server’s certificate changes, the client still trusts the server as long as the public key remains the same.

Both types of pinning have their own advantages and disadvantages, and their suitability depends on the specific use case. 

Certificate pinning is easier to implement but may require frequent updates if the server’s certificate changes frequently. Public key pinning provides more flexibility in certificate management but requires more technical expertise to implement.

## [Advantages of Certificate Pinning](https://www.indusface.com/learning/what-is-ssl-pinning-a-quick-walk-through/#:~:text=Advantages%20of%20SSL%20Pinning)
- **Enhances Security**. One of the most significant benefits of SSL pinning is that it enhances the security of network communications by adding an extra layer of protection against man-in-the-middle attacks. By requiring a pre-configured certificate or public key, SSL pinning ensures that the client device communicates only with the intended server and not with an imposter;
- **Mitigates Certificate-Based Attacks**. Certificate-based attacks, where an attacker can compromise a certificate authority (CA) or issue fake certificates, can be prevented using SSL pinning. By hard-coding the certificate or public key of the intended server, SSL pinning prevents attackers from presenting their own fake certificates and decrypting encrypted traffic;
- **Improves Performance**. Another advantage of SSL pinning is that it can improve the performance of applications. Since SSL pinning eliminates the need for the client device to validate the server’s SSL certificate with the trusted CAs, it can save processing time and reduce latency;
- **Ensures Trust**. By requiring that the client device verify the server’s identity through a trusted certificate or public key, SSL pinning ensures trust between the client and server.

## [What are the Disadvantages of Certificate Pinning?](https://www.ssl.com/blogs/what-is-certificate-pinning/#:~:text=What%20are%20the%20Disadvantages%20of%20Certificate%20Pinning%3F)
Certificate pinning is not without its challenges. While it can be a tool in preventing certain types of cyberattacks, it comes with its own set of disadvantages:
- **Maintenance Complexity**. Certificate pinning necessitates that clients maintain a list of trusted certificates or public keys. However, this list must be continuously updated to reflect changes in server certificates. As certificates have expiration dates and are regularly renewed, the process of keeping pinned certificates up-to-date can be cumbersome, prone to human error, and may lead to disruptions in service;
- **Reduced Flexibility**. In dynamic and cloud-based environments where server certificates change frequently (e.g., content delivery networks or microservices), certificate pinning can pose operational challenges. The inflexibility of pinned certificates can hinder smooth transitions during server updates and complicate certificate management;
- **Risk of Breaking Connections**. Pinning a certificate to an application introduces the risk of connectivity loss if the pinned certificate becomes compromised or expires. This could result in service interruptions for users until the client application is updated with the new pinned certificate;
- **Lack of Scalability**. Certificate pinning can be impractical for large-scale applications or services that need to communicate with numerous servers, each with its own certificate. Managing a multitude of pinned certificates becomes unwieldy and may undermine the benefits of certificate pinning itself.

## [How do I Pin Certs in android app?](https://ivision.com/blog/what-is-certificate-pinning/#:~:text=mc4wp_form%20id%3D%E2%80%9D1512%E2%80%B3%5D-,Android,-Although%20many%20more)
Although many more ways exist for pinning certs in your Android app, the following three are most often used:
- **TrustManager**. A [`TrustManager`](https://developer.android.com/reference/javax/net/ssl/TrustManager) instance decides whether the app should accept the credentials from the server. This involves multiple changes to your application and can easily be implemented incorrectly, leading to bugs;
- **OkHttp and CertificatePinner**. Using [OkHttp](https://square.github.io/okhttp/) for your server calls provides a simple mechanism for implementing certificate pinning. The cert fingerprint should be injected into your application at build time. Then, once you have your CertificatePinner ready, add it to your HTTP client, and you’re ready to go;
- **Network Security Configuration (NSC)**. With [NSC](https://developer.android.com/training/articles/security-config), you must add certificate pinning into your configuration using XML files that contain fingerprints.

# Links
[What Is Certificate Pinning?](https://www.ssl.com/blogs/what-is-certificate-pinning/)

[Certificate Pinning – What it Is, its Benefits, and Drawbacks](https://www.sapphire.net/security/certificate-pinning/)

[What is SSL Pinning? – A Quick Walk Through](https://www.indusface.com/learning/what-is-ssl-pinning-a-quick-walk-through/)

[What is Certificate Pinning and What Does it Mean?](https://ivision.com/blog/what-is-certificate-pinning)

# Further reading
[3 Ways How To Implement Certificate Pinning on Android](https://www.netguru.com/blog/android-certificate-pinning)

[How to Securely Implement TLS Certificate Checking in Android Apps](https://www.guardsquare.com/blog/how-to-securely-implement-tls-certificate-checking-in-android-apps)

[SSL Pinning in Android](https://medium.com/@anandgaur22/ssl-pinning-in-android-14851dc41703)

[Android SSL Certificate pinning with retrofit](https://medium.com/@dhananjay_yaa/android-ssl-certificate-pinning-with-retrofit-39fb69e61a69)
