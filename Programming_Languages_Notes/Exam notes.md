


# Oz cheatsheet

```Oz
% Compile:
% ozc -x file_name.oz -o fileoutput

% Run
% ozengine ./fileoutput
% ./fileoutput

```


# 2 - Syntax; The Form of Languages


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


Micro dc:
- Numeral: Any positive integer number, seperated by white space.
- Operators: +, -, \*, /
- Display commands: p (print top element) and f (print all top down)
- How:
	- Pushes integers onto stack.
	- Operators perform on top two elements, then pushed onto stack
	- P prints top, f prints all.
### Programming Languages Specifications

Syntax:
- Definition of the form of programs in a language. Specifies which sequences of symbols are valid (are programs), and which are not.
- Informal specification: Free-text description.
- Formal specification: Formal grammars.

Semantics:
- Definition of the meaning of programs in a language. Specifies what the computer has to do during an execution of a program.

Formal language:
![[{EF45916B-2BC3-4A29-B97E-7D992D7E4730}.png]]![[{9CE4D428-9C6B-4F8C-B908-475ACD7CD196}.png]]


Formal grammars:
- Used to specify syntax formally.
- How to generate and construct a syntactically valid string.
- Following examples produces all even-length palindromes over \(\{a,b\}\), including the empty string:
![[{4957CD54-968C-47E5-A85A-38F20433C5BE}.png]]![[{AF0D510D-8646-407B-9D94-062D37678DCF}.png]]Backus-Naur Form:
- Special notation (BNF) which grammars are usually written in.
- Terminals (symbols) written in a monotype font, enclosed in quotation marks etc.
- Non-terminals (variables) are written in italics, using angle brackes etc.
- Rules written as strings which contain a non-terminal, special symbol (::=), and a sequence of terminals/non-terminals or symbol e (empty).
- Use \[] to enclose a subsequence that that appears in the string at most once.
- use \{} to enclose subsequence that appears in the string any number of times.
![[{5C5447D1-C5EC-4744-A7B1-B9B1DF651721}.png]]
![[{CFE83873-7CA5-4FCC-9F09-BC8A78A33412}.png]]

Noam chomsky defined four classes of languages:

![[{08AE6B20-5886-424F-B0F6-7D04EE16D921}.png]]
Regular languages:
- Left side always a single variable.
- Right side either a symbol, a symbol followed by variable or empty.
![[{04E23834-E8B0-4818-9268-BE7D2EBD3649}.png]]

Context-free languages:
- Left side always a single variable.
- Right side any sequence of variables and symbols.
- Example:
```
<S> ::= a<S>b | ε
```

Context-sensitive languages:
- Left side can include both variable and sequence of variables/symbols, where only the the exact sequence can be input.
- Right side inlucdes any sequence of variables and symbols.
- Example: 
```
<S> ::= a<A>b 
a<A>b ::= acb
```

Unconstrained languages:
- Any sequence of variables and symbol from V union S (left side non empty).
- Can delete symbols (neighbours), unlike context sensitive.
- Example:
```
<S>    ::= a<A><B>b
<A><B> ::= ε
```



# 3 - Grammars, Derivations, Parse Trees, Scanning introduction to Oz


### Syntactic Analysis of Programs

How are programs processed:
- Input is a sequence of symbols from the alphabet of characters.
- Lexical analyzer (scanner, lexer, tokenizer): Reads sequence of characters and outputs a sequence of tokens.
- Parser: Reads sequence of tokens and outpus a structured internal representation of the program (syntax tree / parse tree).
- Interpreter/compiler: Processes the syntax tree.
![[{73505A4B-DFFB-4237-B2EC-1C629578D664}.png]]

Derivations:
- Can derive sentences in language L(T), by a grammar T in a sequence of steps. 
- First step is start variable Vs, and last form is a valid sentence of only symbols.
- Leftmost derivation: Each step the leftmost variable is replaced.
- Rightmost derivation: Each step the rightmost variable is replaced.
- There can be no valid derivation, exactly one or more than one.

![[{BDE11D7C-186E-4C02-8A46-7EFA13A08835}.png]]
![[{07498DBC-DCF1-49EC-9F60-AC87E9866413}.png]]

Syntax tree:
- Made from tokens in the parser.
- Used to generate target code in compiled languages or interpreted in interpreted languages.
- Used as an intermidate code, and can be opimized or analyzed.
- Number of trees (levels) can (not always) matter since what actions will be taken and in which order depends on how the program is understood by the compiler.  
- It is important that the syntax is unambigious and is understood by the programmer.

![[{B2839E7D-3A8E-4CBA-BF20-1DC68089B0A1}.png]]
Ambiguity:
- If a program can be parsed to several parse trees.
- A program having more than one leftmost derivation, doesnt mean its automatically unambiguous, since it often is still the same parse tree. 

![[{C362CDA6-6329-475F-8877-B992FB52EAD4}.png|552]]![[{B5C910A2-A97F-4146-8F0E-C17F0BF225EA}.png]]

Avoiding ambiguity:
- 1. solution: Enforcing paranthesis around the expression.
![[{80ACA2F3-E9A2-4B88-8F06-019470DFE8A4}.png]]
- 2. Solution: High priority and low priority operator. 

![[{40BF0E1A-BA5B-484D-A407-90A5C99BC952}.png]]
- 3. Solution: Associativity of operators:
![[{A8DC06C8-2DC6-446B-91A9-2744552BFFE4}.png]]
- 4. solution: Combine all of the above. 

![[{718CB839-3930-458E-BEAB-447FEEBA8A3F}.png]]

Scanning (Lexical analyzer):
- Translate programs from string of characters input into sequence of tokens (words/numbers). 
- Lexemizer takes the string and returns sequence of lexemes. 
- Tokenizer takes lexemes and returns a sequence of tokens (type + value). 

Scanner implementation:
1. Specification of the microsyntax (lexical structure), typically using regex (recognizer that returns bool). Decides what single lexemes are valid.
2. A nondeterministic finite automation (NFA) is built based on the regexes, that recognizes the lexemes of the language.
3. A deterministic finite automation (DFA), equivalent to NFA.
4. The DFA is implemented using a nested control structure that processs the input one character at a time.
![[{2327218B-EB98-4D0E-971D-8D9D6D894E66}.png]]
![[{78B65499-5918-4EA2-ABDE-17C3F5649EFA}.png]]![[{6026C5EA-6F5B-47D6-88CD-169A01E23C23}.png]]
Lex:
- Automatic scanner builder tool.
- Takes lexical specification as input in form of regular expression and returns source code implementing the DFA.
![[{32B1753C-5229-46F9-8635-778098CCA38E}.png]]
### Introduction to Oz

