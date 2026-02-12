# Gas2D
This project aim to simulate the behaviour of a gas in two dimensions following a hard disk model.

## Introduction
This project is a rewrite in Rust of an undergraduate Python project.
The original project focused on Maxwell-Boltzmann distribution of a 2D gases, pressure measurment and a visulisation.

Due to being coded in Python at the time the visual side was rather limited in the amount of particles and calculations were very limited in terms of performance.
For instance it was near impossible to have a density high enough for crystallization phase change.

## Physics of 2D gases
We will use here a hard disk model. In this model gas molecules are treated as perfect disks or radius $R$ and have perfect elastic collisions.
The gas molecules are place inside a box (which will here corespond to the window of our program) of size $L$.
Each particles will collide both with the walls of the box and with eachother.

### Wall collisions
Wall collsions are very simple. The center position of the molecule $M$ can be split into 2 components $x$ and $y$ as well as a velocity $\vec{v}$ with 2 components $v_x$ and $v_y$.
So a wall collision happens whenever the ditance $d < R$. Whenever this conditon is fullfileld, we flip the components along the normal of the wall collided with.

### Particle collisions

## Numerical simulation
For this simulation we will be using the Rust programming language. It's a low level language akin to C, but enforces memory safety through the borrow checker.
It combines the advantages of a compilled language with explicit memory control and the memory safety of languages that are typically garbage collector language (such as Python)

### The data structure
We will define here the molecules by their position and speed $(x,y,v_x,v_y)$ and use Rust vectors. 
Unlike in C or C++, arrays aren't very handy and any efficiency gained from data being on the stack is negated by their difficutlt usage in Rust.

We will bundle the components in an SoA (Structure of Arrays) to maximize speed throught memory contingency and thus avoid cache misses as possible.
A simple pre-allocation will be performed when a fixed number of particles is given. This avoid costly resizing and alloacates a contiguous memory block on the heap from the start.

We will also use what is called spatial hashing, which will be discussed in further sections. we will use a Hashmap struture for organizing these partitions.
