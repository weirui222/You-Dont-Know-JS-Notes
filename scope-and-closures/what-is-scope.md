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

###Parsing -
Taking a stream (array) of tokens and turning it into a tree of nested elements, which collectively represent the grammatical structure of the program. This tree is called an “AST” (abstract syntax tree).
The tree for var a = 2; might start with a top-level node called VariableDeclaration, with a child node called Identifier (whose value is a), and another child called AssignmentExpres sion, which itself has a child called NumericLiteral (whose value is 2).

###Code-Generation - 
The process of taking an AST and turning it into executable code. This part varies greatly depending on the language, the platform it’s targeting, and so on.
So, rather than get mired in details, we’ll just handwave and say that there’s a way to take our previously described AST for var a = 2; and turn it into a set of machine instructions to actually create a variable called a (including reserving memory, etc.), and then store a value into a.

For JavaScript, the compilation that occurs happens, in many cases, mere microseconds (or less!) before the code is executed. To ensure the fastest performance, JS engines use all kinds of tricks (like JITs, which lazy compile and even hot recompile, etc.) that are well beyond the “scope” of our discussion here.


##Understanding Scope

The way we will approach learning about scope is to think of the pro‐ cess in terms of a conversation. But, who is having the conversation?

###The Cast - 
Let’s meet the cast of characters that interact to process the program var a = 2;, so we understand their conversations that we’ll listen in on shortly:

**Engine:**
  Responsible for start-to-finish compilation and execution of our JavaScript program.

**Compiler:**
  One of Engine’s friends; handles all the dirty work of parsing and code-generation (see previous section).

**Scope:**
  Another friend of Engine; collects and maintains a look-up list of all the declared identifiers (variables), and enforces a strict set of rules as to how these are accessible to currently executing code.

For you to fully understand how JavaScript works, you need to begin to think like Engine (and friends) think, ask the questions they ask, and answer those questions the same.

So, let’s break down how Engine and friends will approach the program var a = 2;.
The first thing Compiler will do with this program is perform lexing to break it down into tokens, which it will then parse into a tree. But when Compiler gets to code generation, it will treat this program somewhat differently than perhaps assumed.

A reasonable assumption would be that Compiler will produce code that could be summed up by this pseudocode: “Allocate memory for a variable, label it a, then stick the value 2 into that variable.” Unfortu‐ nately, that’s not quite accurate.

Compiler will instead proceed as:
1. Encountering var a, Compiler asks Scope to see if a variable a already exists for that particular scope collection. If so, Compiler ignores this declaration and moves on. Otherwise, Compiler asks Scope to declare a new variable called a for that scope collection.
2. Compiler then produces code for Engine to later execute, to han‐ dle the a = 2 assignment. The code Engine runs will first ask Scope if there is a variable called a accessible in the current scope col‐ lection. If so, Engine uses that variable. If not, Engine looks else‐ where (see “Nested Scope” on page 8).

If Engine eventually finds a variable, it assigns the value 2 to it. If not, Engine will raise its hand and yell out an error!

To summarize: two distinct actions are taken for a variable assignment: First, Compiler declares a variable (if not previously declared) in the current Scope, and second, when executing, Engine looks up the vari‐ able in Scope and assigns to it, if found.

##Engine/Scope Conversation

```JS

function foo() {
  console.log(a); //2
}

foo(2)

```
Let’s imagine the above exchange (which processes this code snippet) as a conversation. The conversation would go a little something like this:

**Engine:** Hey Scope, I have an RHS reference for foo. Ever heard of it? 

**Scope:** Why yes, I have. Compiler declared it just a second ago. It’s a
function. Here you go.

**Engine:** Great, thanks! OK, I’m executing foo.

**Engine:** Hey, Scope, I’ve got an LHS reference for a, ever heard of it?

**Scope:** Why yes, I have. Compiler declared it as a formal parameter to foo just recently. Here you go.

**Engine:** Helpful as always, Scope. Thanks again. Now, time to assign 2 to a.

**Engine:** Hey, Scope, sorry to bother you again. I need an RHS look- up for console. Ever heard of it?

**Scope:** No problem, Engine, this is what I do all day. Yes, I’ve got console. It’s built-in. Here ya go.

**Engine:** Perfect. Looking up log(..). OK, great, it’s a function. 

**Engine:** Yo, Scope. Can you help me out with an RHS reference to a.
I think I remember it, but just want to double-check.

**Scope:** You’re right, Engine. Same variable, hasn’t changed. Here ya go.

**Engine:** Cool. Passing the value of a, which is 2, into log(..). ...

##Review

Scope is the set of rules that determines where and how a variable (identifier) can be looked up. This look-up may be for the purposes of assigning to the variable, which is an LHS (lefthand-side) reference, or it may be for the purposes of retrieving its value, which is an RHS (righthand-side) reference.

LHS references result from assignment operations. Scope-related as‐ signments can occur either with the = operator or by passing argu‐ ments to (assign to) function parameters.

The JavaScript engine first compiles code before it executes, and in so doing, it splits up statements like var a = 2; into two separate steps:
1. First, var a to declare it in that scope. This is performed at the beginning, before code execution.
2. Later, a = 2 to look up the variable (LHS reference) and assign to it if found.

Both LHS and RHS reference look-ups start at the currently executing scope, and if need be (that is, they don’t find what they’re looking for there), they work their way up the nested scope, one scope (floor) at a time, looking for the identifier, until they get to the global (top floor) and stop, and either find it, or don’t.
g
Unfulfilled RHS references result in ReferenceErrors being thrown. Unfulfilled LHS references result in an automatic, implicitly created global of that name (if not in Strict Mode), or a ReferenceError (if in Strict Mode).




















