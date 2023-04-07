**Q1** in c# I have a method MethodDdd(MyClass1 param1){}. I want to use this method also with another class MyClass2 like this MethodDdd(MyClass2 param1){}. How can I do this?

**A1** In C#, you can achieve this by using interfaces. An interface defines a contract that classes can implement, allowing them to be used interchangeably in methods that expect objects of that interface type. Here's an example of how you can modify your code to achieve this:
```c#
// Define an interface that both MyClass1 and MyClass2 will implement
public interface IMyInterface
{
    // Declare any common methods or properties here
}

// Implement the interface in both MyClass1 and MyClass2
public class MyClass1 : IMyInterface
{
    // Class implementation of IMyInterface
}

public class MyClass2 : IMyInterface
{
    // Class implementation of IMyInterface
}

// Modify MethodDdd to accept objects of IMyInterface type
public void MethodDdd(IMyInterface param1)
{
    // Method implementation here
}

```
By using the IMyInterface interface in the method signature of MethodDdd, you can now pass objects of both MyClass1 and MyClass2 to that method, as both classes implement the IMyInterface interface. This allows you to reuse the same method for different classes that share a common interface.

**Q2** what about if MyClass1 and MyClass2 have different properties that I need to access inside MethodDdd method?

**A2** If MyClass1 and MyClass2 have different properties that you need to access within the MethodDdd method, you can declare those properties in the IMyInterface interface. Here's an updated example:
```c#
// Define an interface that both MyClass1 and MyClass2 will implement
public interface IMyInterface
{
    // Declare any common properties here
    string CommonProperty { get; set; }
}

// Implement the interface in both MyClass1 and MyClass2
public class MyClass1 : IMyInterface
{
    // Implement IMyInterface properties
    public string CommonProperty { get; set; }

    // MyClass1-specific properties
    public int MyClass1Property { get; set; }
}

public class MyClass2 : IMyInterface
{
    // Implement IMyInterface properties
    public string CommonProperty { get; set; }

    // MyClass2-specific properties
    public string MyClass2Property { get; set; }
}

// Modify MethodDdd to accept objects of IMyInterface type
public void MethodDdd(IMyInterface param1)
{
    // Access IMyInterface properties
    string commonProperty = param1.CommonProperty;

    // Check the actual type of the object and access class-specific properties
    if (param1 is MyClass1)
    {
        MyClass1 obj1 = (MyClass1)param1;
        int myClass1Property = obj1.MyClass1Property;
        // Use myClass1Property as needed
    }
    else if (param1 is MyClass2)
    {
        MyClass2 obj2 = (MyClass2)param1;
        string myClass2Property = obj2.MyClass2Property;
        // Use myClass2Property as needed
    }

    // Method implementation here
}
```
By declaring the common properties in the IMyInterface interface and implementing them in both MyClass1 and MyClass2, you can access those properties within the MethodDdd method regardless of whether you're dealing with an object of MyClass1 or MyClass2 type. You can also use type checking (is and as keywords) to access class-specific properties if needed.

**A3** if (param1 is MyClass1) is violating SOLID principles, how can this be re written?
You're correct, using type checking with if (param1 is MyClass1) can be considered a violation of the SOLID principles, specifically the Open/Closed Principle (OCP), which states that software entities should be open for extension but closed for modification. One way to refactor the code to adhere to SOLID principles is to use polymorphism to avoid type checking. Here's an example:
```c#
// Define an interface that both MyClass1 and MyClass2 will implement
public interface IMyInterface
{
    // Declare any common properties here
    string CommonProperty { get; set; }

    // Declare a common method here
    void CommonMethod();
}

// Implement the interface in both MyClass1 and MyClass2
public class MyClass1 : IMyInterface
{
    // Implement IMyInterface properties
    public string CommonProperty { get; set; }

    // Implement IMyInterface method
    public void CommonMethod()
    {
        // Implementation for MyClass1
    }

    // MyClass1-specific properties
    public int MyClass1Property { get; set; }
}

public class MyClass2 : IMyInterface
{
    // Implement IMyInterface properties
    public string CommonProperty { get; set; }

    // Implement IMyInterface method
    public void CommonMethod()
    {
        // Implementation for MyClass2
    }

    // MyClass2-specific properties
    public string MyClass2Property { get; set; }
}

// Modify MethodDdd to accept objects of IMyInterface type
public void MethodDdd(IMyInterface param1)
{
    // Access IMyInterface properties
    string commonProperty = param1.CommonProperty;

    // Call the common method
    param1.CommonMethod();

    // Method implementation here
}

```
By declaring a common method CommonMethod in the IMyInterface interface and implementing it in both MyClass1 and MyClass2, you can avoid type checking and instead invoke the common method on the IMyInterface parameter directly. This adheres to the SOLID principles, as the code is open for extension (you can add new classes that implement IMyInterface) but closed for modification (you don't need to modify MethodDdd when adding new classes that implement IMyInterface).
