# Spring Boot

#Top 15  must to know keywords:

##In this blog we'll look together to 15 different main keywords that you must know about spring boot, these topics are simple but maybe confusing for the first time, so

let's start:

1. Bean

A bean in spring boot is an instance or object from a class managed by IoC container

2. Inversion of Control (IoC)

In a traditional java program, you will create your own classes and create instances to use the class
but in spring boot you define a class, and spring boot will automatically create and manage an instance or bean for this class
so the framework will take control to manage your beans

You only have to tell Springboot that you want this class simply by using annotations like @Controlle, @Service, @Repository @Bean...


3. Dependency injection (DI)

Now we know that spring will control all the beans in our application, so what we should do if class A depends on some class B

in a traditional way we will write the following:
A a = new A(new B());

but in SpringBoot We just tell the Spring IoC container to inject the Bean where we want, the code will be:

```java
class A {
    	  @Autowired
    	  B b;
    }
```

the annotation @Autowired = "hey please inject b here"


4. Auto Configuration 

By default spring boot have a lot of auto configurations, for example spring boot will scan your files to detect what beans you want to create,  inject dependencies
configure a web server to run the program, configure database

5. Starters

A starter is simply a collection of dependencies related to a certain task.
In order to avoid complications and headache when installing dependencies starters are here to simplify this task

for example you have spring-starter-web which will include dependencies related to web pages, a web server, support for defining controllers, rest services... 
also you don't have to specify a version, the starter will use your spring version in order to avoid conflicts between versions.

for a full list of starters check the spring initializer.


6. Annotations 

Spring boot relies on annotations in order to simplify your code, you only have to add annotations and the magic will happen on the background

for example annotations to create a class:
@Controller, @Service, @Repository...
annotations for DI: @Autowired...
annotations for validations and much more


7.  Layers

This not really a Spring boot keyword but usually you will define three layers in a web application 

Controller layer: this is the layer where you define your routes, if a request matches a route name you will enter the correspendant  function

``` java
@Controller
class App{
   @GetMapping("test")
   private String test(){
     return "success";
   }
}
```
In this example when the client send a request to server_ip:port/test he will get a response of "success"


Service layer: usually we don't return immediately in the controller like the previous example instead we have some business logic to execute this is where the service become handy, the Controller will call the methods from the serive layer

```java
@Service
class AppService{
   business logic here
}
```

Repository: finally the repository layer is the layer that will talk to your database or your data storage it also can handle caching tools like Redis


In summary: Request -> Route -> Controller -> Service -> Repository -> DB


8. Properties file

In the property file you define some configurations for example the database configuration, spring boot will read the properties and do the corresponding work, this is also related to the auto Configuration part of Spring boot.

For example you add the properties `spring.db.url=url`, `spring.db.username=username` and `spring.db.password=password` and spring boot will automatically connect to your db 

You can also add your own properties and inject them in the code 

9. @Configuration

The @Configuration is used to annotate a class,Spring boot will scan this class and check for functions annotated with @Bean, if any found spring boot will
get the returned object from the function and add it to the IoC container

this way is generaly used when defining a bean from an already existing class, for example

```java

@Bean
public PasswordEncoder passwordEncoder(){
    return new BCryptPasswordEncoder();
}
```

in this example we create a password encoder, later we can inject this password encoder were we want


10. Command line runner

Command line runner is an interface that allow you yo execute code on the startup of the program

```java 
 @Bean
  public CommandLineRunner startup() {
      return args -> {
          System.out.println("Hello Spring!!");
      };
   }
```

Hello Spring!!, will be printed at the startup of the application, the command line runner could be used to generate data for database at startup


11.  @Scheduler

A scheduler is used to bind a task to a specified interval of time, just add the annoation @Scheduled(fixedDelay = 1000) to a method

12. JPA

Spring boot has many packages to interact with database, spring-data-jpa is one of them, jpa have a lot of features that will make your life easier while
interacting with the database

  - JpaRespository only by extending this interface you will get a list of handy methods pre-configured for you, to create, update, read and delete records
  - Jpa also can create your tables based on the entity classes that you defined in your application
  - Jpa has query methods you only define the methode name in the interface, and it will be implemented for you, for example you define a method called
    List<Student> findByName(String name); and you don't have to implement it, jpa will implement the code for you

14. Interfaces 

Spring boot will help us to rely on interfaces instead of classes to manage dependencies, this means that class A depends on Interface B,
this is so helpul, A don't care about the implementation of B, so if you decided in the future the change the implementation A won't be affected.

In our code we can use `@Autowride private InterfaceB interfaceB`, Spring boot will inject in this field an instance of class B that implements the interface
B, if the class B is the only implementation, if there is more than one implementation specify which one to inject usin `@Qualifier(classB)`


15. Security

The Spring Security package is a huge package that have a lot of features, encrypting passwords, different authentication types (basic, session based...), a way to
store and retrieve users, filters before the request, permissions and roles, activation users...


A bonus Topic: Error Handling, you catch all the thrown error in a class annotated with @ControllerAdvice, this class is very helpful and simple to manage errors.


#Finally...

Spring boot still has a lot of topis that we didn't cover here, also we covered some topics in a general way, I hope that this blog will helpful for beginner readers


Author: Wajdi Jomaa
Date: 11/May/2024
