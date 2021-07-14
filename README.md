# Java basics

### Q. What is JDK, JRE and JVM?

- JDK - Java Development Kit is set of tools that can translate Java code into byte code. It includes for example Java compiler and debugger.

- JRE - Java Runtime Environment is set of different libraries that are necessary to run Java application on given machine. It comes with Java Launcher and JVM.

- JVM - Java Virtual Machine is kind of abstract computing machine, that is able to translate Java byte code into machine code that is understandable by CPU of given machine. That means that application compiled once, can run on different operation systems and CPU architectures, because JVM takes care of translating compiled Java code into set of correct CPU instructions.


### Q. What is JavaBean?

This is simple class that contains getters, setters, default constructor and implements `Serializable` interface.

### Q. What are access modifiers in Java?

- `private` - visible inside the class that declared it and in inner classes
- package-scoped (no modified) - visible only inside package where it was declared
- `protected` - visible to subclasses of class that declared it
- `public` - visible everywhere

### Q. What is `transient` variable?

### Q. What is the difference between `interface` and `abstract class`?

JDK 8 introduced `default` method for interfaces, which can contain implementation. The same version introduces `static` method inside interfaces as well.

- abstract class may contain state (data members) and/or implementation methods 
- subclass can implement many interfaces but can extend only one abstract class
- abstract class can be inherited without providing implementation for abstract methods, however in that case, subclass must be annotated as `abstract` as well
- abstract class can contain `final` methods
- abstract class can contain constructor

### Q. Can you `@Override` private method?

No, because `private` method is only visible in class that declared it.

### Q. Can you `@Override` static method?

Overriding in Java simply means that the particulat method would be called based on the run time typ eof the object, therefore it is not possible to override static methods.
Parent class methods that are static are not part of child class (although they are accessible), so there is no question of overriding it. Even if you add another static method in a subclass, identical to the one in its parent class, the subclass static method is unique and distinct from the static method in its parent class.

Consider this example:

```java
abstract class A {
    public static void fun() { System.out.println("A"); }
}

class B extends A {
    public static void fun() { System.out.println("B"); }
}

class C extends B {
    public static void fun() { System.out.println("C"); }
}

A a = new C();
a.fun(); // here result is "A"

B b = new C();
b.fun(); // here result is "B"

```

### Q. What is `@FunctionalInterface`?

It is usefull for compilation time checking of your code. It is used to annotate `interface` that has exactly one abstract method, therefore it can be used with lambdas. Instances of functional interfaces can be created with lambda expressions, method references or constructor references. 

### Q. Is Java pass-by-value or pass-by-reference? 

TLDR: Java is pass-by-value. 

- In Java we have primitives (`byte`, `short`, `long`, `float`, `double`, `char`, `boolean`) and reference types
- All data for primitive type variables is stored on the stack
- For reference types, the stack holds a pointer to the object on the heap
- When setting a reference type variable equal to another reference type variable, a copy of only the pointer is made

