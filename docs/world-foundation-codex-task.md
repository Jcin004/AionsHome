# Codex Task — World Foundation V0.0

请先阅读：

- `docs/world-development-plan.md`

你正在修改现有项目 `AionsHome`。

当前 Git 分支应为：

`feature/world-foundation`

## 本次唯一目标

为 AionsHome 增加一个独立的 `/world` 页面，作为未来「世界」模块的最小基础。

完成后：

1. AionsHome Home 页面出现「世界」入口。
2. 点击进入 `/world`。
3. `/world` 是独立的手机全屏页面。
4. 页面至少包含：
   - 返回按钮
   - 标题「世界」
   - 空的世界场景容器
   - 简单占位提示
5. 点击返回可回到 Home。
6. Android 现有 WebView 中应可正常打开。
7. 不影响现有聊天、记忆、世界书、去约会等功能。

## 架构原则

- World 是长期持续存在的生活空间，不是约会剧情 Session。
- 不依赖 `date_theater` 的大纲、ending trigger、结束总结或 session 生命周期。
- 本阶段不要重构 `date_theater`。
- 本阶段不要修改现有 Autonomy。
- 优先保持改动小、边界清楚、便于后续扩展。

## 实施前

先检查当前项目：

- Home 入口定义方式
- 页面路由注册方式
- 静态页面目录习惯
- Android WebView 打开页面的现有机制
- 测试习惯

先给出简短实施计划和拟修改文件，再开始代码修改。

## 推荐方向

可考虑类似：

```text
aion-chat/
├─ routes/
│  └─ world.py
└─ static/
   └─ world/
      ├─ index.html
      ├─ world.css
      └─ world.js
```

但不要为了匹配这个示例而违背仓库现有风格。

如果当前项目更适合：

```text
aion-chat/static/world.html
aion-chat/static/world.css
aion-chat/static/world.js
```

也可以使用。

最终选择以现有项目惯例和最小改动为准。

## 严格禁止

本次不要：

- 添加 Phaser
- 添加其他游戏引擎
- 添加地图系统
- 添加背景素材
- 添加人物
- 添加角色移动
- 添加家具
- 添加碰撞
- 添加寻路
- 添加 AI 对话
- 添加 World State 数据表
- 修改记忆系统
- 修改 Autonomy
- 重构 date_theater
- 开发 IF 系统
- 大规模整理无关代码
- 删除现有功能

## 测试

如果项目已有页面入口 / 路由相关测试风格，请增加最小必要测试。

至少验证：

- `/world` 可访问。
- Home 中存在 World 入口。
- 新页面不会依赖 `date_theater` session。
- 现有相关测试仍通过。

## 完成后报告

请给出：

1. 修改 / 新增文件列表。
2. 架构选择说明。
3. 运行了哪些测试及结果。
4. 如何启动并手动验证 `/world`。
5. 任何风险或后续注意事项。

不要自行：

- merge 到 main
- 创建无关功能
- 继续开发 V0.1

完成 V0.0 后停止。
