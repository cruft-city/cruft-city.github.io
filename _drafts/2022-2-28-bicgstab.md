---
layout: post
title: Hardware accelerated linear system solver

image: why-the-pendulum/pendulum_trajectory.png

---

Ever wanted to solve linear systems <em>really fast</em>? Yeah, me too.

### What is a linear system anyway and why should I care?

Let's get the definitions out of the way quickly. Linear systems of equations or linear systems are sets of equations that need to solved together to get a result. These equations are all of the linear variety (no sines or cosines or other funny business). Let's do an example:
Let's get the definitions out of the way quickly. Linear systems of equations are sets of equations that need to solved together to get a result. These equations are all of the linear variety (no sines or cosines or other funny business). In the example below, we'd need to solve for x1 and x2 which work to make both equations true at the same time.

$$ 1 x_1 + 2 x_2 = 5 $$

$$ 1 x_1 + 1 x_2 = 3 $$

Linear systems can also be represented in a more compact way using matrices. We can represent our example above in matrix form as follows:

$$ \begin{bmatrix}1 & 2 \\ 1 & 1\end{bmatrix}    \begin{bmatrix}x_1 \\x_2\end{bmatrix}  = \begin{bmatrix}5 \\3\end{bmatrix} $$

By looking at them, it might be clear how they are analogous. We can further simplify by giving each matrix and vector above a name. We will call the matrix \\(A\\), the vector of x's will be called \\(x\\) and the vector of numbers after the equals sign will be called \\(b\\). Now we have:
The first row corresponds to the first equation, and the second row corresponds to the second equation. We can further simplify by giving each matrix and vector above a name. We will call the matrix \\(A\\), the vector of x's will be called \\(x\\) and the vector of numbers after the equals sign will be called \\(b\\). Now we have:

$$ Ax = b $$


These kinds of systems show up in middle school math (maybe not using matrices, but its the same idea). As it turns out, linear systems can be a tool for solving hard problems, and a useful one at that. Additionally, we have really good tools for solving linear systems very quickly. In school, you probably solved small linear systems of maybe two equations and two unknowns like our example above, but linear systems can be huge and in practice they often are. Our little exercise substituting one equation into another works generally, but when there are hundreds of thousands of equations to solve, or when we need to solve equations in nanoseconds, it becomes the work of a computer.
But why should you care? You solved linear systems like this in middle school (maybe not using matrices, but its the same idea). As it turns out, linear systems can be a tool for solving hard optimization problems, and a useful one at that. Additionally, we have really good algorithms for solving linear systems very quickly. In school, you probably solved small linear systems of maybe two equations and two unknowns like our example above, but linear systems can be huge and in practice they often are. To solve the example above, we'd substitute one equation into another. This works generally, but when there are hundreds of thousands of equations to solve, or when we need to solve equations in nanoseconds, it becomes the work of a computer. 

Such large linear systems often appear in optimization problems where the goal is to maximize something according to some constraints. As such, they are useful almost everywhere optimization is found from vaccine design, to robotics, to event planning (logistics), personal finance (economics), and many other fields.






### Performance
So how fast is this FPGA implementation? As it turns out, matrices can have significant structure which makes some algorithms mush better than others. In this case, I made this specifically with the idea of solving linear systems in quadratic programs resulting from a model predictive controller (posts on these forthcoming), so let's test it on some of those matrices. Up to this point, I haven't implemented any optimizations which exploit the structure of these matrices, but maybe that will come later.

I am going to benchmark against Numpy, which is comparable to C (perhaps slightly slower). I am most interested in how the performance of the implementation scales. In the following test, I will solve matrices of increasing size and compare the speed between Numpy and my FPGA implementation. I benchmark against two algorithms using Numpy, the first is NP_BiCGSTAB, which is the same algorithm as implemented on the FPGA, but running in python. The second python benchmark is NP_LAPACK which uses Numpy's linear system solver (which in turn uses LAPACK's _gesv routine).

The test uses matrices of sizes between 10 and ###### the largest problem that my implementation can fit in the FPGA.


Here a plot of the performance:
### What is a hardware acceleration?

The other bit of background we need to get out of the way is, what does it mean to "hardware accelerate" something? All of our computers and phones have a CPU, this is a general purpose chip which does most of the calculations you request by interacting with the computer. However, there are some tasks that have to happen so often, and so fast, that its better to design a specific chip to do them extra-fast rather than burdening the general purpose CPU with them. No doubt, the CPU could do these tasks, but because of the requirement on speed or the frequency at which they need to be done, it would bog the CPU down. An example of this is the GPU. GPUs draw graphics on the screen, a task which needs to happen often and would therefore slow down the CPU. GPUs specialize in doing vector calculations (among other things) which are essential operations for rendering videogames and apparently mining bitcoins among other things. So the GPU is a hardware accelerator for a large number of tasks including calculating light rays bouncing around in a digital world. 

(REFLECTION BOX RENDER)

So, when we want to hardware accelerate something, we need to design a chip to do that task. A CPU might be able to do one multiply per clock cycle, but maybe we could make our chip do two multiplies per clock cycle at the cost of being general purpose. Our chip will just do one task, but hopefully fast. It turns out, getting your own chip fabricated is an expensive ordeal (~$1 million), so we're not going to do that. Also, even if we were going to do that, it's a good idea to prototype the chip first before the big money is spent. The solution to our problems is the field programmable gate array (FPGA). FPGAs are an exciting type of chip which are programmable at the hardware level. The FPGA is made up of many many copies of <em>logic blocks</em>. These logic blocks are often a lookup table (often with four or six entries), an adding circuit, and a little piece of memory called a flip flop. When you program an FPGA, you tell it what it should do at the hardware level by setting the values of those lookup tables and the connections between those logic blocks. When you set the value of a lookup table, you set what comes out for each input. In this way, the lookup table can simulate a small block of digital logic. By setting a bunch of lookup tables and connecting them together, we can simulate big pieces of digital logic. This is how we will make our hardware accelerator.

(PICTURE OF FPGA FABRIC OR LOGIC GATES OR SOMETHING FROM VIVADO)

### The meat and potatos

Okay, now that we have that out of the way. Let's get down to it. Algorithms for solving linear systems come in two main flavors: solving the equation by inverting the system directly, or iteratively converging to a solution. To solve the system directly, we'd take the inverse of the \\(A\\) matrix in the example above, or we'd keep substituting each equation into other ones until we'd found the answer. This is nice because it gives you the right answer, but not so nice because it can take a long time, and until you've finished solving the problem, you don't have a good idea of what the final solution will look like. In the iterative methods, we do a set of steps which approximates the answer to the system. By repeating these steps, our approximation becomes better until it is effectively identical to the solution. These kinds of algorithms are a good fit for hardware acceleration because they involve doing the same simple calculations repeatedly. They are also handy because, if for some reason you need to get an answer faster, you can just take one of the earlier estimates of the solution. In this situation, you will be trading off accuracy for speed. This isn't an option when we are solving the system directly. 

The iterative algorithm that I chose was a modified version of the the biconjugate gradient method. Algorithms derived from the biconjugate gradient method are useful because they can solve systems where the \\(A\\) matrix is not symmetrical. The application I have in mind for this solver requires solving asymmetrical systems, making biconjugate gradient derived algorithms a clear favorite. The exact flavor of biconjugate gradient algorithm I implemented is the <em>biconjugate gradient stabilized</em> method (BiCGSTAB).

### The algorithm


### The implementation


### But how fast does it go?