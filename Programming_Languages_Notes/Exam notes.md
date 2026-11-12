
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
- Neste records in a tree.
- Each parsed statement is a partial parse tree represented as a record.
- Each parsed statement is of a certain type, and may have some content. 
- Encode the type in the records label.
- We can encode the content in the record’s fields.

![[{ADD002EF-6E67-4BE5-AF09-5E97A7BB6193}.png]]![[{133B84DF-581F-48C4-BF76-31722127028D}.png]]
![[{4A121191-265A-4D98-9363-EBA4C4385243}.png]]![[{0C846961-B57B-4DE0-B754-9FEC70CFC6E8}.png]]

Representing single assignment store:

Representing an en environment:
