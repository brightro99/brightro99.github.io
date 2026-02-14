# Commit Convention

## Format

```text
[Type] Short description in English
```

## Types

| Type         | Description                              | Example                                    |
|--------------|------------------------------------------|--------------------------------------------|
| `[Feat]`     | New feature or functionality             | `[Feat] Add category tree to left sidebar` |
| `[Fix]`      | Bug fix                                  | `[Fix] Fix dark mode toggle on desktop`    |
| `[Style]`    | CSS/UI/design changes                    | `[Style] Adjust search bar alignment`      |
| `[Post]`     | Blog post add/edit/delete                | `[Post] Add Hugo customization article`    |
| `[Config]`   | Configuration file changes               | `[Config] Update site title`               |
| `[Docs]`     | Documentation changes                    | `[Docs] Add commit convention guide`       |
| `[Refactor]` | Code refactoring without behavior change | `[Refactor] Simplify sidebar template`     |
| `[Chore]`    | Build, deploy, maintenance tasks         | `[Chore] Update Hugo module version`       |

## Rules

1. **Language**: English
2. **Length**: Subject line under 72 characters
3. **Mood**: Imperative (Add, Fix, Update, Remove - not Added, Fixed)
4. **Scope**: One commit per logical change when possible
5. **Body** (optional): Add blank line after subject, then explain "why" not "what"
6. **No Co-Authored-By**: Do not include `Co-Authored-By` lines

## Examples

```bash
# Good
[Feat] Add sticky top header with navigation
[Style] Center page layout and adjust sidebar widths
[Fix] Align category tree last-child vertical line
[Post] Add new article about Docker setup
[Config] Enable dark mode toggle

# Bad
[Feat] updated stuff          # past tense, vague
fixed bug                     # no type prefix
[Style] CSS                   # too vague
```
