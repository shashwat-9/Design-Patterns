#### Adapter pattern

 - This is a Structural pattern used for making third party SDKs, legacy systems, external APIs, etc. compatible.
 - To make the incompatible class compatible, we can wrap the incompatible class into compatible, by implementing the interface or extending the class.
 - Adding a new SDK only involves creating a new class, implementing the adapter and the client code passing this adapter to business logic.
 - Adapter converts the interface of one class into another interface expected by the client.
 - There are two Adapters: 
1. Object Adapter -> Uses composition, Adapter(PayPalAdapter) implementing common interface, PaymentProcessor, contains Adaptee(PayPalSDK).
Preferred in java.
2. Class Adapter -> Adapter(e.g. PayPalAdapter) extends Adaptee(e.g. PaypalSDK class) implements Common Interface (PaymentProcessor).
Rarely used in java.

Advantages:
1. Existing code untouched.
2. Reuse third-party SDKs. Very common.
3. Loose coupling. Client doesn't know the implementation.
4. Follows SRP, OCP and DIP

```java

interface PaymentProcessor {
    PaymentReponse pay(PaymentRequest paymentRequest);
}

class StripeAdapter implements PaymentProcessor {
    
    private StripeSDK stripeSDK;
    
    public StripeAdapter(StripeSdk sdk) {
        this.stripeSDK = sdk;
    }
    
    PaymentReponse pay(PaymentRequest paymentRequest) {
        //Translate PaymentRequest Information to Stripe args
        Striperesponse response = stripeSDK.makePayment();
        //Translate stripeResponse to PaymentResponse
        return paymentResponse;
    }
}

```

 - Apart from renaming the function name via interface, Adapter also works on translating the input and output.
 - E.g. Translating the PaymentRequest to ProviderRequest, and ProviderResponse to PaymentResponse, and ProviderException
to PaymentException and vice-versa.
 - Carries out data, unit conversion, logging, calculates metrics for the third party sdk, such as latency etc.
 - Maybe used to add retry logic as well.
 - If the adapter class is taking on all the responsibilities like retries, logging, metrics, circuit breaker, caching
, then the class will become huge, and in such cases combining with Decorator pattern will make code structure cleaner and manageable.

```
Checkout
↓
Logging Decorator
↓
Retry Decorator
↓
Metrics Decorator
↓
PaypalAdapter
↓
PaypalSDK
```

1. Purpose: Convert one interface into another expected by the client.
2. Best implementation in Java: Object Adapter using composition. 
3. What gets adapted: Both requests and responses, not just method names. 
4. Typical use cases: Third-party SDKs, legacy systems, and external APIs. 
5. Patterns used together: Adapter is commonly combined with Factory, Strategy, and Dependency Injection. 
6. Keep adapters focused: Avoid stuffing them with retries, logging, and metrics—delegate those concerns to decorators or other infrastructure components.
