#Lab 2
Michelle Soult

Partner: Olivia Abrant

###Question 1
####(a) Rewrite grammar using judgement forms

____________		____________			
a &epsilon VObjects		b &epsilon VObjects			//axioms

s &epsilon VObjects		
____________
s &epsilon AObjects									// successors


s1 &epsilon AObjects		s2 &epsilon AObjects
_______________________________
s1 & s2 &epsilon AObjects

####(b) Show the parse tree

```
		A 									A 
	A   &   A							A   &   A
	v	  A & A                       A & A     v
   a  b    ...                         ...     a  b
	
This is ambiguous!  More than one parse tree can be created for the same syntax.  
```

####(c) Describe the language

S ::= A | B | C

= aA | bB | cC

= aaA | bbB | ccC

=a...A | b...B | c...C

The A and C repetitions recurse n times, whereas the B repetitions recurse m times, as the B can terminate with &epsilon.
Therefore:

L(G) = {a^n or b^m or c^n | n > 0; m >= 0 }

####(d) Demonstrate whether a sentence is described by the grammar by giving derivations.

1. baab
S ::= AaBb
-> baBb   //Rule 2
-> baab   //Rule 3

baab is described by the grammar.  

2. bbbab
S ::= AaBb
-> AbaBb   //Rule 2
-> AbbaBb   //Rule 2
-> bbbaBb   //Rule 2
-> bbbaab (minimally) //Rule 3

bbbab is *not* described by the grammar.

3. bbaaaaa
S ::= AaBb
The sequence must end in "b"

bbaaaaa is *not* described by the grammar.  

4. bbaab
S ::= AaBb
-> AbaBb   //Rule 2
-> bbaBb   //Rule 2
-> bbaab   //Rule 3

bbaab is described by the grammar.  

####(e) Demonstrate whether a sentence is described by the grammar by giving parse trees.

1. abcd

```
		S 
	*a*   S   *c*    B 
	    *b*        *d* 

```
abcd is described by the grammar.


2. acccbd

```

```




###Question 2