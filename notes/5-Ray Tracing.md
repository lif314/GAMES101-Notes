# Ray Tracing

- [Shadow Mapping](#Shadow Mapping)





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























## Ray Tracing 3 (Light Transport & Global Illumination)



















## Ray Tracing 4 (Monte Carlo Path Tracing)



















































