# 2026-06-14: Commit/Push Approval Gate

## Summary

Added a rule that commit and push require one combined user confirmation after reviewing changes.

## Why

The user wants to review changes before they become Git history or are sent to GitHub, but does not want separate commit and push confirmations.

## Rule

Before commit and push:

- show changed files
- summarize behavior changes
- show verification status
- propose a Korean commit message
- show target remote and branch
- ask once whether to commit and push

After approval, run commit and push as one continuous Git publishing step.
