# Abtract Factory

The Factory Design Pattern is a creational pattern that provides a way to create objects without specifying the exact class of the object that will be created. The main idea is to use a common interface or base class for all created objects, and a factory method or class to handle the instantiation.

## Key Concepts
- **Factory Method**: 
A method that returns an instance of a class. The type of the returned object can be determined at runtime.
- **Product Interface or Abstract Class**:
 An interface or abstract class defines the common behavior of objects that the factory will create.
- **Concrete Product Classes**: 
Classes that implement the product interface or extend the abstract class, representing different variations of the object that can be created.

```C# 
// Step 1: Create the Product interface
public interface IVehicle
{
    void Drive();
}

// Step 2: Create Concrete Products
public class Car : IVehicle
{
    public void Drive()
    {
        Console.WriteLine("Driving a car.");
    }
}

public class Bike : IVehicle
{
    public void Drive()
    {
        Console.WriteLine("Riding a bike.");
    }
}

// Step 3: Create the Factory Class
public class VehicleFactory
{
    public static IVehicle CreateVehicle(string vehicleType)
    {
        switch (vehicleType)
        {
            case "Car":
                return new Car();
            case "Bike":
                return new Bike();
            default:
                throw new ArgumentException("Invalid vehicle type");
        }
    }
}

// Step 4: Client Code
class Program
{
    static void Main(string[] args)
    {
        IVehicle car = VehicleFactory.CreateVehicle("Car");
        car.Drive();

        IVehicle bike = VehicleFactory.CreateVehicle("Bike");
        bike.Drive();
    }
}

```