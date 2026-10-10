# World Development Plan

> AionsHome 的「世界（World）」长期开发计划。
>
> 核心原则：**世界不是一场会结束的故事，而是一个持续存在的生活空间。**

## 1. 产品目标

在现有 AionsHome 基础上新增独立的 2D 像素生活世界。

未来目标包括：

- User 可创建自己的像素小人并自由移动。
- CHAR 以独立像素角色存在于世界中。
- User 可走到 CHAR 面前进入第一人称互动，也可直接点击 CHAR 进入。
- 房间和地点可探索。
- 家具是独立对象，可布置、碰撞和交互。
- CHAR 具有世界状态、日程、自主行动和持续记忆。
- 第一人称互动复用现有「去约会」的显示能力，但不继承强制大纲和结束机制。
- 后续可扩展多世界与 IF 独立沙盒。

## 2. 关键设计原则

### 2.1 世界是持续空间，不是剧情 Session

World 不依赖以下 date_theater 机制：

- 约会大纲
- ending trigger
- 强制结束
- 约会总结
- 独立约会 session 生命周期

世界应允许角色持续生活、行动、聊天和恢复状态。

### 2.2 第一人称是一种视角，不是一种剧情模式

第一人称应逐步抽成可复用的表现层：

- 场景背景
- 人物展示
- 人物动作
- 对话框
- 输入
- TTS / 音频展示能力

这些能力可被不同模式复用：

- 世界自由互动
- 原有去约会
- 特殊事件
- 后续 IF 故事模式

### 2.3 AI 决策与世界执行分离

遵循：

```text
CHAR 人格 / 记忆 / 当前状态
        ↓
Autonomy / Planner
        ↓
Action Intent
        ↓
World Action Gate
        ↓
World Runtime
        ↓
真实位置 / 动画 / 家具交互
        ↓
World State
        ↓
必要时写入 Memory
```

原则：

- AI 负责“想做什么”。
- World Runtime 负责“实际怎么做”。
- AI 不直接控制人物每一帧。
- 文字描述不得与世界真实状态脱节。

### 2.4 现有 AionsHome Autonomy 优先复用

AionsHome 已存在：

- `autonomy.py`
- `autonomy_state.py`
- `routes/autonomy.py`

后续 World 不新建第二套彼此独立的角色自主系统。

方向：

- 原 Autonomy = “脑”
- 新 World Runtime = “身体”

### 2.5 世界状态、记忆、日程职责不同

- **World State**：现在在哪里、正在做什么、当前姿态、当前目标。
- **Schedule**：准备什么时候做什么。
- **Memory**：实际发生过且值得保留的经历。
- **Persona / Character Anchor**：CHAR 是谁、性格、关系、自我认知。
- **Worldbook**：世界规则、地点、背景设定。

## 3. CHAR 的世界内认知

默认目标：

- CHAR 以“自己正在这里生活”的视角理解世界。
- 知道自己是谁、在哪里、正在做什么、认识谁、近期经历。
- 不默认用“玩家 / NPC / 游戏角色”口吻理解世界。
- 若用户直接询问 AI 身份，应保持真实边界，不强制伪装成现实人类。

未来允许不同 CHAR / 世界配置不同的身份认知。

## 4. 第三人称视觉方向

- 手机竖屏。
- 精致像素 / JRPG / Stardew-like 正向俯视房间。
- 非等距 / 非 4 度俯角插画。
- User 与 CHAR 均为独立 Sprite。
- 方向至少：前 / 后 / 左 / 右。
- 角色比例偏修长，不使用原版 Stardew 的短比例。

### 场景分层

建议未来结构：

```text
room shell / floor / walls
        +
separate furniture
        +
character sprites
        +
foreground / occlusion
        +
UI
```

家具必须独立，不能永久烘焙进背景图。

## 5. 家具对象长期目标

每个家具实例未来至少具备：

- asset/type id
- position
- footprint
- collision region
- direction / variant（如需要）
- draw / occlusion information
- interaction anchors
- supported actions
- movable flag

例如：

- bed → sleep
- chair → sit
- sofa → sit / chat / hug
- bookshelf → read
- computer → use
- wardrobe → change clothes
- door → transition

