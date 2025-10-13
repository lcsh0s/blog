# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a terminal-inspired coding blog built with Astro, featuring a highly customizable theming system with 50+ Shiki themes. The blog supports MDX content with custom remark/rehype plugins for enhanced markdown capabilities.

## Development Commands

```bash
# Start development server
npm run dev

# Build site for production (includes Pagefind search index generation)
npm run build

# Preview production build
npm run preview

# Format code with Prettier
npm run format
```

## Architecture

### Content System

The site uses Astro's content collections defined in `src/content.config.ts`:

- **posts**: Blog posts stored in `src/content/posts/` as `.md` or `.mdx` files
  - Frontmatter schema includes: title, published date, draft status, description, author, series, tags, coverImage, and toc
  - Draft posts are excluded in production builds (see `getSortedPosts()` in `src/utils.ts:217`)

- **home**: Homepage content from `src/content/home.md`
  - Supports avatar image and optional GitHub calendar widget

- **addendum**: Additional content shown in post footers from `src/content/addendum.md`

### Theming System

The theming system is one of the most complex parts of this codebase:

1. **Configuration**: `src/site.config.ts` defines theme mode, included themes, default theme, and color overrides
   - Three modes: `single` (one theme), `select` (user chooses from multiple), `light-dark-auto` (automatic light/dark switching)

2. **Theme Resolution**: `src/utils.ts` contains `resolveThemeColorStyles()` which:
   - Loads Shiki themes via `loadShikiTheme()`
   - Maps TextMate token scopes to semantic color keys (defined in `unresolvedStyles`)
   - Applies user-defined overrides from `site.config.ts`
   - Overrides can be literal colors (hex/rgb/hsl) or references to other theme keys

3. **Theme Keys**: Defined in `src/types.ts:76-113` - includes semantic keys like `foreground`, `background`, `accent`, markdown heading colors, admonition colors, and terminal colors

4. **Components**: Theme switching UI lives in:
   - `src/components/SelectTheme.astro` - dropdown selector for multiple themes
   - `src/components/LightDarkAutoButton.astro` - toggle button for light/dark/auto mode
   - Corresponding loader components handle initial theme application before hydration

### Custom Markdown Plugins

Located in `src/plugins/`, configured in `astro.config.mjs:30-66`:

**Remark plugins** (process markdown AST):
- `remark-description.ts` - Auto-generates post descriptions from content (max 200 chars)
- `remark-reading-time.ts` - Calculates reading time and adds to frontmatter
- `remark-github-card.ts` - Renders GitHub repository cards with live stats
- `remark-admonitions.ts` - Processes `:::note`, `:::tip`, `:::warning`, etc. directives
- `remark-gemoji.ts` - Converts emoji shortcodes (`:smile:`) to Unicode emoji
- `remark-math` - Adds LaTeX math support

**Rehype plugins** (process HTML AST):
- `rehype-title-figure.ts` - Wraps image titles in `<figure>` elements
- `rehype-pixelated.ts` - Handles pixelated image rendering
- `rehype-autolink-headings` - Adds anchor links to headings
- `rehype-external-links` - Adds `target="_blank"` to external links
- `rehype-unwrap-images` - Removes wrapping `<p>` tags from images
- `rehype-katex` - Renders LaTeX math expressions

### Routing Structure

- `src/pages/index.astro` - Homepage
- `src/pages/about.md` - About page (markdown)
- `src/pages/posts/[slug].astro` - Individual post pages (dynamic routing)
- `src/pages/posts/[...page].astro` - Paginated post listing
- `src/pages/series/` - Series archive pages
- `src/pages/tags/` - Tag archive pages
- `src/pages/social-cards/` - Dynamic OG image generation with Satori
- `src/pages/rss.xml.ts` - RSS feed generation

### Layout System

- `src/layouts/Layout.astro` - Base layout with head tags, theme loaders, navigation
- `src/layouts/MarkdownLayout.astro` - Wrapper for markdown pages (about page)

### Utility Functions

`src/utils.ts` contains critical utilities:

- `resolveThemeColorStyles()` - Theme color resolution (line 161)
- `getSortedPosts()` - Fetches and sorts posts, filters drafts in production (line 216)
- `SeriesGroup.build()` - Collates posts by series (line 288)
- `TagsGroup.build()` - Collates posts by tags (line 308)
- `slugify()` - Converts titles to URL-safe slugs (line 321)

### Key Components

- `src/components/Search.astro` - Integrates Pagefind search (generated in postbuild)
- `src/components/GitHubActivityCalendar.astro` - GitHub contribution calendar widget
- `src/components/GiscusLoader.astro` - Giscus comments integration
- `src/components/PostPreview.astro` - Post card for listings
- `src/components/PostInfo.astro` - Post metadata display (date, reading time, tags)
- `src/components/ScrollUpButton.astro` - Scroll-to-top functionality

## Important Notes

- The `postbuild` script runs Pagefind to generate the search index after building the site
- TypeScript types are centralized in `src/types.ts`
- Site configuration lives in `src/site.config.ts` - this is the main file users customize
- The `dev/` directory contains auto-generated theme data for reference only (not required for builds)
- When modifying theme-related code, remember the three-layer system: Shiki themes → TextMate scopes → semantic theme keys
- Posts are sorted chronologically by `published` date, oldest first (line 220-222 in utils.ts)
- Collation classes (SeriesGroup, TagsGroup) use factory methods due to async data fetching requirements
