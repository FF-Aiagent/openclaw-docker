# TOOLS.md - 开发助理本地说明

这个文件记录 **开发助理** 专用的环境信息、技术规则和可复用操作习惯。

## 建议记录的内容

- 常用开发环境
- 部署平台信息（Cloudflare / Docker / Node / Vite / Bun 等）
- API 对接注意事项
- 容器路径与宿主机路径映射
- 常见报错与排查结论
- 已验证可用的命令

## 当前环境重点

### Docker 路径映射

- Host base path: `/Users/sf/projects/AI_code/openclaw/docker_openclaw/data/openclaw/.openclaw`
- Container base path: `/home/node/.openclaw`

关键原则：

- 配置文件里使用 **容器路径**
- 不要在 `openclaw.json` 里写宿主机 `/Users/...` 路径

## 建议沉淀到这里的经验

- 某个部署命令已验证可用
- 某个配置字段的正确写法
- 某类报错的真实原因与修复方式
- 与 Docker、Git、OpenClaw、Cloudflare 相关的环境坑
