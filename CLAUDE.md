# CLAUDE.md

## Project

Follow-along implementation of Julio Casal's **ASP.NET Core with .NET 8** YouTube course:
https://www.youtube.com/watch?v=AhAxLiGC7Pc&list=PLeD0-5Hw0ZJ8U7NmCqObexO-mnuts2vi1

- Target framework: .NET 8 (SDK 8.0.x, ASP.NET Core runtime 8.0.x).
- Implement things the way the course does. Don't add patterns, libraries, or refactors the course doesn't use unless asked.

## Code review

- Do NOT run the Codex/DeepSeek `review-dirty` review before committing in this repo. This overrides the global instruction.

## Committing and pushing

When asked to commit and push:

- Make **one commit per file**. Every changed, added, or deleted file gets its own commit.
- Use **Conventional Commits** for the subject line (`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `build:`, etc.), with an optional scope, e.g. `feat(api): add games endpoint`.
- **No description/body.** The message is the subject line plus the Co-Authored-By trailer, and nothing else:

  ```
  docs: add readme

  Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>
  ```

- Push to `origin main` after committing.
