> **[中文化] MemOS-Cloud-OpenClaw-Plugin**
>
> 此项目是 [MemTensor/MemOS-Cloud-OpenClaw-Plugin](https://github.com/MemTensor/MemOS-Cloud-OpenClaw-Plugin) 的镜像仓库。
> 上游已自带中文 README，本仓库在此基础上整理中文内容，代码与上游保持一致。
> - 原项目 Stars: 368
> - 主语言: JavaScript
> - 许可证: Apache-2.0
> - 整理日期: 2026-10-09
> - 上游中文 README: README_ZH.md（上游原文件，未改动）
>
> 著作权归上游作者所有。功能改动请提到上游仓库。

---
## 这是什么

这是 MemTensor 维护的官方 OpenClaw 生命周期插件。它在每次 agent 运行前从 MemOS Cloud 召回记忆，运行结束后把新消息写回 MemOS Cloud，从而给 OpenClaw 提供长期记忆。插件使用 Token 认证。

## 核心能力

- **Recall**：在 `before_prompt_build`（旧版 OpenClaw 宿主上是 `before_agent_start`）阶段调用 `/search/memory` 召回记忆。
- **Add**：在 `agent_end` 阶段调用 `/add/message` 写入新消息。
- **Config UI**：启动 gateway 时会同时起一个本地配置页，默认地址 `http://127.0.0.1:38463`，直接读写宿主配置文件。
- **Token 认证**：请求头使用 `Authorization: Token <MEMOS_API_KEY>`。
- **多 agent 支持**：通过 `agent_id` 参数支持多 agent 架构，开启后不同 agent 的记忆和消息历史互相隔离。
- **每 agent 配置**：用 `agentOverrides` 为每个 agent 单独设置知识库、召回条数、relativity 阈值等参数；未覆盖的字段继承全局配置。
- **召回过滤**：可选地先让模型对召回候选做一轮筛选，只注入返回 `keep` 的条目；失败时可回退到未过滤的召回。
- **Direct Session User ID**：会话 key 形如 `agent:main:<provider>:direct:<peer-id>` 时，可复用 `<peer-id>` 作为 MemOS 的 `user_id`。

## 快速开始

```bash
openclaw plugins install @memtensor/memos-cloud-openclaw-plugin@latest
openclaw gateway restart
```

Windows 用户若遇到 `Error: spawn EINVAL`，这是 OpenClaw 插件安装器在 Windows 上的已知问题，请改用上游 README 里的手动安装方式（从 NPM 下载 `.tgz` 后解压到本地目录，并在 `~/.openclaw/openclaw.json` 中配置 `plugins.load.paths`）。

## 与上游的差异

本仓库不做功能改动，只补充中文文档。如需提交功能改动，请提到上游仓库 [MemTensor/MemOS-Cloud-OpenClaw-Plugin](https://github.com/MemTensor/MemOS-Cloud-OpenClaw-Plugin)。

## 许可证

上游项目使用 Apache License 2.0。本仓库的代码部分沿用上游同一许可证。
