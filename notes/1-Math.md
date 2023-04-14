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

<img src="1-Math.assets/image-20230413162748448.png" alt="image-20230413162748448" style="zoom:67%;" />

- Measure how close  two directions are

- Decompose a vector
- Determine forward/backward

<img src="1-Math.assets/image-20230413163042639.png" alt="image-20230413163042639" style="zoom:67%;" />





### Cross Product in Graphics

$$
\vec{a}\times \vec{b}=-\vec{b}\times \vec{a}
$$

$$
||\vec{a}\times \vec{b}||=||\vec{a}||||\vec{b}||\sin{\theta}
$$



<img src="1-Math.assets/image-20230413164013038.png" alt="image-20230413164013038" style="zoom: 67%;" />

- Cross product is orthogonal to two initial vectors 
- Direction determined by right-hand rule
- Useful in constructing coordinate systems

<img src="1-Math.assets/image-20230413163823732.png" alt="image-20230413163823732" style="zoom:67%;" />

- Determine left / right

<img src="1-Math.assets/image-20230413163915545.png" alt="image-20230413163915545" style="zoom: 67%;" />

- Determine inside / outside $\vec{AB} \times \vec{AP}, \vec{BC} \times \vec{BP}, \vec{CA} \times \vec{CP} $ 

<img src="1-Math.assets/image-20230413163941684.png" alt="image-20230413163941684" style="zoom:67%;" />

**corner case, up to you**

### Orthonormal bases and coordinate frames

<img src="1-Math.assets/image-20230413165010254.png" alt="image-20230413165010254" style="zoom: 67%;" />



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

旋转矩阵的逆就是它的转置：正交矩阵  $A^{-1} = A^T$

<img src="1-Math.assets/image-20230414101022500.png" alt="image-20230414101022500" style="zoom: 50%;" />

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

### View/Camera transformation

> 拍照流程：（**MVP变换**）
>
> - Model transformation（模型变换）: 人物找到一个好的位置
>
> - **View transformation**（视图变换）：相机在一个好的视角，相机放在什么位置
> - Projection transformation（投影变换）：将3D场景变为2D图片



----

**Define the camera 定义相机**

- 相机位置
- 相机看的方向
- 相机的向上方向

<img src="1-Math.assets/image-20230414105324416.png" alt="image-20230414105324416" style="zoom:67%;" />

*任何相机的运动都可以看作模型在动，而相机不动，因此可以将相机定义在**原点**，**$y$轴**为向上方向，永远向**$-z$轴**看*

<img src="1-Math.assets/image-20230414105529752.png" alt="image-20230414105529752" style="zoom:67%;" />

----

**View Transformation** : 将任意相机移到原点标准相机setting

<img src="1-Math.assets/image-20230414110028931.png" alt="image-20230414110028931" style="zoom:67%;" />

<img src="1-Math.assets/image-20230414110045491.png" alt="image-20230414110045491" style="zoom:67%;" />

*旋转矩阵是正交矩阵，逆矩阵就是转置矩阵*



---

**Summary**

<img src="1-Math.assets/image-20230414110455966.png" alt="image-20230414110455966" style="zoom:67%;" />



### Projection transformation

> - 投影变换：3D –> 2D
>   - 透视投影：近大远小，人眼成像
>   - 正交投影：没有近大远小，工程制图

<img src="1-Math.assets/image-20230414110945294.png" alt="image-20230414110945294" style="zoom:67%;" />

#### Orthographic projection

> 正交投影

---

**Definition**

- A simple way of understanding

<img src="1-Math.assets/image-20230414111112476.png" alt="image-20230414111112476" style="zoom:67%;" />

- *任意长方体映射成一个标准正方体* 
  - 注意远近$[f,n]$是沿$-z$方向看（右手系）
  - 没有考虑旋转

<img src="1-Math.assets/image-20230414111339364.png" alt="image-20230414111339364" style="zoom:67%;" />

---

**Transformation matrix**

<img src="1-Math.assets/image-20230414112609073.png" alt="image-20230414112609073" style="zoom:67%;" />



#### Perspective projection

> 透视投影：近大远小；平行线不再平行，会相交

<img src="1-Math.assets/image-20230414112932717.png" alt="image-20230414112932717" style="zoom:50%;" />

----

**Definition**

- 先将截锥体“挤压”成长方体
- 然后再进行正交投影

<img src="1-Math.assets/image-20230414113349772.png" alt="image-20230414113349772" style="zoom:67%;" />



----

**“squish” Matrix: $M_{persp \rightarrow ortho}$**

- *similar triangle: 挤压$y$轴*

<img src="1-Math.assets/image-20230414113916388.png" alt="image-20230414113916388" style="zoom:67%;" />

- *所有点都要经过和$y$轴一样的上下“挤压”变换，然后考虑前后移动*

<img src="1-Math.assets/image-20230414114042800.png" alt="image-20230414114042800" style="zoom:67%;" />

- *取特殊点求解未知数*

<img src="1-Math.assets/image-20230414114434038.png" alt="image-20230414114434038" style="zoom:67%;" />

- Observation: the third row is responsible for z’ 

  - **Any point on the near plane will not change**

  <img src="1-Math.assets/image-20230414114647939.png" alt="image-20230414114647939" style="zoom: 33%;" />

  -  **Any point’s z on the far plane will not change**

  <img src="1-Math.assets/image-20230414114839981.png" alt="image-20230414114839981" style="zoom:33%;" />



- *解未知数*

<img src="1-Math.assets/image-20230414115135401.png" alt="image-20230414115135401" style="zoom:67%;" />

> 中间点的位置变化
