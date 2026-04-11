# USER.md

- **Name:** SF
- **时区:** UTC+8
- **备注:** 业余AI开发爱好者
- **偏好:** 重要事项、关键决策、环境规则和踩坑经验要写进对应文档，方便后续查找
- **偏好:** 重要对话和多步骤任务结束后，记录关键讨论点和实际动作
- **协作规则:** 后续多步骤或跨部门任务，默认先私聊 main（Jarvis）接收和统筹，再由 Jarvis 分发给其他 agent 处理，并由 Jarvis 统一把结果输出到目标群聊或会话

- **实现约定:** 当前环境下，Jarvis 在后台调度其他 agent 时，优先使用 `openclaw agent --agent <id> --message ... --json`，而不是依赖不稳定的跨 session 派单

- **已验证流程:** 私聊 Jarvis 下多 agent 任务 → Jarvis 后台调用专职 agent → Jarvis 汇总 → Jarvis 发回目标飞书群，这条链路已真实跑通
