# openclaw-docker

SF 的 OpenClaw Docker 工作区仓库。

这个仓库用于统一管理 **OpenClaw 主配置 + 多 Agent workspace 文档与初始化文件**，方便长期维护、回滚和同步。

---

## 仓库目的

这个仓库主要管理三类内容：

1. **OpenClaw 顶层配置**
   - `openclaw.json`
   - `openclaw.json.bak*`
   - `completions/`

2. **主 Agent 与多个专职 Agent 的 workspace 文件**
   - `workspace/`
   - `workspace-dev/`
   - `workspace-content/`
   - `workspace-ops/`
   - `workspace-law/`
   - `workspace-finance/`

3. **长期可复用的文档和角色设定**
   - `SOUL.md`
   - `USER.md`
   - `IDENTITY.md`
   - `AGENTS.md`
   - `TOOLS.md`
   - `memory/*.md`
   - `skills/`

---

## 多 Agent 结构

当前团队成员：

- **main（Jarvis 🤖）** — 统筹全局、调度团队、汇报结果
- **dev（开发助理 💻）** — 代码开发、技术架构、部署
- **content（内容助理 ✍️）** — 公众号文章、文案、内容创作
- **ops（运营增长 📈）** — 用户增长、社交媒体、市场推广
- **law（法务助理 ⚖️）** — 合同审查、合规咨询
- **finance（财务助理 💰）** — 成本核算、预算管理

每个 Agent 都有自己的独立 workspace，用于隔离：

- persona / identity
- 用户资料
- 记忆文件
- 任务上下文
- 专业分工

---

## 目录结构

```text
/home/node/.openclaw
├── openclaw.json
├── completions/
├── workspace/                # main / Jarvis
├── workspace-dev/            # dev
├── workspace-content/        # content
├── workspace-ops/            # ops
├── workspace-law/            # law
├── workspace-finance/        # finance
└── .gitignore
```

说明：

- `workspace/` 是主协调 Agent `main (Jarvis)` 的工作区
- 其他 `workspace-*` 是各专职 Agent 的独立工作区
- 所有这些 workspace 现在都由**同一个上层 git 仓库**统一管理

---

## Docker 路径映射

OpenClaw 运行在 Docker 容器中。

### 宿主机路径（macOS）

```text
/Users/sf/projects/AI_code/openclaw/docker_openclaw/data/openclaw/.openclaw
```

### 容器内路径

```text
/home/node/.openclaw
```

### 典型映射示例

- Host `.../.openclaw/workspace` ↔ Container `/home/node/.openclaw/workspace`
- Host `.../.openclaw/workspace-dev` ↔ Container `/home/node/.openclaw/workspace-dev`
- Host `.../.openclaw/workspace-content` ↔ Container `/home/node/.openclaw/workspace-content`
- Host `.../.openclaw/workspace-ops` ↔ Container `/home/node/.openclaw/workspace-ops`
- Host `.../.openclaw/workspace-law` ↔ Container `/home/node/.openclaw/workspace-law`
- Host `.../.openclaw/workspace-finance` ↔ Container `/home/node/.openclaw/workspace-finance`

### 重要规则

当在 OpenClaw 运行环境中编辑 `openclaw.json` 时：

- `agents.list[].workspace` 必须填写 **容器内路径**
- 不要填写宿主机 `/Users/...` 路径

正确示例：

```json5
{
  "agents": {
    "list": [
      {
        "id": "dev",
        "workspace": "/home/node/.openclaw/workspace-dev"
      }
    ]
  }
}
```

---

## Git 管理原则

这个仓库当前采用的是：

- **统一仓库根目录：** `/home/node/.openclaw`
- **统一管理所有 workspace**
- **不允许 workspace 内再嵌套独立 git 仓库**

也就是说：

- `workspace/`
- `workspace-dev/`
- `workspace-content/`
- `workspace-ops/`
- `workspace-law/`
- `workspace-finance/`

都属于同一个 repo。

---

## 哪些内容会进 Git

会进 Git 的内容：

- 配置文件
- 各 workspace 的初始化文件
- Agent 角色定义
- 用户偏好文档
- 长期有价值的记忆 markdown
- 技能文件和本地说明文档

适合版本化的典型文件：

