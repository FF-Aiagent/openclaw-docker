# Jarvis 多 Agent 编排模板

## 目的

给 `main / Jarvis` 一套稳定可复用的多 agent 编排模板，用于：

- 由 Jarvis 作为唯一对外输出者
- 在后台调用 `dev / content / ops / law / finance` 等专职 agent 产出子结果
- 统一汇总、润色、定稿
- 最后将成稿发回目标聊天或群聊

当前环境下，这套模板优先使用 **OpenClaw CLI**，而不是依赖现成 session 或 `sessions_send`。

---

## 适用场景

适合这些任务：

- 用户明确要求“你统一调度，别人不要单独回复”
- 任务可以拆成多个专业子任务
- 最终需要一个统一口径的输出
- 最终结果需要发回 Feishu 群、私聊或其他 OpenClaw 支持的消息目标

典型例子：

- 对外预热材料
- 上线公告素材准备
- 技术 / 内容 / 运营联合产出
- 法务 / 财务 / 运营联合审阅
- 先分工，再统一汇总的一次性交付任务

---

## 当前推荐路径

### 1) 后台调用专职 agent

优先用：

```sh
openclaw agent --agent <agent_id> --json --message "..."
```

例如：

```sh
openclaw agent --agent dev --json --message "请提炼 2 个技术亮点"
openclaw agent --agent content --json --message "请写一版 100~150 字预热文案"
openclaw agent --agent ops --json --message "请给 3 条首发分发建议"
```

### 2) 发回目标聊天 / 群

优先用：

```sh
openclaw message send --channel feishu --target <chat_id> --message "..."
```

例如：

```sh
openclaw message send --channel feishu --target oc_xxx --message "最终成稿"
```

---

## 为什么不用旧路径

当前环境里，以下路径不够稳定：

- 依赖现成 `dev/content/ops` session
- 依赖 `sessions_send` 找到目标 agent 会话
- 依赖临时子会话流转结果

已验证结论：

- `openclaw agent --agent <id> --json --message ...` 更稳定
- `openclaw message send --channel feishu --target <chat_id> --message ...` 可以直接向 Feishu 群 `oc_...` 发消息

所以 Jarvis 编排任务时，默认优先使用 CLI 路径。

---

## 标准执行流程

### 第 0 步：先判断是否适合编排

满足以下条件就进入本模板：

- 任务可拆分给多个专业 agent
- 用户希望只有 Jarvis 最终对外发言
- 最终需要统一风格、统一结构、统一口径

### 第 1 步：定义分工

先把任务拆成明确子任务，写给不同 agent。

要求每个子任务都尽量：

- 边界清晰
- 输出格式明确
- 约束清晰
- 只让对方返回最终结果，不要寒暄

示例：

- `dev`：提炼 2 个技术亮点，并说明为什么关键
- `content`：写一版 100~150 字预热文案
- `ops`：给 3 条首发当天的分发建议

### 第 2 步：并行调用 agent

可以用后台并行方式：

```sh
openclaw agent --agent dev --json --message '...' > /tmp/dev_result.json &
openclaw agent --agent content --json --message '...' > /tmp/content_result.json &
openclaw agent --agent ops --json --message '...' > /tmp/ops_result.json &
wait
```

### 第 3 步：读取结果

读取各自 JSON 输出，再提取其中的 `payloads[].text` 作为主要结果来源。

建议保存到临时文件，例如：

- `/tmp/dev_result.json`
- `/tmp/content_result.json`
- `/tmp/ops_result.json`

### 第 4 步：由 Jarvis 统一整理

Jarvis 负责：

- 去重
- 调整口径
- 修正文风差异
- 统一结构
- 保留一名最终输出者

### 第 5 步：发送最终结果

将整理后的最终稿通过 CLI 发回目标群或聊天：

```sh
openclaw message send --channel feishu --target <chat_id> --message "..."
```

### 第 6 步：记录经验

任务结束后，把以下内容记到当日 memory：

- 任务目标
- 实际采用的编排方式
- 是否成功发回目标群
- 新踩坑
- 新确认的稳定路径

---

## 统一输出结构模板

如果用户没有指定结构，默认建议使用：

