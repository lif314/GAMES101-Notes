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

> 最广泛的使用

<div align="center">
    <img src="4-Geometry.assets/image-20230418174158930.png" alt="image-20230418174158930" style="zoom:67%;" />
</div>



**The Wavefront Object File (.obj) Format**

>  三角形面表示物体的文件格式：文本文件，点，法线，纹理坐标分开存储
>
> 立方体例子（存在冗余）：
>
> - v: 8个点
> - vn：6个面的6种不同的法线
> - vt：12个纹理坐标
> - f：三角形点的连接情况，(v/vt/vn)下标

<div align="center">
    <img src="4-Geometry.assets/image-20230418175934279.png" alt="image-20230418175934279" style="zoom:67%;" />
</div>

## Curves

> 曲线：显示几何



### Bézier Curves

> 贝塞尔曲线（显示表示方法）：关于参数t的曲线
>
> - 使用一系列的控制点去定义一条曲线，沿着某一方向，必须经过起止点

---

**Defining Cubic Bézier Curve With Tangents**

<div align="center">
    <img src="4-Geometry.assets/image-20230418182725412.png" alt="image-20230418182725412" style="zoom:67%;" />
</div>

----

**Evaluating Bézier Curves  (de Casteljau Algorithm)**

- Consider three points (quadratic Bezier) 二次贝塞尔曲线

- Insert a point using linear interpolation 假设起点在时间0，终点在时间1，找出时间t点在哪里。
  - 先假设起始点为$b_0b_1$，找到$t$对应的点$b_0^1$

<div align="center"> 
    <img src="4-Geometry.assets/image-20230418183319945.png" alt="image-20230418183319945" style="zoom:80%;" />
</div>

- Insert on both edges
  - 然后假设起始点为$b_1b_2$，找到$t$时刻对应的点$b_1^1$

<div align="center">
    <img src="4-Geometry.assets/image-20230418183351448.png" alt="image-20230418183351448" style="zoom:80%;" />
</div>

- Repeat recursively
  - 最后假设起始点为$b_0^1b_1^1$，找到$t$对应点$b_0^2$。这就是最终的结果

<div align="center">
    <img src="4-Geometry.assets/image-20230418183432604.png" alt="image-20230418183432604" style="zoom:80%;" />
</div>

- Run the same algorithm for every t in [0,1]

<div align="center">
    <img src="4-Geometry.assets/image-20230418184229491.png" alt="image-20230418184229491" style="zoom:80%;" />
</div>

---

**Four input points in total** 递归求解

<div align="center">
    <img src="4-Geometry.assets/image-20230418184545387.png" alt="image-20230418184545387" style="zoom:67%;" />
</div>

> Animation: Steven Wittens, Making Things with Maths, http://acko.net



### Bézier Curves  Algebraic Formula

> - 每两个点之间做线性插值
> - 重复构造

<div align="center">
    <img src="4-Geometry.assets/image-20230418185240824.png" alt="image-20230418185240824" style="zoom:80%;" />
</div>



- 二次贝塞尔曲线示例

<div align="center">
    <img src="4-Geometry.assets/image-20230418185355602.png" alt="image-20230418185355602" style="zoom:67%;" />
</div>

- 由所有控制点的线性组合而成，系数是个多项式(Bernstein polynomial) 二项分布

<div align="center">
    <img src="4-Geometry.assets/image-20230418185439096.png" alt="image-20230418185439096" style="zoom:80%;" />
</div>

- 示例：空间点也可以成立

<div align="center">
    <img src="4-Geometry.assets/image-20230418185509951.png" alt="image-20230418185509951" style="zoom:80%;" />
</div>

---

**Cubic Bézier Basis Functions**

<div align="center">
    <img src="4-Geometry.assets/image-20230418190445545.png" alt="image-20230418190445545" style="zoom:67%;" />
</div>

- **Properties of Bézier Curves**
  - 在**仿射变换**下保持不变：将空间点进行仿射变换后画出的曲线与原始点画出的曲线经过仿射变换后的结果相同
  - 凸包性质：曲线一点在几个控制点的凸包内

<center align="center">
    <img src="4-Geometry.assets/image-20230418190542774.png" alt="image-20230418190542774" style="zoom:80%;" />
</center>

> 凸包：包含所有点的最小多边形

<div align="center">
    <img src="4-Geometry.assets/image-20230418191212940.png" alt="image-20230418191212940" style="zoom:50%;" />
</div>

### Piecewise Bézier Curves

> 逐段贝塞尔曲线

