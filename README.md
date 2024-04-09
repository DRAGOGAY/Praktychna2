# Praktychna2
using System;
using System.Collections.Generic;

class Carriage
{
    public int id { get; set; }
    public string type { get; set; }
    public double length { get; set; }
    public double weight { get; set; }
    public string color { get; set; }  // Adding color attribute

    public Carriage(int id, string type, double length, double weight, string color)
    {
        this.id = id;
        this.type = type;
        this.length = length;
        this.weight = weight;
        this.color = color;
    }

}

class PassengerCarriage : Carriage
{
    public int Capacity { get; set; }
    public string passengerClass { get; set; }  // Adding passenger class attribute

    public PassengerCarriage(int id, double weight, double length, int capacity, string passengerClass) : base(id, "Passenger", weight, length, "White")
    {
        Capacity = capacity;
        this.passengerClass = passengerClass;
    }
}

class FreightCarriage : Carriage
{
    public double LoadCapacity { get; set; }
    public string cargoType { get; set; }  // Adding cargo type attribute

    public FreightCarriage(int id, double weight, double length, double loadCapacity, string cargoType) : base(id, "Cargo", weight, length, "Gray")
    {
        LoadCapacity = loadCapacity;
        this.cargoType = cargoType;
    }
}

class DiningCarriage : Carriage
{
    public bool HasKitchen { get; set; }
    public int seatingCapacity { get; set; }  // Adding seating capacity attribute

    public DiningCarriage(int id, double weight, double length, bool hasKitchen, int seatingCapacity) : base(id, "Dining", weight, length, "Red")
    {
        HasKitchen = hasKitchen;
        this.seatingCapacity = seatingCapacity;
    }
}

class SleepingCarriage : Carriage
{
    public int Beds { get; set; }
    public bool isLuxury { get; set; }  // Adding luxury attribute

    public SleepingCarriage(int id, double weight, double length, int beds, bool isLuxury) : base(id, "Sleeping", weight, length, "Blue")
    {
        Beds = beds;
        this.isLuxury = isLuxury;
    }
}

class Train
{
    private List<Carriage> carriages;

    public Train()
    {
        carriages = new List<Carriage>();
    }
    public void AddCarriage(Carriage carriage)
    {
        carriages.Add(carriage);
    }

    public void RemoveCarriage(int id)
    {
        carriages.RemoveAll(c => c.id == id);
    }
    public Carriage FindCarriage(int id)
    {
        return carriages.Find(c => c.id == id);
    }
    public void DisplayCarriages()
    {
        Console.WriteLine("Carriages in the train:");
        foreach (var carriage in carriages)
        {
            Console.WriteLine($"ID: {carriage.id}, Type: {carriage.type}, Weight: {carriage.weight} tons, Length: {carriage.length} meters, Color: {carriage.color}");
        }
    }
    public int TotalPassengers()
    {
        int totalPassengers = 0;
        foreach (var carriage in carriages)
        {
            if (carriage is PassengerCarriage)
            {
                totalPassengers += ((PassengerCarriage)carriage).Capacity;
            }
        }
        return totalPassengers;
    }
    public FreightCarriage FindMaxLoadCapacityCarriage()
    {
        FreightCarriage maxLoadCapacityCarriage = null;
        foreach (var carriage in carriages)
        {
            if (carriage is FreightCarriage)
            {
                var freightCarriage = (FreightCarriage)carriage;
                if (maxLoadCapacityCarriage == null || freightCarriage.LoadCapacity > maxLoadCapacityCarriage.LoadCapacity)
                {
                    maxLoadCapacityCarriage = freightCarriage;
                }
            }
        }
        return maxLoadCapacityCarriage;
    }
}

class Program
{
    static void Main()
    {
        Train train = new Train();

train.AddCarriage(new PassengerCarriage(1, 50.5, 25.2, 50, "First Class"));
        train.AddCarriage(new FreightCarriage(2, 70.8, 30.0, 150.0, "Liquid"));
        train.AddCarriage(new DiningCarriage(3, 40.0, 20.5, true, 30));
        train.AddCarriage(new SleepingCarriage(4, 60.2, 28.0, 40, true));

        train.DisplayCarriages();

        train.RemoveCarriage(2);

        Console.WriteLine("\nAfter removing carriage with ID 2:");
        train.DisplayCarriages();

        Console.WriteLine($"\nTotal passengers in the train: {train.TotalPassengers()}");

        var maxLoadCapacityCarriage = train.FindMaxLoadCapacityCarriage();
        if (maxLoadCapacityCarriage != null)
        {
            Console.WriteLine($"\nCarriage with the highest load capacity (ID: {maxLoadCapacityCarriage.id}): {maxLoadCapacityCarriage.LoadCapacity} tons");
        }
        else
        {
            Console.WriteLine("\nFreight carriage not available");
        }
    }
}
