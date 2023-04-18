# Geometry

- [Introduction to geometry](#Introduction-to-geometry)
  - [Examples of geometry](#Examples-of-geometry)
  - [Various representations of geometry](#Various-representations-of-geometry)













## Introduction to geometry

### Examples of geometry

<div align="center">
    <img src="4-Geometry.assets/image-20230418161454634.png" alt="image-20230418161454634" style="zoom:50%;" width="40%"/>
    <img src="4-Geometry.assets/image-20230418161533691.png" alt="image-20230418161533691" style="zoom:50%;" width="40%"/>
</div>



### Various representations of geometry

- 隐式表示
  - 
  - 水平集
  - 距离函数
- 显示表示
  - 点云
  - 多边形网格

<div align="center">
    <img src="4-Geometry.assets/image-20230418161708879.png" alt="image-20230418161708879" style="zoom:67%;" />
</div>

#### Implicit Representations of Geometry

- 隐式表示
  - 根据点满足某种特定关系，而不是具体的位置。可以通过对这些点进行归类等
  - 只要满足$f(x,y,z)$=0的点就是几何的表面

<div align="center">
    <img src="4-Geometry.assets/image-20230418162031725.png" alt="image-20230418162031725" style="zoom:50%;" />
</div>

-----

**Implicit Surface – Sampling Can Be Hard**

> 无法通过解析式看出几何形状，不容易找到所有的点

<div align="center">
    <img src="4-Geometry.assets/image-20230418162238621.png" alt="image-20230418162238621" style="zoom:67%;" />
</div>

----

**Implicit Surface – Inside/Outside Tests Easy**

> 便于判断点是否在物体内或外

<div align="center">
    <img src="4-Geometry.assets/image-20230418162409350.png" alt="image-20230418162409350" style="zoom:67%;" />
</div>



#### Explicit Representations of Geometry

> - 显示表达：
>   - 直接给出几何形状点的位置
>   - 通过参数映射的方式定义的表面

<div align="center">
    <img src="4-Geometry.assets/image-20230418162545025.png" alt="image-20230418162545025" style="zoom:67%;" />
</div>

----

**Explicit Surface – Sampling Is Easy**

> 将$(u,v)$映射为$(x,y,z)$,可以很方便找到所有的点

<div align="center">
    <img src="4-Geometry.assets/image-20230418162941034.png" alt="image-20230418162941034" style="zoom:67%;" />
</div>

----

**Explicit Surface – Inside/Outside Test Hard**

> 判断一个点是否在表面内/外较为困难

<div align="center">
    <img src="4-Geometry.assets/image-20230418163137912.png" alt="image-20230418163137912" style="zoom:67%;" />
</div>

-----

**Best Representation  Depends on the Task!**

<div align="center">
    <img src="4-Geometry.assets/image-20230418163239432.png" alt="image-20230418163239432" style="zoom:67%;" />
</div>

### Implicit Representations in  Computer Graphics

---

#### Algebraic Surfaces (Implicit)

> 缺点：不直观

<div align="center">
    <img src="4-Geometry.assets/image-20230418163356787.png" alt="image-20230418163356787" style="zoom:67%;" />
</div>

----

#### Constructive Solid Geometry (Implicit)

> CSG：通过基本几何的基本布尔运算获取复杂的结合。**非常常见的建模方法**

<div align="center">
    <img src="4-Geometry.assets/image-20230418163455273.png" alt="image-20230418163455273" style="zoom:67%;" />
</div>

-----

#### Distance Functions (Implicit)

> - 距离函数
>   - 对于任何一个几何，描述点到几何平面的最小距离，该距离可以是正的，也可以是负的。
>   - 可以将两个几何的距离函数进行融合(blend)，从而获得融合后的几何形状

<div align="center">
    <img src="4-Geometry.assets/image-20230418163638484.png" alt="image-20230418163638484" style="zoom:67%;" />
</div>

> - SDF:有符号距离
>   - 两个物体A和B分别挡住了视口的$1/3,2/3$,求出物体从左到右移动过程的中间状态。如果做线性的blend，则是左边黑色，中间灰色，右边白色，没有展示运动过程的效果。（希望的左边一半是黑的，一半是灰的；中间一半是灰的，一半是白的）
>   - 使用SDF可以模拟这种效果

<div align="center">
    <img src="4-Geometry.assets/image-20230418164633270.png" alt="image-20230418164633270" style="zoom:67%;" />
</div>

- **Blending Distance Functions (Implicit)**

> 任何物体的距离函数，然后进行融合

<div align="center">
    <img src="4-Geometry.assets/image-20230418172651129.png" alt="image-20230418172651129" style="zoom:67%;" />
</div>

- Scene of Pure Distance Functions

<div align="center">
    <img src="4-Geometry.assets/image-20230418172858265.png" alt="image-20230418172858265" style="zoom:67%;" />
</div>

----

#### Level Set Methods (Also implicit)

> - 水平集方法：
>   - 与DF一样，只是函数的表示写在格子中。$f(x)=0$等高线

<div align="center">
    <img src="4-Geometry.assets/image-20230418173028773.png" alt="image-20230418173028773" style="zoom:67%;" />
</div>

- Level Sets from Medical Data (CT, MRI, etc.)

<div align="center">
    <img src="4-Geometry.assets/image-20230418173257446.png" alt="image-20230418173257446" style="zoom:50%;" />
</div>

- Level Sets in Physical Simulation

<div align="center">
    <img src="4-Geometry.assets/image-20230418173328867.png" alt="image-20230418173328867" style="zoom:67%;" />
</div>

----

**Fractals (Implicit)** 分形

<div align="center">
    <img src="4-Geometry.assets/image-20230418173422543.png" alt="image-20230418173422543" style="zoom:67%;" />
</div>

#### Implicit Representations - Pros & Cons

- Pros:  
  - compact description (e.g., a function) 
  - certain queries easy (inside object, distance to surface)  
  - good for ray-to-surface intersection (more later)  容易做光线求交
  - for simple shapes, exact description / no sampling error
  - easy to handle changes in topology (e.g., fluid)  容易表示拓扑
- Cons:  
  - difficult to model complex shapes 复杂场景难以表示，如奶牛



### Explicit Representations in  Computer Graphics

- Many Explicit Representations in Graphics
  - 三角形网格
  - Bezier曲面

<div align="center">
    <img src="4-Geometry.assets/image-20230418173933571.png" alt="image-20230418173933571" style="zoom:67%;" />
</div>

#### Point Cloud (Explicit)

<div align="center">
    <img src="4-Geometry.assets/image-20230418174120701.png" alt="image-20230418174120701" style="zoom:67%;" />
</div>



#### Polygon Mesh (Explicit)

<div align="center">
    <img src="4-Geometry.assets/image-20230418174158930.png" alt="image-20230418174158930" style="zoom:67%;" />
</div>