Screen below explains how primitives and references are stored in memory. Primitives stores the actual values, whereas reference types store the reference to the object.
![image](https://user-images.githubusercontent.com/24475158/125502772-0873889e-36f8-4e51-a7bc-0679e008db00.png)

- When you pass an argument to a function, copy of that argument is created
- In case of primitives, new one is created on the stack and any changes made to it are made on copy of original primitive
- In case of reference types, copy of value is created on the stack, but this value is just a pointer to the object on the stack, therefore any changes made to this copy, are reflected in original object as well 

Code example:

```java
public class DemoApplication {
	public static void main(String[] args) {
		var user = new User();
		var value = 5;
		changeObject(user);
		changePrimitive(value);
		System.out.println(user.name); //displays "changed name"
		System.out.println(value); //displays "5"
	}

	public static void changeObject(User user) {
		user.name = "changed name";
	}

	//passing primitive value means making a copy of that value inside this function
	public static void changePrimitive(int value) {
		value = value * 2;
		System.out.println("inside method: " + value); // displays "5"
	}
}

class User {
	public String name = "initial name";
}
```


### Q. What is lambda?

Lambda is an anonymous function, which can be used to provide implementation of functional interface. It can reduce amount of necessary boilerplate code, because you no longer have to create anonymous inner class. It also translates to faster application startup performance. Under the hood, lambdas are using invokedynamic bytecode instruction, which defer the selection of translation strategy until run time. Translation of lambda expression into bytecode is performed in two steps:
- generate an invokedynamic call site (lambda factory), which when invoked returns an instance of Functional Interface
- convert the body of the lambda expression into a method that will be invoked through the invokedynamic instruction

### Q. What is `Stream`?

- Sequence of objects that supports various methods, which can be pipelined to produce desired result. 
- Stream is not a data structure, instead it takes input from the Collection or I/O channel.
- Stream don't change original data source.
- Each intermediate operation is lazily executed and returns stream as result

### Q. What kind of exceptions are in Java?
- Checked exceptions - compile time exceptions, must be handled by programmer (`try...catch` / `throws`) or else compilation will fail. In most cases they represent external issues, like `FileNotFoundException` that can be recovered.
- Unchecked exceptions - run time exceptions, can be thrown without catching. In most cases they represent programming error, like for example `NullPointerException`.
- Errors - unchecked as well, thrown by JVM when something very wrong is happening, most likely unrecoverable, like for example `OutOfMemory`

### Q. What is the difference between `==` and `equals()`?

### Q. What is the difference between `&` or `|` and `&&` or `||`?

They are both logical AND / OR operators, however `&&` or `||`  will stop evaluation if first condition is met and `&` or `|` will evaulate both conditions. 

For example `if(isEven(5) && isEven(2)) {...}` will execute only first method call, because it is clear that whole condition will evaluate to false. 

However, `if(isEven(5) & isEven(2)) {...}` will execute both method calls.

### Q. What is the difference between `String`, `StringBuilder` and `StringBuffer`?

### Q. How is `String` created? What is String pool?

`String` is sequence of characters, one of most important characteristics of a string in Java is that it is immutable. Once created, the internal state of a string remains the same (String is immutable)

String constant pool is separate place in heap memory where the values of all the strings defined by the program are stored. 

When you create string like this:
`String str1 = "lorem ipsum";` 
Literal "lorem ipsum" is stored in the heap and object string is created in the stack and it points towards value in the heap.

Now, if you create another string like this:
`String str2 = "lorem ipsum";` 
Value "lorem ipsum" already exists in the heap, therefore it is not created, instead there is new reference to it created in the stack.

Finally, if you create yet another string like this:
`String str3 = new String("lorem ipsum");`
Regardless if literal "lorem ipsum" exists or not in the heap, calling constructor directly will always create new value in the heap and corresponding reference in the stack. 

### Q. What is the outcome of each `System.out.println()`? Why?

```java
String s1 = "Java";
String s2 = "Java";
StringBuilder sb1 = new StringBuilder();
sb1.append("Ja").append("va");

System.out.println(s1 == s2); (1)
System.out.println(s1.equals(s2)); (2)
System.out.println(sb1.toString() == s1); (3)
System.out.println(sb1.toString().equals(s1)); (4)
```

- (1) `==` operator is used to compare references, while it shouldn't be used to compare Strings. However, because `s1` and `s2` were created by `=`, and not by constructor, `s2` is just another reference to `s1` that already exists in String pool, therefore `==` will return `true`
- (2) we compare values ("Java") as literals, therefore result is `true`
- (3) `toString()` is internally calling `new String(value)`, therefore even if passed `value` exists in String pool, new one will be created, therefore `==` will return false.
- (4) we are comparing two separate objects, however their values are the same, therefore result is `true`


### Q. Does Java support multiple inheritance?

### Q. Can `Enum` extend other class? Can it implement interface?

It can't extend other class, but can implement interface.
TODO explain why

### Q. Will this compile and work properly? Why?

```java
class Outer {
   class Inner1 {
       private String x = "x";
   }

   class Inner2 {
       public void fun() {
           System.out.println(new Inner1().x);
       }
   }
}
```
Inner classes have access to `private` memebers of outer classes, therefore this will work properly.

### Q. What does `final` do on class level, method level and variable level?

### Q. What naming conventions are used in Java?

### Q. What is the difference between method overriding and overloading?

# Collections

### Q. How is HashMap implemented internally?

HashMap store objects as key value pair. Internally, there is an array of buckets, where each bucket is LinkedList of Nodes (from JDK8 LinkedList was replaced with binary tree)
When client puts an object into HashMap: 
- bucket index is calculated using `key.hashCode() & (n - 1)`, where `n` is array size
- if bucket is empty, value is added at the begining of it 
- if bucket is not empty, new key is evaluated against other keys in this bucket using `key.equals()` method. If key was different, new node is added at the end of bucket's LinkedList. If key was already present, it's value is replaced with new one

Getting elements from HashMap is similar to putting one:
- bucket index is calculated using `key.hashCode() & (n - 1)`, where `n` is array size
- if bucket is empty, first value is returned
- if bucket is not empty, it iterates over LinkedList comparing each value by `equals()` until the result is found

### Q. Why you should always override both `equals()` and `hashCode()`?

Explained in previous question, both `hashCode()` and `equals()` is used to put or get element from Hash based collection.

### Q. What is the difference between HashMap and HashTable?

- HashMap is not synchronized, therefore it is not thread-safe
- HashMap allows one `null` key and multiple `null` values, whereas HashTable does not except it both as key or value

### Q. What is the difference between LinkedList and ArrayList? In what cases would you use one instead of the other?

### Q. What is the difference between List and Set?

### Q. What are most popular Java collections? What are differences between them? 


# Concurrency

### Q. What is Thread?

### Q. What is `volatile`?

The Java volatile keyword is used to mark a Java variable as "being stored in main memory". More precisely that means, that every read of a volatile variable will be read from the computer's main memory, and not from the CPU cache, and that every write to a volatile variable will be written to main memory, and not just to the CPU cache.
It is mostly used if one thread is writing to shared variable and another one is reading it.
![image](https://user-images.githubusercontent.com/24475158/125336621-4f296800-e34e-11eb-9874-4bc67143389f.png)

### Q. When is `volatile` not enough? What else should be use then?

If two or more threads are both reading and writing to shared variable, then using `volatile` keyword for that is not enough, because it is not causing different threads to block other operations. In that case you must use `synchronized`, which will guarantee that the read and write operations are atomic. Alternatively, you may consider using data types found in `java.util.concurrent` package, like for example `AtomicLong` or `AromicReference`.

### Q. How to spawn new thread in Java? 

- `ExecutorService`

```java
ExecutorService executor = Executors.newSingleThreadExecutor();
executor.submit(() -> {
	System.out.println("Hello from " + Thread.currentThread().getName());
});
```

- `Callable` 

```java
Callable<Integer> task = () -> {
	try {
		TimeUnit.SECONDS.sleep(5);
		return 123;	
	} catch(InterruptedException e) {
			throw new RuntimeException(e);
	}
}

ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Integer> future = executor.submit(task);

//Blocking operation, this will wait 5 seconds for task completion, then will display "123" and only then "Completed"
System.out.println(future.get());
System.out.println("Completed");

//Future::get can be called with timeout also, in that case this will throw TimeoutException, because timeout is lower than time needed to completed method execution
System.out.println(future.get(3, TimeUnit.SECONDS));
```

- `CompletableFuture` 

```java
Supplier<Integer> task = () -> {
	try {
			TimeUnit.SECONDS.sleep(2);
			return 123;
	} catch(InterruptedException e) {
			throw new RuntimeException(e);
	}
};
ExecutorService executor = Executors.newSingleThreadExecutor();
CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(task, executor);

completableFuture.thenApply(result -> result * 2).thenAcceptAsync(System.out::println);
completableFuture.get();
```

### Q. Advantages and disadvantages of concurrent programming.

### Q. How to write thread-safe application?

### Q. What is JMS?


# Spring Framework

### Q. How to validate request body?

### Q. Describe Spring Core technologies

- Dependency Injection
- Aspect Oriented Programming
- Application Events
- SpEL

### Q. How Spring Security works?

### Q. What is the difference between Spring and SpringBoot?
**Spring Framework** provides comprehensive infrastructure support for developing Java applications. It comes with features like Dependency Injection and offers various modules that can simplify common use cases and reduce amount of necessary of boilerplate code. Some of most known modules are:
- Spring Web - provides support for building REST APIs
- Spring Data - easy access to databases, ready to use CRUD repositories
- Spring Security - authentication and authorization
- Spring Cloud - support for distributed applications

**Spring Boot** is just another module which is part of Spring Framework. It eliminates even more boilerplate code, by providing set of default autoconfigurations, that covers most of everyday scenarios, but of course, can be overriden if necessary. It also comes with embedded Tomcat, therefore it eliminates the need of running external application server.

### Q. How Spring Boot works internally? 

### Q. What is Bean?

It is a class instance (object) managed by IoC Container.

### Q. What are bean scopes in Spring?

- singleton (default) - single instance per application
- prototype - different instance every time it is request from container
- request - one instance for lifecycle of HTTP request
- session - one instance for whole user session
- application - similar to singleton, however in case of application scape, the same instance of the bean is shared across multiple servlet-based applications running in the same ServletContext, while singleton scoped beans are scoped to a single application context only. This is valid only in the context of web-aware Spring ApplicationContext

### Q. Explain Inversion of Control and Dependency Injection.

Inversion of Control (IoC) is generic term, that means that application is not calling framework, but framework is calling implementation provided by application. In Spring Framework it means that dependencies are created, maintained and injected to application code by framework. Programmer does not need to handle whole lifecycle of object managed by IoC container.

Dependency Injection is simply passing dependency to another object.

Together, in Spring world, it comes down to few points:
- programmer defines which classes should be managed by IoC container, by using for example `@Component` annotation (other options are configuration classes or XML configurations)
- programmer defines what dependencies are necessary for this class to be created, by for example constructor, field or setter injection
- Spring BeanFactory takes this configuration created by programmer, creates Beans out of it and makes it availalbe in IoC Container

### Q. What types of Dependency Injection in Spring you know?

- via constructor
```java
@Service
class SomeService { }

@Service
class AnotherService {
    private final SomeService someService;

    AnotherService(SomeService someService) {
        this.someService = someService;
    }
}
```

- via setter
```java
@Service
class AnotherService {
    private SomeService someService;

    @Autowired
    public void setSomeService(SomeService someService) {
        this.someService = someService;
    }
}
```

- via field
```java
@Service
class AnotherService {
    @Autowired
    private SomeService someService;
}
```

- @Lookup - this is usefull if you want to use prototype scoped bean inside singleton scoped bean. In theory, you can inject it via constructor or any other method, however singleton bean is created on application start and stays in JVM memory for whole duration of application run time. On the other hand, prototype scope is used to create bean on demand, therefore most likely you don't want to bind creation of singleton and prototype beans. That's where you should consider using `@Lookup`, that will provide instance of prototype bean when needed, but not before. 

```java
@Component
@Scope("prototype")
public class ProtoBean {

    public ProtoBean() {
        System.out.println("proto bean created");
    }

    public void call() {
        System.out.println("proto bean called");
    }
}

@Component
public class SingletonBean {

    private final ApplicationContext applicationContext;

    public SingletonBean(ApplicationContext applicationContext) {
        System.out.println("singleton bean created");
        this.applicationContext = applicationContext;
    }

    @Lookup
    public ProtoBean protoBean() {
        return applicationContext.getBean(ProtoBean.class);
    }
    
    public void callProto() {
    	protoBean().call();
    }
}
```

### Q. What are the benefits of different Dependency Injection methods in Spring?

- safety - once your object is created it is safe to use
- testability - you can easily create new instance of your class for testing using `new` operator, it is clear enough which dependencies are necessary
- immutability - you can mark dependencies as `final` and then, once created, they cannot be changed
- cleaner expression of mandatory dependencies 
- cleaner design - it is easy to add more and more `@Autowired` fields, which leads to classes with a lot of dependencies. If you have to add 5th dependency to your constructor, you will start to think if the design is good

### Q. How to externalize variables in property files in Spring Boot application?

- use environment variables inside property file `myprop.javaPath=${JAVA_HOME}`
- provide config location while starting an app `java -jar app.jar --spring.config.location=file://etc/home/new_config.properties`

### Q. What is the use of `@ConfigurationProperties`?

Instead of injecting properties with `@Value`, we can inject type-safe POJOs instead.
We can also define default values on class level.

Firstly, we define some set of properties in `application.properties` 

```
myprop.appName=test name
myprop.appDescription=test description
```

Then we configure class that will read those properties

```java
@ConfigurationProperties(prefix = "myprop")
public class PropertyBean {

    private String appName;
    private String appDescription;
    
    //getter, setter
}

```

We also have to add required annotation to main class

```java
@SpringBootApplication
@ConfigurationPropertiesScan
public class DemoApplication {
	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}
}
```
And finally, we can inject our `PropertyBean` like any other dependency and use it in our code.

```java
@Component
public class SingletonBean {
    private final PropertyBean propertyBean;

    public SingletonBean(PropertyBean propertyBean) {
        this.propertyBean = propertyBean;
        System.out.println(this.propertyBean.getAppName() + " " + this.propertyBean.getAppDescription());
    }
}
```

### Q. What is wrong with this code?
```java
class Service {
   @Autowired
   private Repository repository;
   
   @Transactional
   public void addBook(Book book) {
       repository.save(book);
   }
   
   public void addBooks(List<Book> books) {
       books.forEach(Service::addBook);
   }
}
```

`@Transactional` works only when annotated method is public and invoked from another bean. Otherwise, the annotation will be silently ignored.
At high level, Spring creates proxies for all the class / methods annotated with `@Transactional`. The proxy allows the framework to inject transactional logic before and after running the method. 

### Q. What is the difference between JPA, Hibernate and Spring Data?
**JPA - Java Persistance API** - specification that describes the management of relation data in applications using Java SE/EE. 

**Hibernate** - implementation of the JPA specification.

**SpringData** - abstraction layer on top of Hibernate or any other JPA implementation, which makes it easier to use

### Q. Explain N+1 problem in Hibernate. How can you avoid it?

### Q. Fetch types in Hibernate, what is the difference?


# Design Patterns

## Strategy

## Factory 

## Abstract Factory

## Chain of Responsibility

## Facade

## Mediator

## Builder

## Singleton

# Testing

### Q. What kind of tests do you know? 

### Q. What is the difference between `spy`, `stub` and `mock`? 


# Other

### Q. What is CI/CD?

### Q. What is Maven?

### Q. What is Docker?

### Q. What Dockerfile contains?

### Q. How would you investigate poor performance of application?

### Q. How would you investigate poor performance of database?

### Q. What is the difference between `git merge` and `git rebase`?

### Q. Describe differences between different HTTP codes

### Q. Design REST API for simple CRUD 

### Q. What kinds of authentications you know? 

- Basic auth
- Bearer token
- OAuth2

### Q. How to monitor application in production?

- meaningful logs
- track number of requests and their status codes
- track CPU, memory, disk usage
- track JVM metrics (garbage collecting, etc)
- healthchecks
- alerts 

### Q. What is MQ?

### Q. What is exchange?

### Q. What is the difference between queue and topic?