- Higher-Order Bézier Curves?
  - 高阶贝塞尔曲线形成的曲线不容易控制

<div align="center">
    <img src="4-Geometry.assets/image-20230418191457480.png" alt="image-20230418191457480" style="zoom:67%;" />
</div>

- Piecewise Bézier Curves 逐段，**通常是4个点** PS种的钢笔工具，沿着两边可以拉动曲线
  - demo: David Eck, http://math.hws.edu/eck/cs424/notes2013/canvas/bezier.html
  - 曲线连续：导数要连续，控制杆两端等距共线

<div align="center">
    <img src="4-Geometry.assets/image-20230418191612408.png" alt="image-20230418191612408" style="zoom:67%;" />
</div>

- Piecewise Bézier Curve – Continuity 连续性

<img src="4-Geometry.assets/image-20230418191709395.png" alt="image-20230418191709395" style="zoom:67%;" />

- $C^0$连续：第一段终止点是第二段的起始点

<div align="center">
    <img src="4-Geometry.assets/image-20230418192212234.png" alt="image-20230418192212234" style="zoom:50%;" />
</div>

- $C^1$ continuity：第2段也是连续（一阶导数连续）

<div align="center">
    <img src="4-Geometry.assets/image-20230418193923134.png" alt="image-20230418193923134" style="zoom:50%;" />
</div>



----

**Other types of splines**

- Spline(样条)

<div align="center">
    <img src="4-Geometry.assets/image-20230418194228433.png" alt="image-20230418194228433" style="zoom:67%;" />
</div>



- B-splines 基函数样条，贝塞尔曲线的扩展，极其复杂

<div align="center">
    <img src="4-Geometry.assets/image-20230418194251323.png" alt="image-20230418194251323" style="zoom:50%;" />
</div>

> 更多曲线学习(NURBS)：Prof. Shi-Min Hu’s course: https://www.bilibili.com/video/ av66548502?from=search&seid=65256805876131485



## Surfaces

### Bézier Surfaces

<div align="center">
    <img src="4-Geometry.assets/image-20230418203654182.png" alt="image-20230418203654182" style="zoom:80%;" />
</div>



**Bicubic Bézier Surface Patch**

> 贝塞尔曲线–>贝塞尔曲面：4*4控制点

<img src="4-Geometry.assets/image-20230418203731348.png" alt="image-20230418203731348" style="zoom:67%;" />

**Visualizing Bicubic Bézier Surface Patch**

> - 在两个方向上使用贝塞尔曲线–> 双线性插值
>
> - Animation: Steven Wittens, Making Things with Maths, http://acko.net

<img src="4-Geometry.assets/image-20230418203755098.png" alt="image-20230418203755098" style="zoom:80%;" />

### Evaluating Bézier Surfaces

**Evaluating Surface Position For Parameters (u,v)**

> 两个时间$u,v$

<img src="4-Geometry.assets/image-20230418204204996.png" alt="image-20230418204204996" style="zoom:67%;" />





**Method: Separable 1D de Casteljau Algorithm**

> 先沿着$u$找4个点，然后沿着$v$。显示表示–>参数映射形式

<img src="4-Geometry.assets/image-20230418204236661.png" alt="image-20230418204236661" style="zoom:80%;" />

<img src="4-Geometry.assets/image-20230418204253144.png" alt="image-20230418204253144" style="zoom:80%;" />

## Mesh Operations: Geometry Processing

> 几何处理：网格细化，网格简化，网格正规化

<img src="4-Geometry.assets/image-20230418204322919.png" alt="image-20230418204322919" style="zoom:80%;" />

- Mesh Subdivision (upsampling)  上采样：增加分辨率

<div align="center">
    <img src="4-Geometry.assets/image-20230419084334906.png" alt="image-20230419084334906" style="zoom:67%;" />
</div>

- Mesh Simplification (downsampling) 下采样：降低分辨率，几何保持

<div align="center">
    <img src="4-Geometry.assets/image-20230419084408634.png" alt="image-20230419084408634" style="zoom:67%;" />
</div>

