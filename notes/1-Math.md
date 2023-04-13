# Math

- [A Swift and Brutal Introduction to Linear Algebra](#A Swift and Brutal Introduction to Linear Algebra)
  - [Dot Product in Graphics](#Dot Product in Graphics)
  - [Cross  Product in Graphics](#Cross  Product in Graphics)
  - [Orthonormal bases and coordinate frames](#Orthonormal bases and coordinate frames)


- [Transformation](#Transformation)
  - [Matrices](#Matrices)
  - [2D transformations: rotation, scale, shear](#2D transformations: rotation, scale, shear)
  - [Homogeneous coordinates](#Homogeneous coordinates)
  - [Composing transforms](#Composing transforms)
  - [  3D transformations](#  3D transformations)





## A ==Swift== and ==Brutal== Introduction to Linear Algebra

- Basic Mathematics
  - Linear algebra, calculus, statistics
- Basic physics
  - Optics, Mechanics
- Misc
  - Signal processing
  - Numerical analysis



### Dot Product in Graphics

- Find angle between two vectors
- Find ==projection== of one vector on another

![image-20230413162748448](1-Math.assets/image-20230413162748448.png)

- Measure how close  two directions are

- Decompose a vector
- Determine forward/backward

![image-20230413163042639](1-Math.assets/image-20230413163042639.png)





### Cross  Product in Graphics

$$
\vec{a}\times \vec{b}=-\vec{b}\times \vec{a} \\
||\vec{a}\times \vec{b}||=||\vec{a}||||\vec{b}||\sin{\theta}
$$

<img src="1-Math.assets/image-20230413164013038.png" alt="image-20230413164013038" style="zoom:80%;" />

- Cross product is orthogonal to two initial vectors 
- Direction determined by right-hand rule
- Useful in constructing coordinate systems

![image-20230413163823732](1-Math.assets/image-20230413163823732.png)

- Determine left / right

<img src="1-Math.assets/image-20230413163915545.png" alt="image-20230413163915545" style="zoom:50%;" />

- Determine inside / outside $\vec{AB} \times \vec{AP}, \vec{BC} \times \vec{BP}, \vec{CA} \times \vec{CP} $ 

![image-20230413163941684](1-Math.assets/image-20230413163941684.png)

==corner case, up to you==

### Orthonormal bases and coordinate frames

<img src="1-Math.assets/image-20230413165010254.png" alt="image-20230413165010254" style="zoom:80%;" />



## Transformation

### Matrices

- **Non-commutative**: AB and BA are different in general

- Associative and distributive  
  - $(AB)C=A(BC)$
  - $A(B+C) = AB + AC$
  - $(A+B)C = AC + BC$

- $(AB)^ T = B^T A^T$
- $AA^{-1}= A^{-1}A=I$
- $(AB)^{-1}=B^{-1}A^{-1}$

- Vector multiplication in Matrix form

<img src="1-Math.assets/image-20230413170022351.png" alt="image-20230413170022351" style="zoom:67%;" />

### 2D transformations: rotation, scale, shear









### Homogeneous coordinates









### Composing transforms









###   3D transformations





























































































































