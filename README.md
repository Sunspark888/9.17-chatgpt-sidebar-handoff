# ChatGPT 侧栏交接 Skill

用于把文字或本地文件交接到指定的现有 ChatGPT 对话，并区分“本地就绪、已附加、已发送、实际可读”。整理日期：2026-09-17。

## 内容

- [Skill 入口](chatgpt-sidebar-handoff/SKILL.md)：目标定位、文字直发、附件上传、超时恢复、送达验收。
- [事故与恢复记录](chatgpt-sidebar-handoff/references/incident-patterns.md)：经过实际交接观察或主任务报告的证据，保留未知原因。
- [界面元数据](chatgpt-sidebar-handoff/agents/openai.yaml)。

## 使用

将 `chatgpt-sidebar-handoff` 文件夹放进本机 Codex 的 skills 目录，指定目标对话、待交接内容和验收要求后调用。它依赖当前 Codex 环境提供的线程工具及浏览器控制能力，不是独立浏览器扩展，也不是绕过登录的上传 API。

保留现有授权边界；超时不等于未发送，单个旧标签或滞后的历史读取也不等于没有回复。不能由本 Skill 的校验通过推断浏览器扩展已经修复。历史耗时仅是当时观测，不是速度保证。

## 维护与同步

本机维护源为 `C:\Users\HP\.codex\skills\chatgpt-sidebar-handoff`。用户要求：以后修改该 Skill 时，同时更新本仓库。流程是修改、UTF-8 校验、检查远端变化、同步这三个 Skill 文件及相关说明、提交推送并核实远端。同步失败必须明确报告，不声称已经上传。

这不是后台自动监控；手工在其他地方修改文件不会自行推送。仓库初始设为私有，未包含论文附件、完整聊天记录或凭据。换机器使用时需调整 Skill 中本机路径；不要修改无关配置。
