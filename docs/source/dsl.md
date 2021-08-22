### Chapter 1

- Separation of common code from variable code or configuration code. 

#### Configuration code using xml

- Ship jar with XML to be read when the machine starts up. 

- Any changes to the behavior can be done without having to distribute a new JAR.  

- Declarative approach in many ways reads much more clearly. 
- Express configuration with limitations reduces mistakes.
- Feels like more programming with XML compared to a programming language.

#### Configuration code using custom syntax

- Easier to write and, easier to read compared to XML.
- Language is a domain-specific language and suitable  for a very narrow purpose. 
- While there are difficulties in building a DSL that acts as a communication medium with domain experts and business analysts, the benefit of bridging the communication gap is worth it.

#### External DSL

DSL represented in a separate language to the main programming language it’s working with. External DSL will usually be parsed by a code in the host application. Examples of external DSLs include regex, SQL, Awk, and XML configuration files like Struts and Hibernate.

#### Internal DSL

DSL represented within the syntax of a general-purpose language. E.g. use of general-purpose language for method chaining or fluent interface. DSL only uses a subset of the language’s features in a particular style to handle one small aspect of the overall system. The result should have the feel of a custom language, rather than its host language. 

#### Semantics

You often hear them talk about syntax and semantics. The syntax captures the legal expressions of the program — everything that in the DSL is captured by the grammar. The semantics of a program explains what it does when it executes.  Semantic Model is a vital part of a DSL and you should almost always use a Semantic Model. Semantic Model provides a clear separation of concerns between parsing a language and the resulting semantics; and can enhance the capabilities of a model. 

### Implementing Internal DSL

- You are constrained by your host language. In case of Java - also called fluent interface. 
- Helps emphasis on readability of DSL, but can be continual rehash of personal preferences

#### Command-query separation

Principle that various methods should be divided into commands and queries;

- query returns a value, but has no side effect - call multiple times, change order of using them
- command may have a side effect, but doesn’t return a value

#### Method chaining

```java
computer()
    .processor()
    	.core(2)
    	.speed(2500)
    	.i5()
    .disk()
    	.size(150)
    	.speed(7200)
    	.sata()
    .end();
```

- Breaks the command-query separation model as each method alters the state and returns an object to continue the chain. So method chaining interfaces are different from command-query interfaces - creates design problems for separation of concerns. 
- Use Expression Builders to maintain the separation of command-query and method chaining interfaces, hence avoid using both patterns on the same object.
- With chaining, method names should be less about individual methods, and more about the overall sentence they can form

#### Function sequence

```java
computer();
    processor();
    	core(2);
    	speed(2500);
    	i5();
    disk();
    	size(150);
    	speed(7200);
    	sata();
```

- What should we use - method chaining or function sequence?
- With method chaining, the method are necessary only in the context of chaining
- With use of functions in a sequence, you have to ensure that the functions resolve properly. For example `size()` needs to know the context of which disk is the size to be executed on. 
  - Use Object scoping, nested functions or Context variables to resolve the parsing logic - but avoid context variables if possible.

#### Nested function

```java
computer(
    processor(
        core(2),
        speed(2500),
        i386
    ),
    disk(
        size(75),
        speed(7200),
        SATA
    )
);
```

