
What is Top-Down Parsing?
-------------------------

Top-down parsing starts constructing the parse tree **from the root (start symbol)** and tries to derive the input string **left to right**.

👉 It predicts which grammar rule to use **before reading the entire input**.

### Real-Life Example:

While reading a sentence, we first assume:

> “This is a sentence”

Then we check word by word whether it fits grammar.

Types of Top-Down Parsing
-------------------------

1.  Recursive Descent Parsing
    
2.  Predictive Parsing (LL(1))
    

FIRST SET
=========

What is FIRST Set?
------------------

**FIRST(X)** is the set of terminals that can appear as the **first symbol** in strings derived from X.

### Rules to Compute FIRST:

1.  FIRST(X) = { X }
    
2.  ε ∈ FIRST(X)
    
3.  If X → Y1 Y2 ... Yn
    
    *   FIRST(Y1) is added
        
    *   If FIRST(Y1) contains ε, check Y2, and so on
        

### Example Grammar:
E → T E'  E' → + T E' | ε  T → id   `

### FIRST Sets:
FIRST(E)  = { id }  FIRST(E') = { + , ε }  FIRST(T)  = { id }   `

FOLLOW SET
==========

What is FOLLOW Set?
-------------------

**FOLLOW(A)** is the set of terminals that can appear **immediately after A** in some sentential form.

### Rules to Compute FOLLOW:

1.  $ is in FOLLOW(start symbol)
    
2.  If A → αBβ, then FIRST(β) − {ε} ⊆ FOLLOW(B)
    
3.  If A → αB or A → αBβ where FIRST(β) contains ε, then FOLLOW(A) ⊆ FOLLOW(B)
    

### Example:
FOLLOW(E)  = { $, ) }  FOLLOW(E') = { $, ) }  FOLLOW(T)  = { +, $, ) }   `

Real-Life Example:
------------------

FOLLOW is like:

> “What can come **after** this word in a sentence?”

LL(1) PARSING
=============

What is LL(1)?
--------------

*   **L** → Left to right scanning
    
*   **L** → Leftmost derivation
    
*   **1** → One input symbol lookahead
    

LL(1) Conditions
----------------

A grammar is LL(1) if:

1.  No left recursion
    
2.  Left factored
    
3.  FIRST sets of alternatives are disjoint
    
4.  FIRST(A) − {ε} ∩ FOLLOW(A) = ∅
    

PREDICTIVE PARSING
==================

What is Predictive Parsing?
---------------------------

A non-recursive top-down parsing technique that uses:

*   Parsing table
    
*   Stack
    
*   Input buffer
    

### Predictive Parsing Table

Entries are filled using FIRST and FOLLOW sets.

### Example:
 E → T E'  E' → + T E' | ε   `

Table entry:
M[E, id] = E → T E'  M[E', +] = E' → + T E'  M[E', $] = E' → ε   `

Real-Life Example:
------------------

Predicting next word while reading a sentence.

RECURSIVE DESCENT PARSING
=========================

What is Recursive Descent Parsing?
----------------------------------

*   One recursive function for each non-terminal
    
*   Uses procedure calls
    

### Example
 void E() {     T();     Eprime();  }   `

### Advantages:

✔ Simple✔ Easy to implement

### Disadvantages:

❌ Cannot handle left recursion❌ Manual backtracking needed

ERROR RECOVERY IN TOP-DOWN PARSING
==================================

Techniques:
-----------

1.  Panic Mode Recovery
    
2.  Synchronizing Tokens (FOLLOW set)
    

### Example:

If error occurs in E, skip input symbols until one in FOLLOW(E) is found.

BOTTOM-UP PARSING (LR PARSING)
==============================

What is Bottom-Up Parsing?
--------------------------

Bottom-up parsing builds the parse tree **from leaves to root**.

👉 It reduces input string back to start symbol.

Shift-Reduce Parsing
--------------------

### Operations:

1.  **Shift** – Push symbol onto stack
    
2.  **Reduce** – Replace RHS with LHS
    
3.  **Accept**
    
4.  **Error**
    

### Example:

Grammar
 E → E + id | id   `

Input:
 id + id   `

HANDLE PRUNING
==============

What is a Handle?
-----------------

A **handle** is a substring that matches the RHS of a production and can be reduced.

Handle pruning removes handles until only the start symbol remains.

Real-Life Example:
------------------

Solving math expression from smallest part:
 2 + 3 * 4   `

First evaluate 3 \* 4 (handle)

VIABLE PREFIX
=============

What is a Viable Prefix?
------------------------

A viable prefix is a prefix of a right-sentential form that:

*   Does not go beyond a handle
    

Used to detect errors early.

VALID ITEMS
===========

LR(0) Item
----------

An LR(0) item is a production with a dot:
 A → α . β   `

Dot shows parsing position.

LR(0) AUTOMATON
===============

What is LR(0) Automaton?
------------------------

A DFA constructed using:

*   LR(0) items
    
*   Closure and GOTO functions
    

Used to create parsing tables.

LR PARSING ALGORITHM
====================

Uses:

*   Stack
    
*   Input buffer
    
*   ACTION and GOTO tables
    

SLR(1) PARSING
==============

What is SLR(1)?
---------------

Simple LR parser:

*   Uses FOLLOW sets
    
*   Less powerful than LR(1)
    

✔ Easy❌ More conflicts

LR(1) PARSING
=============

What is LR(1)?
--------------

*   Uses lookahead symbol
    
*   More powerful
    
*   Large tables
    

Item format:
 [A → α . β, a]   `

LALR(1) PARSING
===============

What is LALR(1)?
----------------

*   Combines LR(1) states with same cores
    
*   Used in practice
    

✔ Smaller tables✔ Powerful✔ Used by YACC

YACC (Yet Another Compiler Compiler)
====================================

What is YACC?
-------------

*   Parser generator
    
*   Generates LALR(1) parser
    
*   Works with LEX
    

Structure of YACC Program
-------------------------
%{  C declarations  %}  %%  Grammar rules  %%  User code   `

Example YACC Specification
--------------------------
%{  #include  %}  %%  E : E '+' T    | T    ;  T : 'id'    ;  %%  int main() {    yyparse();    return 0;  }  int yyerror(char *s) {    printf("Syntax Error");    return 0;  }   `

ERROR RECOVERY IN YACC
======================

Methods:
--------

1.  Panic Mode
    
2.  Error Token
    

### Example:
stmt : error ';' { yyerrok; }   `

