---
layout: post
title: Hardware accelerated linear system solver

image: why-the-pendulum/pendulum_trajectory.png

---

Ever wanted to solve linear systems <em>really fast</em>? Yeah, me too.

### What is a linear system anyway and why should I care?

Let's get the definitions out of the way quickly. Linear systems of equations or linear systems are sets of equations that need to solved together to get a result. These equations are all of the linear variety (no sines or cosines or other funny business). Let's do an example:

$$ 1 x_1 + 2 x_2 = 5 $$

$$ 1 x_1 + 1 x_2 = 3 $$

Linear systems can also be represented in a more compact way using matrices. We can represent our example above in matrix form as follows:

$$ \begin{bmatrix}1 & 2 \\ 1 & 1\end{bmatrix}    \begin{bmatrix}x_1 \\x_2\end{bmatrix}  = \begin{bmatrix}5 \\3\end{bmatrix} $$

By looking at them, it might be clear how they are analogous. We can further simplify by giving each matrix and vector above a name. We will call the matrix \\(A\\), the vector of x's will be called \\(x\\) and the vector of numbers after the equals sign will be called \\(b\\). Now we have:

$$ Ax = b $$


But why should you care? You solved linear systems like this in middle school (maybe not using matrices, but its the same idea). As it turns out, linear systems can be a tool for solving hard problems, and a useful one at that. Additionally, we have really good tools for solving linear systems very quickly. In school, you probably solved small linear systems of maybe two equations and two unknowns like our example above, but linear systems can be huge and in practice they often are. Our little exercise substituting one equation into another works generally, but when there are hundreds of thousands of equations to solve, or when we need to solve equations in nanoseconds, it becomes the work of a computer. 

Such large linear systems often appear in optimization problems where the goal is to maximize something according to some constraints. As such, they are useful almost everywhere optimization is found from vaccine design, to robotics, to event planning (logistics), personal finance (economics), and many other fields.

