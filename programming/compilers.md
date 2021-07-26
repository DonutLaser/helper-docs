# Grammar 
Grammar uses symbols commonly used in regular expressions  

Grammar explaining arithmetic expressions like "7 * 4 / 2 * 3":  
```
expr: 	factor((MUL/DIV) factor)*
factor: INT
```

**Grammar rule/production**: `expr: factor((MUL/DIV) factor)*`  
**Head/left-hand side**: `expr`  
**Body/right-hand side**: `factor((MUL/DIV) factor)*`  
**Terminals**: `MUL, DIV`  
**Non-terminals**: `factor, expr`  

The head of the first rule in the grammar is called **start symbol**

### Mapping a grammar to code
1. Each rule, **R**, becomes a method with the same name and references to the method call
become **R()**.
2. Alternatives **(a1|a2|aN)** become an **if-elseif-else** 
3. An optional grouping **(...)** becomes a **while** statement that can loop 0 or more times
4. Each token reference T becomes a call to the method **eat: eat(T)**, that consumes the current token and gets the next one  

# Associativity and precedence
In ordinary arithmetic and most programming languages `+, -, / and *` are **left-associative**.
**Left-associative** means that when a number has the same operator on both sides (or 2 different operators that have the same precedence),
the number associates with the operator on the left.
Examples: 
* `1 + 2 + 3`: 2 will associate with the + on the left and after the expression will basically be `3 + 3`
* `1 - 2 + 3`: 2 will associate with the - as the + and - precedence is the same and expression will basically be `-1 + 3`

When the operators are different and their precendence is different, a number associates with the operator with a higher precedence.
Examples:
* `1 + 2 * 3`: 2 will associate with the * as * has higher precedence than + and the expression will basically be `1 + 6`

For all our operators, we can write a precedence table:

| precedence level | associativity | operators |
| ---------------- | ------------- | --------- |
| 3                | left          | + - (b)   |
| 2                | left          | * /       |
| 1                | right         | + - (u)   |

b - binary, u - unary

Precedence levels should be defines in ascending order, that is, starting from lowest.

### Constructing rules of a grammar from the precedence table
1. For each level of precedence, define a non-terminal. The body of it should contain arithmetic operators from that level and non-terminals for the next higher level of precedence.
2. Create and aditional non-terminal factor for basic units of expression. If you have **N** levels of precedence, you will need **N + 1** non-terminals in total: one non-terminal for each level plus one non-terminal for basic units of expression.

For a grammar that supports expressions such as `1 + 2 * 3` and `5 * 6 - 2 * 5`, the grammar would look like this:
```
expr: 	term((PLUS|MINUS) term)*
term: 	factor((MUL/DIV) factor)*
factor: INT
```

If we generated a code from the grammar, it would basically first evaluate the multiplication/division first and then addition/subtraction

# AST
*Parse-tree (concrete syntax tree)* is a tree that represents the syntactic structure of a language constuct accordingto the
grammar definition. The call stack of the parser inmplicitly represents a parse tree and is automatically built in memory by the parser.
A parse-tree might look something like the following:
![alt text](https://ruslanspivak.com/lsbasi-part7/lsbasi_part7_parsetree_01.png)
Parse-trees are not used in interpreting the code

*Abstract syntax tree (AST)* is a tree that represents the abstract syntactic structure of a language construct where each interior
node and the root node represents an operator and the children of the node represents the operands of that operator.

To encode operator precedence in AST, we need to put higher precedence operator lower in the tree.

To generate an AST, parser should just generate nodes when it recognizes a language construct instead of immediatelly interpreting the code.

There are 3 types of depth-first traversal:
* *Preorder traversal*, where actions, such as calculating a value for a + operator executed before visiting the children of the node
* *Inorder traversal*, where actions are executed after visiting the left child
* *Postorder traversal*, where actions are executed after visiting both children

To navigated an AST, you use *postorder traversal*, which starts at the root node and recirsively visits the children of each node from
left to right. The postorder traversal visits nodes as far awayt from the root as fast as it can.

# Symbol table
When assigning values to variables, the values are stored in a *symbol table*, where you can later look up the variable value by
its name.

# Misc
When an interpreter and parser code is mixed together and the interpreter evaluates and expression as soon as the parser recognizes a
certain language construct like addition or subtraction, it is called a *syntax-directed interpreter*. It is useful for basic language
applications.

# Questions
1. How to properly come up with a grammar? What makes one grammar better than the other if using both it is possible to generate a working parser
2. Visitor pattern: how does it work?
