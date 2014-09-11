# Lab 1
Michelle Soult

Partner: Olivia Abrant

Include your writeup for the Lab 1 questions here. Use correct
Markdown markup. For more details, start with the article
https://help.github.com/articles/github-flavored-markdown

###Question 1  
####*Scala Basics: Binding and Scope*
#####(a)
The use of pi at line 4 is bound at line 3.  Pi is declared as a val within the scope of the function circumference at line 3 and then used immediately afterward in line 4.  

The use of pi at line 7 is bound at line 1.  Since pi is not defined within the scope of the function area, it is a free variable with respect to area and therefore takes its definition at line 1, where pi is declared as a value prior to the function (one scope level up).  

#####(b)

###Question 2  
####*Scala Basics: Typing*
Yes, the body of g is well-typed with type ((Int, Int), Int)

<blockquote>
(a, b) is an (Int, (Int, Int)) because
    a:Int because
        1:Int
    b:(Int, Int) because
        x:Int
        3:Int 
g:((Int, Int), Int) because
    b:(Int, Int) because
        x:Int
        3:Int
    1:Int
    a+2:Int because	
        a:Int
        2:Int
</blockquote>
