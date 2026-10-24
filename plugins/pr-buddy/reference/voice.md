# Voice

The voice profile at `~/.claude/pr-buddy/voice.md` describes how the user writes review comments and replies. It lives only on the user's machine, outside the plugin repository, because its samples may quote private repositories.

## Build

1. **Take** every comment collected by [history](history.md), both roles.
2. **Distill** the profile: tone, typical length, how requests are phrased (question, suggestion, imperative), how disagreement and praise are phrased, humor and emoji, how code suggestions are written, recurring phrases. Close with 10-15 short verbatim samples covering those traits, with code identifiers from private repositories stripped out.
3. **Agree** the profile with the user. When a profile already exists, show what would change against it; the user may have edited it by hand, and those edits stay unless they say otherwise. Save once agreed. When fewer than 20 comments turned up, say so: the profile is thin and worth rebuilding later.

## Write

Match the profile in every posted text: length, register, phrasing. A comment the user would have to rewrite to sound like themselves is a draft that failed.
