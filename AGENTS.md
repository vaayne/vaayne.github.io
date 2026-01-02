# AGENTS.md

## Build Commands

- `mise run serve` - Local dev server with drafts
- `mise run build` - Build site
- `mise run new <post-name>` - Create new post
- `mise run clean` - Clean public directory
- `mise run deploy` - Build for production (minified)

## Architecture

- **Framework**: Hugo static site generator (v0.152.2 via mise)
- **Theme**: PaperMod (git submodule in `themes/`)
- **Content**: Markdown posts in `content/posts/`
- **Config**: `hugo.yml` for site settings
- **Deployment**: GitHub Pages via `.github/workflows/`

## Content Guidelines

- Posts use YAML frontmatter with: `date`, `draft`, `slug`, `title`, `category`, `tags`, `description`
- Permalinks format: `/posts/:year/:slugorcontentbasename/`
- Set `draft: false` for published posts
- Chinese/English mixed content supported

## Code Style

- Keep markdown files clean with proper heading hierarchy
- Use descriptive slugs for SEO-friendly URLs
- Categories as arrays: `['年终总结']`
- Tags as arrays: `['AI', 'Topic1', 'Topic2']`
