Here is your arranged .md (Markdown) version:

# Java Constructors & Inheritance

## ✅ Constructor in Java
A constructor is a special method that runs automatically when an object is created.  
Its main purpose is to initialize the object.

### ⭐ Real-life definition
Think of a constructor like the mobile phone setup process you do the first time you switch it on.  
Language, WiFi, Google login — all these steps prepare the mobile (object) for use.  
This setup = constructor.

---

## ✅ Types of Constructors in Java

### 1. Default Constructor
- Created automatically by Java (if no constructor is written).
- No parameters.

```java
class A {
   A() { 
      System.out.println("Object created");
   }
}


2. Parameterized Constructor
Constructor that accepts parameters to initialize values.
class Student {
   Student(String name, int age) {
      this.name = name;
      this.age = age;
   }
}


3. Copy Constructor
Java doesn’t have a built-in copy constructor, but you can create one manually.
Student s2 = new Student(s1);


✅ Inheritance in Java
Inheritance allows one class to acquire the properties and methods of another class.
It supports code reusability.
⭐ Real-life example
Just like a child inherits house, car, and other assets from a father → inheritance.

Types of Inheritance in Java
1. Single Inheritance
One parent → One child
A → B
2. Multilevel Inheritance
Parent → Child → Grandchild
A → B → C
3. Hierarchical Inheritance
One parent → Multiple children
   A
  / \
 B   C

❌ Multiple Inheritance
Java does not support multiple inheritance using classes
(because it creates ambiguity).
But Java does support it using interfaces.

OOP (Object-Oriented Programming)
OOP is a way of programming where everything revolves around objects.
Benefits of OOP:


Easy to understand


Easy to manage


Easy to reuse


Real-life like structure


⭐ Real-life example
A city has people, cars, buildings — each has properties (color, height, speed) and behaviors (walk, drive, open door).
Similarly, OOP programs are made of objects with data + actions.

What is a Class?
A class is a blueprint/template to create objects.
⭐ Real-world example
A house blueprint — you can build many houses using the same blueprint.
class Car {
   String color;
   int speed;

   void drive() { }
}


What is an Object?
An object is a real instance created from a class.
⭐ Real-world example
If the class is a mobile model,
your actual mobile = object.
Car myCar = new Car();

Objects have:


Properties (color, model, speed)


Behaviors (drive, stop)



⭐ Simple Real-life Mapping
Class → Mobile Model
Object → Your personal mobile
Class has: RAM, storage, camera
Object has: 8GB RAM, 128GB storage, blue color

---

If you want, I can now also generate the *PDF* again — just say *"make pdf"*.