# Voice Wake Gate

An agent skill for designing and reviewing consent-based local voice enrollment, speaker verification, and optional wake-phrase gating.

This repository contains **instructions for a coding agent**. It does not contain a working voice recognition engine, microphone service, pretrained model, anti-spoofing detector, or liveness detector. Installing the skill does not enable voice authentication in ChatGPT, Codex, or an operating system.

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

本仓库没有声纹识别引擎、模型权重、录音服务、反伪造或活体检测器。安装技能不会直接开启 ChatGPT、Codex 或系统的声纹认证，也不表示任何真实环境、回放防护、识别精度或安全测试已经通过。无法验证必要来源或反伪造证据时，应拒绝自动认证；声纹匹配不能单独授权高风险操作。

安装时，将本仓库内容放入智能体宿主的技能目录，文件夹命名为 `voice-wake-gate`。保留已有版本后再替换；按宿主需要新开会话，然后输入：

```text
使用 $voice-wake-gate 检查我的本地助手声纹注册与唤醒流程。
先说明麦克风和播放控制边界、逐段身份依据及尚未验证的项目。
本次不要采集录音、下载模型或修改应用。
```

若要实际实现，需要另行接入经授权的录音、声纹模型、来源与反伪造检查、播放控制和命令授权组件。本地旁路程序不能仅凭读取录音或转写，就声称能阻止官方 ChatGPT/Codex 语音前端先行回应。

MIT 许可只覆盖本仓库技能文本与说明，不附带任何第三方模型、服务、数据或个人生物识别资料的使用权。