“碰撞”第一阶段理解为：角色不能穿过去，会绕路。无需上真实刚体物理。

## 6. 第一人称复用策略

现有 `date_theater` 保留原样。

未来只在确实需要时提取共用能力，不提前大重构。

禁止把整个 World 直接塞进：

- `date_theater.js`
- `date_theater.py`

World 的自由互动不得依赖“必须先生成大纲”。

## 7. IF 线（未来，不是当前任务）

未来可作为独立沙盒分支：

- 独立 World State
- 独立记忆作用域
- 可选择自由 IF 或故事 IF
- 故事 IF 可选大纲，但大纲是参考而非硬轨道

可选能力：

> 将 IF 经历作为梦境 / 故事分享给主世界 CHAR。

默认不自动污染主世界记忆。

## 8. 开发阶段

### World V0.0 — World Foundation

目标：

- Home 出现「世界」入口。
- 新增独立 `/world` 页面。
- 可进入、可返回。
- 有空白场景容器。
- Android 现有 WebView 能打开。
- 不影响现有功能。

本阶段不做 Phaser、不做角色、不做地图逻辑。

### World V0.1 — 第一间静态房间

- 确定视觉规格。
- 加入一间真正的像素房间。
- 先只显示，不移动。

### World V0.2 — 独立家具对象

- 房间 shell 与家具拆分。
- 固定位置显示家具。
- 先不做编辑器。

### World V0.3 — User Sprite

- 显示 User 像素小人。
- 四方向基础素材。

### World V0.4 — 移动 + 碰撞

- User 移动。
- 墙壁 / 家具碰撞。
- 基础镜头逻辑。

### World V0.5 — CHAR Sprite

- CHAR 独立显示。
- CHAR 与角色数据绑定。
- 暂不做复杂自主行为。

### World V0.6 — CHAR 交互

两种进入第一人称的方法：

1. User 走到 CHAR 面前互动。
2. 直接点击 CHAR。

### World V0.7 — 第一人称自由互动

- 复用 / 提取 date_theater 的表现能力。
- 使用自由世界对话上下文。
- 不生成大纲。
- 不强制结束。

### World V0.8 — 家具布置

- 网格 / 吸附。
- 拖动。
- 保存布局。
- 更新碰撞与交互点。

### World V0.9 — World State

开始持久化：

- location
- position
- current activity
- interaction target
- selected world / room

### World V1.0 — 基础自主生活

基于现有 Autonomy 扩展 world actions：

- move_to
- interact_with
- sit
- sleep
- read
- talk_to
- follow
- hug
- idle

AI 输出 intent，World Runtime 执行。

## 9. 当前明确不做

在 V0.0 阶段禁止：

- Phaser
- 游戏引擎接入
- 人物移动
- CHAR
- 家具系统
- 碰撞
- AI 对话
- World State 数据库
- Autonomy 修改
- date_theater 重构
- IF 系统
- 多世界系统
- 大规模项目清理 / 重构

## 10. 开发工作流

稳定分支：

`main`

当前开发分支：

`feature/world-foundation`

原则：

- 每个阶段独立小任务。
- Codex 先读本计划，再做当前阶段。
- 不直接在 main 上实验。
- 每阶段运行相关测试。
- 完成后先检查 diff，再提交。
- 通过 PR 合并到 main。

## 11. 参考项目

仅用于学习架构和交互思路，不直接复制受限代码 / 美术：

- AionsHome 现有 date_theater
- AionsHome 现有 autonomy
- StardewLivingNPCs
- SmartNPC
- StardewValley-MCP
- ValleyTalk
- EvolvingAIConversation（公开资料 / 行为参考）

重点参考的共同思想：

- 人格与状态独立。
- 计划与执行独立。
- 行动必须经过校验。
- 世界系统执行真实动作。
- 结果再反馈给状态和记忆。

## 12. 当前下一步

只执行：

**World V0.0 — World Foundation**

完成标准：

- AionsHome Home 能进入「世界」。
- `/world` 是独立页面。
- 有返回。
- 有空场景容器。
- 不引入玩法逻辑。
- 不影响现有功能。

完成 V0.0 后，再开始第一间真正的房间。