Models of computation:
- A programming language with formally defined syntax and semantics.
- Declarative language: a small subset of Oz.
- Declarative kernel language is a subset of the declarative language.
- All models of computation are extentions of the declarative model, and are realized as extensions of the declarative kernel language.

Declarative model:
- Model is sequential.
- All computations are independent of any external state and are themselves stateless and deterministic.
- Supports both pure functional programming and pure deterministic logic programming.

Kernel language:
- Define the semantics of simple kernel language.
- Contains the constructs essential for the desired model of computation.
- Define a practical language by providing translations frrom its constructs to the constructs in the kernel language.

![[{6878BF1E-FD1C-4CA1-9C76-603178F0D0DF}.png|445]]

Syntax of the Declarative Kernel Language:
- Syntactic constructions in the kernel language are all valid in the practical language, but no vice versa. 
- Practical language provides features to make it easier to use.

![[{FB00B268-5321-451C-8FAF-CA9C1FE06016}.png]]![[{1456DF94-A4BF-4C92-B0AE-54F13B2FF304}.png]]![[{2D83115D-1076-485C-94A5-6123DFC511BC}.png]]![[{A41A2156-B0CA-4FAC-83A0-3075EAD60867}.png]]
``` Oz
% Oz declarative kernel language
% Each example is a separate snippet. Variables must be in scope before use.

% STATEMENTS

skip                                 % Do nothing
skip skip                            % Run two statements in order
local X in X = 42 end                % Declare one local variable
X = Y                                % Unify two variables
X = 42                               % Bind a variable to a value

if B then X = 1 else X = 2 end       % B is a Boolean variable

case R of pair(left:A right:B) then % Match a record and capture its fields
   X = A
else
   X = 0
end

{P X Y}                             % Call procedure P with two arguments
{P}                                 % Call procedure P with no arguments

% VALUES AND PATTERNS

X = Y                               % Identifier
X = 42                              % Integer
X = 3.14                            % Float
X = leaf                            % Atom (a literal and a zero-field record)
X = true                            % Boolean literal (true or false)
X = pair(left:A right:B)            % Record with a literal label and fields

% Each record field has the form feature:Identifier.
% Features can be atoms, Booleans, or integers and must be unique.
% A pattern has the same shape as a record, e.g. leaf or pair(left:A right:B).

P = proc {$ A B} B = A end           % Procedure value with two parameters
P = proc {$} skip end                % Procedure value with no parameters

% OPERATIONS SHOWN SEPARATELY IN THE LECTURE

Sum = A + B                         % Add A and B, then bind Sum
Same = A == B                       % Test equality, then bind Same
```



# 4 - The declarative sequential kernel language (DSKL)


Type:
- Set of values together with a set of operations on these values.
- Example: Integer, double, boolean.
- Built in or user defined (typedef in C/C++)

![[{D1BB9001-13B8-43B8-8A61-7716A1CF9004}.png|536]]

Type checking:
- Validation of programs aiming at detecting illegal application of operations. 
- Static: Performed before a program is executed, like in compilation.
- Dynamic: During execution of program.

```Oz
% Each indented type is a kind of the type above it.
%
% Value
%   Number
%     Int          42                 whole number
%       Char       &H                 character code (72)
%     Float        3.14               floating-point number
%   Record         person(age:A)      labeled data with fields
%     Tuple        pair(1:X 2:Y)      record with numbered fields
%       Literal    leaf               value with zero fields
%         Atom     alice              a symbol
%         Bool     true / false       used in conditions
%       List       [1 2 3]            ordered items
%         String   "Hi"               list of character codes
%   Procedure      proc {$} skip end  code you can call
%
% [1 2 3] and "Hi" are shorthand. nil is the empty list.
```

### The practical language

The practical language:
- On top of the kernel language.
- Syntactic sugar: Syntactic conveniences that allow for more concise and readable syntax.
- Linguistic abstractions: Syntactic conveniences that introduces new programming structures, without extending model of computation.

Syntactic sugar:
- Nested values: Allows syntax of records/patterns to include values as arguments and not only indentifiers.
![[{906113EC-3F0A-4187-9B47-0567FC937BCE}.png]]
- Multiple variable definition: Declare more than one variable in one go instead of nest.
![[{937F31D2-A8B2-4C6C-AF78-9476477898F5}.png]]
- Variable initialization: Declare variables at the same time initialize. 
![[{7BD1E012-BB00-4FAE-BE00-0F6AA577ED6D}.png]]
- Nested markers: Turn statements into expressions.

![[{0AD9EECB-7D34-4245-9C87-C56A0EB283CC}.png]]

Linguistic abstractions:
- Functions: Procedure with return value. Also procedures can be used as if they were functions. 
![[{BD153D04-49D7-45DE-BB90-015502113966}.png]]
![[{283E26AD-EAA0-49A7-BB7D-0576B1488FDC}.png]]
- Record field access. Allows for partial matching of record fields.
![[{9CB31FD0-0799-4F4A-92B8-46B7BD5A4DA3}.png|553]]![[{EB50969F-54FC-4355-BB7F-F1E662AE2F7B}.png|553]]
Furthur extentions and comments:
- Finding label or a record variable with `Record.lable R`.
![[{A378C2A9-10BF-4884-9C9C-34322825760C}.png]]
- Can also use `Record.is R` and `Record.arity R`
![[{82E498DB-AF64-4BBE-9535-14E6838B252D}.png]]
Keyword vs. reserved word:
- Keyword: Lexume with special meaning.
- Reserved word: Set aside for that special use.
- In oz, keywords are reserved words.
- Atom is lexeme matching \[a-z]\[a-zA-Z0-9_]\*, except for reserved words such as local, true and etc.
- '\[\^'\\\\]+' is also an atom.

![[{DCA5807E-950C-4191-A1AD-D73B1C3B8ABB}.png]]


### Declarative programming

DSKL:
- An operation is declarative if, whenever called with the same arguments, it returns the same results independent of any other computation state.
- Alternatively: A program is declarative if it specifies what should be achieved, not how it should be achieved.

Variables and indentifiers:
- Variables have to be declared before use, either local with `local X in` or global with `declare X in`
![[{2442C081-BB8C-469A-BDA4-BE104F4ECE0B}.png]]


Declarative variables (Dataflow):
- In oz, variables are declarative, meaning after binding they cannot be reassigned.

![[{1C26FD7F-69ED-4397-9D5C-F3409F3F4ABC}.png]]
Partial values:
- Complex values that contain unbound variables.

