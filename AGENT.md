# AGENT.md

This repository is an AI development harness. Agents working here must operate from explicit context, selected skills, and recorded decisions.

## Operating loop

1. Identify the project profile, if any.
2. Classify the request type.
3. Select the smallest relevant skill set.
4. Read the required context files listed by the project profile and skill.
5. Execute the workflow.
6. Verify the result against rules and evals.
7. Record durable decisions or changes in living documentation.

## Context rules

- Do not assume project details when a project profile exists.
- Do not read every file by default. Use the context router to choose relevant files.
- If required context is missing, create a clear note in `reports/` or ask for the missing file.
- Keep common skills generic. Put project-specific facts in `project-profiles/<project>/`.

## Safety rules

- Do not write to production systems.
- Do not approve business decisions automatically.
- Do not delete or rewrite user-owned history unless explicitly requested.
- Do not expose secrets, tokens, private data, or credentials in docs or reports.

## Documentation rules

- Durable architectural decisions go in the active project profile.
- Reusable workflow improvements go in `skills/`.
- Cross-project rules go in `docs/rules/`.
- Study-oriented concept explanations go in `docs/concepts/` and should be written in Korean by default.
- Study-oriented concept docs should include English names and Korean names together when introducing concepts.
- Study-oriented concept filenames should include English and Korean names together when practical, using `english-name--한국어-이름.md`.
- One-off results go in `reports/`.

When the user is learning a concept while building the harness, update `docs/concepts/` with a concise Korean explanation before or alongside skill changes. Include both the English term and Korean term when the concept has a common English name, including the filename when practical.

## Git message rules

- Commit messages should use Korean summaries by default for readability.
- Keep the conventional commit type and optional scope when useful, then write the human-readable summary in Korean.
- Example: `feat(context-router): 컨텍스트 라우팅 스킬 확장`

## Git approval rules

- Before committing and pushing, stop and show the user the changed files, short change summary, verification status, target remote/branch, and proposed Korean commit message.
- Ask once whether to commit and push.
- After approval, run `git commit` and `git push` as one continuous Git publishing step.
- Do not ask separately for commit and push unless the user explicitly requests a split workflow.
- Do not skip the review step unless the user explicitly asks to skip confirmation for that specific operation.

## Default skill selection

- Broad request or unclear scope: `skills/supervisor`
- Find needed files/context: `skills/context-router`
- Prepare implementation context: `skills/generate-feature-context`
- Update docs after work: `skills/update-living-documentation`
- Check consistency: `skills/verify-context-integrity`
- Backend domain modeling: `skills/backend-ddd`
- Event-driven backend work: `skills/backend-eda`
- Frontend feature work: `skills/frontend-feature`
- Test-first work: `skills/testing-tdd`
- Commit preparation: `skills/git-workflow`