- Mesh Regularization (same #triangles) 规则化，提升质量

<div align="center">
    <img src="4-Geometry.assets/image-20230419084441488.png" alt="image-20230419084441488" style="zoom:67%;" />
</div>

### Subdivision

#### Loop Subdivision

> **细分：** “loop” — family name
>
> - 引入更多的三角形
> - 并使**形状发生变化**，使几何更加的光滑

<div align="center">
    <img src="4-Geometry.assets/image-20230419090635110.png" alt="image-20230419090635110" style="zoom:67%;" />
</div>



- 增加三角形的数量：每个三角形内部分为4个小三角形
- 调整三角形的位置：将三角形顶点区分为旧和新，分别使用不同规则改变其位置

<div align="center">
    <img src="4-Geometry.assets/image-20230419090648672.png" alt="image-20230419090648672" style="zoom:67%;" />
</div>



- 对于新的顶点：找到共享变边$AB$，不共享的点为$CD$，然后按照$3/8$和$1/8$进行加权平均

<div align="center">
    <img src="4-Geometry.assets/image-20230419090701150.png" alt="image-20230419090701150" style="zoom:67%;" />
</div>



- 对于旧的点：由自己和相邻旧的顶点决定。$n$为该点的度，$u$是与度有关的数，更新规则为如下：

<img src="4-Geometry.assets/image-20230419090713460.png" alt="image-20230419090713460" style="zoom:67%;" />

- Loop subdivision results



<img src="4-Geometry.assets/image-20230419090743533.png" alt="image-20230419090743533" style="zoom:67%;" />



#### Catmull-Clark Subdivision (General Mesh)

> Catmull 图灵奖得主

- Loop细化局限性：假设网格是三角形网格
- Catmull-Clark: 适用于任意网格
  - Non-quad face: 不是四边形的面
  - 奇异点：度不是4的点

<img src="4-Geometry.assets/image-20230419090833496.png" alt="image-20230419090833496" style="zoom:67%;" />

- 每一条边取中点，每一个面也取一个中点，并将边的中点和面的中点进行连接

> - 只要在非四边形面内加一个点，就是奇异点
> - 经过一次细化后所有非四边形面都消失了，每个四边形面引入一个奇异点后，非四边形面消失
> - 做一次细化，增加了非四边形面数的奇异点，并去除了所有的非四边形面

<img src="4-Geometry.assets/image-20230419090845468.png" alt="image-20230419090845468" style="zoom:67%;" />

- 继续在两点直接插入新的点

<img src="4-Geometry.assets/image-20230419093452440.png" alt="image-20230419093452440" style="zoom:50%;" /><img src="4-Geometry.assets/image-20230419093504000.png" alt="image-20230419093504000" style="zoom:50%;" />

- 调整点和边的位置：旧的点(下)，新的点(上)

**FYI: Catmull-Clark Vertex Update Rules (Quad Mesh)**

<img src="4-Geometry.assets/image-20230419093537588.png" alt="image-20230419093537588" style="zoom:67%;" />

- Convergence: Overall Shape and Creases 适用于各种形状的面

<img src="4-Geometry.assets/image-20230419093714548.png" alt="image-20230419093714548" style="zoom:67%;" />

### Simplification

> 曲面简化：减少三角形的数量

<img src="4-Geometry.assets/image-20230419100026628.png" alt="image-20230419100026628" style="zoom:67%;" />

---

**Edge collapsing**: 边坍缩，把连接边的两个点捏在一起

> - Collapsing An Edge 折叠边
> - 如何判断那些边可以“捏”在一起？

<img src="4-Geometry.assets/image-20230419100047614.png" alt="image-20230419100047614" style="zoom:67%;" />



- Quadric Error Metrics 二次误差度量：哪些边可以“捏”在一起？

> - 求点的平均，5个点“捏”成三个点（左）
> - 求二次误差：最小化该点到连接平面内所有点的距离平方和

<img src="4-Geometry.assets/image-20230419100110078.png" alt="image-20230419100110078" style="zoom:67%;" />

- Quadric Error of Edge Collapse 根据二次误差进行坍缩

> - 每一条边都假设进行坍缩，求解其对应的二次度量误差。然后取最小的
> - 但问题是一条边的坍缩会引起其它边的二次度量误差，所以需要取最小，并动态更新受影响的边的二次度量误差
> - 但这种方式只是局部最优，不能保证全局最优 — 贪心算法

<img src="4-Geometry.assets/image-20230419100135162.png" alt="image-20230419100135162" style="zoom:67%;" />

- Simplification via Quadric Error
  - Iteratively collapse edges  Which edges? Assign score with quadric error metric* 
    - approximate distance to surface as sum of distances to  planes containing triangles
    - iteratively collapse edge with smallest score
    -  greedy algorithm... great results!



- Example：

<img src="4-Geometry.assets/image-20230419102049208.png" alt="image-20230419102049208" style="zoom:67%;" />