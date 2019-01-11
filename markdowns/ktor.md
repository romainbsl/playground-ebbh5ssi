# What is Ktor ?

**Ktor** is a Kotlin framework that allow developers to write asynchronous clients and servers applications, in Kotlin. 
While it's not entirely compatible, Ktor is targeting multiple platforms as JVM, JavaScript, Android and iOS. 

> JetBrains is working on making other platforms, like MacOS, Linux and Windows compatible.   

For this overview, we will focus on the server side of **Ktor**. While **Ktor** is not fully compatible with Kotlin Native, 
we'll work on the JVM aspect. It can be deployed with some servlet containers (Tomcat / Jetty) or through Docker. 

# HowTo declare a simple App

```kotlin
fun main(args: Array<String>) { // (1)
    val server = embeddedServer(Netty, port = 8080) { // (2)
        routing { // (3)
            get("/") { // (4)
                call.respondText("Hello World!") // (5)
            }
        }
    }
    server.start(wait = true) // (6)
}
```

- (1) : Declare the main module of the app 
- (2) : Define the server with its Engine (e.g. Netty) and its listening port (8080) 
- (3) : Declare a routing feature that allow the app to handle HTTP requests
- (4) : Define a path that will make the url http://127.0.0.1:8080/ callable
- (5) : Implement the Pipeline that will treat the HTTP call on http://127.0.0.1:8080/. This will send the text "Hello World!"
- (6) : Boot up the server

# Under the hood

In this part we will describe how **Ktor** acts to run an asynchronous application server.

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

	PipelineContext generally contains a `ApplicationCall` that is composed of the rquest information (Request / Response / Environment, Attributes). When using routes, every node has its own ApplicationCallPipeline instance. That means that every route will execute its own Pipeline.

##  [Modules][]

**Ktor** modules are extension functions, that take for receiver the Application. They are the configuration of the Application.  
So, we can configure the server, with its Engine, its listening port, or even install specific features for each modules 
(routing, session, logging...).

> Here is an example of how to use modules

```kotlin
fun main(args: Array<String>) {
    val server = embeddedServer(Netty, port = 8080) {
        landingModule()
        loginModule()
    }
    server.start(wait = true)
}

/**
 * Module that handle the home page of the app
 */
fun Application.landingModule() {
    routing {
        get("/") {
            call.respondText("Hello Visitor!")
        }
    }
}

/**
 * Module that handle the login of the app
 */
fun Application.loginModule() {
    routing {
        get("/login") {
            call.respondText("Please Login!")
        }
    }
}
```

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