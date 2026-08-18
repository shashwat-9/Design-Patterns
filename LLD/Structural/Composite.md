#### Composite Pattern

 - Compose objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects
and compositions uniformly.
 - The core intent of the **Composite Design Pattern** is to have a common interface for a group of related entities. 
For example, in a filesystem, both files and folders could share a common interface, allowing functions to operate on 
them uniformly without relying on checks like `if (obj instanceof File)` in the business logic.
 - A client shouldn't care whether an object is a single entity of a group of entities, the interface should provide all the
required methods.
 - One subtle but powerful aspect of the Composite pattern is that it is inherently recursive.

```java
    public interface FileSystemComponent {
    
        void display();
    
        int getSize();
    }


    public class File implements FileSystemComponent {
    
        private String name;
        private int size;
    
        public File(String name, int size) {
            this.name = name;
            this.size = size;
        }
    
        @Override
        public void display() {
            System.out.println(name);
        }
    
        @Override
        public int getSize() {
            return size;
        }
    }
    
    public class Folder implements FileSystemComponent {
    
        private String name;
    
        private List<FileSystemComponent> children = new ArrayList<>();
    
        public Folder(String name) {
            this.name = name;
        }
    
        public void add(FileSystemComponent component) {
            children.add(component);
        }
    
        public void remove(FileSystemComponent component) {
            children.remove(component);
        }
    
        @Override
        public void display() {
    
            System.out.println(name);
    
            for (FileSystemComponent child : children) {
                child.display();
            }
        }
    
        @Override
        public int getSize() {
    
            int total = 0;
    
            for (FileSystemComponent child : children) {
                total += child.getSize();
            }
    
            return total;
        }
    }

    public class Main {
    
        public static void main(String[] args) {
    
            Folder root = new Folder("Root");
    
            Folder movies = new Folder("Movies");
    
            movies.add(new File("Avengers.mp4", 500));
            movies.add(new File("Batman.mp4", 700));
    
            Folder docs = new Folder("Documents");
    
            docs.add(new File("Resume.pdf", 20));
            docs.add(new File("Notes.txt", 10));
    
            root.add(movies);
            root.add(docs);
            root.add(new File("Photo.jpg", 100));
    
            root.display();
    
            System.out.println(root.getSize());
        }
    }
```

 - There are two approaches for implementing Composite:
 1. **Transparent Composite:**
 - In this approach, all methods (common and specific) are declared in the common interface (`Component`) to make the design uniform and "transparent."
   - Both leaf (individual objects, like `File`) and composite (group objects, like ) classes implement all the methods from the interface. `Folder`
   - If a method is not applicable to a particular class (e.g., calling `add()` on a leaf object), it can either:
       - Throw an exception (). `UnsupportedOperationException`
       - Be left as a no-op (does nothing).

   - **Advantage:** Transparency—clients interact uniformly with leaf and composite objects.
   - **Disadvantage:** Removes type safety, as some methods are irrelevant to certain classes (e.g., `remove()` for a `File`).

 2. **Safe Composite:**
 - In this approach, only **common methods** (applicable to both leaf and composite) are declared in the common interface (`Component`).
 - Methods specific to composite objects (e.g., `add()`, `remove()`) are implemented directly in the composite class (e.g., ), and are not part of the leaf class (`File`). `Folder`
 - **Advantage:** Type safety—clients cannot invoke irrelevant methods on objects that do not support them (at compile time).
 - **Disadvantage:** Reduces transparency—clients need to differentiate between leaf and composite objects.
 - Violates ISP(Interface Segregation Principle), Clients should not be forced to depend on methods they do not use.

##### Advantages
 - Treats individual objects and groups uniformly.
 - Eliminates repeated instanceof checks.
 - Makes recursive tree operations simple.
 - Supports the Open/Closed Principle by allowing new component types without changing client code.
 - Naturally models hierarchical structures.
##### Drawbacks
 - It can become difficult to enforce rules such as "only folders may have children."
 - The common interface may need methods that don't make sense for every implementation.
 - Deep recursive trees can affect performance or even risk stack overflows in extreme cases.
