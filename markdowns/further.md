In this part we will describe how *Ktor* acts to run an asynchronous application server.

> Reminder: For this overview, we will focus on the server side of *Ktor*. 

## Application Server

A *Ktor* server is a Server Engine (Tomcat, Jetty, Netty or Coroutine I/O) listening on a port. 
It is composed by modules, that are themselves composed of features (e.g. routing, sessions, logging). 

When a request (HTTP / WebSocket) step into the *Ktor* server its serialized into an `ApplicationCall` that contains 
the context (Request / Response / Environment / Attributes). Afterward, this `ApplicationCall` is transmitted and 
manipulated through a [Pipeline][].

##  Lifecycle

When a **Ktor** server starts, it waits for connections on a specific port. To handle connections and furthermore, 
**Ktor** works with [Pipelines][], they are everywhere and are what's make **Ktor** asynchronous.  
 
A [Pipeline][] is a asynchronous chain of code blocks or functions that run asynchronous computation, 
with the implementation of coroutines. A [Pipeline][] is configured by the modules and features 
loaded at the application startup. It's composed of phases and interceptors, that are executed in a specific and predefined order.
The application is a [Pipeline][] itself, that will handle incoming request through a specific path, established at the server loading. 

For example, in the case of an HTTP call, the application's [Pipeline][] (also called ApplicationCallPipeline), 
will execute multiple phases, like treating the request, or even send the HTTP response.

	PipelineContext generally contains a `ApplicationCall` that is composed of the rquest information (Request / Response / Environment, Attributes). 
	When using routes, every node has its own ApplicationCallPipeline instance. That means that every route will execute its own Pipeline.

##  [Modules][]

**Ktor** modules are extension functions, that take for receiver the Application. They are the configuration of the Application.  
So, we can configure the server, with its Engine, its listening port, or even install specific features for each modules 
(routing, session, logging...).

##  [Features][]

Features are singletons that extend the application capabilities, like:

- [Routing][]:  Defining the HTTP routes that will handle incoming requests.
- [Session][]: Keeping a link, or information that identify the client/caller, giving the ability to handle request 
with a context. For example, limiting the data access for some specific users.
- [WebSockets][]: WebSockets support for **Ktor**, that goes beyond the 'one-shot' HTTP calls, and aims to keep the client 
and server linked, allowing the server to send notification to the client. 

[pipeline]: https://ktor.io/servers/lifecycle.html#pipelines
[modules]: https://ktor.io/servers/application.html#modules
[features]: https://ktor.io/servers/features.html
[routing]: https://ktor.io/servers/features/routing.html
[session]: https://ktor.io/servers/features/sessions.html 
[websockets]: https://ktor.io/servers/features/websockets.html