![[{722E4A38-2519-4C69-AC42-C9A7BA683320}.png]]

Unbound values in computation:
- In sequential model of computation it means that the whole execution halts.
- In a concurrent environment, it is just one thread that is kept waiting, while the other threads may provide the needed value.


Browse:
- Seperate thread, therefore not part of the declarative sequential model of computation.
- Therefore, the browse thread will also wait for assignment for unassigned variables before printing. 


Expressions and Statements:
- Expression: is syntactic construct which specifies an operation that evaluates to a value.
- Statement: Syntactic construct which specifies an operation that does not evaluate to a value.
- A program in DSKL is composed exclusively of statements.
![[{8EF1DA0B-1F80-4C2A-AE1D-BBD2403CFD58}.png]]![[{4E24C2C3-D34B-40B0-BF32-89E226EADA44}.png]]
![[{19F6270A-1F8C-491B-B61A-DCB84B382666}.png]]

Functions and procedures:
- Functions are linguistic abstractions over procedures.
- A function of n arguments is a procedure with n+1 arguments.
- A procedure may have more than one result argument, may marked with ?.

![[{4DACB322-78F2-4C96-9829-EA21AEDAFA8A}.png]]
Data structures:
- Atom: `Atom == atom()` is true. `{Label atom} == atom()` is also true. Atom is a record with no fields.
- Tree: Can use a record with left/right fields and tree/leaf values.
![[{024B9316-9CE8-41CA-A3D6-DC7CE27B3BBA}.png]]
![[{7C8172D3-058F-4C2C-8F06-316D069B13FD}.png]]
Records:
- `X#Y#Z` is equal to ` "#"(1:X 2:Y 3:Z)` or simply `"#"(X Y Z)`
- `case X#Y#Z of 1#2#3 then ... end`



Lists:
- Ordered collecion of items.
- Lists are trees where the left child is always a leafnode.
- `[one two three]` or `nil`
- `[]` and `one | two` are not a list
- `[one [two three] ]` nested two element list
- Nested sequence of records.
- `[one two]` is actually `'|' (one '|'(two nil))` and also `one|two|nil`

Pattern matching lists:
- Pattern `[First Second]` is equivalent to `'|'( 1:First 2:'|'(1: Second 2:nil))`
- `one|_` is equivalent to `'|'(1:one 2:_)`
- `Head|Tail` can be used to seperate first element from rest. Head/ tail are just identifires and can be called anything.
- More complex patterns possible.

![[{D74DDFEA-08B2-4094-876A-524BB6981B77}.png]]![[{6F4E6CF4-4FD1-4470-A7C3-3956F28D72C2}.png]]
![[{9274A212-BFFE-4E46-BAA1-5E7AA9433DCE}.png]]

Map:
- `{Map [1 2 3] fun{$ X} X+1 end}` returns `[2 3 4]`
- `{Map [fun {$ X} X end fun {$ X} X+1 end] fun {$ F} {F 1} end}` returns `[1 2]`
  


# 5 - The declarative sequential kernel language (DSKL) abstract Machine


### The abstract kernel machine language

The DSKL abstract machine is a simplified model of a computer that executes DSKL code step by step. It lets you track exactly what happens when a program runs.
- Sequence of states of the VM. S is semantic stack and o is the single assignment store. 
	- Step 1: M(t1) = (S1, o1)
	- Step 2: M(t2) = (S2, o2)
	- Step n: M(tn) = (Sn, on)
![[{58105972-0DDF-4A75-A7E9-11F7DBEFE1A3}.png]]
The semantic stack:
- Semantic stack S is a list of semantic statements:  
	- S = \[s1, s2, ...]
- A semantic statement is a pair of a statement and and environment: 
	- si = (\<statement>i, Ei)
- Environment E is a set of indentifier - variable pairs. 
	- E = {\<id>1 -> v1, \<id>2 -> v2, ...}


Single Assignment store:
- Contains declarative/dataflow variables.
- Operations on these variables allowed:
	- Creation of new unbound variable. 
		- `... declare X in ...`
	- Binding unbound -> unbound variable. 
		- `... X = Y ...`
	- Binding of unbound -> bound variable (value).
		- `... X = tree(leaf leaf) ...`
		- `... X = 3 ...`
	- Unification of two bound variables (of two values).
		- `... X = tree(_ leaf); Y = tree(leaf _); X = Y ...`
	- Binding variable to partial value.
		- `... X = record(Y) ...`
	- Testing whether a variable is bound or not.
		- `... {Browse {IsDet X}} ...` returns true/false if bound/unbound.
	- Retrieving value of bound variable.
		- `... {Browse X} ...`
	- Testing equivalence between two variables.
		- `... {Browse X == Y} ...` returns true if bound to same, false if never have the same value, and suspendend if partial value is equal but not complete.
- Single assignment store is a set of bound/unbound variables.
	- σ = {v1, v2 = 1, v3 = rec(feat : v1), . . .}
- Variable names are not variables, as the same identifier in code can map to different variables in one program execution.



Operations on Environments:
- E = {\<id>1 -> v1, \<id>2 -> v2, ...}
- Adjunction: Adds mappings via replacement or extension
	- E' = E + {\<id>3 -> v3, ...}
![[{35FDF931-B697-4090-8C38-D82AEB567070}.png]]
- Restriction: Removes mappings creating a new reduced environment with only the requested indentifiers:
	- E'  = E | {\<id>, ...}
![[{21110301-A6A3-41DB-B51D-BF4CB3ACDFFF}.png]]
- Lookup: Retrieves variable based on id and environment:
	- v = E(\<id>)


Computations:
- A stepwise execution of the program, one step involves following operations on the abstract machine:
	1. Pop the top semantic statement from stack.
	2. Execute the enclosed statement using mappings in the environment, and bindings in the single assignment store.
	3. Modify semantic stack and store as needed.
	4. Repeat if semantic stack if not empty.
- Empty stack means finished execution, execution step may push one or more semantic statements onto the stack.

Setting up machine:
![[{D6592850-4DEE-4AF6-9C40-C96C0B270DA2}.png]]

Running the machine:
![[{74CD765C-B825-4139-AB2A-DF82B9C144F0}.png]]


### Execution of Statements on the Abstract Machine


Skip statement:
![[{8EC99B28-642C-41AB-8B7A-DABD9EA33DCD}.png]]
![[{8AD7D436-7E6F-48EE-907B-92E5A400C176}.png]]
Local statement:
![[{230C2C09-CF45-4E82-9DED-65FBA2CD8250}.png]]
The variable - variable binding statement:
![[{82BC97C9-7592-4AFB-A926-446C05513AA0}.png]]
Other situations:
![[{334E28A2-1630-46FF-9A75-A2D4A32C062B}.png]]![[{6846BCDB-A4CA-41AF-B810-E0ED2717E202}.png]]
Value Creation statement:
![[{1E945856-98B0-46E2-8BDA-364F5D323AC2}.png]]

