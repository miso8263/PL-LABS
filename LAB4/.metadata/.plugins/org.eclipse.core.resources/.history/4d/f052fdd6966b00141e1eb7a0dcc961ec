object Lab4 extends jsy.util.JsyApplication {
  import jsy.lab4.ast._
  import jsy.lab4.Parser
  
  /*
   * CSCI 3155: Lab 4
   * Catherine dewerd
   * 
   * Partner: Olivia Abrant
   * Collaborators: <Any Collaborators> Jessica Lynch, Erik Eakins, CSEL people 
   */

  /*
   * Fill in the appropriate portions above by replacing things delimited
   * by '<'... '>'.
   * 
   * Replace 'YourIdentiKey' in the object name above with your IdentiKey.
   * 
   * Replace the 'throw new UnsupportedOperationException' expression with
   * your code in each function.
   * 
   * Do not make other modifications to this template, such as
   * - adding "extends App" or "extends Application" to your Lab object,
   * - adding a "main" method, and
   * - leaving any failing asserts.
   * 
   * Your lab will not be graded if it does not compile.
   * 
   * This template compiles without error. Before you submit comment out any
   * code that does not compile or causes a failing assert.  Simply put in a
   * 'throws new UnsupportedOperationException' as needed to get something
   * that compiles without error.
   */
  
  /* Collections and Higher-Order Functions */
  
  /* Lists */
  
  //A is a generic type
  def compressRec[A](l: List[A]): List[A] = l match {
    case Nil | _ :: Nil => l
    case h1 :: (t1 @ (h2 :: _)) => if (h1 == h2) compressRec(t1) else h1 :: compressRec(t1)
    //t1 is name of thing in parens (h2::_)
  }
  
  def compressFold[A](l: List[A]): List[A] = l.foldRight(Nil: List[A]){
    (h, acc) => if (!(acc.isEmpty) && acc.head == h) acc    else h::acc
     //h never points to nil, acc starts out at nil, then goes backwards 
     //WHy does order matter, .isEmpty has to be first, no such element, head of empty list
    //implicit match
  
  }
  
  def mapFirst[A](f: A => Option[A])(l: List[A]): List[A] = l match {
    case Nil => l
    case h ::  t => f(h) match {
      case Some(thing) => thing::t 
      case None => h::mapFirst(f)(t)
      
    } 
    //will keep mapping until it finds h, looks for a value in the list, maps - to it to keep the list whole
    //h::t => if (!f(h).isEmpty) f(h).get::t else h:: mapFirst(f)(t) Jessica's code
   
  }
  
  /* Search Trees */
  
  sealed abstract class Tree {
    def insert(n: Int): Tree = this match {
      case Empty => Node(Empty, n, Empty)
      case Node(l, d, r) => if (n < d) Node(l insert n, d, r) else Node(l, d, r insert n)
    } 
    
    
    //d is "data" so its an int
    def foldLeft[A](z: A)(f: (A, Int) => A): A = {
      def loop(acc: A, t: Tree): A = t match {
        case Empty => acc
        case Node(l, d, r) => {
	          val v1 = loop(acc,l)
	          val v2 = f(v1,d)
	          loop(v2,r)
        }
      }
      loop(z, this)
    }
    
    def pretty: String = {
      def p(acc: String, t: Tree, indent: Int): String = t match {
        case Empty => acc
        case Node(l, d, r) =>
          val spacer = " " * indent
          p("%s%d%n".format(spacer, d) + p(acc, l, indent + 2), r, indent + 2)
      } 
      p("", this, 0)
    }
  }
  case object Empty extends Tree
  case class Node(l: Tree, d: Int, r: Tree) extends Tree
  
  def treeFromList(l: List[Int]): Tree =
    l.foldLeft(Empty: Tree){ (acc, i) => acc insert i }
  
  def sum(t: Tree): Int = t.foldLeft(0){ (acc, d) => acc + d }
  
  
  //pass in a tuple of option ints- only accepts none(no integer value) or some
  def strictlyOrdered(t: Tree): Boolean = {
    val (b, _) = t.foldLeft((true, None: Option[Int])){
      case ((newBoolVal, None), element) => (newBoolVal && true ,  Some(element))
      case ((newBoolVal, Some(thing)), element) => if (thing < element) (newBoolVal && true , None: Option[Int]) else ((newBoolVal &&false), None: Option[Int])
    }
    b
  }
  

  /* Type Inference -small step*/ 
  
  // A helper function to check whether a jsy type has a function type in it.
  // While this is completely given, this function is worth studying to see
  // how library functions are used.
  def hasFunctionTyp(t: Typ): Boolean = t match {
    case TFunction(_, _) => true
    case TObj(fields) if (fields exists { case (_, t) => hasFunctionTyp(t) }) => true
    case _ => false
  }
  
  def typeInfer(env: Map[String,Typ], e: Expr): Typ = {
    // Some shortcuts for convenience
    //?? implement code here
    def typ(e1: Expr) = typeInfer(env, e1)
    def err[T](tgot: Typ, e1: Expr): T = throw StaticTypeError(tgot, e1, e)

    e match {
      case Print(e1) => typ(e1); TUndefined
      case N(_) => TNumber
      case B(_) => TBool
      case Undefined => TUndefined
      case S(_) => TString
      case Var(x) => env(x)
      
      case ConstDecl(x, e1, e2) => typeInfer(env + (x -> typ(e1)), e2)
      
      case Unary(Neg, e1) => typ(e1) match {
        case TNumber => TNumber
        case tgot => err(tgot, e1)
      }
      
      case Unary(Not, e1) =>typ(e1) match {
    	//you can totally knot strings, but you can't not strings
        case TBool => TBool
        case tgot => err(tgot, e1)
      }
      
      case Binary(Plus, e1, e2) => typ(e1) match{
        case TNumber => typ(e2) match{
            case TNumber =>  TNumber
            case tgot => err(tgot, e2)
         }        
        case TString =>typ(e2) match{
           case TString => TString
           case tgot => err(tgot, e2)
        }        
        case tgot => err(tgot, e1)
      }        
      
      case Binary(Minus|Times|Div, e1, e2) => typ(e1) match{
         case TNumber =>  typ(e2) match{
           case TNumber => TNumber
           case tgot => err(tgot, e2)
         }
         case tgot => err(tgot, e1)      
      }
      
       //? how to have e1, e2 not have a function type//thanks Jessica
      case Binary(Eq|Ne, e1, e2) =>{
        if (hasFunctionTyp(typ(e1))) {err(typ(e1),e1)} 
        else if (hasFunctionTyp(typ(e2))) {err(typ(e2),e2)} 
        else  TBool
      }
      
      
      case Binary(Lt|Le|Gt|Ge, e1, e2) =>typ(e1) match{
        case TNumber => typ(e2) match{
            case TNumber =>  TBool
            case tgot => err(tgot, e2)
         }        
        case TString =>typ(e2) match{
           case TString => TBool
           case tgot => err(tgot, e2)
        }        
        case tgot => err(tgot, e1)
      }        
     
      case Binary(And|Or, e1, e2) =>typ(e1) match{
        case TBool => typ(e2)  match{
          case TBool => TBool
          case tgot => err(tgot,e2)
        }
        case tgot => err(tgot, e1)
      } 
      
      case Binary(Seq, e1, e2) =>typ(e1); typ(e2) // e1 may affect e2
      
      //just a type checker, doesn't matter the actual values of the things.
      case If(e1, e2, e3)=> {
        if (typ(e1)==TBool){
          if ( typ(e2) == typ(e3)){
            typ(e2)
          } else err (typ(e2),e2)
        }else err(typ(e1), e1)
      }
		      
      case Function(p, params, tann, e1) => {
        //tann is the return type, may or not be specified
        // Bind to env1 an environment that extends env with an appropriate binding if
        // the function is potentially recursive.
        val env1 = (p, tann) match {
          case (Some(f), Some(rt)) =>
            val tprime = TFunction(params, rt)
            env + (f -> tprime)
          case (None, _) => env
          case _ => err(TUndefined, e1)
        }
        // Bind to env2 an environment that extends env1 with bindings for params.
        // take what we got from env1 and add all the parameter types '++' adds a whole collection of key value pairs
        /*val env2 = params.foldLeft(env1) {
          case (acc, (xi, ti)) => acc + (xi -> ti)
        }  //  the ++ is shorthand  for this */
        val env2 = env1++params
        // Match on whether the return type is specified.
        tann match {
          case None => {
            val tau = typeInfer(env2, e1)
            val tauPrime = TFunction(params, tau)
            tauPrime
          }
          case Some(rt) => {
            val tau = typeInfer(env2, e1)
            val tauPrime = TFunction(params, tau)
            if (tauPrime != TFunction(params, rt)) err(tau, e1) else TFunction(params, rt)
          }
        }
      }
        /* Bind to env1 an environment that extends env with an appropriate binding if
        // the function is potentially recursive.
	        val env1 = (p, tann) match {
	          case (Some(f), Some(tret)) =>
	            val tprime = TFunction(params, tret)
	            env + (f -> tprime)
	          case (None, _) => env
	          case _ => err(TUndefined, e1)
	        }
        // Bind to env2 an environment that extends env1 with bindings for params.
        val env2 = params.foldLeft(env1)((x,y)=>) y match{
          
        }
        //x is type environment, y is current list element is tuple of string,typ
        //extend environment to all params in params
        // Match on whether the return type is specified.
        
        
        (tann, typeInfer(env2,e1)) match {
          case (None,returnType) => returnType //may need to look at the body, add another check here
          case (Some(tret), returnType) => if (returnType == tret) tret else err(returnType, e1)
        }
      }*/
      
      
      case Call(e1, args) => typ(e1) match { //e maps to t
        //calling a class with an expression here, but everything else is an object.
        case TFunction(params, tret) if (params.length == args.length) => {
          (params, args).zipped.foreach {
            (paramX, argY) => (paramX, argY ) match{//only way to get to the tuple in paramX is by doing a match.
	            case ((str,typeParamater), typeArg) => if (typeParamater != typ(typeArg))  err(typeParamater,typeArg)//type argument
	              //check that the expression matches the type of the paramater, param= list of paramaters and the types
	            //tret is a list of return types
	            }
            };
          tret
        }
        case tgot => err(tgot, e1)
      }
    
      //Currently all fields are mapped to e1 in a key->value map. We want to convert an expression to a new value: "type"
      case Obj(fields) => TObj(fields.map{case (a,b)=>(a, typ(b))})
       //function you want to apply to a, b  after hashrocket      
       //for every object, have a bunch of fields {}=object, if there is a field f with type t, e.f should give 
    		  //typ(e) match to object
    		  //if something is not an object, can't call it
    
      
      case GetField(e1, f) => {
        val e = typ(e1)
        e match {
          //calling a class with an expression
          case TObj(fields) => fields.get(f) match {
            case None => err(e, e1)
            case Some(v) => v
          }
          case _ => err(e, e1)
        }
      }
   	}
  }
  
  
  /* Small-Step Interpreter */
  
  def inequalityVal(bop: Bop, v1: Expr, v2: Expr): Boolean = {
    require(bop == Lt || bop == Le || bop == Gt || bop == Ge)
    ((v1, v2): @unchecked) match {
      case (S(s1), S(s2)) =>
        (bop: @unchecked) match {
          case Lt => s1 < s2
          case Le => s1 <= s2
          case Gt => s1 > s2
          case Ge => s1 >= s2
        }
      case (N(n1), N(n2)) =>
        (bop: @unchecked) match {
          case Lt => n1 < n2
          case Le => n1 <= n2
          case Gt => n1 > n2
          case Ge => n1 >= n2
        }
    }
  }
  
  def substitute(e: Expr, v: Expr, x: String): Expr = {
    require(isValue(v))
    
    def subst(e: Expr): Expr = substitute(e, v, x)
    
    e match {
      case N(_) | B(_) | Undefined | S(_) => e
      case Print(e1) => Print(subst(e1))
      case Unary(uop, e1) => Unary(uop, subst(e1))
      case Binary(bop, e1, e2) => Binary(bop, subst(e1), subst(e2))
      case If(e1, e2, e3) => If(subst(e1), subst(e2), subst(e3))
      case Var(y) => if (x == y) v else e
      case ConstDecl(y, e1, e2) => ConstDecl(y, subst(e1), if (x == y) e2 else subst(e2))
      case Function(p, params, tann, e1) =>
        if (params.exists((t1:(String,Typ))=> t1._1 == x) || p==Some(x)){//._1 get first element of tuple
          Function(p, params, tann,e1)
        }else Function(p, params, tann, subst(e1))
          
      case Call(e1, args) => Call(subst(e1), args map subst) // short for args.map (v1 => subst(v1))
      case Obj(fields) =>Obj(fields.map(s => (s._1, subst(s._2))))
      case GetField(e1, f) => GetField(subst(e1),f)
        
    }
  }
  
  def step(e: Expr): Expr = {
    require(!isValue(e))
    
    def stepIfNotValue(e: Expr): Option[Expr] = if (isValue(e)) None else Some(step(e))
    
    e match {
      /* Base Cases: Do Rules */
      case Print(v1) if isValue(v1) => println(pretty(v1)); Undefined
      case Unary(Neg, N(n1)) => N(- n1)
      case Unary(Not, B(b1)) => B(! b1)
      case Binary(Seq, v1, e2) if isValue(v1) => e2
      case Binary(Plus, S(s1), S(s2)) => S(s1 + s2)
      case Binary(Plus, N(n1), N(n2)) => N(n1 + n2)
      
      case Binary(Minus, N(n1), N(n2))=> N(n1 - n2)
      case Binary(Times, N(n1), N(n2))=> N(n1 * n2)
      case Binary(Div, N(n1), N(n2))=> N(n1 / n2)
      
      case Binary(bop @ (Lt|Le|Gt|Ge), v1, v2) if isValue(v1) && isValue(v2) => B(inequalityVal(bop, v1, v2))
      case Binary(Eq, v1, v2) if isValue(v1) && isValue(v2) => B(v1 == v2)
      case Binary(Ne, v1, v2) if isValue(v1) && isValue(v2) => B(v1 != v2)
      case Binary(And, B(b1), e2) => if (b1) e2 else B(false)
      case Binary(Or, B(b1), e2) => if (b1) B(true) else e2
      case ConstDecl(x, v1, e2) if isValue(v1) => substitute(e2, v1, x)
      
      case Call(expression1, args) if isValue(expression1) && (args forall isValue) =>
        expression1 match {
          case Function(nameOption, params, _, e1) => {
            val elp = (params, args).zipped.foldRight(e1){
               (paramsX, acc)=> paramsX match{
                case((paramName,_), argValue) => substitute(acc, argValue, paramName)
              }
            }
            nameOption match {
              case None => elp
              case Some(x1) => substitute(elp, expression1, x1)
            }
          }
          case _ => throw new StuckError(e)
        }
      
        /*** Fill-in more cases here. ***/
        //case Obj () => ?? 
        case If(B(e1), e2, e3) =>if (e1) e2 else e3
	         
      
        case GetField(Obj(fields), value) => fields.get(value) match{
          case Some(v1)=> v1
          //do we need to check that v1 is a value or can we assume
          case None => throw new StuckError(e)
        }
        
        
      /* Inductive Cases: Search Rules */
      case Print(e1) => Print(step(e1))
      case Unary(uop, e1) => Unary(uop, step(e1))
      case Binary(bop, v1, e2) if isValue(v1) => Binary(bop, v1, step(e2))
      case Binary(bop, e1, e2) => Binary(bop, step(e1), e2)
      case If(e1, e2, e3) => If(step(e1), e2, e3)
      case ConstDecl(x, e1, e2) => ConstDecl(x, step(e1), e2)
      /*** Fill-in more cases here. ***/
      //case Call, case Obj, case GetFields
      case GetField(e1, f) => GetField(step(e1),f)
      case Obj(fields) => Obj(fields.map{case (a,b)=> (a,step(b))})
      case Call(v1, listOfExpr) if isValue(v1) => Call(v1, mapFirst(stepIfNotValue)(listOfExpr))
      case Call(e1, listOfExprs) => Call(step(e1), listOfExprs)
    

      
      
      
      /* Everything else is a stuck error. Should not happen if e is well-typed. */
      case _ => throw StuckError(e)
    }
  }
  
  
  /* External Interfaces */
  
  this.debug = true // comment this out or set to false if you don't want print debugging information
  
  def inferType(e: Expr): Typ = {
    if (debug) {
      println("------------------------------------------------------------")
      println("Type checking: %s ...".format(e))
    } 
    val t = typeInfer(Map.empty, e)
    if (debug) {
      println("Type: " + pretty(t))
    }
    t
  }
  
  // Interface to run your small-step interpreter and print out the steps of evaluation if debugging. 
  def iterateStep(e: Expr): Expr = {
    require(closed(e))
    def loop(e: Expr, n: Int): Expr = {
      if (debug) { println("Step %s: %s".format(n, e)) }
      if (isValue(e)) e else loop(step(e), n + 1)
    }
    if (debug) {
      println("------------------------------------------------------------")
      println("Evaluating with step ...")
    }
    val v = loop(e, 0)
    if (debug) {
      println("Value: " + v)
    }
    v
  }

  // Convenience to pass in a jsy expression as a string.
  def iterateStep(s: String): Expr = iterateStep(Parser.parse(s))
  
  // Interface for main
  def processFile(file: java.io.File) {
    if (debug) {
      println("============================================================")
      println("File: " + file.getName)
      println("Parsing ...")
    }
    
    val expr =
      handle(None: Option[Expr]) {Some{
        Parser.parseFile(file)
      }} getOrElse {
        return
      }
    
    handle() {
      val t = inferType(expr)
    }
    
    handle() {
      val v1 = iterateStep(expr)
      println(pretty(v1))
    }
  }

}