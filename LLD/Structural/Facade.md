#### Facade Pattern

 - Facade is a structural design pattern that provides a simplified interface to a library, a framework, or any other complex set of classes.
 - It is used in reducing complexity visible to the client.
 - When there are too many dependencies and complicated sequence of calls, then Facade acts as a point where all the complexity is managed.
 - Example is, let's suppose we have to place an order, then it could include steps like checking the inventory, acquiring a lock on the product,
doing payment, generating invoice, and calling the shipping module. If we code all of it in the business class, it will 
look messy and ugly; rather we create a Facade that manages all the complexity of creation and calling sequence.
 - If tomorrow some steps, say in payment module, changes then rather changing the business class we simply update the Facade.

Advantages:
1. Simplifies client(business) code, and replaces it with just a call to the Facade method.
2. Reduces Coupling
3. Centralizes workflow into the Facade Class.
4. Easier Maintenance -> Any subsystem change will require change in the Facade only.
5. Easier Testing
 
 - It should not contain business logic unrelated to orchestration. A good facade delegates work and coordinates calls 
rather than implementing the underlying business rules itself.