Example program:
![[{30EAAF13-31B8-436A-9CDA-4F9945374D49}.png]]![[{35C169F2-300A-46FB-8BE5-07133160FABD}.png]]

Value creation procedure values:
![[{2E90BF75-A766-4BDF-B8D3-9E1E57DE4DCF}.png]]
![[{6C3A460D-2A9A-45B1-8D50-F59785C6A846} 1.png]]
Free identifiers:
![[{DA740B5B-D830-47D2-A3B1-6B7D112D33B1}.png]]



Summary non-freezable statements:
- Skip statement:
![[{70D7A901-D875-41E5-A093-BAE85ADE0859}.png]]
- Sequence statement:
![[{2872BB33-F813-4C97-B99E-2A9F010C735E}.png]]
- Local statement:
![[{24826F84-E0E9-4845-A5D9-B7F812B63F0A}.png]]
- Variable - variable statement:
![[{6C6AA71E-2F41-42F2-AA46-10936DA271DD}.png]]
- Value creation statement:
![[{49A552E8-72DC-4B16-8569-485ADB4F2B13}.png]]

### Execution of Freezable Statements on the Abstract Machine

If statement:
![[{E2492307-14E4-45E4-B4C0-4790211B7C8B}.png]]![[{640DEF38-3F86-4383-9272-5C06B167F1D0}.png]]

Case statement:
![[{8BBC47F8-2325-4058-984C-E1A6526765F2}.png]]
![[{A5316133-606A-4846-85F1-C7738470B1B9}.png]]![[{1FC72204-88A9-4F7E-99AC-96A5DDB35DC6}.png]]

The procedure application statement:
- Formal parameter: Parameter, procedure definition.
- Actual parameter: Arguments, procedure application.
![[{5DCCF1B9-6684-4691-9699-6CC999313493}.png]]
![[{AC325B32-879C-47C9-AE0B-8A97928C9366}.png]]
![[{AB3104CE-1AF3-4EC8-971F-4CE9D475C5CA}.png]]

### Implementing Oz in Oz (Oz^Oz)

Interpreting a program:
- Scanner: Scan the source code to produce a sequence of tokens.
- Parser: Parse the list of tokens to produce a parse tree.
- Interpreter: Process the parse tree, performing actions corresponding to its parts in the correct order.

Implementation:
- We will implement interpretation (from parse tree to execution).
- And instead of building a scanner and parser, we will manually perform these stages.

Interpreter: 
- Takes parse tree as input, corresponding to a program.
- Initializes the virtual machine.
- Iteratively processes the input moving the machine from state to state.
- Each state is completely specified by the content of its semantic stack and single assignment store. 
![[{B1C17D32-261E-4DE6-B47F-0221794D229E}.png]]
![[{3DB5D118-CC3C-4344-989E-7112C6DBD38A}.png]]
Required specifications:
- Encoding of parse trees.
- Structure of semantic stack.
- Structure of single assignment store. 

Parse tree encoding:
- Nested records in a tree.
- Each parsed statement is a partial parse tree represented as a record.
- Each parsed statement is of a certain type, and may have some content. 
- Encode the type in the records label.
- We can encode the content in the record’s fields.

![[{ADD002EF-6E67-4BE5-AF09-5E97A7BB6193}.png]]![[{133B84DF-581F-48C4-BF76-31722127028D}.png]]
![[{4A121191-265A-4D98-9363-EBA4C4385243}.png]]![[{0C846961-B57B-4DE0-B754-9FEC70CFC6E8}.png]]

Representing single assignment store:
- Can represent it as a list of Oz variables.
![[{FC35ECBC-E19B-4791-8A1E-9E05FD0BF6A1}.png]]
Representing an en environment:
- Can represent an environment as a list of identifier-variable index pairs.
![[{06EE3005-B96A-44AE-B9FD-C026BB470982}.png]]

Representing a semantic statement:
- Semantic statements are pairs of parsed statements and environments.
- Can also represent the semantic stack containing the statemens as a list of semantic statements.
- Can represent as a record with two fields:
![[{68C3D60F-A23F-4E6D-87E9-7C9DDB3EFBC6}.png]]

Starting the virtual machine with a parsed program: 
- Create a new empty environment, a empty list.
- Create a semantic statement containing the parsed statement and the initial environment.
- represent the initial semantic stack as a list containing the semantic statement.
- represent the initial single assignment store as the empty list.
![[{BA3E4993-D200-4F3B-AB41-AEF1A70E2D65}.png]]
![[{A5DEEC8D-7603-4E92-B49C-69B81FF4034E}.png]]![[{448690A5-B0CE-4CF8-9033-86067F015AF8}.png]]![[{5DA6C790-6A90-4C7C-96CF-24B59389BDDD}.png]]![[{B2D909B7-560E-42C1-A176-3B005B73AA31}.png]]![[{A3020849-1525-4851-A49A-ABC7E1885EAE}.png]]



# 6 - Memory, Bindings, Scope, Recursion, Exceptions and Intro to Higher-order Programming 


## Memory managment, bindings and scope


### Bindings and binding times

Binding:
- The relation between a syntactic element and its meaning during an execution/compiling.

Other definitions:
```oz
local X in
    X = 42
end
```
- Mapping: Identifier (x) -> variable (v1).
- Binding: Variable (v1) -> value (5).

Binding time: 
- Time at which the meaning of a syntactic entity is established. 
- We usually distinguish 6 (or more) key time points where the meaning of syntactic element can be established. All except run time bindings are "early binding", while run time binding is "late binding".

1. Language definition time bindings:
	- The syntactic entities is provieded as part of the specification of language. Such as control flow constructs, type specifiers and more.
	- In java: `void`, `static`, `int`, `if` and other.

2. Language implementation time bindings:
	- The meaning is provided in the specific implementation of the language.
	- In C, the keyword `int` isnt specified until the implementation time, as the size is implementation dependent.

3. Compile time bindings:
	- The meaning of syntactic entities is established when the compiler produces object code. 
	- In C, some identifires (constants, global and static variables), may be mapped onto memory locations at compile time.

4. Link time bindings:
	- The meaning of syntactic entities is established when the object code is linked with eg static libraries. This can be refrences to external variables, procedures, functions. 

