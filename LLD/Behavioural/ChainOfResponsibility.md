### Chain Of Responsibility Principle

Chain of Responsibility Principle is a behavioural design pattern that passes a request along a chain of handlers, allowing
one or more handlers to process it without coupling the sender to the specific receiver.

 - The Client code doesn't need to be aware of all possible handlers, and only can pass it to the chain of handlers,
and the right one will carry out the job.

```java

    abstract class Handler {
        protected Handler next;
        
        public Handler setNext(Handler handler) {
            this.next = handler;
            return handler;
        }
        
        public abstract boolean handle(Request request);
    }
    
    class AuthenticationHandler extends Handler {
    
        /**/
    }

    class AuthorizationHandler extends Handler {

        /**/
    }
    
    //Chain Building
    /*
     * Handler auth = new AuthenticationHandler();
     * Handler authorization = new AuthorizationHandler();
     * Handler rateLimit = new RateLimitHandler();
     * 
     * auth.setNext(authorization).setNext(rateLimit);
     * */
```

#### Two variants of CoR

##### Variant A - Single Handler process the request
 - The request goes through the chain until it doesn't find the handler which can process the request and then returns.
 - Exception Handling/ Approval Hierarchy

##### Variant B - Multiple Handlers can proces the request
 - The request goes through the entire chain and gets processed wherever applicable.
 - Here every Handler can perform some operation or can chose simply to pass to the next one.

###### Pipeline vs CoR
 - In a pipeline, each stage processes the task/request, but in CoR a handler may choose to pass the request without
even processing.

##### Constructor Injection vs Setter Injection
- In setter way of injecting of the next handler, chains are dynamic and are easy to configure, but a downside is that chain
  can temporarily exist in an invalid state.
- In Constructor way of injecting the next handler, the chain is immutable.

##### Properties of CoR
 - Handlers are loosely coupled, Handler knows only `next` handler whose concrete implementation could be anything.
 - Chain can be modified dynamically via the `setNext` method.
 - Client(Sender) doesn't know which concrete handlers will handle the request, it simply pass request to the head of the chain.
 - Follows `SRP` of `SOLID` as for each processor a separate class is created.
 - Follows `OCP` of `SOLID` as adding a new processor doesn't require changing the existing code, but simply adding a new class.
 - Beware of creating circular chains.
 - In case of where no handler can handle the request, we can silently ignore the case or throw some Exceptions.
 - Order of handler can define the behaviour of the system.