- `SOUL.md`
- `USER.md`
- `IDENTITY.md`
- `AGENTS.md`
- `TOOLS.md`
- `memory/YYYY-MM-DD.md`
- `skills/...`

---

## 哪些内容不会进 Git

以下内容属于运行态、敏感信息或纯缓存，已通过 `.gitignore` 排除：

- `credentials/`
- `identity/`
- `devices/`
- `logs/`
- `tasks/`
- 顶层运行态 `memory/`
- `feishu/`
- `exec-approvals.json`
- `*.sqlite`
- `**/.openclaw/`

### 原因

这些内容通常包含：

- 密钥 / 凭据
- 设备配对信息
- 日志和缓存
- 数据库
- 会话运行态文件

它们不适合进入公开或长期版本控制历史。

---

## 常见维护动作

### 查看仓库状态

```bash
cd /home/node/.openclaw
git status
```

### 提交变更

```bash
git add .
git commit -m "your message"
```

### 推送到 GitHub

```bash
git push
```

---

## 初始化与协作约定

每个 Agent workspace 建议至少包含：

- `SOUL.md` — 角色定位与工作风格
- `USER.md` — 服务对象信息
- `IDENTITY.md` — 自我身份
- `AGENTS.md` — 团队通讯录与协作方式
- `TOOLS.md` — 本地环境说明
- `memory/YYYY-MM-DD.md` — 每日关键记录

### 记录原则

- 重要事项：写入对应长期文档
- 重要过程：写入 daily memory
- 环境规则 / 路径映射 / 踩坑经验：优先写入 `TOOLS.md`
- 用户偏好：写入 `USER.md`

---

## 当前仓库用途总结

这是一个面向 **OpenClaw + Docker + 多 Agent + 多 Feishu 机器人** 的工作区仓库。

它的核心目标不是存一切，而是：

- 稳定保存配置
- 保存角色初始化
- 保存长期有价值的工作文档
- 避免路径、workspace、git 管理再次混乱

---

## 备注

如果后续继续扩 Agent，建议沿用现有命名方式：

- `workspace-<agentId>`
- `agents.list[].id = <agentId>`
- `bindings[].match.accountId = <agentId>`（若与渠道账号一一映射）

这样排错和维护成本最低。


---

## 多 Agent + Feishu + Docker 维护 SOP

### 一、新增一个 Agent 的标准步骤

1. 在 `openclaw.json` 中新增 agent：

```json5
{
  "agents": {
    "list": [
      {
        "id": "new-agent",
        "workspace": "/home/node/.openclaw/workspace-new-agent"
      }
    ]
  }
}
```

2. 创建对应 workspace 目录：

```bash
mkdir -p /home/node/.openclaw/workspace-new-agent
```

3. 初始化最小文件：

- `SOUL.md`
- `USER.md`
- `IDENTITY.md`
- `AGENTS.md`
- `TOOLS.md`
- `HEARTBEAT.md`
- `memory/YYYY-MM-DD.md`

4. 如需接渠道账号，再补 `bindings`
5. 完成后执行状态检查

---

### 二、新增一个 Feishu 机器人账号的标准步骤

1. 在 `channels.feishu.accounts.<accountId>` 中补充：

- `appId`
- `appSecret`
- `name`

2. 增加 `bindings`：

```json5
{
  "agentId": "dev",
  "match": {
    "channel": "feishu",
    "accountId": "dev"
  }
}
```

3. 确保 `accountId`、`agentId`、`workspace-<id>` 尽量保持一致
4. 检查 `dmPolicy` / `groupPolicy` 是否符合当前安全需求
5. 使用以下命令验证：

```bash
docker exec -it openclaw-gateway openclaw channels status --probe
```

---

### 三、修改 workspace 路径时的检查步骤

重点：**在配置中始终写容器路径，不写宿主机路径。**

错误示例：

```text
/Users/sf/.../workspace-dev
```

正确示例：

```text
/home/node/.openclaw/workspace-dev
```

检查动作：

1. 看 `openclaw.json` 里的 `agents.list[].workspace`
2. 确认目录在容器内真实存在
3. 确认宿主机挂载路径能映射到该目录
4. 必要时记录到 `TOOLS.md`

---

