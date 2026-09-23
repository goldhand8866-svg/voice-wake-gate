---
name: voice-wake-gate
description: Build, audit, or package a local voiceprint plus wake-phrase gate and consent-based voice enrollment. Use for speaker-confirmed local assistant wakeup, silent rejection, or reusable enrollment workflows; not for ordinary transcription alone.
---

# Voice Wake Gate

Use this skill to guide implementation or review of single-person or shared-device speaker verification and per-turn identity routing. A wake phrase is optional and configurable; it is not the identity check. This package contains agent instructions, not a recognition engine, microphone service, anti-spoofing detector, or liveness detector. Fail-closed routing is a requirement for an implementation to verify, not a property established by installing this skill.

Define which capture sources and evidence producers the integration can trust. Speaker similarity alone does not establish liveness or exclude replay and synthetic speech. No instruction in this skill guarantees that all spoofed audio can be detected. If required source authenticity or anti-spoofing evidence cannot be verified, reject automatic authentication and state the integration limit.

## Establish the controllable boundary first

Classify the target before changing code.

- **Self-owned local/API realtime client:** the client owns the microphone stream and response request. It can gate audio before it is sent to the conversation model.
- **Official ChatGPT/Codex realtime frontend:** do not claim a local script controls, mutes, cancels, or pre-filters it unless a documented supported integration point has been verified. A sidecar that sees a later recording or transcript can protect local archive/execution routes only; it cannot stop the official frontend from responding first.
- **Local archive or execution route:** consume a prior, local identity attestation and keep command authorization separate from wake eligibility.

Use a single microphone producer. Prefer a read-only branch from an existing shared capture path; do not open a second capture process, steal focus, foreground the host app, or degrade an active conversation. If a pre-response branch is unavailable, stop at an offline/sidecar implementation and state that limit.

## Independent people, identity and permissions

Keep a stable person identifier, display name, protected template, consent/enrollment state and permissions for each person. A preferred-name change changes the display name only. Do not overwrite one person's template to enroll another, reuse one template under two identities, or grant a newly recognized person the owner's permissions.

Support single-person mode (only its selected enrolled person) and shared mode (all enabled, independently enrolled people). Keep an open-set unknown result even when only two people are expected. Removing assistant playback and excluding one person does NOT prove the remainder is the other person. Recordings with overlapping voices require target-speaker separation and source-bound validation; unclear residual audio stays unassigned.

Check identity, command intent, and permission separately. A recognized person's ordinary conversation is not a command: require the app's explicit command route or interaction mode. Use configured permissions; never grant a role from a name or voice.

## Per-turn state machine

Start in `closed`.

1. In `closed`, require an unambiguous match to an enabled enrolled person, verified microphone-source provenance, and the source-authenticity, anti-spoofing and speaker-separation checks required by the deployment. Reject missing checks, uncertain results, detected replay/video/playback contamination, and unseparated multi-speaker sources. A real microphone can capture a replay; microphone provenance alone is not liveness evidence. Only when `wakePhraseRequired` is enabled, also require the configured wake phrase. A plain transcript is never a voiceprint result.
2. On success, enter a bounded session bound to that person and source stream. Greet by the verified display name on first recognition or a verified speaker change, then converse without repeated greetings. Every later segment must receive its own fresh identity evidence bound to that segment's audio and command transcript. Never reuse the first person's identity for later speakers. Reauthenticate on a speaker change and do not carry over the earlier person's permissions.
3. Immediately close and silently reject on timeout, source discontinuity, identity mismatch/uncertainty, replay/video/playback signal, new speaker/mixing signal, missing evidence, or explicit end. Do not announce which condition failed.
4. In every state, keep `executionAllowed=false` until a separately designed command-authorization gate has passed. Wake success is not liveness, consent for a high-risk operation, Windows authentication, or permission to execute. Do not use voice verification alone to authorize high-risk actions; require the separate confirmation or authentication appropriate to the action.

Treat unknown as reject. The implementation should silently reject unauthorized people, detected television/video, acoustic playback or synthetic/replayed audio, unseparated overlap, and missing or inconsistent evidence. These are acceptance requirements to test; they are not a claim that this skill detects those inputs. A missing wake phrase is a rejection only when that optional feature is enabled and a new session is being opened. Stop output when the user speaks; do not claim frontend playback cancellation unless it is implemented and verified at a controllable playback boundary.

## Evidence contract

Require a local attestation with enrolled-person identity and protected voiceprint decision; source kind, real-microphone/non-simulated flags, source-session identifier and replay/video status; single-speaker or target-separation decision; phrase transcript only for phrase comparison; evidence hash, timestamp and test-fixture marker. Bind the attestation to the exact audio segment and its command transcript. Protect the evidence producer and integrity of the attestation; a hash or self-declared source flag alone does not authenticate a source or prove liveness.

Do not infer identity from ASR text, wake phrase, language, account name, thread title, model score alone, or a copied recording. Redact transcript and audio path from routine receipts. Fixtures may test `wouldAllow` but must never activate a production session, write personal data, create a template, or execute a command.

## Transparent enrollment without a second reading

For a new user, briefly explain at the beginning of an eligible voice conversation that the system can collect qualifying natural turns from this conversation to build a local voice template, only with explicit consent and no upload/sharing. Do not start raw-audio enrollment collection before consent.

After consent, use the same existing microphone stream while normal conversation continues. Quarantine each natural segment and accept it for template construction only after configured quality, provenance, de-duplication, single-speaker, and contamination checks pass. Never count text-only transcription, synthetic fixtures, imported clips, TV/video, replay, another person, unknown sources, or a mixed segment without verified target-speaker extraction.

Build a protected local template only after the documented quantity/diversity/quality threshold and user acceptance. Report the actual state precisely: collecting, quality-ready, template-created, or rejected. A template is limited to the declared local feature; it is not a universal identity record and does not authorize commands. On consent withdrawal, stop collection and use the product's recoverable deletion procedure.

## Testing and release checks

Before declaring a product route complete, cover single/shared mode, two independently enrolled people, unregistered/disabled people, ambiguous matches, first recognition, continued same-person turns, speaker changes, per-segment source binding, optional wake on/off, TV/video/replay, unknown provenance, overlapping speakers, text-only input, no-consent/duplicate/poor-quality enrollment, timeout/explicit-close/source-discontinuity and non-interference with microphone/window/process state. Verify user speech interrupts output at the actual controllable host. A diagnostic greeting preview must not be spoken or reported as verified recognition.

Run synthetic fixtures and consented real-environment trials separately. Record gaps rather than lowering thresholds or claiming a digital proxy is a live/replay acceptance result. Publishing this instruction package does not establish that any live, replay, anti-spoofing, accuracy, or security test has passed.

## Internal and distribution boundary

A distributable skill may contain generic workflow logic, generic interface contracts, synthetic tests, and installer instructions when needed. Do not include a person's audio, voiceprint, encrypted template, transcript, consent record, session ID, evidence receipt, private name or profile, absolute local path, credential, API key, or model weight. This release contains only instructions and documentation.

Audit code, checkpoint, training-data, hosted-service, jurisdictional consent/retention and branding licenses separately before commercial release. Do not let an open-source code license stand in for model or biometric-data rights.