5. Load time binding:
	- The meaning of syntactic entities is established when the compiled and linked program is loaded into memory, still prior to execution. The physical/virtual addresses of variables, procedures etc. are typically resolved here.

6. Run time bindings:
	- The meaning of entities is established when the program is actually executed. 
	- Example: The location and content of local variables.
	- Scripting and interpreted languages typically rely more on run time binding.


### Memory allocation

```c

int x = 10; // static

int exponentiation(int a){ // parameter: stack or register
	static int exponentiationCount = 0; // static
	exponentiationCount++;
	
	int b = a * a; // Stack or register
	return b;
}

int main (){
	int* a = malloc(sizeof(int)); // pointer in stack, int in heap
	*a = x/2; 
	
	int a_squared = exponentiation(*a); // stack
	
	free(a);
	return 0;
}
```

Static memory:
- The location of objects in memory is fixed at load time, and allocation persists throughout the whole execution of the program.
- Globals, Constants, literals, classic-static variables, static - procedure local variables etc.


Heap (dynamic memory):
- The allocation of objects in memory is done at the run time within a region of memory called the ‘heap’.
- Memory blocks within the heap are organized into regions of various sizes, and can be allocated and deallocated at arbitrary times and in arbitrary order.
- Usually used for dynamically linked data structures (e.g., linked lists, trees, etc.), and for dynamically resizable objects (e.g., arrays in Perl).

Stack memory:
- Allocation of objects is done at compile time, in a last in first out order.
- A procedure typically uses a call frame on the stack to contain the callers address, local variables, result variables, arguments for furthur calls and more.
- Higher-order programming (with procedures as arguments and return values) requires a more complicated scheme.

Memory in Oz:
- semantic statements on the semantic stack correspond to stack-allocated activation records (call frames).
- variables and values in the single assignment store correspond to heap-allocated objects.

### Memory management

Automatic:
- Memory can be both automatically allocated and deallocated (stack and static). 
- typically stored within a function’s stack frame. Each call creates its own instances, and returning releases their storage. A function can return an object’s value without keeping the original local object alive.

Manual:
- (Heap) Blocks of memory are allocated and deallocated as a result of the use, within a program, of syntactic constructs for explicit memory allocation. `malloc` and `free` in C or `new` and `delete` in CPP.

Manual allocation, Automatic deallocation:
- Objects created with `new` on java are allocated a pointer on the stack and the actual object is in heap memory. These objects will be automatically deallocated by the garbage collector when no longer reachable.

Lifetime: 
- Object lifetime: The time between the allocation (automatic or manual) and the deallocation (automatic or manual) of a piece of memory occupied by a run-time entity (in java and other modern programming languages: removed from memory by a garbage collector at an unspecified time after they are no longer needed).
- Reference lifetime: Reference lifetime is how long a particular name-to-runtime-entity mapping exists.

### Lexical and dynamic Scope

Lexical/static scope:
- Lexical/static scope: names are resolved using the surrounding structure of the source code.

Dynamic scope:
- names are resolved using the active chain of function calls.

Examples:
- Here, P would print 1, as environements are immutable.
![[{10F27C7A-3939-4F70-80C2-792C99187589}.png|491]]
- This will print 1.
![[{BA2C1FF6-F9BE-4A03-8A26-F351F7E415CD}.png]]
- In oz: Oz environments are immutable. Adding or replacing a mapping creates a new environment.
![[{8E50680C-331C-407D-97A3-B83180FC89E9}.png]]
- Example dynamic:
![[{FFD7EB4D-8B73-4C17-AB76-87B1B2B6C91B}.png]]

### Scope and object lifetime

Languages with garbage collection (implicit deallocation of heap):
- Java, Python, Oz.
- Objects persist for at least as long as there is some reference to it.
- After, the object space will be reclaimed at unspecified time.
- After the case statement, there is no reference to the list produced by Enumerate, and the whole structure can be removed from heap.
![[{C16C20C1-F360-4A02-B217-1AA105755C45}.png]]

Languages without garbage collection (C/C++):
- Dynamic memory has to be freed when not using, or when the reference to the memory is no longer accessible.

## Recursion, Iteration and Last Call Optimization

What is recursion:
- See recursion.
- Concept in mathematics and computer science.
- Recursive computation: A procedures result depends on the result of each call depends on the subsequent calls, usually with some condition to end the recursivity. The nested call must not be the last statement to be executed within the body.
- In oz: The semantic stack in the abstract machine grows with each nested call and shrinks when nested calls are exited.

![[{88B5A530-D6F3-4465-AF90-8FE80AE4BABC}.png]]

Iteration and tail recursion:
- Tail recursive funcitons in Oz are last call optimized, meaning the function becomes iterative since the function returns immedatly after making the new call.
- Stack kept constant in size instead of growing lineraly and shrinking like with recursive.
- Following example: There is no unfinished multiplication waiting after a recursive call returns.
- With Oz’s last-call optimization, the current call’s execution context can be discarded when proceeding to the next call. Pending calls do not accumulate on the stack
![[{28260B52-7976-4490-8D5C-E8B80D1931B9}.png]]
Kernel-ish version:
![[{3815A1E9-D5A7-439A-84AD-165DCFCE6FBF}.png]]
Summary:
![[{6CBA5BE0-FAF8-4BDE-A348-D8DB663BB2CD}.png]]

```oz
local Factorial in
	local N in
		local Result in
		    Factorial =
		        proc {$ N Result}
		            local Iterate in
		            local Iterator in
		            local Accumulator in
		                Iterate =
		                    proc {$ Iterator Accumulator}
		                        local Test in
		                            Test = Iterator > N
		
		                            if Test then
		                                Result = Accumulator
		                            else
		                                local One in
		                                local NextIterator in
		                                local UpdatedAccumulator in
		                                    One = 1
		                                    NextIterator = Iterator + One
		                                    UpdatedAccumulator =
		                                        Accumulator * Iterator
		
		                                    {Iterate NextIterator
		                                             UpdatedAccumulator}
		                                end
		                                end
		                                end
		                            end
		                        end
		                    end
		
		                Iterator = 2
		                Accumulator = 1
		                {Iterate Iterator Accumulator}
		            end
		            end
		            end
		        end
		
		    N = 3
		    {Factorial N Result}
		end
	end
end
```


![[{788A98A2-1F51-4008-9475-6319E8879F59}.png]]
Another example:
- Recursive:
![[{533ECB23-6F2F-4A1E-820F-4D5BFD95F46D}.png]]
- Tail recursive / iterative:
![[{0021BE97-2CAA-4E96-B97A-71E70D483609}.png]]


