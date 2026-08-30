## *Lecture 1 Introduction and Overview*





## *Lecture 2 Syntax - The Form of Languages*


### Paradigms of Programming

Programming paradigm:
- Style of the language, what way the solutions to problems are implemented.
- Involves extensive use of some constructs of a language and discourages or prohibits the use of others.



Imperative programming:
- Program is a sequence of statements.
- Close to the language of underlying hardware.
- Examples: Fortran, Algol, Cobol, Pascal, C

Procedural programming:
- Program is organized into procedures or subroutines.
- Procedures group sequences of statements into reusable units.
- Emphasizes step-by-step decomposition of a problem into smaller tasks.
- Data and procedures are usually kept separate.
- Examples: C, Pascal, Fortran, Ada.


Object oriented programming:
- Program defines a set of objects that are encapsulations of data and methods.
- Separating class and instance of class.
- Separation of state and behavior.
- Also has inheritance and interfaces.
- Examples: Java, C++, C#, Kotlin, Ruby.

Functional programming:
- Defines set of functions.
- Functions can be both arguments and return values from function calls.
- Pure functional programming: no mutable variables, and no I/O.
- Examples: Haskell, lisp, scheme.

Declarative programming:
- Program describes what result should be computed rather than how to compute it step by step.
- Focuses on expressing relationships, constraints, or desired outcomes.
- Execution strategy is largely handled by the language/runtime.
- Often contrasts with imperative programming.
- Examples: SQL, HTML, Prolog, regular expressions.

Logic programming:
- Program is set of logical assertions.
- An execution of a program follows an inference pattern to prove or disprove a query.
- Examples: Prolog , Datalog, Mercury, OWL, Semantic web languages.

Event-driven programming:
- Program execution is driven by events.
- Events can include user input, messages, sensor input, network responses, or timers.
- Event handlers or callbacks specify what should happen when an event occurs.
- Common in graphical user interfaces and web applications.
- Examples: JavaScript, C#, Java, Swift.

Concurrent/parallel programming:
- Multiple computations can make progress during the same period of time.
- Concurrent programming manages multiple tasks that may overlap in execution.
- Parallel programming executes multiple computations simultaneously, usually on multiple CPU cores or processors.
- Common mechanisms include threads, processes, message passing, actors, and asynchronous tasks.
- Examples: Go, Erlang, Java, C++, Rust.

Reactive programming:
- Program reacts automatically to changes in data or incoming events.
- Data is often represented as streams that change over time.
- Changes are propagated to dependent parts of the program.
- Common in interactive applications, user interfaces, and asynchronous systems.
- Examples: RxJava, RxJS, Reactor, Elm.



Why care about programming paradigms?
-  Various programming paradigms are supported by various features of programming languages.
- Specific problems are best solved using some, but not other paradigms.

Why OZ?
- Oz is a multiparadigm language: no need to learn more than one language to learn the features and paradigms covered in the book.
- While definitely not a mainstream language, acquaintance with Oz will make it much easier for you to learn languages such as Haskell, Erlang, or Scala, which are quite different from Java or C.



### Programming Languages Specifications

Syntax:
- Definition of the form of programs in a language. Specifies which sequences of symbols are valid (are programs), and which are not.
- Informal specification: Free-text description.
- Formal specification: Formal grammars.

Semantics:
- Definition of the meaning of programs in a language. Specifies what the computer has to do during an execution of a program.

Formal language:
![[{86B53448-9C23-4279-8CFC-FB74EA7B2BF9}.png]]

![[{890FB7F2-E0E3-461B-9934-FEF087693AC6}.png]]




