# Ray Tracing

- [Shadow Mapping](#Shadow Mapping)

[toc]



## Shadow Mapping

> - shadow 阴影
> - shading 着色，局部性质，解决不了阴影
> - 古墓丽影 2018  第一次实时光线追踪的游戏
> - Shadow Mapping  阴影映射：在光栅化的情况下做出阴影效果。只能处理点光源，都有明显的阴影边界（硬阴影）



- An Image-space Algorithm 图像空间中进行处理
  - no knowledge of scene’s geometry during shadow  computation 不需要场景几何信息作为先验
  - must deal with aliasing artifacts  但会产生锯齿

- Key idea:  
  - the points NOT in shadow must be seen both  by the light and by the camera 没有在阴影中的点在光源和相机处都可以看见

----

**Pass 1: Render from Light**

> 假设有一个虚拟相机在点光源处，获得光源可以看到的场景，并记录每一个点的深度(Z-Buffer)，不需要进行着色

<img src="5-Ray Tracing.assets/image-20230419110048047.png" alt="image-20230419110048047" style="zoom:50%;" />

**Pass 2A: Render from Eye**

> 从摄像机看向场景，当看到某个点，将其投影会光源虚拟相机处的成像平面上，获取其对应的深度

<img src="5-Ray Tracing.assets/image-20230419110204202.png" alt="image-20230419110204202" style="zoom:50%;" />

**Pass 2B: Project to light**

> 从摄像机看向场景也可以计算该点到达点光源的深度，如果该深度等于虚拟相机成像平面上所记录的深度，则说明该点可以被光源所完全看到，没有处于阴影中

<img src="5-Ray Tracing.assets/image-20230419110234725.png" alt="image-20230419110234725" style="zoom:50%;" />

> 如果虚拟相机的成像平面上的深度（到达光源的最小深度）与摄像机所计算的深度不一致，说明该点在阴影中

<img src="5-Ray Tracing.assets/image-20230419110253906.png" alt="image-20230419110253906" style="zoom:50%;" />

----

- 阴影效果对比

<img src="5-Ray Tracing.assets/image-20230419111709869.png" alt="image-20230419111709869" style="zoom:50%;" />

- 从光源看向场景获取场景的深度图

<img src="5-Ray Tracing.assets/image-20230419111725214.png" alt="image-20230419111725214" style="zoom:50%;" />

- 从实际相机看到场景的信息。看到的深度与虚拟相机记录的深度进行对比

> - Shadow Mapping中两者的深度信息并不能严格判断“相等”
>   - 解决1：实际深度大于虚拟相机深度，即是阴影，也存在精度问题
>   - 解决2：引入bias，实际深度大于(虚拟相机深度+bias)，无法本质上解决问题
> - 记录深度的shadow map的分辨率可能较低，但实际场景分辨率较高，则会导致走样

<img src="5-Ray Tracing.assets/image-20230419111748440.png" alt="image-20230419111748440" style="zoom:50%;" />

----

**Problems with shadow maps**

- Hard shadows (point lights only) 只能适用于硬阴影

  > - 点光源只产生硬阴影
  > - 软阴影是因为光源有一定的大小

<img src="5-Ray Tracing.assets/image-20230419112636223.png" alt="image-20230419112636223" style="zoom:50%;" />



- Quality depends on shadow map resolution (general problem with image-based techniques) 质量受到shadow map的分辨率影响
- Involves equality comparison of floating point depth  values means issues of scale, bias, tolerance 判断深度相等的精度问题

> 使用光栅化解决阴影问题较为困难



## Ray Tracing 1 (Whitted-Style Ray Tracing)

### Why Ray Tracing?

- Rasterization couldn’t handle global effects well 全局效果不好
  - (Soft) shadows 软阴影
  - And especially when the light bounces **more than once** 光线多次弹射

<img src="5-Ray Tracing.assets/image-20230419134627473.png" alt="image-20230419134627473" style="zoom:67%;" />

- Rasterization is fast, but quality is relatively low 虽然快，但质量较低

<img src="5-Ray Tracing.assets/image-20230419134658749.png" alt="image-20230419134658749" style="zoom:67%;" />

- Ray tracing is accurate, but is very slow 光线追踪虽然精确，但非常慢
  - **Rasterization: real-time, ray tracing: offline**
  - ~10K CPU core hours to render one frame in production

<img src="5-Ray Tracing.assets/image-20230419134744927.png" alt="image-20230419134744927" style="zoom:67%;" />

### Basic Ray-Tracing Algorithm

#### Light Rays

> 定义光线

Three ideas about light rays：

- Light travels in straight lines (though this is wrong) 直线传播
- Light rays do not “collide” with each other if they cross  (though this is still wrong) 光线之间不会发生碰撞，相交后互不影响
- Light rays travel from the light sources to the eye (but  the physics is invariant under path reversal - reciprocity 可逆性).  光线从光源出发，经过反射、折射等进入人的眼睛

> “And if you gaze long into an abyss, the abyss also gazes  into you.” — Friedrich Wilhelm Nietzsche (translated) 当你凝视深渊时，深渊也在凝视着你。—尼采







####  Ray Casting

> 光线追踪

- 光线投射：从摄像机穿过像素，遇到场景中某个点。同时与光源连接，判断是否处于阴影中

<img src="5-Ray Tracing.assets/image-20230419140246574.png" alt="image-20230419140246574" style="zoom:67%;" />

----

**针孔相机模型**

> 只考虑与场景中最近的交点

<img src="5-Ray Tracing.assets/image-20230419140310011.png" alt="image-20230419140310011" style="zoom:67%;" />

> - 假设光线遇到场景物体后会发生“完美”的折射和反射。
> - 从眼睛穿过像素的光线遇到场景中最近的一个点，然后连接与光源的连线（shadow ray 判断是否被照亮）。
> - 根据法线、入射方向、出射方向，就可以进行**着色**

<img src="5-Ray Tracing.assets/image-20230419140325057.png" alt="image-20230419140325057" style="zoom:67%;" />

> 局限性：只考虑了光线弹射一次的结果

#### Recursive (Whitted-Style) Ray Tracing

> 递归光线追踪

<img src="5-Ray Tracing.assets/image-20230419141114758.png" alt="image-20230419141114758" style="zoom:67%;" />

- 从眼睛穿过像素打在场景上

<img src="5-Ray Tracing.assets/image-20230419141358100.png" alt="image-20230419141358100" style="zoom:67%;" />

- 假设打在玻璃球(反射、折射)，一部分沿着反射方向继续传播。

<img src="5-Ray Tracing.assets/image-20230419141406459.png" alt="image-20230419141406459" style="zoom:67%;" />



- 另外一部分发生折射，弹射多次

<img src="5-Ray Tracing.assets/image-20230419141418054.png" alt="image-20230419141418054" style="zoom:67%;" />

- 每一个发生折射/反射的点都要与光源进行连线，判断是否可以被照亮，并进行着色

<img src="5-Ray Tracing.assets/image-20230419141430616.png" alt="image-20230419141430616" style="zoom:67%;" />

- 所有点的着色结果最后都要加在像素上

> - Primary ray
>
> - Secondary rays
>
> - Shadow rays

<img src="5-Ray Tracing.assets/image-20230419141443868.png" alt="image-20230419141443868" style="zoom:67%;" />

### Ray-Surface Intersection

#### Ray Equation

> 光线方程：方向是单位向量

<img src="5-Ray Tracing.assets/image-20230419142736900.png" alt="image-20230419142736900" style="zoom:67%;" />



#### Ray Intersection With Sphere

> 光线相较于球：方程联立方程组

<img src="5-Ray Tracing.assets/image-20230419142821974.png" alt="image-20230419142821974" style="zoom:67%;" />

- t：传播多久可以打在球上

<img src="5-Ray Tracing.assets/image-20230419143019022.png" alt="image-20230419143019022" style="zoom:67%;" />

#### Ray Intersection With Implicit Surface

> 光线与隐式表面相交：t是实数，正的解

<img src="5-Ray Tracing.assets/image-20230419143043644.png" alt="image-20230419143043644" style="zoom:67%;" />

#### Ray Intersection With Triangle Mesh

> **光线与三角形网格相交**
>
> - 可见性，阴影，光照情况
> - 可以知道点在内部/外部：奇数个交点，在内部；偶数个交点，在外部
>
> **计算交点**
>
> - 逐一三角形查看是否相交，慢

<img src="5-Ray Tracing.assets/image-20230419143106530.png" alt="image-20230419143106530" style="zoom:67%;" />

-----

**Ray Intersection With Triangle**

> - 转化为光线与平面求交

<img src="5-Ray Tracing.assets/image-20230419143128431.png" alt="image-20230419143128431" style="zoom:67%;" />

----

**Plane Equation**

> 平面定义为一个方向(法线)和平面上某个点

<img src="5-Ray Tracing.assets/image-20230419143159875.png" alt="image-20230419143159875" style="zoom:67%;" />

- **Ray Intersection With Plane**

<img src="5-Ray Tracing.assets/image-20230419143221723.png" alt="image-20230419143221723" style="zoom:67%;" />

- **Möller Trumbore Algorithm**

> **MT算法**：求光线与平面的交点
>
> - 交点在三角形内，可以用中心坐标进行表示。然后解方程

<img src="5-Ray Tracing.assets/image-20230419143240277.png" alt="image-20230419143240277" style="zoom:67%;" />

----

**Ray Tracing – Performance Challenges**

- Simple ray-scene intersection
  - Exhaustively test ray-intersection with every triangle
  - Find the closest hit (i.e. minimum t)

- Problem:
  - Naive algorithm = #pixels ⨉ # traingles (⨉ #bounces) 每个像素都要发射光线然后折射、反射等求交
  - Very slow!



For generality, we use the term objects instead of triangles  later (but doesn’t necessarily mean entire objects)

### Accelerating Ray-Surface  Intersection

#### Bounding Volumes

> 包围盒思想：如果光线碰不到包围盒，则也碰不到内部的物体

<img src="5-Ray Tracing.assets/image-20230419150329182.png" alt="image-20230419150329182" style="zoom:67%;" />

#### Ray Intersection with Axis-Aligned Box

- Ray-Intersection With Box

> 长方体包围盒：长方体是三个不同的**对面**形成的交集
>
> - AABB（轴对齐包围盒）：长方体的任意边都沿着$x,y,z$轴（假设没有经过旋转）

<img src="5-Ray Tracing.assets/image-20230419150356001.png" alt="image-20230419150356001" style="zoom:67%;" />

---

**Ray Intersection with Axis-Aligned Box**

> 2D下光线与包围盒求交：
>
> - 与$x,y$平面相交的时间产生四个时间，而完整的进入和出去包围盒的时间就是4个时间的交集

<img src="5-Ray Tracing.assets/image-20230419150429323.png" alt="image-20230419150429323" style="zoom:67%;" />

> 3D情况下：
>
> - 只有当光线都进入了三组对面，才能说明光线进入了包围盒
>
> - 只要光线离开任意一组对面，则说明 光线离开了包围盒
> - 当进入时间小于出去时间，则说明光线在包围盒中一段时间，也就是**存在交点**

<img src="5-Ray Tracing.assets/image-20230419150518453.png" alt="image-20230419150518453" style="zoom:67%;" />

> - 光线不是一条直线，而是一条射线
> - 当光线离开包围盒的时间小于0，则说明包围盒在光源背后，不存在交点
> - 当光线离开的时间是正的，而进入的时间是负的，则说明光源在包围盒内部，存在交点
> - 总结：当光线与包围盒存在交点，当且仅当$t_{enter}<t_{exit}$ && $t_{exit} >= 0$ 

<img src="5-Ray Tracing.assets/image-20230419150530099.png" alt="image-20230419150530099" style="zoom:67%;" />

---

**Why Axis-Aligned?**

> 容易求交点

<img src="5-Ray Tracing.assets/image-20230419150556508.png" alt="image-20230419150556508" style="zoom:67%;" />

## Ray Tracing 2 (Acceleration & Radiometry)

> - GTC news: DLSS 2.0  - https://zhuanlan.zhihu.com/p/116211994
>
> - GTC news: RTXGI - https://developer.nvidia.com/rtxgi

###  Using AABBs to accelerate ray tracing

#### Uniform Spatial Partitions (Grids)

----

**Preprocess – Build Acceleration Grid**

- 寻找场景的包围盒

<img src="5-Ray Tracing.assets/image-20230419191025770.png" alt="image-20230419191025770" style="zoom:67%;" />

- 将包围盒分割成格子

<img src="5-Ray Tracing.assets/image-20230419191046355.png" alt="image-20230419191046355" style="zoom:67%;" />

- 查询与物体相交的格子（假设物体是空心的，不考虑内部）

<img src="5-Ray Tracing.assets/image-20230419191100636.png" alt="image-20230419191100636" style="zoom:67%;" />

----

**Ray-Scene Intersection**

> 光线与格子相遇，并且格子内有物体，则光线可能与格子内的物体相交，判断光线是否与物体相交

<img src="5-Ray Tracing.assets/image-20230419191126603.png" alt="image-20230419191126603" style="zoom:67%;" />

---

**Grid Resolution?**

> 包围盒需要分割为多少个格子？

<img src="5-Ray Tracing.assets/image-20230419191144572.png" alt="image-20230419191144572" style="zoom:67%;" />

- 密集格子：多次计算与格子求交

<img src="5-Ray Tracing.assets/image-20230419191157497.png" alt="image-20230419191157497" style="zoom:67%;" />

- 经验数：格子不能太稀疏，也不能太密集

<img src="5-Ray Tracing.assets/image-20230419191210858.png" alt="image-20230419191210858" style="zoom:67%;" />

- Uniform Grids – When They Fail 场景中空旷的区域，物体的分布不均匀。比如在较大的体育场中，放一个茶壶。

<img src="5-Ray Tracing.assets/image-20230420081709536.png" alt="image-20230420081709536" style="zoom:67%;" />

#### Spatial Partitions

> 空间划分：在稀疏的地方不需要使用密集的格子

- Spatial Partitioning Examples

> - Oct-Tree: 把场景用包围盒，然后切成8份(3D空间)，子节点继续切成8份。停下来标准较多，视情况而定。随维数指数增长
> - KD-Tree：每次只切一刀，只分为两个区域(2D)。如下，先水平划分，分成两个区域，然后又竖直进行划分。3D中循环绕着$x,y,z$轴依次划分。二叉树
>
> - BSP-Tree：不沿着$x,y,x$抽进行划分，而是自定义一个方向进行划分

<img src="5-Ray Tracing.assets/image-20230420081026441.png" alt="image-20230420081026441" style="zoom:67%;" />

----

**KD-Tree Pre-Processing**

- 先将A划分成两个部分

<img src="5-Ray Tracing.assets/image-20230420081050604.png" alt="image-20230420081050604" style="zoom:67%;" />

- 然后将A的两个部分都水平划分（示例划分一般）

<img src="5-Ray Tracing.assets/image-20230420081105978.png" alt="image-20230420081105978" style="zoom:67%;" />

- B中进行划分，并形成一棵二叉树。只在叶子节点存储相关的信息

<img src="5-Ray Tracing.assets/image-20230420081119590.png" alt="image-20230420081119590" style="zoom:67%;" />

**Data Structure for KD-Trees**

- Internal nodes store
  - split axis: x-, y-, or z-axis
  - split position: coordinate of split plane along axis 不一定要从中间划分
  - children: pointers to child nodes
  -  No objects are stored in internal nodes 中间节点不存储物体信息
-   Leaf nodes store
  - list of objects

----

**Traversing a KD-Tree**

- 光线追踪算法

<img src="5-Ray Tracing.assets/image-20230420081249607.png" alt="image-20230420081249607" style="zoom:67%;" />

- 先判断是否与最大的相交

<img src="5-Ray Tracing.assets/image-20230420081300612.png" alt="image-20230420081300612" style="zoom:67%;" />

- 然后判断是否与子节点相交。如果是子节点，则需要进行求交

<img src="5-Ray Tracing.assets/image-20230420081312221.png" alt="image-20230420081312221" style="zoom:67%;" />

- 与A的子节点查看是否相交

<img src="5-Ray Tracing.assets/image-20230420081327288.png" alt="image-20230420081327288" style="zoom:67%;" />

- 继续与子节点判断是否求交

<img src="5-Ray Tracing.assets/image-20230420081337853.png" alt="image-20230420081337853" style="zoom:67%;" />

- 继续判定

<img src="5-Ray Tracing.assets/image-20230420081351582.png" alt="image-20230420081351582" style="zoom:67%;" />

- 继续判定

<img src="5-Ray Tracing.assets/image-20230420081402193.png" alt="image-20230420081402193" style="zoom:67%;" />

- 相交于叶子节点，判定是否于格子中的物体相交

<img src="5-Ray Tracing.assets/image-20230420081414414.png" alt="image-20230420081414414" style="zoom:67%;" />

> **KD-Tree**
>
> - 如果光线与格子无交点，跳过；如果有交点，则需要与两个子节点判断是否相交，递归。
> - 直到光线交与叶子节点，判断光线是否与其中的物体是否相交
>
> **存在的问题**
>
> - 如何判断物体是否与包围盒相交？**Very Hard**  — **KD-Tree目前用的不多了**
> - 假设对象是三角形网格，很难判断三角形是否与空间划分线是否相交
> - 物体可能与多个划分格子相交，如果都是叶子节点，则需要在每个叶子节点都存储物体信息，冗余。希望一个物体只在一个格子里。





#### Object Partitions &  Bounding Volume Hierarchy (BVH)

> 对象划分：边界体积层次 **BVH应用广泛** 

**Bounding Volume Hierarchy (BVH)**

- 用格子将场景包围

<img src="5-Ray Tracing.assets/image-20230420084343412.png" alt="image-20230420084343412" style="zoom:67%;" />

- 将物体划分为两个部分，然后用分别求包围盒

<img src="5-Ray Tracing.assets/image-20230420084451979.png" alt="image-20230420084451979" style="zoom:67%;" />

- 依次划分，也会形成两个子节点

<img src="5-Ray Tracing.assets/image-20230420084501888.png" alt="image-20230420084501888" style="zoom:67%;" />

- 继续划分

<img src="5-Ray Tracing.assets/image-20230420084514487.png" alt="image-20230420084514487" style="zoom:67%;" />

- 当达到一定条件时，停止划分。可以按照KD-Tree一样按照轴循环划分。在叶子节点中存储物体的信息

<img src="5-Ray Tracing.assets/image-20230420084524943.png" alt="image-20230420084524943" style="zoom:67%;" />

> - 好性质：一个物体只可能在一个包围盒中
> - 空间划分不严格，不同的盒子可能相交

----

**Building BVHs**

- How to subdivide a node? 如何划分节点
  - Choose a dimension to split 像KD-Tree一样循环$x,y,z$轴划分等
  - Heuristic #1: Always choose the longest axis in node 沿着最长的轴划分
  - Heuristic #2: Split node at location of **median** object 在中间的物体处进行划分，保证每个盒子中物体数量差不多，树比较平衡。**无序的数中找第$i$大的数，使用快速选择算法$O(n)$**

- Termination criteria? 何时停止划分？
  - Heuristic: stop when node contains few elements  (e.g. 5) 当盒子里包含较少的物体时

**Data Structure for BVHs**

- Internal nodes store 中间节点只存储包围盒和子节点
  - Bounding box
  - Children: pointers to child nodes 
- Leaf nodes store 叶子节点存储物体信息和包围盒
  - Bounding box 
  - List of objects
- Nodes represent subset of primitives in scene
  - All objects in subtree

---

**BVH Traversal**

<img src="5-Ray Tracing.assets/image-20230420085812575.png" alt="image-20230420085812575" style="zoom:67%;" />

**Spatial vs Object Partitions**

> - 空间划分
> - 对象划分

<img src="5-Ray Tracing.assets/image-20230420085831564.png" alt="image-20230420085831564" style="zoom:67%;" />

### Basic radiometry (辐射度量学)

**Radiometry — Motivation**

> - Blinn-Phong模型中光的强度$I$是一个数，它应该有具体的物理意义
> - Whitted style光线追踪每次折射、反射需要具体能量损失的度量

<img src="5-Ray Tracing.assets/image-20230420091840132.png" alt="image-20230420091840132" style="zoom:67%;" />

**Radiometry**：如何描述光照，定义光在空间中的属性，遵循几何光学原理

> - Radiant flux
>
> - intensity
>
> - irradiance
>
> - radiance

<img src="5-Ray Tracing.assets/image-20230420092302039.png" alt="image-20230420092302039" style="zoom:50%;" />

> - WHY
> - WHAT
>
> - HOW: 最不重要的就是HOW，怎么运作是次要的问题

#### Radiant Energy and Flux (Power)

> - Radiant energy: 电磁辐射的能量，单位焦耳$J$
> - Radiant flux(power): 单位时间内的光源辐射的能量（power类似功率）,单位瓦特$W$。另一个单位$lm$描述了亮度

<img src="5-Ray Tracing.assets/image-20230420092532822.png" alt="image-20230420092532822" style="zoom:67%;" />

**Flux – #photons flowing through a sensor in unit time**

> Flux: 单位时间内穿过平面的光子数。

<img src="5-Ray Tracing.assets/image-20230420092557698.png" alt="image-20230420092557698" style="zoom:67%;" />

**Important Light Measurements of Interest**

> - Radiant Intensity：光源向四面八方辐射的能量
> - Irradiance：物体表面接受到光的能量
> - Radiance：光线传播中的能量

<img src="5-Ray Tracing.assets/image-20230420092616868.png" alt="image-20230420092616868" style="zoom:67%;" />

#### Radiant Intensity

**Radiant Intensity**：单位立体角内辐射出的power(功率)/flux。单位candela

<img src="5-Ray Tracing.assets/image-20230420092639063.png" alt="image-20230420092639063" style="zoom:67%;" />

---

**Angles and Solid Angles**

> - 弧度角：$\theta=\frac{l}{r}$
> - 立体角：三维空间中一个球，从球心出发一个椎对应的球面面积除以半径的平方 $\Omega=\frac{A}{r^2}$

<img src="5-Ray Tracing.assets/image-20230420092703676.png" alt="image-20230420092703676" style="zoom:67%;" />

**Differential Solid Angles**：微分立体角–> 单位立体角(单位面积除以$r^2$)

> $\theta\phi$坐标系对球面不是均匀划分

<img src="5-Ray Tracing.assets/image-20230420092721549.png" alt="image-20230420092721549" style="zoom:67%;" />

- 球对应的立体角：球面积分

<img src="5-Ray Tracing.assets/image-20230420092731449.png" alt="image-20230420092731449" style="zoom:67%;" />

- $\omega$ as a direction vector：三维空间中的方向，可由$\theta,\phi$定义

<img src="5-Ray Tracing.assets/image-20230420092829131.png" alt="image-20230420092829131" style="zoom:67%;" />

**Isotropic Point Source**

> **点光源：** 各向同性，均匀辐射
>
> - flux(power)：沿着球面进行积分，就是整个点光源的power
> - intensity: $I=\frac{\Phi}{4\pi}$ 每个方向上的power/flux

<img src="5-Ray Tracing.assets/image-20230420092838123.png" alt="image-20230420092838123" style="zoom:67%;" />

**Modern LED Light**

<img src="5-Ray Tracing.assets/image-20230420092852179.png" alt="image-20230420092852179" style="zoom:67%;" />

- Review:

<img src="5-Ray Tracing.assets/image-20230420103316812.png" alt="image-20230420103316812" style="zoom:67%;" />

#### Irradiance

- 定义：表面一个点上单位面积内吸收的power（面必须与光线**垂直**，或者投影在垂直方向上）

<img src="5-Ray Tracing.assets/image-20230420103354701.png" alt="image-20230420103354701" style="zoom:67%;" />

**Lambert’s Cosine Law**

<img src="5-Ray Tracing.assets/image-20230420103415703.png" alt="image-20230420103415703" style="zoom:67%;" />

**Why Do We Have Seasons?**

<img src="5-Ray Tracing.assets/image-20230420103443523.png" alt="image-20230420103443523" style="zoom:67%;" />

**Correction: Irradiance Falloff**

> 点光源传输过程中能量的变化。Intensity没有变化，而是irradiance在衰减



<img src="5-Ray Tracing.assets/image-20230420103803932.png" alt="image-20230420103803932" style="zoom:67%;" />







#### Radiance

> 辐射度：描述光线在传播过程中的属性。

<img src="5-Ray Tracing.assets/image-20230420103818403.png" alt="image-20230420103818403" style="zoom:67%;" />

- Radiance：单位立体角和单位投影面积内的power

> 在微小面积（单位面积）上从某个方向（单位立体角）辐射能量

<img src="5-Ray Tracing.assets/image-20230420103830002.png" alt="image-20230420103830002" style="zoom:67%;" />

>**等价**：
>
>- Radiance是单位立体角内的Irradiance。两者区别是是否有方向性。Irradiance是$dA$上来自所有方向所吸收的能量，而Radiance是从某个方向上（立体角）上吸收的能量。
>- Radiance是单位投影面积内的Intensity。Intensity是某个立体角上所辐射出的能量，而Radiance是所有立体角方向（某个小面积）所辐射出的能量。

<img src="5-Ray Tracing.assets/image-20230420103843200.png" alt="image-20230420103843200" style="zoom:67%;" />



**Incident Radiance**

> Irradiance是$dA$上来自所有方向所吸收的能量，而Radiance是从某个方向上（立体角）上吸收的能量。

<img src="5-Ray Tracing.assets/image-20230420103902167.png" alt="image-20230420103902167" style="zoom:67%;" />

**Exiting Radiance**

> Intensity是某个立体角上所辐射出的能量，而Radiance是所有立体角方向（某个小面积）所辐射出的能量。

<img src="5-Ray Tracing.assets/image-20230420103920180.png" alt="image-20230420103920180" style="zoom:67%;" />

**Irradiance vs. Radiance**

> - Irradiance：$dA$上吸收到的所有的power
>
> - Radiance：$dA$上某个方向上吸收的power
> - **Radiance沿着方向进行积分就是Irradiance**

<img src="5-Ray Tracing.assets/image-20230420103938821.png" alt="image-20230420103938821" style="zoom:67%;" />

## Ray Tracing 3 (Light Transport & Global Illumination)

### Bidirectional Reflectance  Distribution Function  (BRDF)

> 双向反射分布函数 BRDF

**Reflection at a Point**

> 从Radiance的角度看反射：从某个方向进来，然后从其它方向反射射出，如何计算能量的吸收与辐射？
>
> - 假设从方向$\omega_i$进入的光线的power为$E$，在面积$dA$处发生反射，从方向$\omega_r$反射出去
> - $dA$接受的能量：irradiance
> - $dA$反射出去的能量：radiance, 但不确定具体某个方向的power,可以定义一个函数来描述

<img src="5-Ray Tracing.assets/image-20230420111954735.png" alt="image-20230420111954735" style="zoom:67%;" />

**BRDF**

> - 定义函数$f_r(\omega_i \rightarrow w_r)$描述从$w_i$入射的光线的power（irradiance）从方向$w_r$辐射的power（radiance）之间的比例关系。
> - BRDF定义了辐射方向的能量分布。如果是镜面反射，则只有反射出去的方向有能量；如果是漫反射，则均匀的分配到不同的方向上
> - 描述了光线与物体是如何作用的，决定了物体不同的材质。

<img src="5-Ray Tracing.assets/image-20230420112012576.png" alt="image-20230420112012576" style="zoom:67%;" />

-----

**The Reflection Equation**

> - BRDF定义了从某个方向$w_i$入射光线的power经过反射到某方向$w_r$的能量分配
> - 反射方程：沿着入射方向进行积分，就能计算$w_r$出射方向上所有的power。（定义了任何一个着色点，在各种不同的光照环境下，任何一个入射光线方向对出射方向的贡献）

<img src="5-Ray Tracing.assets/image-20230420112032721.png" alt="image-20230420112032721" style="zoom:67%;" />

> 任何一种着色模型，只要定义BRDF就可以得到观察方向的power

**Challenge: Recursive Equation**

> - 难点：到达着色点的入射光线可能来自光源，也可能来自多次弹射的结果 **递归**

<img src="5-Ray Tracing.assets/image-20230420112051834.png" alt="image-20230420112051834" style="zoom:67%;" />

---

**The Rendering Equation**

> 渲染方程：如果物体自身会发光，则需要加上自身的power $L_e$，其中$\cos{\theta} = n \cdot w_i$。假设所有方向都是向外的，如$w_i$，即所有光线从半球（因为下半球贡献的power为0，照不到呀！！！）圆心辐射

<img src="5-Ray Tracing.assets/image-20230420112110944.png" alt="image-20230420112110944" style="zoom:67%;" />

> 膜拜大佬祖师爷：https://en.wikipedia.org/wiki/Jim_Kajiya

### Understanding the rendering equation

**Reflection Equation**

> 假设只有一个点光源，一个入射方向，对辐射方向的贡献

<img src="5-Ray Tracing.assets/image-20230420130612205.png" alt="image-20230420130612205" style="zoom:67%;" />

> 如果是多个点光源，则将每个点光源的入射的power求和即可

<img src="5-Ray Tracing.assets/image-20230420130624627.png" alt="image-20230420130624627" style="zoom:67%;" />

> 如果是面光源，则把面上一个点看作点光源，然后进行积分即可

<img src="5-Ray Tracing.assets/image-20230420130644548.png" alt="image-20230420130644548" style="zoom:67%;" />

**Rendering Equation**

> 如果除了光源，还有其它物体的反射光。将物体的反射面当作光源即可。

<img src="5-Ray Tracing.assets/image-20230420130715689.png" alt="image-20230420130715689" style="zoom:67%;" />

**Rendering Equation (Kajiya 86)**

<img src="5-Ray Tracing.assets/image-20230420130802429.png" alt="image-20230420130802429" style="zoom:67%;" />

------

**Rendering Equation as  Integral Equation**：递归，光线多次弹射

> - 先定义物体的材质：$L_e, f$
> - 使用Fredholm积分方程进行改写：

<img src="5-Ray Tracing.assets/image-20230420130829263.png" alt="image-20230420130829263" style="zoom:67%;" />



**Linear Operator Equation**

> - 进一步简化为算子形式（数学证明）
> - $L=E+KL$: 所有光源辐射出来的能量+辐射出来的能量被反射的能量

<img src="5-Ray Tracing.assets/image-20230420130848773.png" alt="image-20230420130848773" style="zoom:67%;" />

-----

**Ray Tracing and extensions**

> - 解方程，求解$L$，$(I-K)^{-1}$泰勒展开。。。

<img src="5-Ray Tracing.assets/image-20230420130910167.png" alt="image-20230420130910167" style="zoom:67%;" />

**Ray Tracing**

> - L：0次弹射，1次反射，…，多次弹射
> - 直接光照：光线弹射1次
> - 间接光照：光线弹射2次以上
> - 全局光照：直接光照+间接光照

<img src="5-Ray Tracing.assets/image-20230420130942123.png" alt="image-20230420130942123" style="zoom:67%;" />

> - 光栅化着色：只包含0次弹射和1次弹射

<img src="5-Ray Tracing.assets/image-20230420130954268.png" alt="image-20230420130954268" style="zoom:67%;" />

-----

**Demo**

- 直接光照：光源直接照射到的地方

<img src="5-Ray Tracing.assets/image-20230420131011526.png" alt="image-20230420131011526" style="zoom:50%;" />

- 一次间接光照：直接光照+间接光照

<img src="5-Ray Tracing.assets/image-20230420131025297.png" alt="image-20230420131025297" style="zoom:50%;" />

- 2次全局光照

<img src="5-Ray Tracing.assets/image-20230420131040455.png" alt="image-20230420131040455" style="zoom:50%;" />

- 4次全局关照：上方玻璃球经过3次以上才会亮<img src="5-Ray Tracing.assets/image-20230420131107126.png" alt="image-20230420131107126" style="zoom:50%;" />

- 8次全局光照

<img src="5-Ray Tracing.assets/image-20230420131125934.png" alt="image-20230420131125934" style="zoom:50%;" />

- 16次全局光照

<img src="5-Ray Tracing.assets/image-20230420131139815.png" alt="image-20230420131139815" style="zoom:50%;" />

> - 全局光照最后会收敛到某一个亮度。能量守恒
> - 如果是相机一直按，会出现过曝的问题，能量一直增加。

### Probability Review

**Random Variables** 随机变量

<img src="5-Ray Tracing.assets/image-20230420131212904.png" alt="image-20230420131212904" style="zoom:67%;" />

**Probabilities** 概率

<img src="5-Ray Tracing.assets/image-20230420131232427.png" alt="image-20230420131232427" style="zoom:67%;" />

**Expected Value of a Random Variable** 随机变量的期望

<img src="5-Ray Tracing.assets/image-20230420131248934.png" alt="image-20230420131248934" style="zoom:67%;" />

**Continuous Case: Probability Distribution Function (PDF)** 概率分布函数

> PDF：取某一值的概率是多少呢？构成一个连续的函数

<img src="5-Ray Tracing.assets/image-20230420131316398.png" alt="image-20230420131316398" style="zoom:67%;" />

**Function of a Random Variable**

<img src="5-Ray Tracing.assets/image-20230420131334027.png" alt="image-20230420131334027" style="zoom:67%;" />



## Ray Tracing 4 (Monte Carlo Path Tracing)

> 蒙特卡洛路径追踪

### Monte Carlo Integration

> 蒙特卡洛积分：数值计算

- 以概率的形式求解较难的积分

<img src="5-Ray Tracing.assets/image-20230420145906838.png" alt="image-20230420145906838" style="zoom:67%;" />

- 黎曼积分：$a,b$之间均匀划分，每一份当作微小的长方形
- 蒙特卡洛积分：随机取一个数，知道对应的$x$和$f(x)$，然后假设整个曲线就是长方形，高度$f(x)$，宽度$ab$。重复多次，最后将每次面积进行平均化

<img src="5-Ray Tracing.assets/image-20230420145920351.png" alt="image-20230420145920351" style="zoom:67%;" />

> - 求定积分的结果
> - 随机采样位置$X_i$
> - 积分结果可以近似为每次$X_i$结果的平均

<img src="5-Ray Tracing.assets/image-20230420145932521.png" alt="image-20230420145932521" style="zoom:67%;" />

----

**Example: Uniform Monte Carlo Estimator**

<img src="5-Ray Tracing.assets/image-20230420150024199.png" alt="image-20230420150024199" style="zoom:67%;" />

- 化简公式

<img src="5-Ray Tracing.assets/image-20230420150033641.png" alt="image-20230420150033641" style="zoom:67%;" />

> **蒙特卡洛积分：**
>
> - 适用于任何函数的积分：根据PDF进行采样即可，不需要关心积分域
> - 采样点越多，准确度越高



<img src="5-Ray Tracing.assets/image-20230420150046924.png" alt="image-20230420150046924" style="zoom:67%;" />

### Path Tracing

> - 路径追踪：Path tracing
> - 光线追踪：Ray tracing,(Whitted-Style) 
>   - 当光线打在光滑物体，发生反射/折射
>   - 当光线打在漫反射物体表面，光线停止

#### Motivation: Whitted-Style Ray Tracing

<img src="5-Ray Tracing.assets/image-20230420152926070.png" alt="image-20230420152926070" style="zoom:67%;" />

> 问题1：左（specular，非常光滑，反射方向接近半程向量），右（Glossy，一般光滑，镜面反射的方向与半程向量差距较大）。

<img src="5-Ray Tracing.assets/image-20230420152939661.png" alt="image-20230420152939661" style="zoom:67%;" />

> 问题2：漫反射材质。Whitted-Style光线打在漫反射物体上没有考虑光线继续传播，并在漫反射物体之间的弹射情况

<img src="5-Ray Tracing.assets/image-20230420152958028.png" alt="image-20230420152958028" style="zoom:67%;" />

**Whitted-Style Ray Tracing is Wrong**

> 求解渲染方程：
>
> - 积分
> - 递归的光线

<img src="5-Ray Tracing.assets/image-20230420153017009.png" alt="image-20230420153017009" style="zoom:67%;" />

#### A Simple Monte Carlo Solution

> 简单场景：只考虑一个点的**直接光照**，假设自身并不发光。所有方向都认为向外。求解该点的直接光照

<img src="5-Ray Tracing.assets/image-20230420153133959.png" alt="image-20230420153133959" style="zoom:67%;" />

> 一个点的渲染方程
>
> - 直接光照：所有的入射光都直接来自光源。$L_i$来自光源就是某个值，不是来自光源就是0

<img src="5-Ray Tracing.assets/image-20230420153149324.png" alt="image-20230420153149324" style="zoom:67%;" />

> **蒙特卡洛求解：**
>
> - 随机选择一个方向，计算$f(x)$
> - PDF：如何对半球进行采样？假设是**均匀采样**，面积分之一

<img src="5-Ray Tracing.assets/image-20230420153159726.png" alt="image-20230420153159726" style="zoom:67%;" />

> **蒙特卡洛积分形式：** 

<img src="5-Ray Tracing.assets/image-20230420153217388.png" alt="image-20230420153217388" style="zoom:67%;" />

> **伪代码：**
>
> - 随机选择一个方向
> - 如果这个方向的光线来自光源，则为$L_o$，否则为0

<img src="5-Ray Tracing.assets/image-20230420153228198.png" alt="image-20230420153228198" style="zoom:67%;" />

#### Introducing Global Illumination

> 直接光照：如果光线不是直接来自光源，而是从某物体反射而来（可以假设Q是一个光源），则**Q辐射到P的光照就是Q点本身的直接光照**

<img src="5-Ray Tracing.assets/image-20230420153248421.png" alt="image-20230420153248421" style="zoom:67%;" />

> 递归：加上Q点所产生的直接光照 –> 全局光照

<img src="5-Ray Tracing.assets/image-20230420153301317.png" alt="image-20230420153301317" style="zoom:67%;" />

> 递归会导致光线数量爆炸。

#### Path Tracing

> 问题1：光线数量会爆炸。

<img src="5-Ray Tracing.assets/image-20230420153403949.png" alt="image-20230420153403949" style="zoom:67%;" />

> - 解决光线爆炸：随机朝**一个**方向进行采样。$N=1$叫做路径追踪，$N\neq1$叫做分布式路径追踪

<img src="5-Ray Tracing.assets/image-20230420153412844.png" alt="image-20230420153412844" style="zoom:67%;" />

**Ray Generation**

> - $N=1$会导致噪声较大（打在物体上随机选择一个方向进行反射）。
> - 对于一个像素，可以发射多条路径来进行采样。使用足够多的paths来获取着色点的值，最后求取平均值。

<img src="5-Ray Tracing.assets/image-20230420153422865.png" alt="image-20230420153422865" style="zoom:67%;" />



> 从摄像机位置，从某像素打出光线。如果光线打在场景中某个物体，求取该点的着色值。

<img src="5-Ray Tracing.assets/image-20230420153446926.png" alt="image-20230420153446926" style="zoom:67%;" />

**Path Tracing**

> 问题2：递归不会停止。如何判断何时停止？

<img src="5-Ray Tracing.assets/image-20230420153519578.png" alt="image-20230420153519578" style="zoom:67%;" />

> 现实中光线也不会停止，应该限制光线弹射的次数。3次效果。

<img src="5-Ray Tracing.assets/image-20230420153531563.png" alt="image-20230420153531563" style="zoom:67%;" />

> 限制光线弹射17次效果。

<img src="5-Ray Tracing.assets/image-20230420153544875.png" alt="image-20230420153544875" style="zoom:67%;" />

> 如何判断合理的停止次数？

#### Solution: Russian Roulette (RR)

> **俄罗斯轮盘赌算法：**
>
> - 装入子弹，有一定的概率会生存

<img src="5-Ray Tracing.assets/image-20230420153609562.png" alt="image-20230420153609562" style="zoom:67%;" />

> **决定一定的概率会停止光线是否继续弹射**
>
> - 定义一个超参数概率$p$，以概率$p$打出光线得到的结果为$L_o/p$；以概率$1-p$不打出光线，返回值为$0$。最后的期望依然是$L_o$，秒！！！

<img src="5-Ray Tracing.assets/image-20230420153626504.png" alt="image-20230420153626504" style="zoom:67%;" />

> - 手动设置概率$P_{RR}$
> - 随机在$[0,1]$内选择一个数$ksi$
> - 如果$ksi>P_{RR}$,则光线停止；否则正常辐射

<img src="5-Ray Tracing.assets/image-20230420153642734.png" alt="image-20230420153642734" style="zoom:67%;" />

> **正确的path Tracing方法**
>
> - 低效！！！
> - SPP (samples per pixel)：每个像素打出多少光线

<img src="5-Ray Tracing.assets/image-20230420153652520.png" alt="image-20230420153652520" style="zoom:67%;" />

**Sampling the Light**

> **低效的原因：**
>
> - 是否能打到光源看概率（均匀的向四面八方采样的结果）
> - 使用更好的PDF进行采样（在光源上采样），而不是均匀采样

<img src="5-Ray Tracing.assets/image-20230420153718060.png" alt="image-20230420153718060" style="zoom:67%;" />

- **Sampling the Light (pure math)**

> **在光源上进行采样：**
>
> - 将设光源的面积为$A$，$PDF=1/A$
> - 渲染方程在物体的积分域上，需要在物体上进行采样。转换积分域，写成在光源上的积分

<img src="5-Ray Tracing.assets/image-20230420153735442.png" alt="image-20230420153735442" style="zoom:67%;" />

> 将$d\omega$转换为$dA$：
>
> - $dA$光源上一个很小的面积
> - $d \omega$是$dA$投影在球表面上的单位立体角

<img src="5-Ray Tracing.assets/image-20230420153746323.png" alt="image-20230420153746323" style="zoom:67%;" />

> **改写渲染方程：**对光源采样，对光源进行积分

<img src="5-Ray Tracing.assets/image-20230420153809085.png" alt="image-20230420153809085" style="zoom:67%;" />

> **光线采样：**
>
> - 来自光源的贡献：直接在光源上采样，不需要RR
> - 来自于其它非光源的贡献：进行RR

<img src="5-Ray Tracing.assets/image-20230420153820529.png" alt="image-20230420153820529" style="zoom:67%;" />

> **伪代码**
>
> - 光源贡献：对光源采样，在光源上积分
> - 非光源贡献：在物体上采样，在物体上积分

<img src="5-Ray Tracing.assets/image-20230420153837746.png" alt="image-20230420153837746" style="zoom:67%;" />

> **上面假设了光源和着色点之间不存在遮挡物**
>
> - 从光源打一条光线，判断是否被遮挡

<img src="5-Ray Tracing.assets/image-20230420153850017.png" alt="image-20230420153850017" style="zoom:67%;" />

> 路径追踪：点光源非常困难！！！可以假设为较小的面积

**Is Path Tracing Correct?**

> The Cornell box — http://www.graphics.cornell.edu/online/box/compare.html

<img src="5-Ray Tracing.assets/image-20230420153924143.png" alt="image-20230420153924143" style="zoom:67%;" />



**Ray tracing: Previous vs. Modern Concepts**

> Ray Tracing现代含义: 所有光线传播的集合

<img src="5-Ray Tracing.assets/image-20230420153948516.png" alt="image-20230420153948516" style="zoom:67%;" />

**Things we haven’t covered / won’t cover**

> - 怎么进行最好的采样？重要性采样理论
> - 如何生成随机数？
> - 什么是最好的PDF来指导采样？

<img src="5-Ray Tracing.assets/image-20230420154013590.png" alt="image-20230420154013590" style="zoom:67%;" />

> - 采样物体和光源：MIS采样方法
> - 每个像素打出多条光线，为什么平均起来就是这个像素？Pixel Reconstruction Filter
> - 如何把radiance转换为颜色值？$\gamma$校正（[gamma correction](https://en.wikipedia.org/wiki/Gamma_correction)）

<img src="5-Ray Tracing.assets/image-20230420154027707.png" alt="image-20230420154027707" style="zoom:67%;" />

