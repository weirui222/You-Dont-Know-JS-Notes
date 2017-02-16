#Lexical Scope

In Chapter 1, we defined “scope” as the set of rules that govern how the engine can look up a variable by its identifier name and find it, either in the current scope, or in any of the nested scopes it’s contained within.

There are two predominant models for how scope works. The first of these is by far the most common, used by the vast majority of pro‐ gramming languages. It’s called lexical scope, and we will examine it in depth. The other model, which is still used by some languages (such as Bash scripting, some modes in Perl, etc) is called dynamic scope.

Dynamic scope is covered in Appendix A. I mention it here only to provide a contrast with lexical scope, which is the scope model that JavaScript employs.

#Lex-time

As we discussed in Chapter 1, the first traditional phase of a standard language compiler is called lexing (a.k.a., tokenizing). If you recall, the lexing process examines a string of source code characters and assigns semantic meaning to the tokens as a result of some stateful parsing.

It is this concept that provides the foundation to understand what lexical scope is and where the name comes from.

To define it somewhat circularly, lexical scope is scope that is defined at lexing time. In other words, lexical scope is based on where variables and blocks of scope are authored, by you, at write time, and thus is (mostly) set in stone by the time the lexer processes your code.

#Review
Lexical scope means that scope is defined by author-time decisions of where functions are declared. The lexing phase of compilation is es‐ sentially able to know where and how all identifiers are declared, and thus predict how they will be looked up during execution.
Two mechanisms in JavaScript can “cheat” lexical scope: eval(..) and with. The former can modify existing lexical scope (at runtime) by evaluating a string of “code” that has one or more declarations in it. The latter essentially creates a whole new lexical scope (again, at run‐ time) by treating an object reference as a scope and that object’s prop‐ erties as scoped identifiers.

The downside to these mechanisms is that it defeats the engine’s ability to perform compile-time optimizations regarding scope look-up, be‐ cause the engine has to assume pessimistically that such optimizations will be invalid. Code will run slower as a result of using either feature. Don’t use them.