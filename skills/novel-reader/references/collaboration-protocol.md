# 双 Agent 文件协作协议

阅读 Agent 与视觉 Agent 默认不直接聊天。用户只负责唤醒 Agent，文件负责传递完整上下文。

```text
Agent A 写入约定文件
→ 用户唤醒 Agent B
→ Agent B 读取当前角色的文件变化
→ Agent B 处理属于自己的下一项待办
→ Agent B 写回约定文件并停止
```

## 项目文件

所有路径都相对 `novel-reader.project.yaml` 所在的项目根目录。默认约定为：

```text
novel-reader.project.yaml
source/
canonical/characters/
visual-design/
└── <character-id>/
    ├── source-brief.md
    ├── reader-requests/
    │   └── Q-001.md
    └── feedback.md
```

`source/`、角色事实目录和 `visual-design/` 都可在项目配置中改名。

## 文件变化的含义

| 文件变化 | 接手者 | 动作 |
| --- | --- | --- |
| 当前角色缺少 `source-brief.md` | 阅读 Agent | 阅读原文并创建材料包 |
| `reader-requests/*.md` 出现 `status: pending` | 阅读 Agent | 定向检索并回答同一文件 |
| 问题变为 `status: answered` | 视觉 Agent | 吸收答案并继续视觉设计 |
| `feedback.md` 更新 | 视觉 Agent | 按用户反馈迭代 |
| 没有属于自己的信号 | 当前 Agent | 立即停止，不制造任务 |

`pending` 与 `answered` 只是问题文件的交接标记，不是项目状态机，也不能阻止其他探索工作。

## 阅读 Agent 的启动顺序

每次被唤醒时只检查：

1. 当前项目配置。
2. 当前角色目录。
3. 当前角色的 `reader-requests/`。
4. 当前角色是否缺少或被明确要求更新 `source-brief.md`。

不得扫描所有角色、历史记录、生成任务、测试或 changelog。

## 何时需要询问用户

只有文件无法消除以下歧义时才询问：

- 无法确定本次角色或时期。
- 原文证据直接冲突，且不同解释会改变硬设定。
- 当前问题实际上要求决定视觉设计，而不是确认原文。
- 用户要求扩大到其他角色、正式资产或批量处理，但范围不明确。

脸型、发色、服装配色等原文空白不退回阅读环节。把它们标为“需要设计”，交给视觉 Agent 提方案。

## 固定唤醒语句

用户可以始终只说：

> 按协作协议处理下一项待办。

用户不需要复制另一个 Agent 的回答，也不需要解释哪个文件发生了变化。
