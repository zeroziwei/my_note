---
tags:
  - doing
progress: 1
creation date: 2024-07-18 14:29
modification date: 星期四 18日 七月 2024 14:29:48
---
## 1	Scheme expressions

1. Atomic expressions  
- Self-evaluating: numbers, booleans  
		`3, 5.5, -10, #t, #f`  
- Symbols: names bound to values  
		`+, modulo, list, x, foo, hello-world`  
		
2. Combinations  

		(<operator> <operand1> <operand2> ...)  

A combination is either a **call expressionor** a **special form expression**  


## 2	Call Expressions

A call expression applies a procedure to some arguments.  


How to evaluate call expressions:  
Step 1. Evaluate the operator to get a procedure.  
Step 2.Evaluate all operands left to right to get the arguments.  
Step 3. Apply the procedure to the arguments.  

- [?] 有一点点类似于中序遍历是吧

![[Pasted image 20240718143741.png]]


![[Pasted image 20240718144004.png]]

从下面开始就都是 Special Form Expressions

```python
(<operator> <operand1> <operand2> ...)  
<operator> : define, if, lambda, etc.
```

## 3	Assigning values to names

	(define <name> <expr>)
	
Step 1. Evaluate the given expression.  
Step 2. Bind the resulting value to the given name in the current frame.  
Step 3. Return the name as a symbol

![[Pasted image 20240718144133.png]]


## 4	Control flow

The if special form allows us to evaluate an expression based on a condition

	(if <predicate> <if-true> <if-false>)
	

```
Step 1. Evaluate the <predicate>.  
Step 2. If <predicate>evaluates to anything but #f, evaluate <if-true>and return  
the value. Otherwise, evaluate <if-false>if provided and return the value
```

![[Pasted image 20240718144540.png]]

## 5	Defining functions with names

	(define (<name> <param1> <param2> ...) <body>)
	
Step 1. Create a lambda procedure with the given parameters and body.  
Step 2. Bind it to the given name in the current frame.  
Step 3. Return the function name as a symbo

![[Pasted image 20240718144648.png]]

## 6	Lambda Expressions

	(lambda (<param1> <param2> ...) <body>)
	
![[Pasted image 20240718145007.png]]

Two equivalent expressions:  

	(define (plus4 x) (+ x 4))  
	(define plus4 (lambda (x) (+ x 4)))
	
## 7	Example: Factorial

![[Pasted image 20240718151618.png]]

## 8	Counting up

![[Pasted image 20240718151701.png]]


## 9	TLDR 

-  Scheme programs consist only of {{c1::expressions}}, all of which can be categorized into either {{c2::atomic expressions}} or {{c3::combinations}}.  
- Combinations are either {{c1::call expressions}} or {{c1::special form expressions}}, and they differ in the value of the operator.  
- Scheme call expressions are evaluated just like they are in Python, but each special  form has its own rules of evaluation.  
- The special forms we learned today are{{c1:: if}}, {{c1::define}}, and{{c1:: lambda}}.  
- Writing some procedures in Scheme will require recursion; there is no special form for  iteration.  
<!--ID: 1721824704239-->



- 🍅 (pomodoro::WORK) (duration:: 25m) (begin:: 2024-07-18 14:34) - (end:: 2024-07-18 14:59)

- 🍅 (pomodoro::WORK) (duration:: 25m) (begin:: 2024-07-18 14:59) - (end:: 2024-07-18 15:24)
- 🍅 (pomodoro::WORK) (duration:: 25m) (begin:: 2024-07-18 15:24) - (end:: 2024-07-18 15:49)