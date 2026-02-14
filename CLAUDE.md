# CLAUDE.md - Project Guidelines

## Conventions

- **Commit**: [docs/commit_convention.md](docs/commit_convention.md) - `[Type] description` format
- **Code**: [docs/code_convention.md](docs/code_convention.md) - Google C++ Style based
- **Blog posts with code**: Follow code_convention.md (camelCase functions, snake_case vars, PascalCase classes, no `using namespace`, 4-space indent)

## Project Structure

- **Theme**: Hugo Theme Stack v4 (v4.0.0-beta.5) via Go modules
- **Theme overrides**: `layouts/` directory overrides theme templates
- **Custom styles**: `assets/scss/custom.scss` (single file for all custom CSS)
- **Config**: `config/_default/` (config.toml, params.toml, menu.toml)

## Key Custom Files

| File | Purpose |
|------|---------|
| `layouts/baseof.html` | Base layout with top header injection |
| `layouts/_partials/header/top-header.html` | Sticky top navigation header |
| `layouts/_partials/sidebar/left.html` | Custom sidebar with category tree + tags |
| `layouts/_partials/article/article.html` | Article template with auto-expanded TOC |
| `layouts/single.html` | Single page with forced right sidebar |
| `assets/scss/custom.scss` | All custom styles |

## Development

```bash
hugo server -D   # Start dev server with drafts
```
