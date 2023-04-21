# Cameras, Lenses and Light Fields

[toc]

## Imaging = Synthesis + Capture

> 成像=合成+捕捉

**Pinholes & Lenses Form Image on Sensor**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421152049536.png" alt="image-20230421152049536" style="zoom:67%;" />

**Sensor Accumulates Irradiance During Exposure**

> 传感器记录的是Irradiance

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421152138955.png" alt="image-20230421152138955" style="zoom:67%;" />

> 相机没有透镜无法拍照，因为任何一个点都能收集来自各个方向的能量，只能收集Irradiance

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421152359551.png" alt="image-20230421152359551" style="zoom:67%;" />

## Field of View (FOV)

> 视场：看到多大的范围。传感器高度为$h$；传感器到小孔距离为焦距$f$

**Effect of Focal Length on FOV**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421153124753.png" alt="image-20230421153124753" style="zoom:67%;" />

**Focal Length v. Field of View**

> 焦距与视场：由于历史原因，通常用35毫米($h$)胶片（36 x 24毫米）上使用的镜头的焦距来指代角视场

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421153146029.png" alt="image-20230421153146029" style="zoom:67%;" />

**Effect of Sensor Size on FOV**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421153223752.png" alt="image-20230421153223752" style="zoom:67%;" />

## Exposure

> **曝光**：
>
> - $H=T\times E$：时间(快门控制)与irradiance的乘积，累积的能量

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421154424381.png" alt="image-20230421154424381" style="zoom:50%;" />

**Exposure Controls in Photography**

> **亮的影响因素：**
>
> - 光圈的大小：$f-stop$控制
> - 快门
> - ISO增益：感光度，后期处理（将接受的光，后期乘上个数等）

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421154446356.png" alt="image-20230421154446356" style="zoom:50%;" />

**Exposure: Aperture, Shutter, Gain (ISO)**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421154930075.png" alt="image-20230421154930075" style="zoom:67%;" />

**ISO (Gain)**

> 放大信号，同时也放大了噪声。结果更亮，也更noise

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421155415909.png" alt="image-20230421155415909" style="zoom:67%;" />

**F-Number (F-Stop): Exposure Levels**

> 描述光圈的大小$F-stop$：可以简单认为$N$是光圈直径的倒数

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421155700973.png" alt="image-20230421155700973" style="zoom:67%;" />

**Physical Shutter** 

> 快门：给多长时间让光可以进去。快门的打开和关闭是一个过程，这个过程中物体可能会移动，导致运动模糊

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421155903235.png" alt="image-20230421155903235" style="zoom:67%;" />

> 越快快门会导致越严重运动模糊(Motion blur)。

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421160110696.png" alt="image-20230421160110696" style="zoom:67%;" />

> 运动模糊不一定是坏事儿！

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421160546693.png" alt="image-20230421160546693" style="zoom:67%;" />

> Rolling shutter问题

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421160654822.png" alt="image-20230421160654822" style="zoom:50%;" />

**Constant Exposure: F-Stop vs Shutter Speed**

> 互相调节，达到相似的曝光度。运动模糊 VS. 景深

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421160326184.png" alt="image-20230421160326184" style="zoom:67%;" />

## Thin Lens Approximation

**Real Lens Designs Are Highly Complex**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161346237.png" alt="image-20230421161346237" style="zoom:67%;" />



**Real Lens Elements Are Not Ideal – Aberrations**

> 非理想透镜存在像差，光没有聚在一个点上

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161502780.png" alt="image-20230421161502780" style="zoom:50%;" />



**Ideal Thin Lens – Focal Point**

> **理想透镜**
>
> - 所有平行光聚集在一个点上
> - 光线穿过焦点后被变成平行光
> - 假设薄透镜可以任意改变其焦距 （现实透镜组可以改焦距）

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161433332.png" alt="image-20230421161433332" style="zoom:67%;" />

**The Thin Lens Equation**

