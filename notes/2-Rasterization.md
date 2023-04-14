# Rasterization

- [Rasterization 1: Triangles](#Rasterization-1:-Triangles)
  - [Perspective Projection](#Perspective-Projection)
  - [Canonical Cube to Screen](#Canonical-Cube-to-Screen)
  - [Rasterization:  Drawing to Raster Displays](#Rasterization: -Drawing-to-Raster-Displays)









## Rasterization 1: Triangles

> 当做完View Transformation后，所有场景都在标准立方体里。下一步就是光栅化：将其画在屏幕上





### Perspective Projection

- *定义视锥*：宽高比、视角$fovY$

<img src="2-Rasterization.assets/image-20230414141450551.png" alt="image-20230414141450551" style="zoom:67%;" />

<img src="2-Rasterization.assets/image-20230414141752594.png" alt="image-20230414141752594" style="zoom:67%;" />

- MVP: 将场景转换到标准立方体内

<img src="2-Rasterization.assets/image-20230414142302176.png" alt="image-20230414142302176" style="zoom:67%;" />



### Canonical Cube to Screen

----

**Screen**

- What is a screen?
  - An array of pixels
  - Size of the array: resolution 分辨率 $1920 \times 1080$
  - A typical kind of raster display
- Raster（光栅） == screen in German
  - Rasterize == drawing onto the screen
- Pixel (FYI, short for “picture element”) 
  - For now: A pixel is a little square with uniform color 最小的单位
  -  Color is a mixture of (red, green, blue)

- **Screen space**

<img src="2-Rasterization.assets/image-20230414143256399.png" alt="image-20230414143256399" style="zoom:67%;" />



----

**Viewport transformation: 视口变换**

>  与z轴无关，直接拉伸，并移动中心到$(w/2, h/2)$

<img src="2-Rasterization.assets/image-20230414144347436.png" alt="image-20230414144347436" style="zoom:67%;" />

<img src="2-Rasterization.assets/image-20230414144410142.png" alt="image-20230414144410142" style="zoom:67%;" />

> 经过视口变换后，下一步就是将各种多边形网格转换为单一的像素

### Rasterization:  Drawing to Raster Displays

- Polygon Meshes



<center><img src="2-Rasterization.assets/image-20230414150342781.png" alt="image-20230414150342781" style="zoom:50%;" /></center>

- Triangle Meshes



<center class="half">
<img src="2-Rasterization.assets/image-20230414150357778.png" alt="image-20230414150357778" width="300" />
<img src="2-Rasterization.assets/image-20230414150448930.png" alt="image-20230414150448930" width="300"/>
</center>

----

**Triangles - Fundamental Shape Primitives**

- Why triangles?
  - Most basic polygon 最基础的多边形
    - Break up other polygons
  - Unique properties
    - Guaranteed to be planar 平面
    - Well-defined interior 内外明确
    - Well-defined method for interpolating values at  vertices over triangle (barycentric interpolation)  内部插值方便
- What Pixel Values Approximate a Triangle?

<center>
    <img src="2-Rasterization.assets/image-20230414151617151.png" alt="image-20230414151617151" style="zoom:67%;" />
</center>

----

**A Simple Approach: Sampling** 采样策略：把函数离散化的过程

- Sample If Each Pixel Center Is Inside Triangle 注意考虑的是像素的中心点

<center class="half">
    <img src="2-Rasterization.assets/image-20230414152005543.png" alt="image-20230414152005543"  width="300" />
    <img src="2-Rasterization.assets/image-20230414152019127.png" alt="image-20230414152019127" width="300"/></center>

- Inside? Recall: Three Cross Products!  （右手螺旋定则）
  - $P_0P_1\times P_0Q$ : 外  
  - $P_1P_2\times P_1Q$：外
  - $P_2P_0\times P_2Q$：内

<center><img src="2-Rasterization.assets/image-20230414152347507.png" alt="image-20230414152347507" style="zoom:50%;" /></center>



- Edge Cases (Literally)：up to you

- 检查所有像素 –> Incremental Triangle Traversal
  - 左边：包围盒(蓝色区域)  根据三点的最小和最大的$x$和$y$获取包围盒，也叫**AABB**（axis-aligned Bounding Box）
  - 右边：每一行设计一个AABB

<center>
    <img src="2-Rasterization.assets/image-20230414153109408.png" alt="image-20230414153109408" width="300"/>
    <img src="2-Rasterization.assets/image-20230414153126571.png" alt="image-20230414153126571" width="300"/>
</center>

   

----

**Aliasing  锯齿**：抗锯齿（反走样）

<center class="half">
    <img src="2-Rasterization.assets/image-20230414154510425.png" alt="image-20230414154510425" width="300"/>
    <img src="2-Rasterization.assets/image-20230414154525554.png" alt="image-20230414154525554" width="300"/>
</center>

<center>
    <img src="2-Rasterization.assets/image-20230414154850259.png" alt="image-20230414154850259" style="zoom:80%;" />
</center>













































