    The core idea of the singleton pattern is to ensure that only one instance of a class is created.
    It provides a global access point to get the instance of the class.

    Core Ingredients:
    1. Private Constructors
    2. static Method to get the instance
    3. static Instance

    Lazy Initialization:
    There are various Thread-unsafe ways to implement lazy initialization. Below are thread-safe ways only:
    1. DCL (Double-Checked Locking) :
     - This ensures a thread-safe singleton object.
     - The static filed should be volatile for JMM ordering guarantees.
    2. Holder Idiom method
     - Thread safe, and instantiated only when first accessed.
    3. Enum based Singleton
     - Every enums extends the class `java.lang.enum`, so in cases where another class extension is required, this can't be used.
     - Every element of the enum is an object of the given enum type. e.g. INSTANCE is an object of the enum Singleton.
     - The enum constant is created only when the enum class is first accessed, hence the approach is lazy.
     - Reflection can't be applied onto ENUMs(JVM prevents it, throws IllegalArgumentExcpetion if one tries reflection to create an instance via reflection).
     - ENUM constructors are always private(or package-private). Therefore new can never be called.
    ```java
        public enum Singleton {
            INSTANCE;

            private int value;

            public void method() {}
        }

        Singleton singleton = Singleton.INSTANCE;
     ```

    Eager Initialization:
    1. static field initialized with new Object
     - This way whenever the class is loaded, the instance is created.
     - Thread safe.

    Problems:
    1. Serialization Problems:
     - A class serialized, followed by deserialization, can create distinct objects.
     - This can be fixed by using the method `readResolve` This tells java serialization to return the same object.
    2. Reflection Problems:
     - Reflection can access the private constructor, and therefore if handled poorly, one can create multiple instances.
     - Thus, to be resilient from reflection, we can add a check of `if null` in the private constructor, thereby ensuring
       that only one instance is created.
        ```java
        private Singleton() {
            if (instance != null) {
                throw new IllegalStateException("Already initialized");
            }
        }
        ```
    3. Cloneable:
     - Cloneable Interface should not be implemented.

    Singleton doesn't guarantee one instance across:
    - Multiple JVMs
    - Multiple Classloaders
    - Multiple Containers/Pods/Machines

```java
public class Singleton {
    public static void main(String[] args) {
        System.out.println(SingletonDCL.getInstance() == SingletonDCL.getInstance());
        System.out.println(SingletonHolder.getInstance() == SingletonHolder.getInstance());
        SingletonEnum.INSTANCE.method();
    }
}

//DCL
class SingletonDCL {
    private static SingletonDCL instance;

    private SingletonDCL() {}

    public static SingletonDCL getInstance() {
        if (instance == null) {
            synchronized(SingletonDCL.class) {
                if (instance == null) {
                    instance = new SingletonDCL();
                }
            }
        }

        return instance;
    }
}

//Bill Pugh
class SingletonHolder {
    //The inner class could be public/private/protected
    private static class Holder {
        final static SingletonHolder INSTANCE = new SingletonHolder();
    }

    public static SingletonHolder getInstance() {
        return Holder.INSTANCE;
    }
}

class SingletonStaticField {
    private final static SingletonStaticField INSTANCE = new SingletonStaticField();

    public static SingletonStaticField getInstance() {
        return INSTANCE;
    }
}

//Enum way of singleton
enum SingletonEnum {
    INSTANCE;

    public void method() {
        System.out.println("Inside Method " + SingletonEnum.class.getName());
    }
}
```