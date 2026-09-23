# Voice Wake Gate

An agent skill for designing and reviewing consent-based local voice enrollment, speaker verification, and optional wake-phrase gating.

This repository contains **instructions for a coding agent**. It does not contain a working voice recognition engine, microphone service, pretrained model, anti-spoofing detector, or liveness detector. Installing the skill does not enable voice authentication in ChatGPT, Codex, or an operating system.

## Why it helps in shared spaces

In a studio where several people talk near one computer, useful voice control needs more than speech-to-text. The skill guides an agent to design a workflow that distinguishes the authorized operator's commands from colleagues' discussions and applies each person's configured permissions. It keeps three decisions separate:

1. **Identity:** whose voice is supported by the current segment's evidence?
2. **Command intent:** is this person explicitly addressing the application through its defined command route or interaction mode?
3. **Permission:** is that person allowed to perform this particular action?

A recognized person's ordinary conversation must not become a command. Roles and permissions come from the application's configured rules; a name, familiar voice, or speaker change must not grant extra privileges. A wake phrase remains optional and does not replace these decisions.

## Suitable scenarios

| Setting | Intended workflow |
|---|---|
| Shared creative studio | During editing or design collaboration, route explicitly addressed commands from the currently authorized operator while keeping nearby discussions out of the command channel. |
| Meetings and demonstrations | Let an authorized presenter request slide or presentation changes through the command mode; audience questions and comments remain conversation. |
| Shared household computer | Keep separate enrolled identities and permissions for each person; an unregistered visitor stays unknown and never inherits the owner's access. |
| Filming or editing with hands occupied | Provide authorized voice assistance for tasks such as playback control or marking a clip, within a defined command mode and the person's permissions. |

These are implementation goals for a local/API client whose capture, playback, and action routing you control. The repository does not ship those behaviors, and the examples do not establish safe operation for safety-critical automation.

## What the skill covers

- Explicit consent before collecting enrollment audio; separate protected templates and permissions for each enrolled person.
- Unknown-speaker rejection, evidence bound to each audio segment and transcript, source continuity, and optional wake phrases.
- Replay, synthetic speech, playback, mixed speakers, and missing evidence as implementation and testing concerns.
- Separate command authorization: a speaker match alone must not authorize high-risk actions.
- Integration boundaries: a local sidecar cannot claim to control the official ChatGPT/Codex voice frontend without a verified, supported integration point.

Microphone provenance alone does not establish liveness. The skill cannot guarantee detection of synthetic or replayed speech. An implementation should reject automatic authentication when its required source or anti-spoofing evidence cannot be verified.

## Install and use

1. Download this repository or clone it:

   ```sh
   git clone https://github.com/goldhand8866-svg/voice-wake-gate.git
   ```

2. Put the repository contents in a folder named `voice-wake-gate` inside your agent host's configured skills directory. For a Codex setup using `CODEX_HOME`, this is `CODEX_HOME/skills/voice-wake-gate`; the default is `~/.codex/skills/voice-wake-gate`. Preserve any existing local version before replacing it.
3. Start a new agent session if your host requires it to discover newly installed skills. Invoke the skill explicitly, for example:

   ```text
   Use $voice-wake-gate to review my local assistant's enrollment and wake flow.
   Identify which components control capture and playback, what evidence is
   needed for a speaker decision, and which tests remain unverified.
   Do not collect audio, download models, or change my application yet.
   ```

For implementation work, supply the authorized codebase and the capture, model, playback, and execution interfaces available in your application. The skill does not install those components or grant permission to collect or upload anyone's voice.

## Release scope

The public package consists of [SKILL.md](SKILL.md), this README, and [LICENSE](LICENSE). It includes no recordings, voiceprints, consent records, credentials, model weights, executable engine, or runtime test results. No live-environment, replay-resistance, recognition-accuracy, or security result is claimed for this release.

The MIT license covers the skill text and documentation in this repository. It does not grant rights to third-party models, code, services, datasets, or biometric data. Check those components separately when integrating them.

## 中文说明

这是给编程智能体使用的**声纹注册、说话人验证与唤醒流程指导技能**。它帮助整理本人同意采集、每人独立模板、逐段证据绑定、未知身份拒绝、回放与合成语音风险，以及命令授权的独立边界。

### 多人环境中的用途

多人工作室里，电脑周围的声音既可能是操作者的指令，也可能是同事之间的讨论。这项技能指导智能体设计一个按人分清指令来源、按个人权限处理操作的流程，分别判断三件事：**谁在说话、是否明确向应用发出指令、这个人是否有权执行该操作**。

即使身份匹配，也不能把这个人的普通聊天当成命令。产品需要定义明确的指令通道或交互模式；权限来自预先配置的个人或角色规则，不能凭姓名、熟悉的声音或说话人切换自动升权。唤醒词仍是可选功能，不能代替身份、意图和权限判断。

### 适合的场景

| 场景 | 希望实现的使用方式 |
|---|---|
| 多人创作工作室 | 剪辑、设计协作时，区分当前获授权操作者向应用发出的指令与旁人的讨论，避免将讨论内容送入执行通道。 |
| 会议与演示 | 主持人或演示者在指令模式下控制投屏内容、翻页；旁听者的提问和发言作为对话处理，不触发操作。 |
| 家庭共用电脑 | 每人有独立注册身份与权限；未注册访客保持未知身份，不冒用或继承主人的权限。 |
| 拍摄或剪辑时双手占用 | 为获授权者提供播放控制、片段标记等语音操作辅助，仍遵守明确的指令模式和个人权限。 |

这些是技能帮助设计和检查的实现目标，需要接入自己能够控制录音、播放与操作通道的本地／API 客户端；不是本仓库已经部署的功能，也不构成安全关键自动化的效果承诺。

本仓库没有声纹识别引擎、模型权重、录音服务、反伪造或活体检测器。安装技能不会直接开启 ChatGPT、Codex 或系统的声纹认证，也不表示任何真实环境、回放防护、识别精度或安全测试已经通过。无法验证必要来源或反伪造证据时，应拒绝自动认证；声纹匹配不能单独授权高风险操作。

安装时，将本仓库内容放入智能体宿主的技能目录，文件夹命名为 `voice-wake-gate`。保留已有版本后再替换；按宿主需要新开会话，然后输入：

```text
使用 $voice-wake-gate 检查我的本地助手声纹注册与唤醒流程。
先说明麦克风和播放控制边界、逐段身份依据及尚未验证的项目。
本次不要采集录音、下载模型或修改应用。
```

若要实际实现，需要另行接入经授权的录音、声纹模型、来源与反伪造检查、播放控制和命令授权组件。本地旁路程序不能仅凭读取录音或转写，就声称能阻止官方 ChatGPT/Codex 语音前端先行回应。

MIT 许可只覆盖本仓库技能文本与说明，不附带任何第三方模型、服务、数据或个人生物识别资料的使用权。
