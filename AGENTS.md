# Repository Guidelines

This repository contains concise, Russian‑language guides on information workflows and digital hygiene. It is a Markdown‑only project; keep changes simple, readable, and easy to review.

## Project Structure & Module Organization
- Root docs: `README.md` (main guide), `productivity-tools-guide.md` (tools overview).
- Locales: `docs/<lang>/` (e.g., `docs/en/`, `docs/uz/`) mirror filenames (`README.md`, `productivity-tools-guide.md`).
- Add new topics as separate `.md` files in the root or a `docs/` folder if the scope grows.
- Store images (when needed) under `assets/` and link with relative paths (e.g., `![Title](assets/example.png)`).

## Build, Test, and Development Commands
- No build pipeline. Preview locally in any Markdown viewer (e.g., VS Code: “Open Preview”).
- Optional checks (if available in your environment):
  - Spelling: `codespell` on the repo root.
  - Links: any Markdown link checker to catch broken URLs.

## Coding Style & Naming Conventions
- Languages: ru (source), en, uz. New filenames use kebab‑case Latin (e.g., `knowledge-base-practices.md`); keep `README.md` per locale folder.
- Headings: one `#` H1 per file; use `##`, `###` for subsections; keep titles clear and action‑oriented.
- Lists: start with `-` and short, direct bullets; prefer examples over theory.
- Links: use descriptive text, full URLs; avoid bare links in prose.
- Code/commands: fenced blocks with language hints when helpful.

## Testing Guidelines
- Self‑review for clarity, accuracy, and consistency with existing sections.
- Verify internal anchors and relative links resolve; check external links open.
- For images, confirm file exists under `assets/` and renders in preview.
- After any edits, regenerate Tables of Contents wherever present (keep locale slugs consistent). RU: После всех изменений пересоздавайте оглавления везде, где они есть.

## Localization & Sync
- Source of truth: Russian files at root (`README.md`, `productivity-tools-guide.md`).
- Localized derivatives live in `docs/<lang>/` mirroring filenames (e.g., `docs/en/README.md`, `docs/uz/README.md`).
- Supported locales: `en`, `uz`. Update the list if adding more.
- Sync policy: translations MUST be fully synchronized across locales. When updating Russian content, update all locales in the same PR. If not ready, add a top note “Needs sync with <file>@<commit>” in each affected locale and open issues `sync(<lang>): <section>` — resolve before release.
- Links must be language‑local (do not cross‑link to another locale).
- Headings/anchors stay parallel across languages; maintain a shared glossary.
- Uzbek quality rule: Uzbek texts MUST be grammatically correct, clear, and respectful; avoid machine translation without human review — this is mandatory.
- RU: При обновлении основной информации на русском — синхронизируйте производные страницы на всех языках проекта (`docs/<lang>/`). Обязательно поддерживайте высокий стандарт качества для узбекского языка.

## Root README Localization Links
- The root `README.md` must include links to all available localized READMEs (e.g., `docs/en/README.md`, `docs/uz/README.md`) and be updated whenever a locale is added or removed.

## Commit & Pull Request Guidelines
- Commits: concise, imperative, scoped messages. Examples:
  - `docs: add quick checklist section`
  - `docs: clarify disk encryption steps`
- PRs: include summary of changes, affected files/sections, and rationale. Link issues with `Closes #<id>` when applicable. Screenshots only if UI rendering matters.

## Security & Content Hygiene
- Do not include secrets, personal data, or proprietary content.
- Prefer first‑party or reputable sources; avoid link rot by citing stable pages.
- Standard reference: link all “3‑2‑1” mentions to https://www.backblaze.com/blog/the-3-2-1-backup-strategy/.
