#What is Scope?

One of the most fundamental paradigms of nearly all programming languages is the ability to store values in variables, and later retrieve or modify those values. In fact, the ability to store values and pull values out of variables is what gives a program state.
Without such a concept, a program could perform some tasks, but they would be extremely limited and not terribly interesting.

But the inclusion of variables into our program begets the most in‐ teresting questions we will now address: where do those variables live? In other words, where are they stored? And, most important, how does our program find them when it needs them?
These questions speak to the need for a well-defined set of rules for storing variables in some location, and for finding those variables at a later time. We’ll call that set of rules: scope. But, where and how do these scope rules get set?

##Compiler Theory

It may be self-evident, or it may be surprising, depending on your level of interaction with various languages, but despite the fact that Java‐ Script falls under the general category of “dynamic” or “interpreted” languages, it is in fact a compiled language. 

In traditional compiled-language process, a chunk of source code, your program, will undergo typically three steps before it is executed, roughly called “compilation”:

The 3 steps are: Tokenizing/Lexing, Parsing, Code-Generation.

####Tokenizing/Lexing - 
Breaking up a string of characters into meaningful (to the lan‐ guage) chunks, called tokens. For instance, consider the program var a = 2;. This program would likely be broken up into the following tokens: var, a, =, 2, and ;. Whitespace may or may not be persisted as a token, depending on whether its meaningful or not.

####Parsing -
taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an “AST” (abstract syntax tree).
The tree for var a = 2; might start with a top-level node called VariableDeclaration, with a child node called Identifier (whose value is a), and another child called AssignmentExpres sion, which itself has a child called NumericLiteral (whose value is 2).

####Code-Generation - 
taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an “AST” (abstract syntax tree).

The tree for var a = 2; might start with a top-level node called VariableDeclaration, with a child node called Identifier (whose value is a), and another child called AssignmentExpres sion, which itself has a child called NumericLiteral (whose value is 2). a variable called a (including reserving memory, etc.), and then store a value into a.

For JavaScript, the compilation that occurs happens, in many cases, mere microseconds (or less!) before the code is executed. To ensure the fastest performance, JS engines use all kinds of tricks (like JITs, which lazy compile and even hot recompile, etc.) that are well beyond the “scope” of our discussion here.


##Understanding Scope

The way we will approach learning about scope is to think of the pro‐ cess in terms of a conversation. But, who is having the conversation?

####The Cast - 
Let’s meet the cast of characters that interact to process the program var a = 2;, so we understand their conversations that we’ll listen in on shortly:

**Engine**
  Responsible for start-to-finish compilation and execution of our JavaScript program.

**Compiler**
  One of Engine’s friends; handles all the dirty work of parsing and code-generation (see previous section).

**Scope**
  Another friend of Engine; collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

For you to fully understand how JavaScript works, you need to begin to think like Engine (and friends) think, ask the questions they ask, and answer those questions the same.

So, let’s break down how Engine and friends will approach the program var a = 2;.











