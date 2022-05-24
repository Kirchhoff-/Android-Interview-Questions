# URL
A **Uniform Resource Locator (URL)**, colloquially termed a **web address**, is a reference to a web resource that specifies its location on a computer network and a mechanism for retrieving it. A URL is a specific type of *Uniform Resource Identifier (URI)*, although many people use the two terms interchangeably. URLs occur most commonly to reference web pages (http) but are also used for file transfer (ftp), email (mailto), database access (JDBC), and many other applications.

In order for computer networks and servers to “talk to one another,” computers rely on a language made up of numbers and letters called an IP address. Every device that connects to the internet has a unique IP address and looks something like this:

```
22.231.113.64 or 3ffe:1900:4545:3:200:f8ff:fe21:67cf
```

In order to navigate easily around the web, typing in a long IP address isn’t ideal, or realistic, to an online user. This is the reason why domain names were created – to hide IP addresses with something more memorable. You could consider the domain name as a “nickname” to the IP address.

A URL incorporates the domain name, along with other detailed information, to create a complete address (or “web address”) to direct a browser to a specific page online called a web page. In essence, it’s a set of directions and every web page has a unique one.

A URL has two main components:
- Protocol identifier: For the URL `http://example.com`, the protocol identifier is `http`;
- Resource name: For the URL `http://example.com`, the resource name is `example.com`.

Note that the protocol identifier and the resource name are separated by a colon and two forward slashes. The protocol identifier indicates the name of the protocol to be used to fetch the resource.

The resource name is the complete address to the resource. The format of the resource name depends entirely on the protocol used, but for many protocols, including HTTP, the resource name contains one or more of the following components:

- **Host Name**. The name of the machine on which the resource lives;
- **Filename**. The pathname to the file on the machine;
- **Port Number**. The port number to which to connect (typically optional);
- **Reference**. A reference to a named anchor within a resource that usually identifies a specific location within a file (typically optional).

# Links
[URL](https://en.wikipedia.org/wiki/URL)

[What is a URL?](https://www.verisign.com/en_US/website-presence/online/what-is-a-url/index.xhtml)

[What Is a URL?](https://docs.oracle.com/javase/tutorial/networking/urls/definition.html)

# Next Questions
[What do you know about URI?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20do%20you%20know%20about%20URI.md)

[What is the difference between HTTP and HTTPS?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/master/General/What%20is%20the%20difference%20between%20HTTP%20and%20HTTPS.md)
