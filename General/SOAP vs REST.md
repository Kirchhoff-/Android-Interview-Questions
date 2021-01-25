# SOAP vs REST
**SOAP** (formerly an acronym for **Simple Object Access Protocol**) is a messaging protocol specification for exchanging structured information in the implementation of web services in computer networks. Its purpose is to provide extensibility, neutrality, verbosity and independence. It uses XML Information Set for its message format, and relies on application layer protocols, most often Hypertext Transfer Protocol (HTTP), although some legacy systems communicate over Simple Mail Transfer Protocol (SMTP), for message negotiation and transmission.

**REST**, or **Representational State Transfer**, is an architectural style for providing standards between computer systems on the web, making it easier for systems to communicate with each other. REST-compliant systems, often called RESTful systems/applications.

SOAP provides the following advantages when compared to REST:
- Language, platform, and transport independent (REST requires use of HTTP);
- Works well in distributed enterprise environments (REST assumes direct point-to-point communication);
- Standardized;
- Provides significant pre-build extensibility in the form of the WS* standards;
- Built-in error handling;
- Automation when used with certain language products.

REST is easier to use for the most part and is more flexible. It has the following advantages over SOAP:
- No expensive tools require to interact with the web service;
- Smaller learning curve;
- Efficient (SOAP uses XML for all messages, REST can use smaller message formats);
- Fast (no extensive processing required);
- Closer to other web technologies in design philosophy.

[Below are the main differences between SOAP and REST](https://www.guru99.com/comparison-between-web-services.html)
| SOAP | REST |
|---|---|
| SOAP is a protocol. SOAP was designed with a specification. It includes a WSDL file which has the required information on what the web service does in addition to the location of the web service.  | REST is an Architectural style in which a web service can only be treated as a RESTful service if it follows the constraints of being: Client Server, Stateless, Cacheable, Layered System, Uniform Interface  |
| SOAP cannot make use of REST since SOAP is a protocol and REST is an architectural pattern.  | REST can make use of SOAP as the underlying protocol for web services, because in the end it is just an architectural pattern.  |
| SOAP uses service interfaces to expose its functionality to client applications. In SOAP, the WSDL file provides the client with the necessary information which can be used to understand what services the web service can offer.  | REST use Uniform Service locators to access to the components on the hardware device. For example, if there is an object which represents the data of an employee hosted on a URL as http://demo.guru99  |
| SOAP requires more bandwidth for its usage. Since SOAP Messages contain a lot of information inside of it, the amount of data transfer using SOAP is generally a lot.  | REST does not need much bandwidth when requests are sent to the server. REST messages mostly just consist of JSON messages.  |
| SOAP can only work with XML format. As seen from SOAP messages, all data passed is in XML format.  | REST permits different data format such as Plain text, HTML, XML, JSON, etc. But the most preferred format for transferring data is JSON.  |

# Links
https://en.wikipedia.org/wiki/Representational_state_transfer  
https://en.wikipedia.org/wiki/SOAP  
https://www.guru99.com/comparison-between-web-services.html  
https://smartbear.com/blog/test-and-monitor/soap-vs-rest-whats-the-difference/  

# Next questions
[What is REST?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/soap_vs_rest/General/What%20is%20REST.md)  
[What do you know about SOAP?](https://github.com/Kirchhoff-/Android-Interview-Questions/blob/soap_vs_rest/General/What%20do%20you%20know%20about%20SOAP.md)

# Further reading
[SOAP vs REST vs JSON - a 2020 comparison](https://raygun.com/blog/soap-vs-rest-vs-json/)
