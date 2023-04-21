# Materials and Appearances

- [Material == BRDF](#Material-==-BRDF)

[toc]



## Material == BRDF

> 材质：光线如何与物体进行作用

#### Diffuse / Lambertian Material (BRDF)

> 漫反射现象：当光线打在材料上时，会被均匀的向各个方向反射

<img src="6-Materials and Appearances.assets/image-20230421095055475.png" alt="image-20230421095055475" style="zoom:67%;" />

> 漫反射材质：任何一个点也有不同的漫反射系数，从而显示不同的颜色

<img src="6-Materials and Appearances.assets/image-20230421095704676.png" alt="image-20230421095704676" style="zoom:67%;" />

> **漫反射材质的BRDF：**
>
> - Blinn-Phong中的漫反射系数（经验）
> - BRDF：（能量守恒）假设入射的光是均匀（常数）的，则出射的光也必须是均匀（常数）的。假设该点不吸收光也不辐射光，则入射和出射的irradiance相等，且两者都是均匀的，则radiance也相等。根据渲染方程可以计算出出射光的power $L_o$，根据能量守恒，可知$L_i=L_o$，所以$f_r=1/\pi$。可以定义反射率albedo，在$[0,1]$之间，表示不同颜色的值

<img src="6-Materials and Appearances.assets/image-20230421095723721.png" alt="image-20230421095723721" style="zoom:67%;" />



#### Glossy material (BRDF)

> 比镜子粗糙，像镜面反射

<img src="6-Materials and Appearances.assets/image-20230421095928418.png" alt="image-20230421095928418" style="zoom:67%;" />

> glossy材质：有光泽的材质

<img src="6-Materials and Appearances.assets/image-20230421095948368.png" alt="image-20230421095948368" style="zoom:67%;" />

#### Ideal reflective / refractive  material (BSDF*)

> 透明物体（玻璃、水）：折射+反射

<img src="6-Materials and Appearances.assets/image-20230421100010051.png" alt="image-20230421100010051" style="zoom:67%;" />

> 理想的反射/折射材料：玻璃内部有颜色，说明折射在玻璃内部被部分吸收

<img src="6-Materials and Appearances.assets/image-20230421100023409.png" alt="image-20230421100023409" style="zoom:67%;" />

----

#### Perfect Specular Reflection (BRDF)

> 反射定律：入射角=出射角

<img src="6-Materials and Appearances.assets/image-20230421101821464.png" alt="image-20230421101821464" style="zoom:67%;" />

> 反射公式：$\omega_o + \omega_i$ 给定入射光和防线，可以计算出射角（左）。方位角方法（右）

<img src="6-Materials and Appearances.assets/image-20230421101911481.png" alt="image-20230421101911481" style="zoom:67%;" />

> 理想镜面反射：对应的BRDF较为复杂

<img src="6-Materials and Appearances.assets/image-20230421102300905.png" alt="image-20230421102300905" style="zoom:67%;" />

#### Specular Refraction

> 镜面折射

<img src="6-Materials and Appearances.assets/image-20230421103226453.png" alt="image-20230421103226453" style="zoom:67%;" />



**Snell’s Law**

> 折射定律：入射角的正弦与折射率的乘积=入射角的正弦与折射率的乘积

<img src="6-Materials and Appearances.assets/image-20230421102322073.png" alt="image-20230421102322073" style="zoom:67%;" />

**Law of Refraction**

> 折射定律：根据入射角，计算折射角的余弦。根据余弦有值可判断折射是否发生：入射介质的折射率是大于折射介质的折射率，则可能不发生折射—**全反射现象**。

<img src="6-Materials and Appearances.assets/image-20230421102401365.png" alt="image-20230421102401365" style="zoom:67%;" />

**Snell’s Window / Circle**

> 全反射现象：水到空气，只能看到锥形区域

<img src="6-Materials and Appearances.assets/image-20230421102424192.png" alt="image-20230421102424192" style="zoom:67%;" />

**Fresnel Reflection / Term**

> 菲涅耳项：计算多少能量发生反射，多少发生折射

<img src="6-Materials and Appearances.assets/image-20230421102438993.png" alt="image-20230421102438993" style="zoom:67%;" />

> 菲涅尔项：反射与法线夹角（入射角）的关系

<div>
    <img src="6-Materials and Appearances.assets/image-20230421102544432.png" alt="image-20230421102544432" style="zoom:50%;" width="45%" />
    <img src="6-Materials and Appearances.assets/image-20230421102557428.png" alt="image-20230421102557428" style="zoom:50%;" width="45%"/>
</div>

**Fresnel Term — Formulae**

> - $R_s, R_p$：极化性质；不考虑极化$R_{eff}$
> - 近似处理：$R(\theta)$ ，$\theta$是入射角

<img src="6-Materials and Appearances.assets/image-20230421102520123.png" alt="image-20230421102520123" style="zoom:67%;" />

### Microfacet Material

> 微表面材质：真正的基于物理的材质模型

- 高光平面：从远处看不到细节，看到的是平的粗糙的。从近处看是凹凸不平的

<img src="6-Materials and Appearances.assets/image-20230421105037995.png" alt="image-20230421105037995" style="zoom:67%;" />

#### Microfacet Theory

> **粗糙表面**：
>
> - 假设物体的表面是粗糙的，从远处看表面是平的粗糙的
> - 从近处看是凹凸不平的表明，且一个非常小的微元是完全的镜面反射物体

<img src="6-Materials and Appearances.assets/image-20230421105400118.png" alt="image-20230421105400118" style="zoom:67%;" />

#### Microfacet BRDF

> 表面平滑：微表面的法线的分布，会趋于宏观表面的法线（分布集中），glossy
>
> 表面粗糙：法线分布较分散，漫反射



<img src="6-Materials and Appearances.assets/image-20230421105742247.png" alt="image-20230421105742247" style="zoom:67%;" />

> - $F(i,h)$: 菲涅尔项，不同入射方向发生不同程度的反射
> - $G(i,o,h)$: 微表面之间可能存在遮挡。修正自遮挡自投影情况（光线方向和观察方向沿着表面水平方向，grazing angle）
> - $D(h)$: 法线分布，表示任何给定方向上的分布情况。$D(h)$表示有多少微表面能够把入射方向的光反射到出射方向 —>只有当微表面的法线方向沿着半程向量方向（微表面是完全镜面）。
> - $h$: 半程向量。

<img src="6-Materials and Appearances.assets/image-20230421105924173.png" alt="image-20230421105924173" style="zoom:67%;" />



**Microfacet BRDF: Examples**

> - 微表面模型非常强大！！！可以描述特别多的物体。完全基于物理的渲染
> - 有很多不同的为表面模型

<img src="6-Materials and Appearances.assets/image-20230421112429420.png" alt="image-20230421112429420" style="zoom:67%;" />



-----

**Isotropic / Anisotropic Materials (BRDFs)**

> 各向同性/各向异性材质

<img src="6-Materials and Appearances.assets/image-20230421110450912.png" alt="image-20230421110450912" style="zoom:67%;" />

> - 各向同性：物体的微表面没有一定的方向性，各个方向比较均匀
> - 各向异性：物体的微表面有一定的方向性

<img src="6-Materials and Appearances.assets/image-20230421110511407.png" alt="image-20230421110511407" style="zoom:67%;" />

**Anisotropic BRDFs**

> **各向异性的BRDF：**
>
> BRDF不满足在方位角上旋转：同时旋转入射光线和出射光线，如果BRDF不变，则是各向同性。如果BRDF与绝对方位角相关，则是各向异性。

<img src="6-Materials and Appearances.assets/image-20230421110529639.png" alt="image-20230421110529639" style="zoom:67%;" />

**Anisotropic BRDF: Brushed Metal**

> 人造各向异性材质

<img src="6-Materials and Appearances.assets/image-20230421110551970.png" alt="image-20230421110551970" style="zoom:67%;" />

> 尼龙材质

<img src="6-Materials and Appearances.assets/image-20230421110606310.png" alt="image-20230421110606310" style="zoom:50%;" />

> 天鹅绒：本质不是表面，而是定义在体积上

<img src="6-Materials and Appearances.assets/image-20230421110617606.png" alt="image-20230421110617606" style="zoom:50%;" />

### Properties of BRDFs

> - 非负性
> - 线性

<img src="6-Materials and Appearances.assets/image-20230421110240113.png" alt="image-20230421110240113" style="zoom:67%;" />



> - 可逆性：交换入射方向和出射方向，BRDF不变
> - 能量守恒：无限次弹射后会收敛

<img src="6-Materials and Appearances.assets/image-20230421110252157.png" alt="image-20230421110252157" style="zoom:67%;" />

> - 各向同性：只与相对方位角有关，方位角不需要考虑正负
> - 各向异性

<img src="6-Materials and Appearances.assets/image-20230421110305329.png" alt="image-20230421110305329" style="zoom:67%;" />

### Measuring BRDFs

> **测量BRDF**

**Measuring BRDFs: Motivation**

<img src="6-Materials and Appearances.assets/image-20230421110715471.png" alt="image-20230421110715471" style="zoom:67%;" />



**Image-Based BRDF Measurement**

> 盯着一个着色点，用灯从四面八方照射，并用相机收集出射方向，从而覆盖整个BRDF

<img src="6-Materials and Appearances.assets/image-20230421110731095.png" alt="image-20230421110731095" style="zoom:67%;" />

**Measuring BRDFs: gonioreflectometer**

> 测量仪器

<img src="6-Materials and Appearances.assets/image-20230421110754635.png" alt="image-20230421110754635" style="zoom:67%;" />

**Measuring BRDFs**

> - 测量所有的入射方向和出射方向（4D）
> - 各向同性只有三维数据

<img src="6-Materials and Appearances.assets/image-20230421110814748.png" alt="image-20230421110814748" style="zoom:67%;" />

**Challenges in Measuring BRDFs**

<img src="6-Materials and Appearances.assets/image-20230421110835698.png" alt="image-20230421110835698" style="zoom:67%;" />



**Representing Measured BRDFs**

- Desirable qualities 存储BRDF的要求
  - Compact representation 
  - Accurate representation of measured data
  - Efficient evaluation for arbitrary pairs of directions  
  - Good distributions available for importance sampling



**Tabular Representation**

> 表格表示法

<img src="6-Materials and Appearances.assets/image-20230421110957689.png" alt="image-20230421110957689" style="zoom:67%;" />
