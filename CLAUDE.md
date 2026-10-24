# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

# pr-buddy

A Claude Code plugin. Layout, ground rules, testing and releasing are in @CONTRIBUTING.md; follow them for every change.

## Shared reference reach

A change to one file in `plugins/pr-buddy/reference/` changes several skills at once:

- `setup.md` is read whole by `review` and `respond`, but `check` follows only the sections it names (Platform, Local checkout, Shell, Guidelines, Language). Renaming a section or changing what it means breaks `check` without any error.
- `reviewing.md` is the review pass shared by `review` and `check`; each skill adds only its own rules on top.
- `review-files.md` belongs to `check` alone: the `.review/` report, the waivers file, cleanup.
- Every `gh` and `glab` call lives in `github.md` and `gitlab.md`, and skills point at an entry by its name (for example "of a branch"). A new platform call goes into both files in the same change, so GitHub and GitLab stay in step.

## Validation

The plugin is the `plugins/pr-buddy/` folder, and only that folder reaches users; this file, `CONTRIBUTING.md`, `.github/` and `.idea/` stay outside it on purpose. `claude plugin validate .` checks the marketplace and `claude plugin validate ./plugins/pr-buddy` the plugin; both pass with no warnings.

The icon is the 512 px PNG next to `plugin.json`, rendered from the SVG beside it; the validator accepts only the PNG, the SVG is the source. The directory holds a version for review when a bundled image's path appears in backticks or a code block, so the plugin's README shows the icon with Markdown image syntax only. The maintainer's portfolio shows a copy of the same PNG, so a change to the icon starts from the SVG and the new PNG goes to both places.

CI also checks the relative Markdown links between skills and references offline. Renaming or moving a file in `plugins/pr-buddy/reference/` means updating every link to it in the same change, or CI fails.

## Trying `check` here

`/pr-buddy:check` run in this repository reviews the plugin's own uncommitted changes and writes `.review/`. On `main` it reviews only uncommitted changes, and `.review/branch-main.md` stays until deleted by hand. `.review/` must be ignored by git; the first run hands over the command.
