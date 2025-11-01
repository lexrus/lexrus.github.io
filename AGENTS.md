# Repository Guidelines

## Project Structure & Module Organization
- `hugo.yaml` centralizes site metadata, menus, and PaperMod settings.
- `content/` stores all Markdown pages (privacy policies, `resume/`); mirror the existing naming pattern such as `cmdx_privacypolicy.md`.
- `archetypes/default.md` provides starter front matter for new pages; run `hugo new path/to/page.md` to scaffold.
- `themes/PaperMod/` is a Git submodule; prefer local overrides under a new `layouts/` folder instead of editing the theme directly.
- `public/` contains generated output for GitHub Pages. Never hand-edit files here—regenerate them with Hugo when content changes.

## Build, Test, and Development Commands
- `hugo server -D` launches the live preview with drafts enabled at `http://localhost:1313/`; use it for iterative content edits.
- `hugo --gc --minify` performs a production build, runs garbage collection on unused assets, and updates `public/`.
- `git submodule update --init --recursive` keeps the PaperMod theme in sync after cloning or pulling.

## Coding Style & Naming Conventions
- Write content in Markdown with YAML front matter; indent lists and front-matter values with two spaces for readability.
- Use descriptive, kebab-case filenames (e.g., `content/liveextractor_privacypolicy.md`) and keep titles in Title Case.
- Keep shortcodes and internal links relative (no hard-coded domains) so both preview and production builds stay consistent.

## Testing Guidelines
- Treat a clean `hugo --gc --minify` run as the primary test; resolve any warnings before committing.
- Review the updated `public/` output in a browser (open `public/index.html`) to spot layout regressions or broken links.
- When shipping new components or layouts, test with `hugo server -D --disableFastRender` to ensure full rebuild coverage.

## Commit & Pull Request Guidelines
- Follow the existing Conventional Commit-style prefixes (`chore:`, `docs:`, `feat:`) seen in `git log`.
- Group related changes in a single commit and include generated `public/` updates when the content output changes.
- Pull requests should summarize user-visible changes, link relevant issues, and include screenshots or URLs for pages that changed.

## Deployment Notes
- The GitHub Actions workflow `.github/workflows/hugo.yml` builds with Hugo `0.147.6`, installs Dart Sass, and publishes to GitHub Pages on `main`.
- Verify the workflow passes after merging; failed builds usually point to invalid front matter or missing assets referenced in `content/`.
