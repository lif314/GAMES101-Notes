# Math

- [A Swift and Brutal Introduction to Linear Algebra](#A-Swift-and-Brutal-Introduction-to-Linear-Algebra)
  - [Dot Product in Graphics](#Dot-Product-in-Graphics)
  - [Cross Product in Graphics](#Cross-Product-in-Graphics)
  - [Orthonormal bases and coordinate frames](#Orthonormal-bases-and-coordinate-frames)


- [Transformation](#Transformation)
  - [Matrices](#Matrices)
  - [2D transformations](#2D-transformations)
  - [Homogeneous coordinates](#Homogeneous-coordinates)
  - [Composing transforms](#Composing-transforms)
  - [  3D transformations](#3D-transformations)

- [Viewing transformation](#Viewing-transformation)
  - [View/Camera transformation](#View/Camera-transformation)
  - [Projection transformation](#Projection-transformation)
    - [Orthographic projection](#Orthographic-projection)
    - [Perspective projection](#Perspective-projection)



## A Swift and Brutal Introduction to Linear Algebra

- Basic Mathematics
  - Linear algebra, calculus, statistics
- Basic physics
  - Optics, Mechanics
- Misc
  - Signal processing
  - Numerical analysis

### Dot Product in Graphics

- Find angle between two vectors
- Find **projection** of one vector on another

![image-20230413162748448](1-Math.assets/image-20230413162748448.png)

- Measure how close  two directions are

- Decompose a vector
- Determine forward/backward

![image-20230413163042639](1-Math.assets/image-20230413163042639.png)





### Cross Product in Graphics

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

**corner case, up to you**

### Orthonormal bases and coordinate frames

<img src="1-Math.assets/image-20230413165010254.png" alt="image-20230413165010254" style="zoom:80%;" />



## Transformation

- Modeling: translation/rotation/scaling
- Viewing: (3D to 2D) projection

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

### 2D transformations

#### Representing transformations using matrices

----

**Scale Matrix**

<img src="1-Math.assets/image-20230414085515151.png" alt="image-20230414085515151" style="zoom:67%;" />



----

**Reflection Matrix**

<img src="1-Math.assets/image-20230414085702508.png" alt="image-20230414085702508" style="zoom:67%;" />

----

**Shear Matrix（切变）**

<img src="1-Math.assets/image-20230414085913030.png" alt="image-20230414085913030" style="zoom:67%;" />

----

**Rotation Matrix**

<img src="1-Math.assets/image-20230414091605734.png" alt="image-20230414091605734" style="zoom:67%;" />

旋转矩阵的逆就是它的转置：正交矩阵 $A^{-1} = A^T$

<img src="1-Math.assets/image-20230414101022500.png" alt="image-20230414101022500" style="zoom:67%;" />

----

**Linear Transforms（线性变换） = Matrices**

<center><img src="1-Math.assets/image-20230414092356178.png" alt="image-20230414092356178" style="zoom:67%;" /></center>



----

**Translation（平移）**：不是线性变换，无法写成矩阵形式

<img src="1-Math.assets/image-20230414092728086.png" alt="image-20230414092728086" style="zoom:67%;" />

<img src="1-Math.assets/image-20230414092910978.png" alt="image-20230414092910978" style="zoom:67%;" />

### Homogeneous coordinates

----

**Definition**

<img src="1-Math.assets/image-20230414093710651.png" alt="image-20230414093710651" style="zoom:67%;" />

向量具有平移不变性

- vector + vector = vector
- point - point = vector
- point + vector = point 点沿着向量移动
- point + point = ?? (point) 两个点的中点

$$
\left(\begin{array}{c}
x \\
y \\
w
\end{array}\right) \text { is the } 2 \mathrm{D} \text { point }\left(\begin{array}{c}
x / w \\
y / w \\
1
\end{array}\right), w \neq 0
$$



---

**Affine Transformations（仿射变换）**

- Affine map = linear map + translation

$$
\left(\begin{array}{c}
x' \\
y' \\
\end{array}\right) 
=
\left(\begin{array}{c}
a & b \\
c & d\\
\end{array}\right)
\cdot
\left(\begin{array}{c}
x \\
y \\
\end{array}\right) 
+ 
\left(\begin{array}{c}
t_x \\
t_y \\
\end{array}\right)
$$

- Using homogenous coordinates

$$
\left(\begin{array}{c}
x' \\
y' \\
1
\end{array}\right) 
=
\left(\begin{array}{c}
a & b & t_x \\
c & d & t_y \\
0 & 0 & 1
\end{array}\right)
\cdot
\left(\begin{array}{c}
x \\
y \\
1
\end{array}\right)
$$



---

**2D Transformations**

<img src="1-Math.assets/image-20230414094923736.png" alt="image-20230414094923736" style="zoom:67%;" />



----

**Inverse Transform**  逆矩阵

<center><img src="1-Math.assets/image-20230414095113639.png" alt="image-20230414095113639" style="zoom:67%;" /></center>



### Composing transforms

组合变换

---

**Transform Ordering Matters**

<img src="1-Math.assets/image-20230414095352885.png" alt="image-20230414095352885" style="zoom:67%;" />

---

**从右到左相乘**

<img src="1-Math.assets/image-20230414095609116.png" alt="image-20230414095609116" style="zoom:67%;" />

矩阵没有交换律，但有结合律

<img src="1-Math.assets/image-20230414095714018.png" alt="image-20230414095714018" style="zoom:67%;" />

---

**Decomposing Complex Transforms**

先变换到原点处，再进行变换

<img src="1-Math.assets/image-20230414100103106.png" alt="image-20230414100103106" style="zoom:67%;" />

###   3D transformations

---

**Homogeneous coordinates representation**

<img src="1-Math.assets/image-20230414100348155.png" alt="image-20230414100348155" style="zoom:67%;" />

<img src="1-Math.assets/image-20230414100411264.png" alt="image-20230414100411264" style="zoom:67%;" />

先进行旋转变换，再进行平移变换

$$
\left(\begin{array}{l}
x^{\prime} \\
y^{\prime} \\
z^{\prime} \\
\end{array}\right)=\left(\begin{array}{lllc}
a & b & c \\
d & e & f \\
g & h & i \\
\end{array}\right) 
\cdot
\left(\begin{array}{l}
x \\
y \\
z \\
\end{array}\right)
+
\left(\begin{array}{l}
t_x \\
t_y \\
t_z \\
\end{array}\right)
$$

----

**3D Transformation Matrices**

<img src="1-Math.assets/image-20230414101824754.png" alt="image-20230414101824754" style="zoom:67%;" />

循环对称性：  $R_y: z\times x \rightarrow y$

<img src="1-Math.assets/image-20230414101846897.png" alt="image-20230414101846897" style="zoom:67%;" />

<img src="1-Math.assets/image-20230414101947135.png" alt="image-20230414101947135" style="zoom:67%;" />

----

**Rodrigues’ Rotation Formula** 矩阵 –>  轴角表达

<img src="1-Math.assets/image-20230414102323483.png" alt="image-20230414102323483" style="zoom:67%;" />

## Viewing transformation

> 观测变换















### View/Camera transformation





### Projection transformation

> 投影变换



#### Orthographic projection

> 正交投影







#### Perspective projection

> 透视投影































































































