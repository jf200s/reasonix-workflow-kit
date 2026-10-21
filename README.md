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
decisions with the user, and delegation that names the engine's real surface
(`task`, `fleet`, `read_only_task`, `write_paths`).

规则覆盖：改动前先调查、验证与改动规模匹配、如实报告、保持工作区整洁、把重要决策
留给用户，以及用引擎真实词汇描述委派（`task`、`fleet`、`read_only_task`、
`write_paths`）。

## Scoped to one agent, by design / 有意只作用于主代理

A `SessionStart` reference reaches the session's agent. In Reasonix a subagent
starts from its task prompt plus the host's own framing around it — a workspace
note, a completion contract, and the `depends_on` answers in a `fleet`. What it
never receives is this startup block, the project's instruction files, or the
parent's conversation. That is deliberate in the host, and no plugin can change
it.

Therefore the delegation rules above constrain the **parent**. If you need a
subagent to follow a rule, write that rule into the prompt you hand it — that is
the only channel. The skill states this requirement rather than assuming
inheritance.

`SessionStart` 参考只送达会话主代理。Reasonix 的子代理只从任务提示词起步，拿不到
这段启动文本、项目指令文件或父代理对话——这是宿主的设计，插件无法改变。

所以上面的委派规则约束的是**主代理**。要让子代理遵守某条规则，必须把它写进你交给
子代理的提示词里，那是唯一通道。技能里因此明说了这一点，而不是假定它会继承。

## Requires / 依赖

Written against the delegation surface of Reasonix (`task`, `fleet`,
`read_only_task`, `write_paths`, `complete_subtask`). Other agents and older
Reasonix builds still get the general sections; only the delegation section
names tools they may not have.

按 Reasonix 的委派接口编写（`task`、`fleet`、`read_only_task`、`write_paths`、
`complete_subtask`）。其他 agent 或较旧的 Reasonix 版本仍可使用通用章节，只有
委派一节会提到它们可能没有的工具。

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
