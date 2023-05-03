Download Link: https://assignmentchef.com/product/solved-cs-3430-assignment-1-function-representation-and-differentiation
<br>
Objectives

<ol>

 <li>Function Representation</li>

 <li>Function Differentiation</li>

 <li>Differentiation Rules</li>

 <li>Converting Function Representations to Python Functions</li>

 <li>Graphing Functions and Derivatives</li>

</ol>

<h1>Introduction</h1>

In this assignment, you’ll write Python functions that differentiate function representations, convert those representations to real Python functions, and graph them with matplotlib, one of the Python graphing libraries.

<h1>Warmup</h1>

If you’re new to differentiation or your differentiation skills are rusty, I suggest that you start by reviewing the first two lectures and, if necessary, reading the handout, and then taking the derivatives of the functions below. If you’re comfortable with differentiation, skip this section and move on.

<ol>

 <li>; answer: 18<em>x</em><sup>2</sup>;</li>

 <li>); answer: ;</li>

 <li>; answer: ;</li>

 <li>; asnwer: 4<em>x</em><sup>3 </sup>+ 3<em>x</em><sup>2 </sup>+ 1;</li>

 <li>; answer: 6(2<em>x </em>+ 4)<sup>2</sup>;</li>

 <li>; answer: ;</li>

 <li>; answer: ;</li>

 <li>; answer: ;</li>

 <li>; answer: 2 + 3(<em>x </em>+ 2)<sup>2</sup>;</li>

</ol>

; answer:       .

<h1>Variables, Powers, Sums, and Products</h1>

Recall that in lectures 1 and 2 we discussed how such mathematical objects as variables, powers, sums, and products can be represented with Python objects.

The files const.py, var.py, pwr.py, plus.py, prod.py contain classes for constants, variables, powers, sums, and products, respectively. The file maker.py contains several functions for constructing objects of these classes. You don’t need to modify these files for this assignment. Let’s play with mathematical object construction a little in the Python interpreter window to get more comfortable with them.

Here’s how we can construct a constant and get its value.

&gt;&gt;&gt; c1 = make_const(1.0)

&gt;&gt;&gt; c1.get_val()

1.0

&gt;&gt;&gt; c2 = const(val=10.0)

&gt;&gt;&gt; c2.get_val()

10.0

Once we have an object, we can use the function isinstance to check if the object is an instance of a given class. The two calls below show us that c1 is an instance of the class const but not of the class var or the class prod.

&gt;&gt;&gt; isinstance(c1, const)

True

&gt;&gt;&gt; isinstance(c1, var)

False

&gt;&gt;&gt; isinstance(c1, prod)

False

Here’s how we can construct a list of constant objects and then extract their values into a new list.

&gt;&gt;&gt; clist = [make_const(i) for i in range(10)]

&gt;&gt;&gt; clist

&gt;&gt;&gt; vlist = [c.get_val() for c in clist]

&gt;&gt;&gt; vlist

