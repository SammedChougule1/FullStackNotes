# Builder Pattern
- The Builder Pattern is a creational design pattern that provides a flexible solution to constructing complex objects step by step. It separates the construction of an object from its representation, allowing the same construction process to create different representations. This pattern is particularly useful when an object needs to be created with many possible configurations or when the creation process involves several steps.
- Can create object of product with some different defaults params. e.g. can create a Builder with Foudation default and rest user input. 

## Key Concepts
- **Builder Interface**: Defines methods for creating different parts of a complex object.
- **Concrete Builder**: Implements the builder interface and provides specific implementations for building the parts of the product.
- **Product**: The complex object that is being constructed.
- **Director**: Controls the construction process. It uses the builder interface to build the complex object step by step. 

## Advantages
- **Improves Readability**: Makes the object creation process more readable and maintainable, especially when dealing with objects with many optional parameters.
- **Encapsulation**: Encapsulates the complex construction logic within a builder class, promoting the Single Responsibility Principle (SRP).
- **Reusability**: Allows the same construction process to create different representations or products.
- **Fluent Interface**: Often implemented using a fluent interface, which allows method chaining to improve the readability of the code.

## Example
```C#
// Step 1: Define the Product class
public class House
{
    public string Foundation { get; set; }
    public string Structure { get; set; }
    public string Roof { get; set; }
    public bool HasGarden { get; set; }
    public bool HasGarage { get; set; }

    public override string ToString()
    {
        return $"Foundation: {Foundation}, Structure: {Structure}, Roof: {Roof}, " +
               $"Garden: {(HasGarden ? "Yes" : "No")}, Garage: {(HasGarage ? "Yes" : "No")}";
    }
}

// Step 2: Define the Builder Interface
public interface IHouseBuilder
{
    IHouseBuilder BuildFoundation(string foundation);
    IHouseBuilder BuildStructure(string structure);
    IHouseBuilder BuildRoof(string roof);
    IHouseBuilder AddGarden();
    IHouseBuilder AddGarage();
    House Build();
}

// Step 3: Implement the Concrete Builder
public class ConcreteHouseBuilder : IHouseBuilder
{
    private House _house = new House();

    public IHouseBuilder BuildFoundation(string foundation)
    {
        _house.Foundation = foundation;
        return this;
    }

    public IHouseBuilder BuildStructure(string structure)
    {
        _house.Structure = structure;
        return this;
    }

    public IHouseBuilder BuildRoof(string roof)
    {
        _house.Roof = roof;
        return this;
    }

    public IHouseBuilder AddGarden()
    {
        _house.HasGarden = true;
        return this;
    }

    public IHouseBuilder AddGarage()
    {
        _house.HasGarage = true;
        return this;
    }

    public House Build()
    {
        return _house;
    }
}

// Step 4: Use the Builder in Client Code
class Program
{
    static void Main(string[] args)
    {
        IHouseBuilder builder = new ConcreteHouseBuilder();

        House house = builder.BuildFoundation("Concrete")
                             .BuildStructure("Wood")
                             .BuildRoof("Tiles")
                             .AddGarden()
                             .Build();

        Console.WriteLine(house);
    }
}

//Director class can be class with creates the House uusing builder and returns House

```