## Exceptions

Context:
- A semantic statement on the stack; a statement paired with its environment. A catching context contains an exception handler that can handle a problem raised during execution.

An exception:
- Signals an abnormal condition and transfers control to a handler.
- Exceptions can carry information about the problem, such as an atom or a record.
- A computation meets a abnormal condition, it can recover by jumping out of execution contexts to the exception contexts (like goto statements).

![[{DAE32D4A-674D-4F58-874B-31993F73BD3F}.png]]
![[{C475D7ED-5FA1-4EF3-989F-76F2B3088D87}.png]]
![[{64FFBBF1-D8BD-4E94-9B4C-AE19CD83214B}.png]]
![[{83455F56-72A6-4030-A8F4-C1774400C8DF}.png]]
## Higher order programming

### Introduction

What is higher order programming:
- Programming with procedures as first-class objects, can be:
	- arguments
	- return values
	- elements in data structures
- A first-order procedure is a procedure where no argument is a procedure.
- A n+1 order procedure is a procedure where at least one argument is a n order procedure.


### Generecity:

- A procedure that takes another procedure as an argument.
- The procedure is generic, the details depend on the procedure-argument.
![[{7EA17481-024D-4DE8-B108-567957D4972B}.png]]
![[{06762B36-87B1-46D8-B762-C3AB88FDB8E6}.png]]![[{DB5DFEB5-704F-4EDD-91E3-2CD535AB7B6D}.png]]
![[{8DB3C11D-B750-461F-A049-D3DA7E9004BF}.png]]![[{28B110C2-544D-41C5-A9FC-AD9570FC19B4}.png]]

### Instantiation

![[{39A1B54D-0BFB-469F-A646-43D6A46286F5}.png]]


# 7 - Higher-Order and Functional Programming, Delcarative Data Structures


## Declarative Programming

### Higher order and Functional programming

Higher order programming (HOP):
- is a programming paradigm where procedures/functions are first class objects and can be;
- Passed as arguments.
- Returned as values.
- Embedded as elements in data structures.

Functional Programming:
- Programming paradigm based on implementing mathematical functions.
- Corresponds roughly to declarative programming.

### Procedural abstractions:

FP and HOP are based, among other, on following four principles:
- Procedural abstraction: Any piece of code can be wrapped into a procedure and reused later by invokation.
- Generecity: A procedure can be generalized by abstracting some of the code in the body to a argument that must be supplied.
- Instantiation: Instead of producing direct results, a generic procedure can return more specific procedures that when called with appropriate arguments, compute the final result.
![[{D8DD5390-8769-464A-8278-187ED5EFFADB}.png]]
- Embedding: Procedures can be stored in data structures, which makes it possible to call a number of procedures in a loop.


### Control abstractions for HOP/FP
- A control abstraction packages a pattern of execution, while letting supplied functions specify the work.
Map tail:
![[{B7B8A6D0-E40F-425A-9072-409BBC1EDE58}.png]]![[{577395E4-90C1-4918-B359-AF10DC4012C3} 1.png]]

Matrix:
![[{0B334B31-901F-4164-9222-FCF473F388D6}.png]]![[{9E7D5850-B299-4682-8B37-83428BFEEA64}.png]]![[{DEC8D28A-FBD3-41E0-92B1-3C17964EE9FE}.png]]
Different control abstractions:
- Map: Apply function to every element in list.
- MapApply: Apply every function in a list to one value.
- MapApplyMap: Apply every function to every value, grouping results by value.
- Cross: Combine every element of one list with every element of another.
### Cryptography with HOP/FP

- The lecture shows how to encrypt a message by changing its letters, then uses functions like Map to do that work.

```Oz
%Character - &a: turn the letter into a position from 0 to 25.
% + Key: move forward.
% mod 26: wrap around when you pass z.
% + &A: turn the position into an uppercase letter.

fun {Encrypt Plaintext Key}
   {Map Plaintext
      fun {$ Character}
         (Character - &a + Key) mod 26 + &A
      end}
end

fun {Decrypt Cryptogram Key}
   {Map Cryptogram
      fun {$ Character}
         (Character - &A - Key + 26) mod 26 + &a
      end}
end

fun {MakeCryptex Key}
   cryptex(
      encrypt:fun {$ Text} {Encrypt Text Key} end
      decrypt:fun {$ Text} {Decrypt Text Key} end
   )
end
```



### More HOPping

Producer:
- Creates a list from interval.
- `{Enumerate 1 4}` -> `[1 2 3 4]`

Transducer: 
- `[1 2 3]` -> `[2 4 6]`

Consumer:
- Takes list and returns one result.
- `{Sum [1 2 3]}` → `6`

Filter:
- Calls a supplied test on every element, then chooses elements to keep.
- `{Filter [1 2 3 4 5] IsOdd}  % [1 3 5]`


Zip:
- Pairs corresponding elements by index.
- `{Zip [1 2 3] [10 20 30]} % [1#10 2#20 3#30]`

Unzip:
- Unzip separates the pairs back into two lists.
- To return two lists, as Unzip does, put them together inside a tuple or another structure.
- `{Unzip [1#10 2#20 3#30]} % [1 2 3] [10 20 30] `


Mux:
- Combine elements from several lists.
```
[1 2 3]
[10 20 30]
     ↓
[11 22 33]
```

Demux:
- split each element into parts and collect those parts into separate lists.
- To return two lists put them together inside a tuple or another structure.

Min/max:
- Using generecity to compare elements and return the correct candidate.


FoldLeft:

```
fun {LeftFold List Op Acc}
   case List of nil then
      Acc
   [] H|T then
      {LeftFold T Op {Op Acc H}}
   end
end
```

Difference List:

```
declare Start End NextEnd D1 D2

Start = 1|2|End
D1 = Start#End

End = 3|NextEnd
D2 = Start#NextEnd

NextEnd = nil

{Browse Start}  % [1 2 3]
```


## Declarative data structures

### Abstract data types

Declarative data structure:
- Data structure that is immutable.

Abstract data types:
- What operations are allowed, and how does  the data type/structure behace.
![[{91F9A6DC-489C-4D2B-8CEF-18AD326AC81A}.png]]
![[{79DB6CF5-03B0-4038-9FE7-5A292059041D}.png]]![[{9F3C3239-BFB5-4A02-BD43-8103886A0A6D}.png]]


### Variations on a Queue

ADT implementation specifications:
- Bundled: Operations embedded in the data structures.
- Unbundled: Operations applied to instanced from outside.
- Secure: Only access is through the specified operations.
- Insecure: Open access to the underlying representations.


### Queue implementations:

