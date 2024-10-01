# Worksheet 3

### 1. Define polymorphism in the context of Java and provide one example where it is valuable?

  Polmorphism is when a variable can take on different type depending on runtime, this is because of dynamic binding. This is valuable when we want to have different types without having to inform the user.

### 2. Consider the following program from the class notes
  ``` java
  public class Ex3 {
  public static void main(String[] args) {
    Random   rand = new Random(System.currentTimeMillis());
    Point    v    = new Point(3, 4);
    LabPoint w    = new LabPoint(5, 2, "A");
    String   x    = "I'm a string";
    Scanner  y    = new Scanner(System.in);

    Object u;
    int i = rand.nextInt(4);

    if( i == 0 )
      u = v;
    else if( i == 1 )
      u = w;
    else if( i == 2 )
      u = x;
    else
      u = y;
    System.out.println(u.toString()); //<--
  }
  }
  ```
  Explain how polymorphism makes this program possible.
  
  Because depending on the random int output, the u variable will be set to a different type, polymorphism allows for that type to be printed successfully.

### 3. What is the output of this program? You should be able to do this without running the program!
  ``` java
  class A {
      public String toString(){
          return "A";
      }
  }
  
  class B extends A{
      public String toString() {
          return "B";
      }
  }
  
  class C extends A {
      public String toString() {
          return super.toString();
      }
  }
  
  class D extends C {
      public String toString() {
          return super.toString();
      }
  }
  
  public class tmp {
      public static void main(final String args[]) {
          D d = new D();
          System.out.println(d.toString());
      }
  }
  ```
  A because it takes the output of the D class toString which calls the C class toString which calls the A class toString and returns A.

### 4. What is the output of this program? You should be able to do this without running the program!

  ``` java
  class A {
      public String toString() {
          return "A";
      }
      
      public String fancyToString() {
          return "~~A~~";
      }
  }
  
  class B extends A {
      public String toString() {
          A letterA = this;
          return letterA.fancyToString();
      }
      public String fancyToString() {
          return "~~B~~";
      }
  }
  
  public class LetterPrinter {
      public static void main(final String args[]) {
          B letterB = new B();
          System.out.print(letterB.toString() + " ");
          
          A letterA = letterB;
          System.out.println(letterA.toString());
      }
  }
  ```
  Returns "~~B~~ ~~B~~". 

### 5.What is the output of this program? You should be able to do this without running the program!

  ```java
  class A {
      public String toString() {
          return "A";
      }
      
      public String fancyToString() {
          return "~~A~~";
      }
  }
  
  class B extends A {
      public String fancyToString(){
          return "~~B~~";
      }
  }
  
  public class LetterPrinter {
      public static void main(final String args[]) {
          B letterB = new B();
          System.out.print(letterB.toString() + " ");
          
          A letterA = letterB;
          System.out.println(letterA.toString());
      }
  }
  ```
  Returns "A A".

### 6.Consider the first two class declarations. What is the output of compiling the program below?

  ```java
  abstract class Letter {
      protected boolean uppercase;
  
      protected abstract String get_name();
  
      protected abstract int get_alphabet_position();
  }
  ```
  ``` java
  class A extends Letter {
      public String toString() {
          return "A";
      }
  
      protected int get_alphabet_position() {
          return 1;
      }
  
      private String get_name() {
          return "A";
      }
  }
  ```
  It won't compile because the get_name method is private.

### 7. If we change the implementation of A to the following, what does the code below output?
  ``` java
  abstract class Letter {
      protected boolean uppercase;
  
      public abstract String get_name();
  
      protected abstract int get_alphabet_position();
  }
  ```
  ```java
  class A extends Letter {
      public String toString() {
          return "A";
      }
  
      public int get_alphabet_position() {
          return 1;
      }
  
      protected String get_name() {
          return "A";
      }
  }
  ```
  ```java
  public class Main {
      public static void main(final String args[]) {
          A a = new A();
          System.out.println("A: " + a.get_alphabet_position());
      }
  }
  ```
This will return A: 1, this expected output becuse all methods are now visible.

### 8. What is the output of this program? You should do this without running the program.
  ```java
  class A {
      public String toString() {
          return "A";
      }
  }
  
  class B extends A {
      public String toString() {
          return "B";
      }
  }
  
  public class PolymorphicOverload {
      public void foo(B letterB1, B letterB2) {
          // 2
          System.out.println("foo2: " + letterB1 + " " + letterB2);
      }
  
      public void foo(A letterA1, A letterA2) {
          // 1
          System.out.println("foo1: " + letterA1 + " " + letterA2);
      }
      public static void main(String args[]) {
          PolymorphicOverload f = new PolymorphicOverload();
          B letterB = new B();
          A letterA = (A) new B();
          f.foo(letterB, letterA);
      }
  }
  ```

  Returns foo1: B B

