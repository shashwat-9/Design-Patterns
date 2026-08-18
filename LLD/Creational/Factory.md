 Factory is a creational pattern, and is used to decouple the creation of an object from the class to a different class.
 There are three types of Factory Pattern:

 1. Simple Factory
 In the simple factory method, we have a class where based on input, it returns an object of a different class.
 ```java
   class CarFactory {
       public static Car getCar(String type) {
           switch (type) {
               case "Sedan":
                  return new Sedan();
               case "SUV":
                   return new SUV();
               default:
                   throw new IllegalArgumentException("Invalid Car Type");
           }
       }
   }
 ```
 This is a simple factory, and it manages all the complexity of creation of an object in one place. Like DI, configs etc
 in the simple factory, instead of the class requiring the object.

 The cons of this method is that with addition of a new product type, there will be changes required in the Simple factory,
 and therefore violates OCP(Open-Closed Principle).

 2. Factory Method
 Factory Method creates different factories for different types of products.
 It creates an abstarct class or interface for the factory, and then creates different concrete factories from these abstractions.
 The client based on the type of product required, passes the corresponding factory to the business logic, and business class uses the
 abstract class/interface methods to create the product.

 This way adding a new product don't require changing the business logic. Ofcourse, the client will have to change the code
 to instantiate the correct factory.

 ```java
 abstract class CarFactory {

 }

 class SedanFactory extends CarFactory {

 }

 class SUVFactory extends CarFactory {}

 interface Car {
 }

 class Sedan implements Car {
 }

 class SUV implements Car {}

 class Client {
   if (type.equals("Sedan")) {
       businessLogic = new BusinessLogic(new SedanFactory());
   }else if (type.equals("SUV")) {
       businessLogic = new BusinessLogic(new SUVFactory());
   } else {
       throw new IllegalArgumentException("Invalid Car Type");
   }

   businessLogic.doSomething();
 }
 ```

 A different factory for each product, that is passed to the business logic based on the requirement/input.

 3. Abstract Factory
 Abstract Factory is a family of related product, it's like a factory of factories.
 Example:
 Let's suppose we have interfaces like Buttons, seekbars, TextBoxes etc.
 Each OS can have its own set of buttons, seekbars, textboxes etc.
 So, We implement the UI components for each OS, and create their own(windows/mac/linux) factories.
 Now, for the client to choose the right factory based on the OS, we create an abstract factory, where based on input, the required factory is returned.
