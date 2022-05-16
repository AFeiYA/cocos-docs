# 阴影

在 3D 世界中，光与影一直都是极其重要的组成部分，它们能够丰富整个环境，质量好的阴影可以达到以假乱真的效果，并且使得整个世界具有立体感。

Creator 3.0 目前支持 **Planar** 和 **ShadowMap** 两种阴影类型。

![shadow](shadow/shadowExample.png)

## 开启阴影

物体开启阴影效果的步骤如下：

1. 在 **层级管理器** 中选中 **Scene**，然后在 **属性检查器** 的 **shadows** 组件中勾选 **Enabled** 属性。

    ![enable-shadow](shadow/enable-shadow.png)

2. 在 **层级管理器** 中选中需要显示阴影的 3D 节点，然后在 **属性检查器** 的 **MeshRenderer** 组件中将 **ShadowCastingMode** 属性设置为 **ON**。

    ![set-meshrenderer](shadow/set-meshrenderer.png)

    若阴影类型是 **ShadowMap**，还需要将 MeshRenderer 组件上的 **ReceiveShadow** 属性设置为 **ON**。

> **注意**：如果阴影无法正常显示，需要调整一下方向光的照射方向。

## shadows 类型

阴影类型可在 shadows 组件的 **Type** 属性中设置。

### Planar shadow

Planar 阴影类型一般用于较为简单的场景。

![planar properties](shadow/planar-properties.png)

| 属性  | 说明  |
| :--- | :--- |
| **Enabled**     | 是否开启阴影效果      |
| **Type**        | 阴影类型             |
| **Saturation**  | 调节阴影饱和度，建议设置为 **1.0**。若需要减小方向光阴影的饱和程度，推荐通过增加环境光来实现，而不是调节该值       |
| **ShadowColor** | 设置阴影颜色         |
| **Normal**      | 阴影接收平面的法线，垂直于阴影，用于调整阴影的倾斜度  |
| **Distance**    | 阴影在接收平面上与坐标原点的距离     |

调节方向光照射的方向可以调节阴影的投射位置。

> **注意**：Planar 类型的阴影只有投射在平面上才能正常显示，不会投射在物体上，也就是说 MeshRenderer 组件中的 **ReceiveShadow** 属性是无效的。

### ShadowMap

ShadowMap 是以光源为视点来渲染场景的。从光源位置出发，场景中看不到的地方就是阴影产生的地方。

![shadow Map 面板细节](shadow/shadowmap-properties.png)

| 属性  | 说明  |
| :--- | :--- |
| **Enabled**                   | 勾选该项以开启阴影效果     |
| **Type**                      | 设置阴影类型    |
| **MaxReceived**               | 最多支持产生阴影的光源数量，默认为 4 个，可根据需要自行调整     |
| **ShadowMapSize**             | 设置阴影贴图分辨率，目前支持 **Low_256x256**、**Medium_512x512**、**High_1024x1024**、**Ultra_2048x2048** 四种精度的纹理     |

> **注意**：从 v3.3 开始，**属性检查器** 中阴影的 **Linear**、**Packing** 项被移除，Creator 将自动判断硬件能力，并选用最优方式进行阴影渲染。

ShadowMap 在开启了物体 **MeshRenderer** 组件上的 **ReceiveShadow** 后，就会接收并显示其它物体产生的阴影效果。

ShadowMap 一般用于要求光影效果比较真实，且较为复杂的场景。但不足之处在于如果不移动光源，那么之前生成的 ShadowMap 就可以重复使用，而一旦移动了光源，那么就需要重新计算新的 ShadowMap。

## 支持动态合批提高性能

对于材质中已经开启 instancing 的模型，平面阴影也会自动同步使用 instancing 绘制，详情请参考 [动态合批](../../../engine/renderable/model-component.md#%E5%85%B3%E4%BA%8E%E5%8A%A8%E6%80%81%E5%90%88%E6%89%B9)。