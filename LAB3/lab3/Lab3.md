#Lab 2
Michelle Soult

Partner: Erik Eakins

###Question 1
#####Give one test case that behaves differently under dynamic scoping versus static scoping (and does not crash).  Explain the test case and how they behave differently in your write-up.
This is the test case:
```
ConstDecl(x,N(2.0),ConstDecl(r,Binary(Plus,Call(Function(None,x,N(3.0)),N(4.0)),Var(x)),Binary(Seq,Print(Var(r)),ConstDecl(x,N(2.0),ConstDecl(g,Function(None,y,Var(x)),ConstDecl(h,Function(None,x,Call(Var(g),N(3.0))),Binary(Seq,Print(Call(Var(h),N(4.0))),ConstDecl(x,N(42.0),ConstDecl(plus,Function(None,x,Function(None,y,Binary(Plus,Var(x),Var(y)))),Print(Call(Call(Var(plus),N(3.0)),N(4.0))))))))))))
```
This is creating constant variables that are set to values and functions.  It is then calling the function with a parameter and printing the call of that function.  Depending on the evaluation scheme, the val will be the parameter to the function or the val declared before the function.    

Big step and small step generate different outputs:
Big Step: 5, 4, 46
Small Step: 5, 2, 7

###Question 2
#####Explain whether the evaluation order in step is deterministic as specified by the judgement form e-->e'.
This is deterministic, as one side of the expression (left) is stepped down to its value first.  Since there is a clear left-to-right evaluation order, this can be considered to be deterministic.  

###Question 3
#####Consider the small-step operational semantics for JAVASCRIPTY shown in Figures 7, 8, and 9.  What is the evaluation order for e1 + e2?  Explain.  How do we change the rules to obtain the opposite evaluation order?
e1 is evaluated prior to e2.  The structure of the small-step semantics rely on the base cases: the DoRules take the value of e1 and the expression e2.  This indicates that e1 should be evaluated first.  This is consistent with the search rules as well.  The best example is the SearchArith1 and SearchArith2.  When two expressions are present, the first is evaluated to a value followed by the second.  To change the evaluation order, one would have to change the base cases (DoRules) to expect an e1 expression and a v2 value.  The Search Rules would have to evaluate the second expression first rather than the first.     

###Question 4
#####Short Circuit Evaluation.
#####(a) Concept.  Give an example that illustrates the usefulness of short-circuit evaluation.  Explain your example
Short-circuit evaluations are useful in Boolean operations in that they allow you to halt evaluation if the outcome is determined.  This is especially useful if evaluation of all expressions would be wasteful.  For example:

```
e1 = false
e2 = !((1&&2)||((5^10000000000000)&&6))&&(!(true))

e1&&e2
```

The expression e1&&e2 will return false regardless of the result of evaluating e2.  Evaluating e2 will be a waste of time, since it is computationally intensive.

#####(b) JAVASCRIPTY.  Consider the small-step operational semantics for JAVASCRIPTY shown in Figures 7, 8, and 9.  Does e1&&e2 short circuit?  Explain.
It does.  If e1 is false it returns e1 and does not evaluate e2, since the overall expression would be false anyway.  This is a good example of short circuiting.  