> 薄透镜方程：物距$z_o$， 像距$z_i$，满足下列关系式

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161548025.png" alt="image-20230421161548025" style="zoom:67%;" />

**Gauss’ Ray Diagrams**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161602940.png" alt="image-20230421161602940" style="zoom:50%;" />

**Gauss’ Ray Tracing Construction**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161619748.png" alt="image-20230421161619748" style="zoom:67%;" />

> 相似三角形

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161637037.png" alt="image-20230421161637037" style="zoom:67%;" />

> 数学推导：

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161647957.png" alt="image-20230421161647957" style="zoom:67%;" />

**Thin Lens Demonstration**

> http://graphics.stanford.edu/courses/cs178-10/applets/gaussian.html

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161710447.png" alt="image-20230421161710447" style="zoom:50%;" />



## Defocus Blur

> 用来解释景深问题

**Computing Circle of Confusion (CoC) Size**

> **CoC：**
>
> - 假设有一平面Focal Plane，经过透镜后成像在Sensor Plane上。如果一个物体Object离Focal Plane比较远，在经过透镜后所成的像Image在Sensor Plane上就是一个圆，因此无法区分具体是来自哪一个物理点。而这个圆就叫做CoC。
> - CoC大小本身大小与Aperture成正比

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161753468.png" alt="image-20230421161753468" style="zoom:67%;" />

**CoC vs. Aperture Size**

> 大光圈越模糊。贵圈真乱！！！

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161811025.png" alt="image-20230421161811025" style="zoom:67%;" />

**Revisiting F-Number (a.k.a. F-Stop)**

> - $F-stop$：焦距除以光圈的直径

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161830354.png" alt="image-20230421161830354" style="zoom:67%;" />

**Example F-Stop Calculations**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161849572.png" alt="image-20230421161849572" style="zoom:67%;" />

**Size of CoC is Inversely Proportional to F-Stop**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161909977.png" alt="image-20230421161909977" style="zoom:67%;" />



## Ray Tracing Ideal Thin Lenses

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421164418907.png" alt="image-20230421164418907" style="zoom:67%;" />

**Ray Tracing for Defocus Blur (Thin Lens)**

> 定义Sensor，透镜属性，根据成像过程进行光线追踪

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161938288.png" alt="image-20230421161938288" style="zoom:67%;" />

> **光线追踪：**

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421161950819.png" alt="image-20230421161950819" style="zoom:67%;" />

## Depth of Field

> 景深：不同光圈影响模糊的范围

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421162005399.png" alt="image-20230421162005399" style="zoom:67%;" />

**Circle of Confusion for Depth of Field**

> 景深经过透镜所得的区域就是CoC, CoC比较小

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421162031542.png" alt="image-20230421162031542" style="zoom:67%;" />

**Depth of Field (FYI)**

> 景深对应的区域就是CoC, CoC比较小

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421162050148.png" alt="image-20230421162050148" style="zoom:67%;" />

**DOF Demonstration (FYI)**

> http://graphics.stanford.edu/courses/cs178/applets/dof.html

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421162106988.png" alt="image-20230421162106988" style="zoom:67%;" />

## Light Field / Lumigraph

> 光场

###  Plenoptic Function

**What do we see?**

> 看到的四面八方的光线

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211301778.png" alt="image-20230421211301778" style="zoom:67%;" />

> 添加幕布，完全与之前看到的场景一样，则人不会察觉。（虚拟现实原理，VR）

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421213236732.png" alt="image-20230421213236732" style="zoom:67%;" />

**The Plenoptic Function**

> 全光函数：描述人看到的所有东西。

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211349403.png" alt="image-20230421211349403" style="zoom:67%;" />

**Grayscale snapshot**

> 人的位置固定，朝任意方向可以看到什么值$P(\theta, \phi)$

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211410497.png" alt="image-20230421211410497" style="zoom:67%;" />



**Color snapshot**

> 引入光线的波长，也就是颜色

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211430748.png" alt="image-20230421211430748" style="zoom:67%;" />

**A movie**

