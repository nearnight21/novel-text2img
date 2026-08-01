# Novel Reader Agent

这是一个面向“小说角色视觉设计”的 Hermes Profile Distribution。它使用长上下文模型阅读小说原文，提取会影响人物成图的事实，并通过共享文件交给视觉 Agent。

这是本项目自己编写的 Agent 资产，不是 Hermes 官方 Agent。Hermes 只负责提供 Profile Distribution 的安装和运行机制。

## 安装

在已经安装 Hermes 的电脑上，直接从本仓库安装：

```bash
hermes profile install https://github.com/nearnight21/novel-text2img.git --alias
```

安装后，为这个 Profile 选择长上下文模型并配置 API：

```bash
novel-reader setup
```

如果系统没有创建 `novel-reader` 命令别名，可以使用：

```bash
hermes -p novel-reader setup
```

## 在小说项目中使用

进入小说项目根目录。首次使用时，明确要求 Agent 初始化项目配置：

```bash
novel-reader chat -q "/novel-reader 初始化当前项目"
```

然后填写生成的 `novel-reader.project.yaml`，声明以下内容的位置：

- 小说正文目录；
- 角色事实档案目录；
- 视觉 Agent 的交接目录；
- 当前要处理的角色和时期。

配置完成后，唤醒 Agent：

```bash
novel-reader chat -q "/novel-reader 按协作协议处理下一项待办"
```

用户只负责唤醒 Agent。Agent 根据项目文件自行判断下一项工作：

- 当前角色缺少 `source-brief.md`：阅读原文并生成角色视觉材料包；
- `reader-requests/*.md` 中有 `status: pending`：回到原文定向检索并回答；
- 没有属于阅读 Agent 的待办：说明后停止。

## 双 Agent 文件协作

阅读 Agent 和视觉 Agent 默认不直接聊天，使用小说项目中的文件交接：

```text
阅读 Agent 写入 source-brief.md
        ↓
视觉 Agent 读取并出图
        ↓
视觉 Agent 在 reader-requests/ 写入原文问题
        ↓
阅读 Agent 回答同一个问题文件
```

阅读 Agent 保存原文事实、时期区分和关键摘录；视觉 Agent 负责脸型、配色、构图、风格和原文空白处的视觉设计。阅读 Agent 不因 `draft`、字段缺失或证据尚未整理完而阻止探索性草图。

## 资产边界

本仓库包含：

- Agent 身份和行为规则；
- `novel-reader` Skill；
- 双 Agent 文件协作协议；
- 角色视觉材料包和定向问题模板。

本仓库不包含：

- 小说正文；
- 角色事实档案；
- 图片和生成记录；
- API Key、会话、记忆和本地运行数据。

## 更新

其他设备可以使用同一个安装地址部署。仓库更新后，在已安装 Hermes 的设备上运行：

```bash
hermes profile update novel-reader
```

当前资产版本：`0.1.0`。
