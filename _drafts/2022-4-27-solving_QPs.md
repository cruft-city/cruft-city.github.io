---
layout: post
title: Solving Quadratic Programs with Active Set on the FPGA

image: why-the-pendulum/pendulum_trajectory.png

---

Quadratic programs are everywhere. They underly many fundamental computational processes like finite element analysis (matching boundary conditions across different domains of the simulation) and force control algorithms for balancing robots (calculating minimum force solutions while respecting constraints). Today we're going to solve some of them using an algorithm called <em>active set</em>.

Active set requires solving linear systems which I described HERE.

#### 1000 ft. view
The goal of a quadratic program is to find the minimum of a cost function. The cost function describes something that we don't want, like spending money or using energy. The solution to the quadratice program produces the least spending or the least energy use. In particular, a quadratic program uses a quadratic cost function. In the simplest case, our quadratic function is just a function of x like so:

$$y = ax^2 + bx + c$$

This is a parabola, which looks like this:

[PICTURE OF PARABOLA]

As before, our goal is to find the minimum of this function. In this case, if \\(a\\) is positive, then the parabola looks like a cup and the minimum is where the derivative is zero. If \\(a\\) is negative, then the parabola looks like an upside down cup and the minimum is at \\(x= \pm \infty\\) because the farther you go, the more negative the function gets. So, the algorithm here is pretty simple.

Now, let's say we have constraints on what \\(x\\) can be. Maybe we want to find the minimum of our parabola in the region where \\(1 < x < 10 \\). This is slightly more complicated, but we can still approach it as before as long as we also check the endpoints (\\x=1\\) and (\\x=10\\)) to see if they are the minimums.

The whole thing gets more complicated when our quadratic program is a function of x and y. Now, we have something that looks like this:

[PICTURE OF 3D PARABOLA]

We can still use our derivative trick from before, but the problems start when we add constraints. Now, instead of just constraints like this: \\(1 < x < 10 \\). We can have complex 2D shapes, like maybe x and y must be inside of a circle centered at (\\(1,2)\\). How do we check all the constraint endpoints points now? The whole thing gets even worse as we increase the dimension of our problem. Like, what about a quadratic program that's a function of 100 variables? Clearly, we need a better way to approach this problem. The first step is to admit that we have a problem. The second step is to describe this new, high dimensional problem mathematically. For that, we'll use matrices.

#### The matrix version
Quadratic programs are optimization problems that look like this:

$$\min_{x=[x_k,...,x_{k+N-1}]} \ \ \frac{1}{2} x^T Q x - c^T x \\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \textrm{s.t.} \ \ Ax = b $$

The first part is the cost function:

$$\min_{x=[x_k,...,x_{k+N-1}]} \ \ \frac{1}{2} x^T Q x - c^T x$$

The first part of the cost function, the \\(\min_{x=[x_k,...,x_{k+N-1}]}\\) tells us that we are looking for the minimum value, and that we can change the values of all of the x variables in order to find that minimum, but not the others.

Now, onwards to the second part of the cost function: \\(\frac{1}{2} x^T Q x - c^T x\\) is the way we write a quadratic equation (\\(\frac{1}{2} qx^2 - cx\\)) in matrix form. \\(\frac{1}{2} qx^2 - cx\\) doesn't quite look like our previous simple quadratic equation: (\\ax^2 + bx + c\\), specifically, we've added a factor of 1/2 and removed the value c. Despite these changes, the resulting equation is basically the same. The factor of 1/2 is just a convention and the value c can be dropped because it just moves our function up or down by a finite amount which changes the minimum value, but doesnt change where the minimum occurs. Since our solver tells us where the minimum value occurs, this doesn't change anything, so we can drop it.

The remaining part of the problem specification is the constraint: \\(textrm{s.t.} \ \ Ax = b\\) means that the cost function needs to be optimized "subject to" the constraint that \\(Ax = b\\). This means that we can change all the x variables however we want, as long as they still satisfy that equation.

Here are some plots of quadratic programs and constraints:

[PICTURES OF QUADRATIC PROGRAMS IN 2D AND 3D]

Now we know how to describe our problem, let's figure out how to solve it.

#### Active set
[Active set]() is the answer to many of our problems. There are other ways to solve QPs like [interior point](), but active set is conceptually simple, so it's a good place to start. To
