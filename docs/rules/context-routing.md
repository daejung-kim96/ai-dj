# Context Routing Rule

The context router decides which files to read for a request.

## Inputs

- User request
- Active project profile
- Skill requirements
- Known source map

## Output

A short list of files to read, grouped by purpose.

## Routing categories

- Product/spec request
- Backend request
- Frontend request
- Data pipeline request
- Testing request
- Documentation request
- Git/release request
- Architecture review

## Rule

Start narrow. Expand only when the first pass reveals missing context.