[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

Let’s represent the function <em>x</em><sup>2</sup>. This power expression has a base of <em>x </em>and a degree of 2. The base is an instance of the class var and the degree is an instance of the class const.

&gt;&gt;&gt; fex = make_pwr(’x’, 2.0)

&gt;&gt;&gt; b = fex.get_base()

&gt;&gt;&gt; d = fex.get_deg() &gt;&gt;&gt; print b x &gt;&gt;&gt; print d

2.0

&gt;&gt;&gt; isinstance(b, var)

True

&gt;&gt;&gt; isinstance(d, const) True

Finally, let’s represent 5<em>x</em><sup>2 </sup>+ 10<em>x </em>− 100.

&gt;&gt;&gt; fex1 = plus(elt1=prod(mult1=make_const(5.0), mult2=make_pwr(’x’, 2.0)),

elt2=prod(mult1=make_const(10.0), mult2=make_pwr(’x’, 1.0)))

&gt;&gt;&gt; fex2 = plus(elt1=fex1, elt2=make_const(-100.0))

&gt;&gt;&gt; fex2 = plus(elt1=fex1, elt2=make_const(-100.0))

&gt;&gt;&gt; print fex2

(((5.0*(x^2.0))+(10.0*(x^1.0)))+-100.0)

Let’s represent 5<em>x</em><sup>2</sup>. The product’s first multiple is 5 and its second multiple is <em>x</em><sup>2</sup>.

&gt;&gt;&gt; fex = prod(mult1=make_const(5.0), mult2=make_pwr(’x’, 2.0))

&gt;&gt;&gt; print fex

(5.0*(x^2.0))

&gt;&gt;&gt; print fex.get_mult1() 5.0

&gt;&gt;&gt; print fex.get_mult2()

(x^2.0)

<h1>Problem 1: (3 points)</h1>

Implement the function deriv that takes a function representation and computes a representation of its derivative. Your implemenation should be able to handle constants, powers, products, and sums. For this assignment, you should handle only simple products and simple powers. We’ll handle more complex products, powers, and quotients later in the course.

A simple product can be recursively defined as a product of two constants, a constant and a simple power, a constant and a plus, and a constant and another simple product.

A simple power expression is recursively defined as an expression whose degree is a constant and whose base is a variable, a simple power, a sum, and a simple product. Below are a few test cases of how your implementation of deriv should work.

Let’s start with <em>x</em><sup>1</sup>.

&gt;&gt;&gt; fex = make_pwr(’x’, 1.0)

&gt;&gt;&gt; print fex

(x^1.0)

&gt;&gt;&gt; drv = deriv(fex)

&gt;&gt;&gt; print drv

(1.0*(x^0.0))

Let’s compute the derivative of another power.

&gt;&gt;&gt; fex = make_pwr(’z’, 5)

&gt;&gt;&gt; print fex

(z^5)

&gt;&gt;&gt; drv = deriv(fex)

&gt;&gt;&gt; print drv (5*(z^4.0))

One more power expression, slightly more complex than the one above.

&gt;&gt;&gt; fex = make_pwr_expr(make_pwr(’x’, 2.0), 2.0)

&gt;&gt;&gt; print fex

((x^2.0)^2.0)

&gt;&gt;&gt; drv = deriv(fex)

&gt;&gt;&gt; print drv

((2.0*((x^2.0)^1.0))*(2.0*(x^1.0)))

Let’s represent and differentiate 5<em>x</em><sup>2 </sup>and <em>x</em><sup>2</sup>5. Note that both expressions differentiate to the same expression.

&gt;&gt;&gt; fex = prod(mult1=make_const(5.0), mult2=make_pwr(’x’, 2.0))

&gt;&gt;&gt; drv = deriv(fex)

&gt;&gt;&gt; print drv

(5.0*(2.0*(x^1.0)))

&gt;&gt;&gt; fex = prod(mult1=make_pwr(’x’, 2.0), mult2=make_const(5.0))

&gt;&gt;&gt; print fex

((x^2.0)*5.0)

&gt;&gt;&gt; drv = deriv(fex)

&gt;&gt;&gt; print drv

(5.0*(2.0*(x^1.0)))

Let’s differentiate (5<em>x</em><sup>10</sup>)<sup>4 </sup>using the power rule.

&gt;&gt;&gt; prd = prod(mult1=make_const(5.0), mult2=make_pwr(’x’, 10.0))

&gt;&gt;&gt; fex = make_pwr_expr(prd, 4.0)

&gt;&gt;&gt; print fex

((5.0*(x^10.0))^4.0)

&gt;&gt;&gt; drv = deriv(fex)

&gt;&gt;&gt; print drv

((4.0*((5.0*(x^10.0))^3.0))*(5.0*(10.0*(x^9.0))))

Here is another application of the power rule to differentiate (<em>x</em><sup>3 </sup>+ 3)<sup>4</sup>.

&gt;&gt;&gt; fex = make_pwr_expr(plus(elt1=make_pwr(’x’, 3.0),

elt2=make_const(3.0)), 4.0)

&gt;&gt;&gt; print fex

(((x^3.0)+3.0)^4.0)

&gt;&gt;&gt; drv = deriv(fex)

&gt;&gt;&gt; print drv

((4.0*(((x^3.0)+3.0)^3.0))*((3.0*(x^2.0))+0.0))

Let’s differentiate a simple polynomial <em>x</em><sup>2 </sup>+ <em>x </em>− 100.

&gt;&gt;&gt; fex = plus(elt1=make_pwr(’x’, 2.0), elt2=make_pwr(’x’, 1.0))

&gt;&gt;&gt; fex2 = plus(elt1=fex, elt2=make_const(-100.0))

&gt;&gt;&gt; print fex2

(((x^2.0)+(x^1.0))+-100.0)

&gt;&gt;&gt; drv = deriv(fex2)

&gt;&gt;&gt; print drv

(((2.0*(x^1.0))+(1.0*(x^0.0)))+0.0)

There is some starter code for you in deriv.py. Write your code for Problem 1 in there.

<h1>Problem 2: (1 points)</h1>

Function representations are really useful but if we want to do scientific computing with them, we must be able to convert them into real Python functions. Write a function tof (this abbreviation stands for ”to function”) that takes a function expression (see Problem 1) and returns a Python function that actually computes the function represented by this expression. Here is a few test runs.

Let’s work with <em>x</em><sup>2 </sup>+ <em>x </em>− 100.

&gt;&gt;&gt; fex = plus(elt1=make_pwr(’x’, 2.0), elt2=make_pwr(’x’, 1.0))

&gt;&gt;&gt; fex2 = plus(elt1=fex, elt2=make_const(-100.0))

&gt;&gt;&gt; print fex2

(((x^2.0)+(x^1.0))+-100.0)

Let’s define a Python function that computes this polynomial.

&gt;&gt;&gt; f = lambda x: x**2.0 + x – 100.0

&gt;&gt;&gt; f

&lt;function &lt;lambda&gt; at 0x7fcf2bf40050&gt;

Let’s run tof on fex2 and save the function returned by tof in the variable tf. We also define a test function and test f and tf on [0<em>,</em>999].

&gt;&gt;&gt; tf = tof(fex2)

&gt;&gt;&gt; tf

&lt;function f at 0x7fcf2bf400c8&gt; &gt;&gt;&gt; def test():

for i in range(1000):

assert f(i) == tf(i)

print ’test passed’ &gt;&gt;&gt; test() test passed

Let’s do another test with 5<em>x</em><sup>2</sup>.

&gt;&gt;&gt; fex = prod(mult1=make_const(5.0), mult2=make_pwr(’x’, 2.0))

&gt;&gt;&gt; print fex

(5.0*(x^2.0))

&gt;&gt;&gt; f = lambda x: 5.0*(x**2.0)

&gt;&gt;&gt; tf = tof(fex) &gt;&gt;&gt; def test():

for i in range(1000):

assert f(i) == tf(i)

print ’test passed’

&gt;&gt;&gt; test() test passed

There is some starter code for you in tof.py. Write your code for Problem 2 in there.

<h1>Problem 3: (1 points)</h1>

Let’s finish it off by writing a function graph_drv that takes an function expression, differentiates it, converts both the function expression and the derivative expression into functions with tof and graphs both functions in the same plot. The function also takes a two-element list of floats, xlim, and another two element list of floats, ylim. These two lists specify the lower and upper bounds for the x-axis and y-axis, respectively, for the plot’s grid. You should study the plotting code segments from the first two lectures (e.g., lec01_01.py and lect02_03.py) on how to use xlim and ylim.

Here is an example of applying graph_drv to the representation of 2<em>x</em><sup>5</sup>. The plot of the function and its derivative for this test is shown in Fig. 1.

&gt;&gt;&gt; prd = prod(mult1=make_const(2.0), mult2=make_pwr(’x’, 5.0))

&gt;&gt;&gt; graph_drv(prd, [-3.0, 3.0], [-50.0, 50.0])

Let’s apply graph_drv to the representation of <em>x</em><sup>4 </sup>+ <em>x</em><sup>3 </sup>+ <em>x</em>. The plot of the function and its derivative for this test is shown in Fig. 2.

&gt;&gt;&gt; fex1 = make_pwr(’x’, 4.0)

&gt;&gt;&gt; fex2 = make_pwr(’x’, 3.0)

&gt;&gt;&gt; fex3 = make_pwr(’x’, 1.0)

&gt;&gt;&gt; fex4 = plus(elt1=fex1, elt2=fex2)

&gt;&gt;&gt; fex5 = plus(elt1=fex4, elt2=fex3)

&gt;&gt;&gt; graph_drv(fex5, [-2.5, 2.5], [-10.0, 10.0])

There is some starter code for you in graphdrv.py. Write your code for Problem 3 in there.

Figure 1: Plots of 2<em>x</em><sup>5 </sup>and 10<em>x</em><sup>4</sup>.

Figure 2: Plots of <em>x</em><sup>4 </sup>+ <em>x</em><sup>3 </sup>+ <em>x </em>and 4<em>x</em><sup>3 </sup>+ 3<em>x</em><sup>2 </sup>+ 1.