> 继续添加时间$t$，就是电影

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211449325.png" alt="image-20230421211449325" style="zoom:67%;" />



**Holographic movie**

> 假设人可以到处移动，才是全息电影

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211509842.png" alt="image-20230421211509842" style="zoom:67%;" />



**The Plenoptic Function**

> 人在任何位置、任何时间、任何方向看到的东西，即世界是7个维度的函数（全光函数）

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211527850.png" alt="image-20230421211527850" style="zoom:67%;" />



**Sampling Plenoptic Function (top view)**

> 全光函数：任意位置，任意方向，任意时间采样的光的值。光场是全光函数的一部分。

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211551487.png" alt="image-20230421211551487" style="zoom:67%;" />

### Light Field

**Ray**

> 光线：位置+方向

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211610534.png" alt="image-20230421211610534" style="zoom:67%;" />

**Ray Reuse**

> 任意两点定义光线（假设方向已知）

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211630357.png" alt="image-20230421211630357" style="zoom:67%;" />

**Only need plenoptic surface**

> - 看到物体的所有情况，由于光的可逆性，所以相当于物体在包围盒上朝任意方向发射光线。
>
> - 光场：物体表面上任意位置(2D)朝任意方向(2D)去的光的强度。4D函数

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211648883.png" alt="image-20230421211648883" style="zoom:67%;" />

**Synthesizing novel views**

> 新视角合成

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211705037.png" alt="image-20230421211705037" style="zoom:67%;" />

**Lumigraph / Lightfield**

> 不需要知道物体是什么，只关心每条光线上的信息。可以使用包围盒简化记录广场的信息

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211724082.png" alt="image-20230421211724082" style="zoom:67%;" />



**Lumigraph - Organization**

> 只需要知道平面左边信息即可。定义点和方向表示广场

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211743034.png" alt="image-20230421211743034" style="zoom:67%;" />

> 任何一个广场可以使用两个平面来定义。两个点确定一条光线

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211858328.png" alt="image-20230421211858328" style="zoom:67%;" />

> 光场参数化$(u,v),  (s,t)$

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211907986.png" alt="image-20230421211907986" style="zoom:67%;" />

> $s,t$和$u,v$互换角度看

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211919452.png" alt="image-20230421211919452" style="zoom:67%;" />

**Lumigraph / Lightfield**

> - 固定$u,v$看所有的$s,t$ 从不同角度看到场景信息（世界在$st$平面右边）
> - 固定$s,t$看所有的$u,v$ ：反向认为是从$u, v$看向$s,t$上同一个点，则能看到同一点的不同视角上的高光信息

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421211939882.png" alt="image-20230421211939882" style="zoom:67%;" />

**Stanford camera array**



<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421212000641.png" alt="image-20230421212000641" style="zoom:50%;" />



**Integral Imaging (Fly’s Eye Lenslets)**

> 苍蝇的眼睛成像的信息就是一个光场。

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421212050382.png" alt="image-20230421212050382" style="zoom:67%;" />

## Light Field Camera

> 光场相机：支持后期的重新聚焦

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421220549146.png" alt="image-20230421220549146" style="zoom:67%;" />

> - 原理：微透镜阵列。将原本成像平面上的像素换成微透镜，将来自个方向的光分散到不同的方向上去，然后在后面记录这些信息。
>
> - 记录一块像素的信息。平均起来就是像素。
> - 从微透镜向左边看就是光场

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421220652911.png" alt="image-20230421220652911" style="zoom:67%;" />



> 得到普通照片：从记录的每一个小区域上选一条光线作为对应微透镜上的像素，根据光线位置的不同选择实现调焦

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421220711335.png" alt="image-20230421220711335" style="zoom:67%;" />

> - 光场包含了所有光线的信息（位置、方向）
> - 缺点：
>   - 分辨率不足
>   - 高成本

<img src="7-Cameras,Lenses and Light Fields.assets/image-20230421220727520.png" alt="image-20230421220727520" style="zoom:67%;" />
