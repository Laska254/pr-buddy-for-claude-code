---
name: learn-voice
description: Learn or rebuild your review voice profile from your past PR/MR comments.
disable-model-invocation: true
---

# Learn voice

Build the user's voice profile from their review history. Shared reference lives in the plugin's `reference/` directory, two levels above this skill's base directory.

Platforms the user enabled for pr-buddy: GitHub `${user_config.github}`, GitLab `${user_config.gitlab}`.

1. **Setup.** Follow the Shell and Language sections of [setup](../../reference/setup.md).
2. **Collect** the history per [history](../../reference/history.md). Done when every enabled, logged-in platform is covered and the counts are reported.
3. **Voice.** Build the profile per [voice](../../reference/voice.md). Done when the user has agreed it and it is saved.
