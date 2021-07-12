# Java basics

### What is JavaBean?

This is simple class that contains getters, setters, default constructor and implements `Serializable` interface.

### What are access modifiers in Java?

- `private` - visible inside the class that declared it and in inner classes
- package-scoped (no modified) - visible only inside package where it was declared
- `protected` - visible to subclasses of class that declared it
- `public` - visible everywhere

### What is the difference between `interface` and `abstract class`?

JDK 8 introduced `default` method for interfaces, which can contain implementation. The same version introduces `static` method inside interfaces as well.

- abstract class may contain state (data members) and/or implementation methods 
- subclass can implement many interfaces but can extend only one abstract class
- abstract class can be inherited without providing implementation for abstract methods, however in that case, subclass must be annotated as `abstract` as well
- abstract class can contain `final` methods
- abstract class can contain constructor

### Can you `@Override` private method?

No, because `private` method is only visible in class that declared it.

### Can you `@Override` static method?

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

### What is `@FunctionalInterface`?

It is usefull for compilation time checking of your code. It is used to annotate `interface` that has exactly one abstract method, therefore it can be used with lambdas. Instances of functional interfaces can be created with lambda expressions, method references or constructor references. 

### What is lambda?

### What is `Stream`?

### What kind of exceptions are in Java?

### What is the difference between `&` and `&&`?

### What is the difference between `|` and `||`?

### What is the difference between `String`, `StringBuilder` and `StringBuffer`?

### How is `String` created? What is String pool?

### What is the outcome of each `System.out.println()`? Why?

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

- (1) - `==` operator is used to compare references, therefore it shouldn't be used to compare Strings. However, because `s1` and `s2` were created by `=`, and not by constructor, `s2` is just another reference to `s1` that already exists in String pool, therefore `==` will return `true`
- (2) - we compare values ("Java") as literals, therefore result is `true`
- (3) - `toString()` is internally calling `new String(value)`, therefore even if passed `value` exists in String pool, new one will be created, therefore `==` will return false.
- (4) - we are comparing two separate objects, however their values are the same, therefore result is `true`


### Does Java support multiple inheritance?

### Can `Enum` extend other class? Can it implement interface?

### Will this compile and work properly? Why?

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


# Collections

### How is HashMap implemented internally?

### Why you should always override both `equals()` and `hashCode()`?

### What is the difference between LinkedList and ArrayList? In what cases would you use one instead of the other?

### What is the difference between List and Set?

### What are most popular Java collections? What are differences between them? 


# Concurrency

### What is Thread?

### What is `volatile`?

The Java volatile keyword is used to mark a Java variable as "being stored in main memory". More precisely that means, that every read of a volatile variable will be read from the computer's main memory, and not from the CPU cache, and that every write to a volatile variable will be written to main memory, and not just to the CPU cache.
It is mostly used if one thread is writing to shared variable and another one is reading it.
![image](https://user-images.githubusercontent.com/24475158/125336621-4f296800-e34e-11eb-9874-4bc67143389f.png)

### When is `volatile` not enough? What else should be use then?

If two or more threads are both reading and writing to shared variable, then using `volatile` keyword for that is not enough, because it is not causing different threads to block other operations. In that case you must use `synchronized`, which will guarantee that the read and write operations are atomic. Alternatively, you may consider using data types found in `java.util.concurrent` package, like for example `AtomicLong` or `AromicReference`.

### How to spawn new thread in Java? 

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

### Advantages and disadvantages of concurrent programming.

### How to write thread-safe application?


# Spring Framework

### What is the difference between Spring and SpringBoot?
**Spring Framework** provides comprehensive infrastructure support for developing Java applications. It comes with features like Dependency Injection and offers various modules that can simplify common use cases and reduce amount of necessary of boilerplate code. Some of most known modules are:
- Spring Web - provides support for building REST APIs
- Spring Data - easy access to databases, ready to use CRUD repositories
- Spring Security - authentication and authorization
- Spring Cloud - support for distributed applications

**Spring Boot** is just another module which is part of Spring Framework. It eliminates even more boilerplate code, by providing set of default autoconfigurations, that covers most of everyday scenarios, but of course, can be overriden if necessary. It also comes with embedded Tomcat, therefore it eliminates the need of running external application server.

### How Spring Boot works internally? 

### Explain Inversion of Control and Dependency Injection.

### What types of Dependency Injection in Spring you know?

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

### What are the benefits of different Dependency Injection methods in Spring?

### How to externalize variables in property files in Spring Boot application?

### What is the use of `@ConfigurationProperties`?

### What is Zookeeper?

### What is wrong with this code?
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

**Answer: ** `@Transactional` works only when annotated method is public and invoked from another bean. Otherwise, the annotation will be silently ignored.
At high level, Spring creates proxies for all the class / methods annotated with `@Transactional`. The proxy allows the framework to inject transactional logic before and after running the method. 

### What is the difference between JPA, Hibernate and Spring Data?
**JPA - Java Persistance API** - specification that describes the management of relation data in applications using Java SE/EE. 
**Hibernate** - implementation of the JPA specification.
**SpringData** - abstraction layer on top of Hibernate or any other JPA implementation, which makes it easier to use


# Testing

### What kind of tests do you know? 

### What is the difference between `spy`, `stub` and `mock`? 


# Other

### What is CI/CD?

### What is Maven?

### How would you investigate poor performance of application?

### How would you investigate poor performance of database?

### What is the difference between `git merge` and `git rebase`?
