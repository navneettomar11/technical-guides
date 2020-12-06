# What is an API?
API stands for Application Programming Interface. An API is a software intermediary that allows two applications to talk each other. In other words, an API is the messenger that delivers your request to the provider that youre requesting it from and then delivers the response back to you.

REST or Restful API design(Representation State Transfer) is designed to take advantage of existing protocols. While REST can be used over nearby any protocol, it usually takes advantage of HTTP when used for Web APIs.

Rest is acronym for Respresentational State Transfer. It is architectural style for distributed hypermedia systeam and was first presented by Roy Fielding in 2000 in his famous dissertation.

> Hypermedia, an extension of the term hypertext, is a nonlinear medium of information that includes graphics, audio, video, plain text and hyperlinks. The designation constrasts with the border term multimedia, which may include non-interctive linear presentation as well as hypermedia.

# Guidung Principles of REST
1. **Client-Server** - By separating the user interface concerns, we improve the portability of the user interface across multiple platforms and improve scalability by simplifying the server components.
2. **Stateless** - Each request from client to server must contain all of the information necessary to understand the request, and cannot take advantage of any stored context on the server. Session state is therefore kept entirely on the client.
3. **Cacheable** - Cache constraints required that the data within a response to a request be implicitly or explicitly labeled as cacheable or non-cacheable. If a response is cacheable, then a client cache is given the right to reuse that response data for later, equivalent requests.
4. **Uniform interface** - By applying the software engineering principle of generality to the component interface, the overall system architecture is simplified and the visibility of interactions is improved. In order to obtain a uniform interface, multiple architectural constraints are needed to guide the behavior of components. REST is defined by four interface constraints: identification of resources; manipulation of resources through representations; self descriptive messages; and hypermedia as the engine of application state.
5. **Layered system** - The layered system style allows an architecture to be composed of hierarchical layers by constraining componet behavior such that each component cannot "see" beyond the immediate layer with which they are interacting.
6. **Code on demand(optional)** - REST allows client functionality to be extend by downloading and executing code in the form of applets or scripts. This simplifies client by reducing the number of features required to be pre-implemented.

# REST Architectural Constraints
REST stands for Representational State Transfer, a term coined by Roy

## Architectural Constraints
REST defines 6 architectural constraints which make any web service - a true RESTFul API.
## Uniform interface
 As the constraint name itself applies, you MUST decide APIs interface for resources inside the systen which are exposed to API consumers and follow religiously. A resource in the system should have only one logical URI, and that should provide a way to fetch related or additional data. It's always better to **synonymize a resource with a web page**.

 Any single resource should not be too large and contain each and everthing in its representation. Whenever relevant a resource should contains links (HATEOAS) pointing to relative URIs to fetch related information.

 Also, the resource representation across the system should follow specific guidelines such as naming conventions, link formats, or data format(XML or/and JSON).

 All resources should be accessible through a common approach such as HTTP GET and similarly modified using consistent approach.
 >> Once a dev


# References: 
[https://restfulapi.net/](https://restfulapi.net/)