In this part we will describe how *Ktor* acts to run an asynchronous application server.

> Reminder: For this overview, we will focus on the server side of *Ktor*. 

## Application Server

A *Ktor* server is a Server Engine (Tomcat, Jetty, Netty ou Coroutine I/O) listening on a port. 
It is composed by modules, that are themselves composed of features (e.g. routing, sessions, logging). 

When a request (HTTP / WebSocket) step into the *Ktor* server its serialized into an `ApplicationCall` that contains 
the context (Request / Response / Environment). 
Afterward, this `ApplicationCall` is transmitted and manipulated through a Pipeline.

##  Lifecycle

##  Modules

##  Features
