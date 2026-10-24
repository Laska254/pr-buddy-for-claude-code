---
name: learn
description: Learn your review voice and draft review guidelines from your past PR/MR comments.
disable-model-invocation: true
---

# Learn

Build the user's voice profile and draft guideline candidates from their review history, collecting that history once. Shared reference lives in the plugin's `reference/` directory, two levels above this skill's base directory.

Platforms the user enabled for pr-buddy (`true` or `on` means enabled, `false` or `off` disabled, and a placeholder left unfilled means enabled): GitHub `${user_config.github}`, GitLab `${user_config.gitlab}`.

1. **Setup.** Follow the Shell and Language sections of [setup](../../reference/setup.md).
2. **Collect** the history per [history](../../reference/history.md). Done when every enabled, logged-in platform is covered and the counts are reported.
3. **Voice.** Build the profile per [voice](../../reference/voice.md). Done when the user has agreed it and it is saved.
4. **Guidelines.** Mine candidates per [mining guidelines](../../reference/guidelines-mining.md). Done when every candidate has the user's decision and the kept ones are saved.
