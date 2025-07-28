# **UnrealEngine Update Tracker**

[English version](README.en.md)
[日本語版](README.jp.md)

本项目是一个自动化服务，定期监控虚幻引擎私有 GitHub 仓库的更新，使用 AI（Google Gemini）总结关键变更（如新功能、规格变动等），并将报告发布到 GitHub Discussions。

<table><tr><td>
<img width="644" alt="报告示例" src="https://github.com/pafuhana1213/Screenshot/blob/master/Report_sample_jp.png" />
</td></tr></table>

> 注意：此图片为报告示例，内容完全为虚拟数据，并非虚幻引擎的真实更新内容。

## 🌟 核心功能

-   **自动更新检查:** 通过 GitHub Actions 按计划（每日北京时间早8点 / UTC 23:00）或手动检查 UE 仓库的最新提交。
-   **AI智能摘要:** 使用 Gemini API 分析提交内容，按「新功能」「规格变更」等类别分类并总结。
-   **讨论区发布:** 将生成的报告以「虚幻引擎每日报告」形式发布到仓库的 GitHub Discussions。
-   **Slack通知:** 同时将报告内容推送到指定 Slack 频道。
-   **Discord通知:** 同时将报告内容推送到指定 Discord 频道。

## 🚀 订阅最新报告

无需自行部署工具，即可订阅更新报告。以下仓库每日定时在 GitHub Discussions 发布生成报告：  
[**订阅 UnrealEngine-UpdateTrackerReport 仓库**](https://github.com/pafuhana1213/UnrealEngine-UpdateTrackerReport)

> **重要:** 此报告仓库为私有仓库，查看需具备 [访问虚幻引擎源代码仓库权限的 GitHub 账号](https://www.unrealengine.com/ja/ue-on-github)。

## ✨ 支持项目发展

若此工具对您的 UE 日常跟进有所帮助，我将深感荣幸！  
本项目由个人开发者利用业余时间开发，咖啡费和 API 使用成本均为自费承担 ☕  
如果您认为「这个工具很棒！」，欢迎通过 GitHub Sponsors 支持，这将成为开发的巨大动力：  
[💖 **通过 GitHub Sponsors 支持**](https://github.com/sponsors/pafuhana1213)

---

**以下是为想要自定义部署本工具的用户提供的文档。**

## 🛠️ 部署指南

1.  **复刻仓库:**  
    点击右上角 **Fork** 按钮，将仓库复制到您的 GitHub 账户。

2.  **配置基础密钥:**  
    在仓库的 `Settings` > `Secrets and variables` > `Actions` 中添加必需密钥：
    -   `UE_REPO_PAT`: 拥有虚幻引擎私有仓库 (`EpicGames/UnrealEngine`) 读取权限的 [Personal Access Token (PAT)](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)。
    -   `GEMINI_API_KEY`: 从 [Google AI Studio](https://aistudio.google.com/app/apikey) 获取的 API 密钥。

3.  **配置通知目标 (至少需设置一项):**  
    #### A) GitHub Discussion 发布设置  
    适合团队讨论和长期存档。
    1.  **启用 Discussions:** 在目标仓库的 `Settings` > `General` > `Features` 启用 Discussions。
    2.  **创建分类:** 在 Discussions 标签页创建分类（如 `Announcements`）。
    3.  **添加密钥:**  
        -   `DISCUSSION_REPO`: 目标**私有**仓库名（例: `MyOrg/MyTeamRepo`）。
        -   `DISCUSSION_REPO_PAT`: 拥有 `DISCUSSION_REPO` 写入权限的 PAT。

    #### B) Slack 通知设置  
    适合实时通知与快速共享。
    1.  **创建 Incoming Webhook:** 按 [Slack 文档](https://slack.com/intl/ja-jp/help/articles/115005265063-Slack-%E3%81%A7%E3%81%AE-Incoming-Webhook-%E3%81%AE%E5%88%A9%E7%94%A8)生成 Webhook URL。
    2.  **添加密钥:**  
        -   `SLACK_WEBHOOK_URL`: 生成的 Webhook URL。
        -   `SLACK_CHANNEL`: 通知频道名（例: `#ue-updates`）。

    #### C) Discord 通知设置  
    类似 Slack，适合实时通知。
    1.  **创建 Webhook:** 按 [Discord 文档](https://support.discord.com/hc/ja/articles/228383668-%E3%82%B5%E3%83%BC%E3%83%90%E3%83%BC%E3%81%A7Webhooks%E3%82%92%E4%BD%BF%E3%81%86%E3%81%AB%E3%81%AF)生成 Webhook URL（URL 本身已包含频道信息，无需额外指定）。
    2.  **添加密钥:**  
        -   `DISCORD_WEBHOOK_URL`: 生成的 Webhook URL。

> **⚠️ 安全运营准则**  
> 虚幻引擎更新内容受 Epic Games 许可协议保护，仅限授权账号访问。  
> **为防止信息泄露，未配置至少一个通知目标时工具将停止运行。**  
> **强烈建议:** 报告发布目标（`DISCUSSION_REPO` 或 Slack 频道）需设置为 **仅限拥有 UE 源代码访问权限的成员可访问的私有空间**（如 UE 仓库复刻库），以确保合规。

## 🏃‍♀️ 运行方式

-   **自动运行:** 按计划自动执行（默认每日北京时间早8点 / UTC 23:00）。
-   **手动运行:** 在仓库的 `Actions` 标签页选择 `Unreal Engine Update Tracker` 工作流，点击 `Run workflow`（**仅管理员可操作**）：  
    -   **Report Language:** 报告语言（例: `Japanese`, `English`），默认 `Japanese`。
    -   **Commit Scan Limit:** 扫描的最近提交数（默认：过去24小时）。
    -   **Discussion Category:** Discussion 分类名，默认 `Daily Reports`。
    -   **Gemini Model:** 使用的 AI 模型，默认 `gemini-2.5-pro`。
    -   **Slack/Discord Webhook URL:** 临时覆盖密钥值。
    -   **Slack Channel:** 临时覆盖频道名。

-   **修改默认值:**  
    在 `Settings` > `Secrets and variables` > `Actions` > `Variables` 设置以下变量：
    -   `REPORT_LANGUAGE`: 默认报告语言（例: `English`）
    -   `DISCUSSION_CATEGORY`: 默认分类名（例: `Daily Reports`）
    -   `GEMINI_MODEL`: 默认 AI 模型（例: `gemini-2.5-pro`）
    -   `UE_BRANCH`: 监控的分支（例: `release`），默认为 `ue5-main`。

## 🎨 自定义配置

### 修改报告格式
报告分类、摘要风格等由 AI 提示词（prompt）控制。  
如需调整格式，请直接编辑根目录的 `prompts/report_prompt.md` 文件，无需修改 Python 代码。

## 📝 许可协议与重要声明

**使用前请务必阅读：**

-   **用户责任:** 本工具设计符合 UE 许可协议，但最终责任由用户承担。**必须将 `DISCUSSION_REPO` 设置为访问受限的私有仓库**，公开发布可能导致违约。
-   **API 密钥与费用:**  
    -   使用 Google Gemini API 可能产生费用。  
    -   **复刻本仓库的用户需自行承担 API 费用。**  
    -   **强烈建议使用禁止 AI 学习数据的 API 密钥以符合 UE 条款。**
-   **安全设计:**  
    -   为降低合规风险，**工具不会向 AI 发送 UE 源代码或差异文件**，仅分析提交消息和文件路径。
-   **运行注意:**  
    -   本脚本会实际发布内容，测试时请谨慎。  
    -   各 API 存在使用限制（速率限制）。
