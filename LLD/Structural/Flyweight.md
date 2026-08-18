#### Flyweight Pattern

 - Flyweight Pattern minimizes memory usage by sharing common object state among multiple objects while keeping unique state external.
 - It separates the object state into two categories : 
 1. Intrinsic State → Shared, Never changes, Can be reused.
 2. Extrinsic State → Unique per object, cannot be shared.

##### Components
1. Flyweight
 - Contains intrinsic state. e.g. TreeType
2. Concrete Flyweight
 - Actual shared implementation. e.g. OakTreeType. PineTreeType
3. Flyweight Factory
 - Returns already created objects or create if not created and return.
4. Context
 - Contains Extrinsic state.

 - With Flyweight, the memory is optimized and is used very less.

Consider using Flyweight when all of these are true:
 - You need to create a very large number of similar objects.
 - Many fields are identical across those objects.
 - The shared fields can be made immutable.
 - Memory usage or object creation overhead is becoming a bottleneck.
 - The unique state can be passed in from outside (extrinsic state).

Flyweight vs Object Pool

| Flyweight                                                  | Object Pool                                                        |
| ---------------------------------------------------------- | ------------------------------------------------------------------ |
| Shares immutable state across many logical objects         | Reuses entire objects after they're no longer in use               |
| Multiple clients can use the same flyweight simultaneously | One pooled object is typically checked out by one client at a time |
| Goal: reduce memory footprint                              | Goal: reduce expensive creation/destruction cost                   |
| Shared objects are usually immutable                       | Pooled objects are usually mutable and reset before reuse          |

Flyweight = Memory Optimization Through Sharing

Core Principle
--------------
Intrinsic State  -> Shared (immutable)
Extrinsic State -> Supplied by client

Typical Structure
-----------------
Client
↓
FlyweightFactory
↓
Shared Flyweight Cache

JDK Examples
------------
- String Pool -> JVM creates string pool, and every variable pointing to the same constant gets the same instance.
The String object can be created with a string constant, but 
- Integer Cache -> -127 to 128 is the range for which `Integer val = x` points to the same ref of x as any other with the same value.
- Character Cache -> Likewise Integer, Character is also cached.
- Boolean.TRUE/FALSE -> Always Cached
- Enum Constants
- Class Objects
- Constant Pool

Best Use Cases
--------------
- Millions of similar objects
- Game engines
- Text editors
- Maps
- Rendering systems
- Chess
- Particle systems

Key Requirement
---------------
Shared state should be immutable. Otherwise, the state would change unexpectedly for others.