Declarative Unbundled insecure:
- Output: `Start#End#Length` tuple
- Usage: `{Enqueue Q a}`
- Simple, but callers can access the open end and bind it incorrectly, breaking the queue.
![[{0AD8A07E-D94B-4DC1-ACBE-698B42C834CC}.png]]![[{B6071BE4-081F-417B-A3BD-817C8C9FBF34}.png]]![[{50124065-2BD9-4404-B891-5A1DCC305835}.png]]![[{F9820E49-4A34-4BFE-8C5B-C64EC2A8620B}.png]]![[{680567F3-0090-42D5-B157-9002080BAFD7}.png]]
Declarative unbundled secure:
- Output: The tuple hidden behind Wrap.
- Usage: `{Enqueue Q a}`
- Separate queue functions privately use `Safe.unwrap` to access the data, then wrap the result again.
![[{8EB697B5-10FD-428F-814B-B01D88763D21}.png]]![[{F85C2090-F906-43A7-95B1-17AA698A26F7}.png]]
Declarative bundled secure queue:
- Output: A record containing `enqueue`, `dequeue`, and `isEmpty` functions.
- Usage: `{Q.enqueue a}`
- Each function privately remembers its queue data. No separate wrapper or secret key is needed.
![[{1DDB9F77-CFB5-464D-AA24-7D07CBA490D4}.png]]![[{867ADBB8-ABF1-4C44-9E2F-833A22146021}.png]]
Bundled secure with preview:
- Output: The same record but with preview function.
- Usage: `{Q.preview}`
- Lets callers examine the stored list while protecting its open ends with `!!`
![[{EBEA788A-1846-4EFB-827E-E2E988F49121}.png]]
Bundled, secure with dispatch:
- Output: One function that accepts an operation request.
- Usage: `{Q enqueue(a)}`
- One function remembers the queue data and selects the requested operation. The actual operation functions are shared between queues.
![[{F5253DA8-3D3A-48C6-B376-6FF20CD786D2} 1.png]]

# 8 - Declarative Concurrency, streams.

### Limitations of Sequential model of computation

The sequential model of computations:
![[{40E0CE4D-BE74-4E23-9572-C1C88ACB595C}.png]]![[{EDFBF34E-406A-40BF-820A-575D7CAF0919}.png]]
Limitations of sequential model:
- A later function can stop computation if a argument is not sucessfully parsed, meaning the earlier functions were wasted computations.
- In concurrent model: Peformance can be improved by parallelizing the computation, and freezanle statements are useful for synchronizing the threads.
- Eg: following example freezes if X is unbound.
![[{E4339200-D23B-4621-ABC7-F77FA2C4C679}.png]]

### Declarative concurrency

DCKL:
- The declarative concurrent model of computation.
- An extension of the declarative sequential model.


Two ways to extend the DSKL with concurrency:
- Data-driven concurrency: Thread of computation can be executed as soon as it has all data it needs.
- Demand-driven concurrency: A thread of computation can be executed no sooner than when the result of its execution is needed by another thread.


Example data drive concurrency:
- Displays list `[0 1 2 3 4 5 6 7 8 9]`
![[{28C16F35-3D7C-4A8C-9810-65E73EB30615}.png]]![[{A0B44BD9-7744-4A97-811B-26CD5810562D}.png]]
- The following example; (1) only shows when the complete list has been constructed. (2) we first see record `_|_` created in map, then iteratively extended for each fib number computed.
![[{3B88B978-F4C5-43BE-9AA6-507E9583785B}.png]]![[{41934AAD-5384-429C-98E6-983F33E97EE9}.png]]

Concurrent map:
- Spawns a new thread for each element in map.
![[{F76964C0-7C4B-421A-9BFB-557E3F7FDD32}.png]]![[{4EF22E03-96B4-4F80-8893-F2D94D9A3139}.png]]


### The Declarative Concurrent Model of Computation
![[{15908C9F-8BB4-4C0C-941C-C76A2232747E}.png]]![[{FF13D901-A666-4FEC-9D2C-CF57CB2E59BA}.png]]![[{4CD8FAFD-542A-4AA9-AEFD-7788D476018A}.png]]![[{E6DFDA6B-D4D6-4679-A5DE-1B8555DCEBF5}.png]]
Semantics for computing with threads:
- A semantic statement is excutable if the top statement is not suspended/frozen.
- At each step of computation, an executable semantic stack is chosen where the topmost statement is executed.
- The choice of stack is not specified by the semantics, but a detail of the scheduler. We assume that the scheduler is fair.
- A semantic stack is removed when empty, and computation is finished when no stacks left.
![[{84AB3BC5-ADF3-4DF6-A2CD-E0FFE955F6AA}.png]]
![[{35B912CB-262F-4034-9909-0B2E5213350F}.png]]


### Concurrency, Determinism and Declarativness

Dependencies:
- Which statements are executed in order, and which are dependent?

Non determinism:
- A feature of computation which, at a certain point, can arbitrarily choose among two or more ways to proceed.
- We can only predict the ways a computation can proceed (dependencies and program order), but not the exact way.

To be declarative:
- All executions must have same result (or failure).

Data driven concurrent model:
- Is declarative as result is always the same or failed, but the order of computation might vary.
- A non deterministic program can therefore be declarative.

Exceptions:
- With exceptions in the extended DSKL model wit concurrency, this leads to a non delclarative model of computation.
- This is because the unification failure (of binding same variable in seperate threads) doesnt crash the program, and the execution is therefore random/non deterministic as to which unification executed first.

Challenging the CTMCP:
- Is this declarative? 
- Implicit exception/error handling use?
- Only the thread that fails stops, not the working one.
![[{B1614EA8-AE4E-4308-8E3A-1270170D7D8C}.png]]

Deadlock:
![[{1251AB72-BCDB-4C95-8E61-E974490C3DF0}.png]]


The thread module:
- May lead to non-declarativeness.
![[{42DA914C-E818-416F-8AEC-D6750CFDBD87}.png]]
![[{863113B6-6CF8-4A2A-9BFE-B3A11B95B28B}.png]]
![[{CDEBCBDA-C2FA-4043-860B-CBC8496B291E}.png]]

### Streams

A stream:
- Potentially infinite data structure.
- Elements can be read from the beginning.
- Elements can be added to the end.
- Both read and extended.
- Producer: A process that adds elements to the stream.
- Consumer: A process (or multiple) that reads element from the producer.

How:
- Can either have a producer and consumer at the same time in seperate threads (data driven).
- Or; Have the form of a sequence of delayed computations that are enforced/produced when there is a demand (demand driven). Can be realized by lazy execution.