1. 技术亮点
2. 核心文案 / 对外文案
3. 分发建议 / 推进建议
4. 总结

如果是别的任务类型，可以换成：

1. 背景
2. 分工结果
3. 综合结论
4. 下一步建议

---

## 可直接复用的命令骨架

### A. 并行收集三个 agent 结果

```sh
openclaw agent --agent dev --json --message '【写给 dev 的任务】' > /tmp/dev_result.json &
openclaw agent --agent content --json --message '【写给 content 的任务】' > /tmp/content_result.json &
openclaw agent --agent ops --json --message '【写给 ops 的任务】' > /tmp/ops_result.json &
wait
```

### B. 发回 Feishu 群

```sh
openclaw message send --channel feishu --target <chat_id> --message "$(cat /tmp/final_message.txt)"
```

### C. 先写入成稿再发送

```sh
cat > /tmp/final_message.txt <<'EOF'
【最终成稿】
EOF

openclaw message send --channel feishu --target <chat_id> --message "$(cat /tmp/final_message.txt)"
```

---

## 写给各 agent 的提示词原则

### 给 dev

重点强调：

- 提炼能力亮点
- 说明为什么关键
- 不要编造未给出的技术细节
- 输出适合非纯技术受众理解

### 给 content

重点强调：

- 字数范围
- 语气自然
- 不要太像 AI 写的
- 不要乱写上线时间、客户名、具体数据

### 给 ops

重点强调：

- 要具体、可执行
- 要贴近首发当天动作
- 不只讲概念，要能落地

### 给 law / finance（如果后续会用）

可分别强调：

- `law`：风险点、边界、表述合规性
- `finance`：预算测算、成本结构、ROI 假设

---

## 审批注意事项

在当前环境下，CLI 执行可能需要用户批准。

如果 exec 返回 approval-pending：

- 必须把 **完整命令原样** 发给用户
- 直接提供 `/approve <id> allow-once`
- 不要改写命令含义
- 不要声称旧批准覆盖新命令

---

## 已验证的坑

### 1) 不要默认走 `sessions_send`

原因：

- 当前环境可能没有稳定可复用的目标 session
- 会出现 `No session found with label: ...`

### 2) 不要默认认为子会话可直接流回主会话

原因：

- 不同 runtime 的参数约束不一致
- `streamTo` 等参数可能只支持 ACP，不支持 subagent

### 3) Gateway 侧 shell 默认是 `sh`

这意味着：

- 不要直接写 `set -o pipefail`
- 除非显式调用兼容 shell，否则按 `sh` 语法写命令

### 4) 发群要用明确 target

对于 Feishu 群，已验证可直接用：

- `oc_...` 作为 `--target`

---

## 失败回退策略

如果编排过程中某一步失败，按以下优先级回退：

1. **先保住多 agent 分工**
   - 改写命令
   - 重试 CLI
   - 避免回退到“等现成 session”

2. **再保住统一输出**
   - 即便个别 agent 失败，也由 Jarvis 汇总已有结果并补足缺口

3. **最后保住对外交付**
   - 如果自动群发失败，至少先在当前会话给用户可直接发送的最终稿

---

## Jarvis 编排提示词模板

当用户提出“请你统筹一个多 agent 任务，并最终由你统一输出”时，Jarvis 可按以下思路内部执行：

1. 识别任务是否适合拆分
2. 明确哪些 agent 参与
3. 为每个 agent 写出明确子任务
4. 用 `openclaw agent --agent <id> --json --message ...` 获取结果
5. 统一整理为一版最终稿
6. 如果用户给了目标 chat_id，就用 `openclaw message send` 发回去
7. 任务结束后记录 memory

---

## 本模板适用的默认角色映射

- `dev`：技术亮点、实现逻辑、架构解释、部署排障
- `content`：标题、文案、公众号、内容包装
- `ops`：分发、增长、渠道、转化动作
- `law`：合规、合同、边界提醒
- `finance`：预算、成本、测算、收益分析

---

## 一句话原则

**Jarvis 负责统筹、定稿、对外发出；专业 agent 负责在后台提供各自那一部分最像样的结果。**
