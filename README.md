# AI role-playing online chat skills

基于 **CBVTM（人物言行思模式）** 的两个中文 agent skills，专门用于纯线上聊天里的 AI 角色扮演：一个负责写人物设定，一个负责把设定演下去。

> 集合原型，非指所有个体。不用于临床诊断、真实个体标签化或决定论预测。

## 两个 skill

| | character-generation | chat-evolution |
| --- | --- | --- |
| 定位 | 写设定：一句话原型 → 可直接交给 AI 扮演的 14 栏人物说明书 | 演角色：按设定在聊天窗口里持续扮演，每轮 1—3 条消息 |
| 输入 | 一句话原型，或已有的人物雏形 | 人物设定；可选读取 `cbvtm` 契约块 |
| 输出 | 14 栏正文，可选附契约块 | 只有角色发出去的字 |
| 不做什么 | 不写剧情、不写对话脚本、不负责扮演 | 不写设定、不写旁白/动作/场景、不推剧情 |

两个 skill 互相独立：`character-generation` 可以在设定末尾附一个可选的 `cbvtm` 契约块，`chat-evolution` 读到就用、读不到就整套走默认，不会要求用户补信息。契约是唯一耦合点，且是单向可选的。

## 核心特点

- **只写发得出去的字**：每条性格都落成一句话、一次沉默或一个表情，不写动作和旁白。
- **三档拒绝原话**：软拒绝 / 硬拒绝 / 特殊拒绝，各一句可以直接抄进台词的台词。
- **无知区与禁用词**：防「AI 万能」、防角色走味。
- **后台判定**：`chat-evolution` 按情境参数判断该不该拒、怎么拒，参数与判定不进入台词。
- **记账**：羞耻、焦虑、欠补偿、越界累积等状态跨轮维护，防止角色越演越像客服。

## 目录结构

```text
ai-role-playing-online-chat-skills/
├── character-generation/
│   ├── SKILL.md
│   └── references/
│       ├── contract.md
│       ├── examples.md
│       ├── quality-gate.md
│       └── template.md
├── chat-evolution/
│   ├── SKILL.md
│   └── references/
│       ├── contract.md
│       ├── judgement.md
│       ├── output.md
│       └── state.md
├── LICENSE
└── README.md
```

`references/contract.md` 在两侧各存一份，内容逐字一致。

## 安装

把需要的 skill 目录整个复制到宿主的 skills 目录，重启会话后生效。

- ZCode：`~/.zcode/skills/`（Windows：`C:\Users\<用户名>\.zcode\skills\`）
- Claude Code：`~/.claude/skills/`

```bash
git clone https://github.com/123-qingluo/ai-role-playing-online-chat-skills.git
cp -r ai-role-playing-online-chat-skills/character-generation ~/.zcode/skills/
cp -r ai-role-playing-online-chat-skills/chat-evolution ~/.zcode/skills/
```

两个 skill 都设置了 `disable-model-invocation: true`，需要显式调用，不会被模型自动触发。

## 使用

- 写设定：`用 character-generation 给我一个夜班店长的人物设定`
- 扮演：`用 chat-evolution 演这个角色，用她的话回我`
- 从设定到扮演：先用 `character-generation` 出 14 栏，再把正文（可带 `cbvtm` 契约块）交给 `chat-evolution`

## 相关 skill

需要更完整的角色建模、四层视角或轻量快速卡，见 [character-design-skills](https://github.com/123-qingluo/character-design-skills)。

## 许可

[MIT](LICENSE)

## English

Two Chinese-language agent skills for AI role-playing in pure online chat, based on **CBVTM**, a behavioral model of how a character thinks, speaks and acts. `character-generation` turns a one-line archetype into a 14-field character sheet that can be handed directly to a role-playing agent, including three tiers of refusal lines, an ignorance zone and banned phrases. `chat-evolution` performs that character in a chat window: one to three messages per turn, with background judgement and bookkeeping, optionally reading a `cbvtm` contract block produced by the generator. Install by copying a skill folder into `~/.zcode/skills/` or `~/.claude/skills/`. MIT licensed.
