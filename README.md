# Remotion Product Promo Skill

把产品宣传片做成可理解的故事，而不是功能清单。这个 Agent Skill 会先确认横屏或竖屏，核对品牌素材，让封面和后续产品画面保持叙事与视觉上的联系，再制作真实产品演示、音画同步和最终合成检查。

技能遵循 [Agent Skills 开放格式](https://agentskills.io/specification)。核心文件位于 [`skills/remotion-product-promo/SKILL.md`](skills/remotion-product-promo/SKILL.md)，案例和实现细节位于 [`references/production-patterns.md`](skills/remotion-product-promo/references/production-patterns.md)。随附白色光标 SVG、轻点击、柔和页面切换和分享 whoosh 音效。**不包含背景音乐或 Roadbook 项目源码**；制作新片时应使用该产品自己的真实界面和有使用权的音乐。

## 一行安装

需要已安装 Node.js 和 npm；当前 `skills` CLI 声明需要 Node.js 22.20.0 或更新版本。在任意终端执行以下命令，把技能安装到当前用户的 Cursor、Claude Code 和 Codex：

```bash
npx skills add Hxd123459/remotion-product-promo-skill --skill remotion-product-promo -g -a cursor -a claude-code -a codex --copy -y
```

`-g` 表示所有项目可用，`--copy` 使用独立文件副本，在 Windows 上也不依赖符号链接权限。只用一种工具时，保留相应的 `-a cursor`、`-a claude-code` 或 `-a codex` 即可。在项目根目录去掉 `-g`，则只安装到该项目。

安装命令依赖 [Vercel 的 skills CLI](https://github.com/vercel-labs/skills)。首次执行 `npx` 会下载这个 CLI；它从本仓库读取技能文件。若无法访问 GitHub 或 npm，先解决网络连接，或按下文手动复制。

## 验证、使用、更新

```bash
npx skills list -g
```

列表里应出现 `remotion-product-promo`。技能内容不会自动生成视频；使用时仍需让代理访问产品代码、素材和 Remotion 项目。

| 环境 | 使用方式 | 常见安装位置 |
| --- | --- | --- |
| Cursor Agent | 输入 `/remotion-product-promo`，或描述产品宣传片任务让 Agent 选择技能 | `~/.cursor/skills/remotion-product-promo/` |
| Claude Code | 输入 `/remotion-product-promo`，或直接描述任务 | `~/.claude/skills/remotion-product-promo/` |
| Codex | 输入 `$remotion-product-promo`，或直接描述任务 | `~/.codex/skills/remotion-product-promo/` |

可以这样下达任务：

> 使用 remotion-product-promo。先让我选择横屏、竖屏或双画幅。阅读这个产品的官网演示和实际操作代码，核对正式 Logo 与界面，再设计封面、开场和首个产品镜头的连续画面。列出节拍表后制作宣传片；每次点击都检查光标、按钮反馈和音效。保留现有已确认的视频版本。

更新和移除用户级安装：

```bash
npx skills update remotion-product-promo -g -y
npx skills remove remotion-product-promo -g -a cursor -a claude-code -a codex -y
```

如果代理没有看到技能，重新打开会话，然后检查 `npx skills list -g`、安装目录和 `SKILL.md` 的 YAML 头。项目级安装要在项目根目录运行 `npx skills list`。

## DeepSeek 怎么用

DeepSeek 是模型，不是统一的本地技能安装目标。**在 Cursor 或 Claude Code 等支持 Agent Skills 的工具中选择 DeepSeek 模型时**，按该工具对应的 `-a` 参数安装，本技能照常由工具读取。DeepSeek 的[官方编程代理接入说明](https://api-docs.deepseek.com/guides/coding_agents/)列出了可使用 DeepSeek 模型的工具。

**DeepSeek 网页聊天或直接调用 DeepSeek API**不会自动扫描电脑上的 `SKILL.md`。网页聊天可以把 [`docs/deepseek-chat-prompt.md`](docs/deepseek-chat-prompt.md) 复制为对话开头，用来规划脚本和镜头；但没有文件系统、终端和 Remotion 环境时，它无法替你渲染视频。自行开发 API 代理时，需要由你的程序读取 `SKILL.md` 和按需读取 `references/`，再把内容提供给模型；音效和 SVG 仍需复制进视频项目。

## 手动安装和素材

把整个 `skills/remotion-product-promo` 文件夹复制到对应目录，不要只复制 `SKILL.md`，否则参考文档和音效链接会失效：

```text
~/.cursor/skills/remotion-product-promo/
~/.claude/skills/remotion-product-promo/
~/.codex/skills/remotion-product-promo/
```

跨平台的简洁做法是将技能放在项目 `.agents/skills/remotion-product-promo/`；Cursor 和 Codex 均可发现该路径。Claude Code 使用 `.claude/skills/` 最直接。CLI 会替你处理不同工具的目录。

素材位于 [`assets/`](skills/remotion-product-promo/assets/)。按需要复制进 Remotion 项目的 `public/`，通过 `staticFile()` 引用。音效是可调整的起点，不能替代新项目的混音试听；不要把某支片子的音量数值直接复用。可复用光标的点击热点和脉冲节奏见案例文档。

## 维护者：部署与发布

本仓库的 `main` 分支是公开分发源。修改技能后，先检查 `SKILL.md`、参考文件和素材，再在干净目录里运行安装测试。之后提交并推送：

```bash
git add README.md LICENSE skills docs
git commit -m "Improve product promo skill"
git push origin main
```

发布后检查 [GitHub 技能目录](https://github.com/Hxd123459/remotion-product-promo-skill/tree/main/skills/remotion-product-promo)，再运行：

```bash
npx skills add Hxd123459/remotion-product-promo-skill --list
```

应列出 `remotion-product-promo`。重要版本可以建立 `v1.0.0` 等 Git 标签以方便回溯；普通用户的一行安装命令读取当前默认分支。若修改了技能名称或仓库地址，需要同步修改本页安装、更新和验证命令。不要把受限许可的背景音乐、真实账户数据或产品密钥放入公开仓库。

## 依据

- [Agent Skills 格式规范](https://agentskills.io/specification)
- [skills CLI 安装与更新](https://github.com/vercel-labs/skills)
- [Cursor Agent Skills](https://prod.cursor.com/docs/skills)
- [Claude Code Skills](https://code.claude.com/docs/en/skills)
- [DeepSeek 编程代理接入](https://api-docs.deepseek.com/guides/coding_agents/)

本仓库以 [MIT 许可证](LICENSE)发布。
