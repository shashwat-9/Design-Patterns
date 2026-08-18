 Builder pattern is a creational pattern used to create complex objects.
 It avoids monstrous and huge number of constructors

 Setters can be used to create object step-wise just like Builders, but there are problems:
 1. Object exists in incomplete state for some moment.
 2. Objects are mutable
 3. Not thread safe, multiple threads can change the setters.

 In this pattern, we create another class called Builder, which is used to build the object.
 Builder collect values, and then `build` method on a builder object creates the final object.

 ```java
     Computer computer
       = new Computer.Builder()
               .ram(16)
               .cpu(i5)
               .hardDisk(ssd)
               .build();
 ```

 Since every method setting values returns the builder object, it enables method chaining, also called fluent interface.



 There are primarily two interpretations of the builder pattern:
 1. GoF Builder
 2. Java Builder

 The below implementation is called Java style builder.
 ```java
   public class Computer {
       private final Ram ram;
       private final Cpu cpu;
       private final HardDisk ssd;

       private Computer(Computer.Builder builder) {
            this.cpu = builder.cpu;
            this.ram = builder.ram;
            this.ssd = builder.ssd;
       }

       public static class Builder {
           private CPU cpu;
           private Ram ram;
           private HardDisk ssd;

           public Builder setCpu(CPU cpu) {
               this.cpu = cpu;
               return this;
           }

           public Builder setRam(Ram ram) {
               this.ram = ram;
               return this;
           }

           public Builder setHardDisk(HardDisk ssd) {
               this.ssd = ssd;
               return this;
           }

           public Computer build() {
               return new Computer(this);
           }
       }
   }
 ```

 The fields in the Product class are final, so that they cannot be changed after the object is created.
 This final makes the object thread-safe and avoid accidental invalid state, as it is immutable.
 The build method can also be used to verify the validity of the properties that will be set in the product class.

 GoF Builder:
 1. Separates the construction of a complex object from its representation.

 The participants in the builder patterns are:
 1. Product - The class whose object is created by the builder.
 2. Builder - The class that creates the product.
 3. ConcreteBuilder - The class that implements the builder interface.
 4. Director - The class that creates the builder object.
 5. Client - The class that uses the director or builder object.
 The director only knows the order of construction that will be invoked on the builder(or concrete builder).
 Client can choose/change the value of the builder object, and then the director invokes the build method on the builder object.
 
```java

   class Client {
       ComputerBuilder builder = new HPCComputerBuilder();
       Director director = new Director();
       Computer computer = director.construct(builder);
   }

   class Director {
       public Computer construct(ComputerBuilder builder) {
           builder.buildCpu();
           builder.buildRam();
           builder.buildHardDisk();
           builder.buildOS();

           return builder.build();
       }
   }

   abstract class ComputerBuilder {

   }

   class HPCComputerBuilder extends ComputerBuilder {

   }

   class HPCComputer extends Computer {

   }

   class Computer {
   }
 ```
 The director could be using builder for any computer, but the order of construction will be same for all.

 To make certain values madatory for the product class, we can create builder constructors with those fields as mandatory.

```java
   //Mandatory Fields in Builder constructor:

   public class ComputerBuilder {
       private final CPU cpu;
       private final Ram ram;
       private final HardDisk ssd;
       private final OS os;
       private final NetworkCard networkCard;

       public ComputerBuilder(CPU cpu, Ram ram, HardDisk ssd) {
           this.cpu = cpu;
           this.ram = ram;
           this.ssd = ssd;
       }

       public ComputerBuilder setOS(OS os) {
           this.os = os;
           return this;
       }

       public ComputerBuilder setNetworkCard(NetworkCard networkCard) {
           this.networkCard = networkCard;
           return this;
       }

       public Computer build() {
           //
       }
   }
 ```

 Step Builder:
 If we use a common Builder class object in the return of each setter, then one can invoke setter methods of builder object in any order.
 Compiler would allow any order, but maybe business requirements dictate that the order should be fixed.

 In such cases we can use Step Builder, that is, create different interfaces for each step, and return the next Step builder object.
 This way, the order of construction can be fixed.

```java
 interface EngineStep {
    TransmissionStep engine(String engine);
 }
 interface TransmissionStep {
    BodyStep transmission(String transmission);
 }

 interface BodyStep {
     PaintStep body(String body);
 }

 interface PaintStep {
     BuildStep paint(String color);
 }

 interface BuildStep {
     Car build();
 }

 Car car = Car.builder()
             .engine("V8")
             .transmission("Automatic")
             .body("SUV")
             .paint("Black")
             .build();
 ```