### 四、Git 提交前的自检步骤

在宿主机或容器中确认：

```bash
cd /home/node/.openclaw
git status
```

重点检查：

- 是否误提交 `credentials/`
- 是否误提交 `logs/`
- 是否误提交 `identity/`
- 是否误提交 sqlite / 运行态缓存
- 是否新增了应记录的重要文档改动

提交：

```bash
git add .
git commit -m "your message"
git push
```

---

### 五、单个 Feishu 账号异常时的排查顺序

如果出现这种情况：

- 大部分机器人正常
- 只有一个机器人 `probe failed`

优先排查：

1. `appId` 是否手误
2. `appSecret` 是否手误
3. 飞书应用权限是否一致
4. `accountId` 绑定是否写对
5. 该 agent 的 workspace 路径是否存在

### 本项目已验证过的一个典型坑

- `ops` 机器人曾出现 `probe failed`
- 最终原因是 `appId` 手误：
  - 错误：`Acli_a950b267aab91bca`
  - 正确：`cli_a950b267aab91bca`

结论：

> 当只有单个 Feishu 机器人异常时，优先检查该账号的 `appId/appSecret` 是否录入错误。


### 六、推荐协作方式（最新）

对于多步骤、跨部门、多 agent 协作任务，推荐使用以下方式：

1. **用户私聊 Jarvis 下达任务**
2. **Jarvis 在后台调用 dev / content / ops / law / finance**
3. **Jarvis 统一汇总结果**
4. **Jarvis 再把最终结果输出到目标群聊或指定会话**

#### 为什么这样更稳

因为如果多个机器人同时在同一个群里直接接任务，容易出现：

- 抢答
- 重复响应
- 会话查找失败
- 多个 agent 各自尝试统筹，导致混乱

因此当前推荐模式是：

- **群聊作为结果展示面板**
- **Jarvis 作为唯一协作入口**
- **其他 agent 作为后台协作角色**

#### 私聊 Jarvis 的推荐模板

```text
请你统筹一个多 agent 任务，并在完成后把最终结果发到飞书协作群（chat_id: <目标群 chat_id>）。

任务分工：
- dev：<开发相关任务>
- content：<内容相关任务>
- ops：<运营相关任务>
- law：<法务相关任务，可选>
- finance：<财务相关任务，可选>

要求：
1. 由你统一调度，不要让我分别联系其他 agent
2. 只保留你作为最终对外输出者
3. 最终按清晰结构整理后再发到目标群聊
```


### 七、Jarvis 后台调度专职 agent（当前最稳方案）

#### 已验证成功的闭环

该协作模式已完成一次真实验证，闭环如下：

1. 用户私聊 Jarvis 下任务
2. Jarvis 在后台调用专职 agent（如 `dev/content/ops`）
3. Jarvis 汇总各 agent 结果
4. Jarvis 将最终结果发回指定飞书群（使用群 `chat_id`）

这说明当前环境中，这套“单一对外输出者 + 后台多 agent 协作”的方式已经可以实际工作。


当前环境下，如果要让 Jarvis 统筹多个专职 agent，最稳的做法不是依赖 `sessions_send`，而是使用 OpenClaw CLI 直接调度指定 agent：

```bash
openclaw agent --agent dev --message "<任务内容>" --json
openclaw agent --agent content --message "<任务内容>" --json
openclaw agent --agent ops --message "<任务内容>" --json
```

#### 为什么这么做

因为目前高层 `sessions_list / sessions_send` 对专职 agent 的可复用会话暴露还不稳定，而：

- `openclaw agent --agent <id>` 可以直接命中目标 agent
- 会进入对应 agent 的独立 workspace
- 会使用对应 agent 的 persona / memory / rules
- 能返回结构化结果，适合 Jarvis 再汇总

#### 当前推荐实现

- 用户私聊 Jarvis 下任务
- Jarvis 在后台通过 `openclaw agent --agent <id> --message ... --json` 调用专职 agent
- Jarvis 汇总各 agent 结果
- Jarvis 再输出到目标群聊或目标会话

#### 适用场景

- 技术 / 内容 / 运营 / 法务 / 财务分工任务
- 需要明确角色边界的任务
- 需要 Jarvis 做统一汇总的任务
