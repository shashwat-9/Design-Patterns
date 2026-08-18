#### Proxy Pattern

 - Proxy provides a placeholder or surrogate for another object to control access to it.
 - The client believes it is talking to the real object, but it is actually talking to a proxy object.
 - The proxy decides whether, when, and how to forward the request.
 - The proxy has the same interface as the real object. This allows the client to remain completely unaware that a proxy exists.

A proxy sits between Client and Real Object and intercepts every call.
It may delay creation, cache, authenticate, log, encrypt, forward remotely, retry, rate limit, before invoking the real object.

###### Participants
 - Common Interface
 - Real Class
 - Proxy Class

Client uses the interface and doesn't care if it's a Real or proxy class, it invokes the method in the interface.

There are various kinds of proxies:

| Type                  | Purpose                          |
| --------------------- | -------------------------------- |
| Virtual Proxy         | Lazy initialization              |
| Protection Proxy      | Access control                   |
| Remote Proxy          | Represents remote object         |
| Cache Proxy           | Cache results                    |
| Logging Proxy         | Logging                          |
| Smart Proxy           | Extra behavior before/after call |
| Synchronization Proxy | Thread safety                    |

###### Advantages
 - Lazy loading
 - Security
 - Caching
 - Logging
 - Retry
 - Monitoring
 - Remote communication abstraction
 - Open/Closed Principle (add behavior without modifying the real object)

###### Disadvantages
 - Extra level of indirection
 - Slight performance overhead
 - More classes
 - Can make debugging harder because the actual object is hidden behind a proxy
