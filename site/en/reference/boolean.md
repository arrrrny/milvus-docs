---
id: boolean.md
title: Boolean Expression Rules
---

# Boolean Expression Rules

[EBNF](https://en.wikipedia.org/wiki/Extended_Backus%E2%80%93Naur_form) grammar rules describes boolean expressions rules. Boolean expression rules are as follows:

```
Expr = LogicalExpr | NIL

LogicalExpr = LogicalExpr BinaryLogicalOp LogicalExpr 
              | UnaryLogicalOp LogicalExpr
              | "(" LogicalExpr ")"
              | SingleExpr;

BinaryLogicalOp = "&&" | "and" | "||" | "or";

UnaryLogicalOp = "not";

SingleExpr = TermExpr | CompareExpr;

TermExpr = IDENTIFIER "in" ConstantArray;

Constant = INTERGER | FLOAT

ConstantExpr = Constant
               | ConstantExpr BinaryArithOp ConstantExpr
               | UnaryArithOp ConstantExpr;
                                                          
ConstantArray = "[" ConstantExpr { "," ConstantExpr } "]";

UnaryArithOp = "+" | "-"

BinaryArithOp = "+" | "-" | "*" | "/" | "%" | "**";

CompareExpr = IDENTIFIER CmpOp IDENTIFIER
              | IDENTIFIER CmpOp ConstantExpr
              | ConstantExpr CmpOp IDENTIFIER
              | ConstantExpr CmpOpRestricted IDENTIFIER CmpOpRestricted ConstantExpr;

CmpOpRestricted = "<" | "<=";

CmpOp = ">" | ">=" | "<" | "<=" | "=="| "!=";
```

The following table lists the description of each symbol in the above Boolean expression rules.


| **Notation**      | **Description** |
| ----------- | ----------- |
| =      | Definition.       |
| ,      | Concatenation.       |
| ;      | Termination.        |
| |      | Alternation.       |
| {...}   | Repetition.        |
| (...)      | Grouping.       |
| NIL   | Empty. The expression can be an empty string.        |
| INTEGER      | Integers such as 1, 2, 3.       |
| FLOAT   | Float nubmers such as 1.0, 2.0.        |
| CONST      | Integers or float numbers.       |
| IDENTIFIER   | Identifier. In Milvus, the IDENTIFIER represents the field name.        |

| LogicalOp      | A LogicalOp is a logical operator that supports combining more than one relational operation in one comparison. Returned value of a LogicalOp is either TRUE (1) or FALSE (0). There are two types of LogicalOps, including BinaryLogicalOps and UnaryLogicalOps.    |
| UnaryLogicalOp   | UnaryLogicalOp refers to the unary logical operator "not".        |
| BinaryLogicalOp   |  Binary logical operators that perform actions on two operands. In a complex expression with two or more operands, the order of evaluation depends on precedence rules.       |
| ArithmeticOp   | An ArithmeticOp, namely an arithmetic operator, performs mathematical operations such as addition and subtraction on operands.         |
| UnaryArithOp      | A UnaryArithOp is an arithmetic operator that performs an operation on a single operand. The negative UnaryArithOp changes a positive expression into a negative one, or the other way round.      |

| BinaryArithOp   | A BinaryArithOp, namely a binary operator, performs operations on two operands. In a complex expression with two or more operands, the order of evaluation depends on precedence rules.        |
| CmpOp   | CmpOp is a relational operator that perform actions on two operands.        |
| CmpOpRestricted      |  CmpOpRestricted is restricted to "Less than" and "Equal".       |
| ConstantExpr   | ConstantExpr can be a Constant or a BinaryArithop on two ConstExpr or a UnaryArithOp on a single ConstantExpr. It is defined recursively.        |
| ConstantArray      | ConstantArray is wrapped by square brackets, and ConstantExpr can be repeated in the square brackets. ConstArray must include at least one ConstantExpr.       |
| TermExpr   | TermExpr is used to check whether the value of an IDENTIFIER appears in a ConstantArray. TermExpr is represented by "in".        |
| CompareExpr      | ACompareExpr, namely comparison expression can be relational operations on two IDENTIFIERs, or relational operations on one IDENTIFIER and one ConstantExpr, or ternary operation on two ConstantExprs and one IDENTIFIER.       |
| SingleExpr   |  SingleExpr, namely single expression, can be either a TermExpr or a CompareExpr.      |

| LogicalExpr      | A LogicalExpr can be a BinaryLogicalOp on two LogicalExprs, or a UnaryLogicalOp on a single LogicalExpr, or a LogicalExpr grouped within parentheses, or a SingleExpr. The LogicalExpr is defined recursively.    |

| Expr   | Expr, an abbreviation meaning expression, can be LogicalExpr or NIL. |

## Operators

### Logical operators:


| **Symbol**| **Operation** | **Example** | **Description**           |
| ----------| ------------- | ----------- | ------------------------- |
| 'and' &&  | and           | expr1 && expr2   | True if both expr1 and expr2 are true. |
| 'or' \|\|  | or           | expr1 \|\| expr2     | True if either expr1 or expr2 are true.  |




### Binary arithmetic operators:

| **Symbol**| **Operation** | **Example** | **Description**           |
| ----------| ------------- | ----------- | ------------------------- |
| +         | Addition      | a + b       | Add the two operands.     |
| -         | Subtraction   | a - b       | Subtract the second operand from the first operand.  |
| *         | Multiplication| a * b       | Multiply the two operands.     |
| /         | Division      | a / b       | Divide the first operand by the second operand.     |
| **        | Power         | a ** b      | Raise the first operand to the power of the second operand.     |
| %         | Modulo        | a % b       | Divide the first operand by the second operand and yield the remainder portion.    |


### Relational operators:

| **Symbol**| **Operation** | **Example** | **Description**           |
| ----------| ------------- | ----------- | ------------------------- |
| <         | Less than      | a < b      | True if a is less than b.     |
| >         | Greater than   | a > b       | True if a is greater than b.  |
| ==        | Equal          | a == b      | True if a is equal to b.    |
| !=        | Not equal       | a != b     | True if a is not equal to b.     |
| <=        | Less than or equal          | a <= b     | True if a is less than or equal to b.     |
| >=        | Greater than or equal         | a >= b      | True if a is greater than or equal to b.    |


## Operator precedence and associativity

The following table lists the precedence and associativity of operators. Operators are listed top to bottom, in descending precedence.


| Precedence | Operator  | Description   | Associativity |
|------------|-----------|---------------|---------------|
| 1          | + -       | UnaryArithOp  | Left-to-right |
| 2          | not       | UnaryLogicOp  | Right-to-left |
| 3          | **        | BinaryArithOp | Left-to-right |
| 4          | * / %     | BinaryArithOp | Left-to-right |
| 5          | + -       | BinaryArithOp | Left-to-right |
| 6          | < <= > >= | CmpOp         | Left-to-right |
| 7          | == !=     | CmpOp         | Left-to-right |
| 8          | && and    | BinaryLogicOp | Left-to-right |
| 9          | \|\| or     | BinaryLogicOp | Left-to-right |


Expressions are normally evaluated from left to right. Complex expressions are evaluated one at a time. The order in which the expressions are evaluated is determined by the precedence of the operators used. 

If an expression contains two or more operators with the same precedence, the operator to the left is evaluated first. 

<div class="alert note">
For example, 10 / 2 * 5 will be evaluated as (10 / 2) and the result multiplied by 5. 
</div>

When a lower precedence operation should be processed first, it should be enclosed within parentheses. 

<div class="alert note">
For example, 30 / 2 + 8. This is normally evaluated as 30 divided by 2 then 8 added to the result. If you want to divide by 2 + 8, it should be written as 30 / (2 + 8). 
</div>


Parentheses can be nested within expressions. Innermost parenthetical expressions are evaluated first.