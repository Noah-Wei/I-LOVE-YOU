一个结合了**文字粒子动画**和**爱心粒子效果**的网页，用于展示一段浪漫的文字表达，比如倒计时后显示“我喜欢你”或爱心粒子效果。
97行修改内容
------

### **1. 整体结构**

- 页面主要包含三个<canvas>元素
  - `#canvas`：显示动态的文字粒子动画（如“I LOVE U”）。
  - `#countdownCanvas`：显示倒计时文字的粒子动画（如“3, 2, 1, 我喜欢你”）。
  - `#pinkboard`：用于显示爱心粒子效果。
- 页面还有隐藏的 `<div id="child">`，在倒计时结束后显示一段文字“💗我永远为你着迷”。

------

### **2. 功能模块**

代码分为几个主要功能模块：

#### **(1) 动态文字粒子动画（`#canvas`）**

- 核心功能
  - 动态生成文字“I LOVE U”，每个字母以粒子的形式从顶部随机落下。
  - 粒子不断更新位置，形成类似“下雨”的效果。
- 关键实现
  - `canvas` 的宽高设置为窗口大小。
  - 使用 `ctx.fillText()` 在画布上绘制单个字母，字母的垂直位置由 `drops` 数组控制。
  - 粒子到达画布底部或随机重置位置，模拟“循环掉落”的效果。

------

#### **(2) 倒计时粒子动画（`#countdownCanvas`）**

- 核心功能
  - 倒计时显示“3, 2, 1, 蔡徐坤, 我喜欢你”，每个文字以粒子形式动态显示和消失。
  - 倒计时文字通过粒子组成，文字形成后保持一段时间，然后粒子分散。
- 关键实现
  - 文字粒子生成
    - 使用临时 `<canvas>` 绘制文字，通过 `getImageData()` 提取文字的像素数据。
    - 根据像素的不透明度生成粒子（即文字像素点变为粒子）。
  - 粒子动画
    - 粒子分为三个阶段：
      1. **forming**：粒子从随机位置移动到目标位置，形成文字。
      2. **holding**：文字保持一段时间（约 1 秒）。
      3. **dispersing**：粒子从文字位置分散开，模拟爆炸效果。
  - 倒计时逻辑
    - 倒计时文字存储在 `countdownTexts` 数组中，按顺序显示。
    - 倒计时结束后，隐藏倒计时画布，显示隐藏的 `<div>`，并启动爱心粒子效果。

------

#### **(3) 爱心粒子效果（`#pinkboard`）**

- 核心功能
  - 在倒计时结束后，显示一个动态的爱心粒子效果。
- 关键实现
  - 爱心形状生成
    - 使用数学公式 `pointOnHeart(t)` 生成心形路径的点。
    - 将心形路径绘制到临时 `<canvas>` 上，生成心形图片。
  - 粒子系统
    - 粒子从心形中心发射，沿着心形路径扩散。
    - 粒子大小、透明度、速度随时间变化，形成动态效果。
  - 粒子池
    - 使用 `ParticlePool` 管理粒子，避免频繁创建和销毁对象，提升性能。

------

### **3. 样式与布局**

- 全局样式
  - 页面背景为黑色，`<canvas>` 元素覆盖整个窗口。
  - 隐藏的 `<div>` 显示在屏幕中央，字体为楷体（`STKaiti`），颜色为粉色。
- 粒子样式
  - 粒子为圆形，颜色为粉色（`#f584b7` 或 `#ea80b0`）。
  - 粒子有发光效果（通过 `shadowBlur` 和 `shadowColor` 实现）。

------

### **4. 动画逻辑**

- 倒计时动画
  - 使用 `setInterval` 或 `requestAnimationFrame` 不断更新粒子位置和状态。
  - 动画分为三个阶段（形成、保持、分散），通过 `countdownStage` 控制。
- 爱心动画
  - 粒子从心形路径发射，速度和透明度随时间变化，形成动态效果。
  - 使用 `requestAnimationFrame` 实现平滑动画。
