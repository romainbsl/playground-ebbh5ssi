## What is Ktor ?

**Ktor** is a Kotlin framework that allow developers to write asynchronous clients and servers applications, in Kotlin. 
While it's not entirely compatible, Ktor is targeting multiple platforms as JVM, JavaScript, Android and iOS. 

> JetBrains is working on making other platforms, like MacOS, Linux and Windows compatible.   

For this overview, we will focus on the server side of **Ktor**. While **Ktor** is not fully compatible with Kotlin Native, 
we'll work on the JVM aspect. It can be deployed with some servlet containers (Tomcat / Jetty) or through Docker. 

##e HowTo declare a simple App

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

> (1): Declare the main module of the app 
> (2): Define the server with its Engine (e.g. Netty) and its listening port (8080) 
> (3): Declare a routing feature that allow the app to handle HTTP requests
> (4): Define a path that will make the url http://127.0.0.1:8080/ callable
> (5): Implement of the Pipeline that will treat the HTTP call on http://127.0.0.1:8080/. This will send the text "Hello World!"
> (6): Boot up the server