Benefits:
- Stream can be consumed without it being finished produced. 
- Not necessary to keep all elements of stream in memory.
- Performance can be improved, if the producer/consumer split cpu time or they run on different cores.
- possible to perform operations on potentially infinite amounts of data.

![[{DDA1AC1B-89B5-4C69-9637-42A5ECB8D7CF}.png]]
![[{BB836AD8-C04F-4AB2-9727-ABAD1FFE7A5A}.png]]

Foldright:
- StreamMap can produce output one element at a time, even for an infinite input.
![[{7583704A-A6CA-4B5D-B261-CCB0A8296D8E}.png]]

```Oz
declare Produce Consume Cons Numbers Doubled

fun {Cons Head Tail}
   Head|Tail
end

% PRODUCER: supply 1, 2, 3, then finish.
fun {Produce N}
   if N>3 then nil
   else
      {Delay 500}                 % Wait half a second
      N|{Produce N+1}
   end
end

% CONSUMER: read and display each available item.
proc {Consume Stream}
   case Stream
   of nil then skip               % Stream has ended
   [] Head|Tail then
      {Browse Head}
      {Consume Tail}
   end
end

% Run the three stages:
thread Numbers = {Produce 1} end

thread
   Doubled = {StreamMap Numbers fun {$ X} X*2 end}
end

thread {Consume Doubled} end
```

Foldleft:
- Its not ready to before the end of input is complete.
- Therefore doesnt work with infinite stream, only finite.

Fib:
```Oz
declare StreamAdd Fibs

fun {StreamAdd Xs Ys}
   case Xs#Ys
   of (X|Xr)#(Y|Yr) then
      (X+Y)|thread {StreamAdd Xr Yr} end
   else nil
   end
end

Fibs = 1|1|{StreamAdd Fibs Fibs.2}

{Browse {List.take Fibs 10}}
% [1 1 2 3 5 8 13 21 34 55]
```




# 9 - Streams, lazy execution

## Demand driven concurrency

### Stream pipelines

Performance of producer-stream-consumer pipeline:
- May run out of memory if producer is faster than consumer, unless the producers stops.
- If else; The consumer thread may be idling a lot.
### Demand driven Stream pipelines
- A solution to the problem of running out of memory, due to producer speeding ahead.

How:
1. The producer freezes over an unbound variable at the end of the stream. 
2. Consumber then adds a new unbound variable (a demand) as an element to the stream.
3. The producer binds the variable to an item, and waits for the next demand.
![[{19D65D3D-029B-4281-B9E4-BC869677A71F} 1.png]]
![[{9CD029CA-20C3-48D8-8345-550761DB4928}.png]]
### Buffered Stream Pipelines




## Lazy evaluation

### The demand driven model of computation





# 10 - Logic Programming

![[{FD59DE34-44CC-4CDE-A23E-06AF5F6A9A41}.png]]
## Facts rules and queries

Clauses:
- A fact or rule, marked stop with a full stop '.'

Predicate and atom:
- `Predicate(atom)`, where predicate is the name of predicate and atom is the subject who is 'predicate'.
- Atom: starts with lowercase letter, and can contain all letters, digits, underscore. Can also be arbitrary sequence if enclosed in single quotes.
- Example atoms: butch, big_kahuna_burger, playGuitar, 'Vincent', 'Five dollar shake', '@$%'
- Special characters: : , ; . :-

Numbers:
- Integers: 12, -34, 22342
- Floats: 34573.3234, 0.3435

Facts and rules:
- A predicate with an atom is a fact.
- A relationship between facts are a rule. 

Variables in queries:
- Returns all instances of variable=atom where predicate(variable) is a fact.
- Must start with upper letter or underscore, no quotes. Can contain all letters, digits and underscore.

Arity:
- Number of arguments in complex term

Logic:![[{F84625A1-F7D7-4197-9836-EF4472DD7A1D}.png]]
Examples:
![[{CDEDA3C3-9B11-4CEA-A8A3-9E0E234FE068}.png]]
![[{6BCF44BD-F43A-4591-A674-88A849E4A0C0}.png]]![[{163435A7-DD86-447E-83CF-CB252A1737E6}.png]]
![[{C17CEFEC-B834-4C3F-A197-5E7EF1FF3B42}.png]]![[{8F6AEB8F-F468-43D5-8471-DC2DE5251CEA}.png]]![[{59CCADF3-60FD-486F-B072-E686724637B4}.png]]![[{9E1DD768-A0AF-428B-B845-18B84E89B581}.png]]

## Unification

```Prolog
?- bread = bread.                                       % Yes
?- 'Bread' = bread.                                     % No
?- 'bread' = bread.                                     % Yes
?- Bread = bread.                                       % Yes
?- bread = sausage.                                    % No
?- food(bread) = bread.                                 % No
?- food(bread) = X.                                     % Yes
?- food(X) = food(bread).                               % Yes
?- food(bread, X) = food(Y, sausage).                    % Yes
?- food(bread, X, beer) = food(Y, sausage, X).            % No
?- food(bread, X, beer) = food(Y, kahuna_burger).         % No
?- food(X) = X.                                         % Yes (default SWI-Prolog)
?- meal(food(bread), drink(beer)) = meal(X, Y).           % Yes
?- meal(food(bread), X) = meal(X, drink(beer)).           % No
```


```Prolog
% Knowledge base
house_elf(dobby).
witch(hermione).
witch('McGonagall').
witch(rita_skeeter).

magic(X) :- house_elf(X).
magic(X) :- wizard(X).
magic(X) :- witch(X).

% Logical answers, assuming undefined wizard/1 fails:
?- magic(house_elf).     % No
?- wizard(harry).        % No
?- magic(wizard).        % No
?- magic('McGonagall').  % Yes
?- magic(Hermione).      % Yes
```


## Search tree
![[{50578EEB-483F-462A-8294-1C4494711FE2}.png]]![[{945E6B57-33DA-4299-B6F3-CDCD674A6F0C}.png]]

## Recursion

![[{39C6BA08-9725-4AE0-BA0B-399E9D480652}.png]]![[{4D0F47C0-7E25-45EF-A000-13A07BE7323F}.png]]
![[{C49A9991-1964-408E-8DCD-A4701A91849C}.png]]
![[{E80539B5-5876-40F6-9B4E-8F78F3B3E38A}.png]]

## Successor
![[{CBFFA086-9B35-4FA6-9164-7F69D2727CAB}.png]]
## Addition

![[{0F7434E6-006D-48B4-B91E-8D61D90318AE}.png]]
## Lists
