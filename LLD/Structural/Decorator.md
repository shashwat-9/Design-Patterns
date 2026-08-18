#### Decorator Pattern
 - Decorator adds new behavior dynamically without modifying existing classes.
 - Decorator Pattern attaches additional responsibilities to an object dynamically by wrapping it inside another object 
that implements the same interface.
 - The problem is that if we don't add functionalities dynamically, then we would have to create alot of subclasses, and that
that might not be the ideal approach, as the numbers could explode hugely and also the order in which subclass inherit other
classes could also matter. The explosion in the number of classes could be due to various combinations of added functionalities with the base classes.
 - Example: Let's suppose we have a coffee machine and any customer can add add-ons to the coffee:
If we create a subclass for all possible combinations of add-ons with coffee, then it will cause us to create alot of classes.
 - The ideal approach could be to use a decorator pattern as below:

```java
interface Coffee {
    double cost();

    String description();
}

class Espresso implements Coffee {

    @Override
    public double cost() {
        return 10;
    }

    @Override
    public String description() {
        return "Espresso";
    }

}

abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }

    @Override
    public double cost() {
        return coffee.cost();
    }

    @Override
    public String description() {
        return coffee.description();
    }
}

//Decorator IS-A Coffee

//Decorator HAS-A Coffee

class MilkDecorator extends CoffeeDecorator {

    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double cost() {
        return coffee.cost() + 20;
    }

    @Override
    public String description() {
        return coffee.description() + ", Milk";
    }
}

class SugarDecorator extends CoffeeDecorator {

    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double cost() {
        return coffee.cost() + 10;
    }

    @Override
    public String description() {
        return coffee.description() + ", Sugar";
    }
}

class Main {
    public static void main(String[] args) {
        Coffee coffee = new Espresso();
        coffee = new MilkDecorator(coffee);
        coffee = new SugarDecorator(coffee);
        coffee = new ChocolateDecorator(coffee);
        System.out.println(coffee.description());
        System.out.println(coffee.cost());
    }
}

```

| Inheritance            | Decorator               |
| ---------------------- | ----------------------- |
| Compile-time extension | Runtime extension       |
| Rigid                  | Flexible                |
| Class explosion        | Few reusable decorators |
| Tight coupling         | Composition             |
| Static behavior        | Dynamic behavior        |

Typical Requirements:
1. "Users can choose any combination of features."
2. "Behavior should be configurable at runtime."
3. "Avoid creating dozens of subclasses."
4. "Add cross-cutting capabilities independently."
5. "Each feature should be reusable and composable."

Use the Decorator Pattern when:
 - You want to add responsibilities without modifying the original class.
 - Features are optional and independently reusable.
 - Multiple features can be combined dynamically.
 - Different combinations are required at runtime.
 - Do not use the Decorator Pattern when classes represent alternative implementations (for example, Email vs SMS).
Those are better modeled using inheritance or the Strategy Pattern.