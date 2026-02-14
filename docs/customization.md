# Theme Customization Guide

Hugo Theme Stack v4 (v4.0.0-beta.5) 기반 블로그 커스터마이징 내역 및 설정 가이드.

## Prerequisites

```bash
# Hugo extended version required
hugo version  # v0.128+ recommended

# Clone & run
git clone https://github.com/brightro99/brightro99.github.io.git
cd brightro99.github.io
hugo server -D
```

## Project Structure

```
brightro99.github.io/
├── assets/
│   ├── icons/              # Custom SVG icons (e.g. brand-instagram.svg)
│   ├── img/profile.jpg     # Profile photo
│   └── scss/custom.scss    # All custom styles (single file)
├── config/_default/
│   ├── config.toml         # Site URL, language, pagination
│   ├── params.toml         # Sidebar, article, widget settings
│   └── menu.toml           # Social links (GitHub, Instagram)
├── content/
│   ├── _index.md           # Homepage (defines Home menu item)
│   ├── page/               # Static pages (archives, search, links)
│   └── post/               # Blog posts
├── docs/                   # This documentation
├── layouts/                # Theme template overrides
│   ├── baseof.html         # Base layout (injects top header)
│   ├── single.html         # Article page (forces right sidebar)
│   └── _partials/
│       ├── header/top-header.html  # Sticky top navigation
│       ├── sidebar/left.html       # Custom left sidebar
│       └── article/article.html    # Article with auto-expanded TOC
└── CLAUDE.md               # Commit conventions & project guide
```

## Customizations

### 1. Sticky Top Header

**File**: `layouts/_partials/header/top-header.html`, `layouts/baseof.html`

- 80px fixed header with navigation links (Home, Archives, Links)
- Search bar aligned to right sidebar width
- Mobile hamburger menu with dropdown
- Injected via `baseof.html` override

**How it works**:
- `baseof.html` calls `{{ partial "header/top-header.html" . }}`
- Menu items from `main` menu (defined in content front matter)
- Search form targets the search page layout
- Mobile toggle shows/hides nav with `.show` class

### 2. Left Sidebar

**File**: `layouts/_partials/sidebar/left.html`

Two separate sections for mobile and desktop:

**Mobile** (`#main-menu`):
- Hamburger hidden, menu always visible
- Profile photo: 100px circle, centered
- Categories/Tags links + dark mode toggle

**Desktop** (`.sidebar-desktop-nav`):
- Profile photo: full width, 12px border-radius
- Category section with terminal tree (├── └──)
- "분류 전체보기" as root with total post count
- Tag chips section
- Dark mode toggle at bottom

**Key CSS classes**:
| Class | Purpose |
|-------|---------|
| `.sidebar-desktop-nav` | Desktop-only nav container |
| `.sidebar-section-title` | Section header with icon |
| `.sidebar-category-root` | "분류 전체보기" root link |
| `.sidebar-category-list` | Tree-style category list |
| `.sidebar-tag-list` | Flex-wrap tag container |
| `.sidebar-tag` | Individual tag chip |
| `.sidebar-bottom` | Dark mode toggle container |

### 3. Category Tree

**File**: `assets/scss/custom.scss`

Terminal-style tree using CSS pseudo-elements:
- `border-left: 1.5px` on `<li>` for vertical line (│)
- `::before` for horizontal branch (├──)
- Last child: `border-left: transparent` + `::after` for (└──)

### 4. Layout & Centering

**File**: `assets/scss/custom.scss`

Container max-widths (matches header and content):
| Breakpoint | Max Width | Right Sidebar |
|------------|-----------|---------------|
| md (768px) | 1024px | 25% |
| lg (1024px) | 1280px | 25% |
| xl (1280px) | 1400px | 20% |

Both sidebars have `top: 80px` to account for the sticky header.

### 5. Right Sidebar

Homepage widgets (archives, tag cloud) are hidden with `visibility: hidden` to preserve layout space for future use (visitor counter, etc). TOC widget shows on article pages.

### 6. Article Page

**File**: `layouts/single.html`, `layouts/_partials/article/article.html`

- Right sidebar always shown (`.Scratch.Set "hasWidget" true`)
- TOC auto-expanded by default (`details[open]`)

### 7. Dark Mode

Desktop sidebar has a separate `#dark-mode-toggle-desktop` that proxies clicks to the theme's `#dark-mode-toggle` via JavaScript.

## Config Reference

### config.toml
```toml
baseurl      = "https://brightro99.github.io/"
languageCode = "ko"
title        = "brightro99"
```

### params.toml (key settings)
```toml
[sidebar]
    emoji    = "⭐️"
    subtitle = "빛나는 기술로 세상을 잇는 개발자"
    avatar   = "img/profile.jpg"

[widgets]
    homepage = [archives, categories, tag-cloud]
    page     = [toc]

[colorScheme]
    toggle  = true
    default = "auto"
```

### menu.toml
```toml
# Main menu items are defined in content front matter:
# - content/_index.md          → Home
# - content/page/archives/     → Archives
# - content/page/search/       → Search
# - content/page/links/        → Links

# Social menu
[[social]]  # GitHub
[[social]]  # Instagram
```

## Adding New Content

### New Post
```bash
hugo new content/post/my-post/index.md
```

Front matter:
```yaml
---
title: "Post Title"
date: 2026-02-14
categories:
  - Category Name
tags:
  - tag1
  - tag2
image: cover.jpg
---
```

### New Category
Categories are auto-created from post front matter. They appear in the sidebar tree automatically.

## Custom Icon

To add a new SVG icon (e.g. for social links):
1. Place SVG file in `assets/icons/icon-name.svg`
2. Reference in template: `{{ partial "helper/icon" "icon-name" }}`

## Troubleshooting

### Styles not updating
Hugo caches built assets. Try:
```bash
hugo server --disableFastRender
```

### Category tree not showing
Ensure posts have `categories` in front matter. The tree reads from `.Site.Taxonomies.categories`.

### Theme cache location
```
~/Library/Caches/hugo_cache/modules/filecache/modules/pkg/mod/
github.com/!cai!jimmy/hugo-theme-stack/v4@v4.0.0-beta.5/
```