### 9. Suppose you had the following class structures
  ``` java
  public class Species {
      String genus;
      String species;
      public Species(String g, String s) {
          genus = g;
          species = s;
      }
      
      public Species(Species s) {
          genus = s.genus;
          species = s.species;
      }
      
      public String toString() {
          return genus + " " + species;
      }
  }
  
  public class Breed extends Species {
      protected String breed;
  
      public Breed(String b, String g, String s) {
          super(g, s);
          breed = b;
      }
  
      public Breed(String b, Species s) {
          super(s);
          breed = b;
      }
  
      public String toString() {
          return super.toString() + "(" + breed + ")";
      }
  }
  
  public class Pet {
      String name;
      Species species;
  
      public Pet(String n, Species s) {
         name = n;
         species = s;
      }
  
      public String toString() {
          String ret = "Name: " + name + "\n";
          ret += "Species: " + species;
          retunr ret;
      }
  }
  ```
  ```java
   Species dog = new Species("Canis","Familaris");
   Breed shorthair = new Breed("shorthair", new Species("Felis","Catus"));
   Pet fluffy = new Pet("fluffy", new Breed("pomeranian", dog));
   Pet george = new Pet("george", dog);
   Pet brutus = new Pet("brutus", (Species) shorthair);
   
   System.out.println(fluffy);
   System.out.println(george);
  System.out.println(brutus);
  ```
  It will return
  
  Name: Fluffy
  
  Species: Canis Familiaris (pomeranian)
  
  Name: george
  
  Species: dog
  
  Name: brutus
  
  Species: shorthair
  
  What is the output of the following snippet of code? If there is an ERROR, describe the error. You should not need to run the code to determine the output.

### 10. Consider the following classes
  ``` java
  public class A {
      public int foo() {
          return 42;
      }
  
      public int bar() {
          return foo() + 8;
      }
  }
  
  public class B extends C {
      public int foo() {
          return 41;
      }
  
      public char baz() {
          return "y";
      }
  }
  
  public class C extends A {
      public char baz() {
          return "x";
      }
  }
  
  public class D extends A {
      public int bar() {
          return 7;
      }
  }
  
  public class E extends C {
      public int bar() {
          return foo() + 20;
      }
  }
  ```
  Consider a mystery function that returns a object of the given class. You do not know the definition of the mystery function, other than it compiles properly and returns an object of the class. For each of the following method calls marked below, indicate the value of the output, if the output cannot be determined, or if there is an error.
  ```java
  
  A a = mysteryA(); //<-- mystery function, this line compiles (the below may not!)
  System.out.println(a.foo()); //<-- Mark A.1
  System.out.println(a.bar()); //<-- Mark A.2
  System.out.println(a.baz()); //<-- Mark A.3
  
  
  B b = mysteryB(); //<-- mystery function, this line compiles (the below may not!)
  System.out.println(b.foo()); //<-- Mark B.1
  System.out.println(b.bar()); //<-- Mark B.2
  System.out.println(b.baz()); //<-- Mark B.3
  
  D d = mysteryD(); //<-- mystery function, this line compiles (the below may not!)
  System.out.println(d.foo()); //<-- Mark D.1
  System.out.println(d.bar()); //<-- Mark D.2
  System.out.println(d.baz()); //<-- Mark D.3
  ```
  The lines will return the following:
  
  Can't be determined
  
  Can't be determined
  
  Error

  41
  
  50
  
  y

  42
  
  7
  
  Error 
  
### 11. What is the difference between a class and an abstract class? From a software engineering perspective, why would you ever want to use an abstract class instead of a regular class?

Because if you have a "base" case that has a bunch of characteristics that cannot actually work as a class, Then you don't want to accidentally create an object using that class.

### 12. If you were to create an abstract class for a Car – what features could be defined in the implemented class vs. what could be defined in the abstract class? Provide justifications for your design.

Some features for the implemented class could be color, shape/type, brand, #of seats, things that vary between cars. These may should be in the implement class because they are not shared among all cars. In the abstract class, 
features like has 4 wheels, had a steering wheel, has windows, things that all cars have. This should be in the abstract class because they are shared between all cars. However if you tried to make a car based on that description, it would be lacking in essential details.
