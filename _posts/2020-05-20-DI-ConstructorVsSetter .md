---
layout: post
title: Dependency Injection - Constructor Injection vs. Setter Injection
---

## Constructor Injection vs. Setter Injection
Is there a difference? 

> Constructor injection - is the act of statically defining the list of 
required Dependencies by specifying them as parameters to the class's constructor. 

> Setter injection -  is the act of statically defining the list of 
required Dependencies by using setter methods. A client may first call the no 
argument constructor and then calls the setters.

On first glance, setter based injection seems more flexible and thus more durable.
Setter based injection can even work if some dependencies have been injected using
the constructor.  

In the spring framework, the @Required notation enables you to dictate which dependencies are
required.  BeanInitializationException will be thrown if the bean is fetched from the context
without the dependency properly injected.


#### Some History
In the early 2000's there were two primary dependency injection frameworks, Spring and PicoContainer.
PicoContainer focused on constructor injection and Spring focused on setter injection.  Both containers
also supported the other injector variant, but publically published their recommendations. 
[The PicoContainer team recommends constructor injection](http://picocontainer.com/setter-injection.html), 
while [Spring in 3.x actively recommended setter injection](https://docs.spring.io/spring/docs/3.1.x/spring-framework-reference/html/beans.html#d0e2778).


#### Considerations
Setter injection it is!  I still do not have my mind made up, but if someone asked me for a formal
recommendation, I would say, it depends.  Are you building software that is highly configurable?  
Are many of those configurations optional?  If you are building an application, the answer is more 
likely no, but if you are building a framework, well, then maybe yes.  

I am currently working on application, there is some configuration, but most of it is required, not 
optional.  These configurations also do not cascade much into the implementation or type
of dependency. In this particular instance, I'm living in the constructor injection camp.  

Constructor injection is great for mandatory dependencies.  Those, which are required for the object's
methods to function properly.  By specifying the dependencies in the constructure, you guarantee 
that the object is ready to be called after it is initialized.  The constructor also provides a sensible 
location for checking the required dependencies for a class.  

Another benefit in java is that fields assigned in the constructor can be made final.  If the dependency
is to be completely immutable, communicate that in your code.  With setter injection, you cannot assign
dependencies to final fields, rending your objects mutable. 

With flexibility, also produces chaos.  Using setter injection makes it very easy to add new 
dependencies without any critical thought.  Adding dependencies when using constructor injection will
surface a moment of thought, when the number of constructor params becomes high.  It might mean that
something is wrong.  Does the class have too many responsibilities?  Is there a good separation of 
concerns?  A large number of constructor params can be a good indicator to inspect and look for good
code refactoring opportunities.  

Circular dependencies are often mentioned as a drawback for it is possible to create an unresolvable 
circular dependency scenario.
For example: Class A requires an instance of class B through constructor injection, and class B requires 
an instance of class A through constructor injection.  The drawback has workarounds and also provides
a nice point to reflect on your architecture, so I don't see it as a drawback.  


#### Conclusion
Start off with constructor injection, if you are unsure.  Understand the reasoning, and moved to setter
injection in cases where reconfiguration or reinjection happens.  Setter injection is also valid for
optional dependencies.  
[In Spring 4.x, the recommendation has changed to constructor injection.](https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/beans.html#beans-setter-injection)
     
