Interface and Abstract Class Design Decision:

While Designing applicaiton we need to design carefully. Basically, Interface is used in most of the cases where we need to use contract. Contract will have list of method signatures only and we will created classes that implements that interface. Where as Abstract class is used in those scenario where some method can have default implementation on subclass and some method would need to implements in subclass per their requirements.

Interface or Abstract Class
Whether to chose between Interface or abstract class for providing contract for subclasses is a design decision and depends on many factors, lets see when Interfaces are best choice and when can we use abstract classes.

Java doesn’t support multiple class level inheritance, so every class can extend only one superclass. But a class can implement multiple interfaces. So most of the times Interfaces are a good choice for providing base for class hierarchy and contract. Also coding in terms of interfaces is one of the best practices for coding in java.
If there are a lot of methods in the contract, then abstract class is more useful because we can provide default implementation for some of the methods that are common for all the subclasses. Also if subclasses don’t need to implement particular method, they can avoid providing the implementation but in case of interface, the subclass will have to provide implementation for all the methods even though it’s of no use and implementation is just empty block.
If our base contract keeps on changing then interfaces can cause issues because we can’t declare additional methods to the interface without changing all the implementation classes, with abstract class we can provide the default implementation and only change the implementation classes that are actually going to use the new methods.

We can override all the interface methods in abstract parent class and in child class override those methods only which is required by that particular child class.

!(https://github.com/TyataAcademy/Java-Notes/blob/master/Interface_Pic1.PNG)


Use Abstract classes and Interface both:

Scenario 1:
Actually most of the times, using Interfaces and abstract classes together is the best approach for designing a system, for example in JDK java.util.List is an interface that contains a lot of methods, so there is an abstract class java.util.AbstractList that provides skeletal implementation for all the methods of List interface so that any subclass can extend this class and implement only required methods.

Scenario 2:
We should always start with an interface as base and define methods that every subclasses should implement and then if there are some methods that only certain subclass should implement, we can extend the base interface and create a new interface with those methods. 



JavaEE vs JavaSE
The JavaEE differs from JavaSE in terms of libraries it provides to deploy: 
- Fault-tolerance
- distributed and 
- multi-tier Java software, based largely on modular components running on an application server.


