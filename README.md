# workflow-kit

A small, portable working agreement for Reasonix sessions.

Reasonix 会话可用的通用工作约定插件。

It ships one skill and one session-start reference. Nothing here is specific to
a particular machine, repository, language, or vendor: no absolute paths, no
credentials, no personal configuration. Install it and it behaves the same way
for everyone.

本插件包含一个技能和一份会话启动参考。内容不绑定任何机器、仓库、语言或厂商，
不含绝对路径、凭据或个人配置，任何人安装后行为一致。

## What it does / 它做什么

- **Session start reference** — the one-page agreement is added to the first
  user turn of every new session, so the rules are present without permanently
  occupying the cached system prompt.
- **`working-agreement` skill** — the fuller version, loaded on demand when the
  model needs the detail or when you invoke `/workflow-kit:working-agreement`.

- **会话启动参考** — 每个新会话的首轮会附带这份一页纸的约定，规则必然在场，
  同时不长期占用缓存系统提示词。
- **`working-agreement` 技能** — 完整版，按需加载，或用
  `/workflow-kit:working-agreement` 主动调用。

The rules cover investigation before editing, verification proportional to the
change, honest reporting, leaving the workspace clean, keeping consequential
decisions with the user, and small, reviewable delegation.

规则覆盖：改动前先调查、验证与改动规模匹配、如实报告、保持工作区整洁、把重要决策
留给用户、以及小而可复核的委派。

## Install / 安装

From a local checkout:

```sh
reasonix plugin install /path/to/reasonix-workflow-kit --dry-run
reasonix plugin install /path/to/reasonix-workflow-kit --yes
reasonix plugin doctor workflow-kit
reasonix plugin show workflow-kit
```

From a Git source:

```sh
reasonix plugin install git:github.com/jf200s/reasonix-workflow-kit --dry-run
reasonix plugin install git:github.com/jf200s/reasonix-workflow-kit --yes
```

If you fork this repository, substitute your own account for `jf200s`.

For local authoring, link the directory instead of copying it, then restart the
session after edits:

```sh
reasonix plugin install /path/to/reasonix-workflow-kit --link --replace --yes
```

在 Studio 中：**设置 → 扩展 → 已安装 → 添加**，选择本仓库目录，先「查看内容」
再安装。

## Use / 使用

- Start a new session. The startup reference arrives with your first prompt
  inside `<hook-context event="SessionStart">`.
- Invoke the full skill explicitly with `/workflow-kit:working-agreement`, or
  let the model select it by description.
- Skills from installed packages are also listed in `/skills`; `/plugins show
  workflow-kit` prints the contribution inventory.

## Make it yours / 按需调整

- Edit `context/startup.md` for the text every session sees; keep it short.
- Edit `skills/working-agreement/SKILL.md` for the detailed version.
- After editing a **copy** install, reinstall with `--replace`; a **link**
  install picks up edits at the next session start.
- Teams that keep shared instructions in a repository can commit equivalent
  text to that repository's own instruction files instead of installing this
  package. Both work; do not run both with conflicting rules.

## Disable and remove / 停用与卸载

```sh
reasonix plugin disable workflow-kit
reasonix plugin enable workflow-kit
reasonix plugin remove workflow-kit --yes
```

Disabling stops the startup reference in newly built sessions. Context already
sent in an existing conversation stays in its history; start a new session to
check the change.

停用后，新建会话不再注入启动参考；已发送过的上下文仍留在既有会话历史中。

## Scope / 范围

This package carries general working habits only. It intentionally does not
contain project, language, or toolchain rules — those belong in the project's
own instruction files, where they can be reviewed with the code they govern.

本包只包含通用工作习惯，刻意不含项目、语言或工具链规则；那些规则应写进项目自己的
指令文件，与它们约束的代码一起评审。

## License

MIT. See [LICENSE](LICENSE).
