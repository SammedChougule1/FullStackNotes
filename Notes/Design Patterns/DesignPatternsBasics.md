# Design Patterns

## Types of Design Patterns

### 1. Creational Design Patterns
- **Abstract Factory Pattern**: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
- **Builder Pattern**: Separates the construction of a complex object from its representation, allowing the same construction process to create different representations.
- **Factory Method Pattern**: Defines an interface for creating objects but lets subclasses alter the type of objects that will be created.
- **Prototype Pattern**: Creates new objects by copying an existing object, known as the prototype.
- **Singleton Pattern**: Ensures that a class has only one instance and provides a global point of access to that instance.

### 2. Structural Design Patterns
- **Adapter Pattern**: Allows incompatible interfaces to work together by converting one interface into another expected by the client.
- **Bridge Pattern**: Decouples an abstraction from its implementation, allowing them to vary independently.
- **Composite Pattern**: Composes objects into tree-like structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions uniformly.
- **Decorator Pattern**: Adds behavior or responsibilities to objects dynamically without affecting other objects of the same class.
- **Facade Pattern**: Provides a simplified interface to a complex subsystem, making it easier for the client to interact with the subsystem.
- **Flyweight Pattern**: Reduces memory usage by sharing as much data as possible with similar objects.
- **Proxy Pattern**: Provides a surrogate or placeholder for another object to control access, reduce cost, or delay instantiation.

### 3. Behavioral Design Patterns
- **Chain of Responsibility Pattern**: Passes a request along a chain of handlers, where each handler decides either to process the request or to pass it to the next handler.
- **Command Pattern**: Encapsulates a request as an object, allowing for parameterization of clients with queues, requests, and operations.
- **Interpreter Pattern**: Defines a grammar for a language and provides an interpreter to interpret sentences in the language.
- **Iterator Pattern**: Provides a way to access elements of an aggregate object sequentially without exposing its underlying representation.
- **Mediator Pattern**: Reduces coupling between communicating objects by introducing a mediator object that handles communication.
- **Memento Pattern**: Captures and externalizes an object's internal state so that it can be restored to this state later without violating encapsulation.
- **Observer Pattern**: Defines a one-to-many dependency between objects, so when one object changes state, all its dependents are notified and updated automatically.
- **State Pattern**: Allows an object to alter its behavior when its internal state changes, making it appear as if the object changes its class.
- **Strategy Pattern**: Defines a family of algorithms, encapsulates each one, and makes them interchangeable. The strategy pattern allows the algorithm to vary independently from clients that use it.
- **Template Method Pattern**: Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. The template method allows subclasses to redefine certain steps of an algorithm without changing its structure.
- **Visitor Pattern**: Represents an operation to be performed on elements of an object structure, allowing new operations to be defined without changing the classes of the elements on which it operates.
