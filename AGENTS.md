# AGENTS.md
This file provides guidance to AI coding assistants working in this repository.

**Note:** CLAUDE.md, .clinerules, .cursorrules, .windsurfrules, .replit.md, GEMINI.md, .github/copilot-instructions.md, and .idx/airules.md are symlinks to AGENTS.md in this project.

# ZenCash

ZenCash is a Jekyll-based static website for a crypto trading platform. The site uses Tailwind CSS for styling with a minimalist, dark theme design featuring monospace fonts and markdown-like visual elements.

## Architecture

- **Static Site Generator**: Jekyll
- **Styling**: Tailwind CSS (jekyll-tailwindcss plugin)
- **Build Tool**: mise (task runner)
- **Deployment**: GitHub Pages via GitHub Actions
- **Theme**: Dark mode with red/white/black color scheme

### Directory Structure

```
zencash.ro/
├── _config.yml           # Jekyll configuration
├── _layouts/             # HTML templates
│   └── default.html
├── _includes/            # Reusable HTML components
│   ├── header.html
│   └── footer.html
├── assets/
│   └── css/
│       └── tailwind.css  # Custom Tailwind styles and theme
├── *.md                  # Content pages (markdown only)
├── Gemfile               # Ruby dependencies
├── mise.toml             # Task definitions
├── reports/              # All project reports and documentation
└── temp/                 # Temporary files and debugging
```

## Build & Commands

All commands are run through mise, which handles dependency installation automatically.

### Development Commands

- **Install dependencies**: `mise run install`
  - Installs missing Ruby gems
  - Automatically runs before dev/build tasks

- **Development server**: `mise run dev`
  - Starts Jekyll server with live reload
  - Auto-opens browser
  - Runs on http://localhost:4000
  - **Note**: A VSCode task keeps the dev server running in the background - no need to start manually

- **Production build**: `mise run build`
  - Builds static site to `_site/` directory
  - Used by GitHub Actions for deployment

- **Git UI**: `mise run lazygit`
  - Opens lazygit terminal UI

### JavaScript Runtime

- **Use `bun` for JavaScript/TypeScript**: Always use `bun` instead of `node` for running JS/TS scripts
  - Example: `bun script.js` instead of `node script.js`

### Direct Jekyll Commands

If needed, you can run Jekyll commands directly:

```bash
bundle exec jekyll serve --livereload --open-url
bundle exec jekyll build
bundle exec jekyll build --baseurl "/custom-path"
```

### Deployment

Deployment is automated via GitHub Actions:
- Triggers on push to `main` branch or manual workflow dispatch
- Uses Ruby 3.3 with bundler cache
- Runs `bundle exec jekyll build` with dynamic baseurl
- Deploys to GitHub Pages automatically

## Code Style

### Critical Rules - NO EXCEPTIONS

1. **NO HTML IN MARKDOWN**: NEVER add ANY HTML tags to `.md` files
   - Use pure markdown syntax only
   - No `<div>`, `<span>`, `<style>`, or any other HTML elements
   - Let Jekyll convert markdown to HTML

2. **CSS-ONLY STYLING**: All styling must be done through CSS only
   - Edit `assets/css/tailwind.css` for all visual changes
   - Use CSS selectors and pseudo-elements to style generated HTML markup
   - Never add inline styles or HTML classes in markdown files

3. **SEPARATION OF CONCERNS**:
   - Content (markdown files) - structure and text only
   - Presentation (CSS files) - all styling
   - Layouts (HTML templates) - minimal structure

### Markdown Conventions

- Use standard markdown syntax:
  - `# Heading 1` for top-level headings
  - `## Heading 2` for sections
  - `* Item` or `- Item` for lists
  - `**bold**` for emphasis
  - `[link text](url)` for links
  - `` `code` `` for inline code

- Front matter required on all content pages:
  ```yaml
  ---
  layout: default
  title: Page Title
  ---
  ```

### CSS/Tailwind Conventions

- Use CSS custom properties (CSS variables) for theming
- All theme colors defined in `:root` selector
- Dark mode styles in `@media (prefers-color-scheme: dark)`
- Pseudo-elements (::before, ::after) for markdown-like decorators
- Monospace font family: `'Courier New', Courier, monospace`

### Layout Conventions

- Keep layouts minimal and focused on structure
- Use Jekyll includes for reusable components
- Use Liquid templating: `{{ content }}`, `{% include header.html %}`
- Add SEO tags: `{% seo %}`
- Reference assets with relative_url: `{{ '/assets/css/tailwind.css' | relative_url }}`

### File Naming

- Markdown files: lowercase with hyphens (e.g., `about-us.md`)
- Layouts: lowercase with hyphens (e.g., `post-layout.html`)
- Includes: lowercase with hyphens (e.g., `nav-menu.html`)

## Testing

This is a static site project with no automated test suite. Testing is done manually:

### Manual Testing Checklist

1. **Local development**: Run `mise run dev` and verify:
   - All pages render correctly
   - Links work as expected
   - Styles apply properly in both light and dark modes
   - Live reload functions correctly

2. **Production build**: Run `mise run build` and verify:
   - No Jekyll build errors
   - `_site/` directory generated correctly
   - All assets included

3. **Cross-browser testing**:
   - Test in Chrome, Firefox, Safari
   - Verify dark mode respects system preferences
   - Check mobile responsiveness

4. **Accessibility**:
   - Semantic HTML structure
   - Proper heading hierarchy
   - Sufficient color contrast

### Testing Philosophy

**When implementing new features, ensure they work in practice.**

Key principles:
- **Manual verification required** - Always test locally before committing
- **Visual regression** - Compare rendered output to previous version
- **Build validation** - Ensure Jekyll builds without errors
- **Multi-browser testing** - Dark mode may behave differently across browsers

## Security

### General Security Practices

- No user input or dynamic content - static site only
- No authentication or sensitive data handling
- All content committed to public repository
- Secrets (if any) must never be committed to repository

### GitHub Actions Security

- Uses official GitHub Actions only
- Minimal permissions defined in workflow
- No custom scripts that could introduce vulnerabilities

### Content Security

- All external links should be reviewed
- No third-party JavaScript libraries currently used
- Keep Jekyll and gem dependencies updated for security patches

## Configuration

### Environment Setup

1. **Ruby**: Version 3.3 (specified in GitHub Actions)
   - Uses `.ruby-version` file for local development

2. **Bundler**: Manages Ruby gem dependencies
   - Gems defined in `Gemfile`
   - Lock file: `Gemfile.lock`

3. **mise**: Task runner and tool version manager
   - Installs: claude, gh, lazygit, bun
   - Tasks defined in `mise.toml`

### Jekyll Configuration

Key settings in `_config.yml`:

```yaml
title: ZenCash
description: Crypto trading platform
url: "https://zencash.ro"
markdown: kramdown
permalink: pretty

plugins:
  - jekyll-tailwindcss    # Tailwind CSS integration
  - jekyll-seo-tag        # SEO metadata
  - jekyll-sitemap        # Automatic sitemap
```

### Required Files

- `Gemfile` - Ruby dependencies
- `_config.yml` - Jekyll configuration
- `mise.toml` - Task definitions
- `.ruby-version` - Ruby version specification

### Environment Variables

No environment variables currently required. Site configuration is in `_config.yml`.

## Directory Structure & File Organization

### Reports Directory

ALL project reports and documentation should be saved to the `reports/` directory:

```
zencash.ro/
├── reports/              # All project reports and documentation
│   └── *.md             # Various report types
├── temp/                # Temporary files and debugging
└── [other directories]
```

### Report Generation Guidelines

**Important**: ALL reports should be saved to the `reports/` directory with descriptive names:

**Implementation Reports:**
- Phase validation: `PHASE_X_VALIDATION_REPORT.md`
- Implementation summaries: `IMPLEMENTATION_SUMMARY_[FEATURE].md`
- Feature completion: `FEATURE_[NAME]_REPORT.md`

**Testing & Analysis Reports:**
- Test results: `TEST_RESULTS_[DATE].md`
- Performance analysis: `PERFORMANCE_ANALYSIS_[SCENARIO].md`
- Security scans: `SECURITY_SCAN_[DATE].md`

**Quality & Validation:**
- Code quality: `CODE_QUALITY_REPORT.md`
- Dependency analysis: `DEPENDENCY_REPORT.md`

**Report Naming Conventions:**
- Use descriptive names: `[TYPE]_[SCOPE]_[DATE].md`
- Include dates: `YYYY-MM-DD` format
- Group with prefixes: `TEST_`, `PERFORMANCE_`, `SECURITY_`
- Markdown format: All reports end in `.md`

### Temporary Files & Debugging

All temporary files, debugging scripts, and test artifacts should be organized in a `/temp` folder:

**Temporary File Organization:**
- **Debug scripts**: `temp/debug-*.js`, `temp/analyze-*.py`
- **Test artifacts**: `temp/test-results/`, `temp/coverage/`
- **Generated files**: `temp/generated/`, `temp/build-artifacts/`
- **Logs**: `temp/logs/debug.log`, `temp/logs/error.log`

**Guidelines:**
- Never commit files from `/temp` directory
- Use `/temp` for all debugging and analysis scripts created during development
- Clean up `/temp` directory regularly or use automated cleanup
- Include `/temp/` in `.gitignore` to prevent accidental commits

### Git Ignore Patterns

Current `.gitignore` includes:

```gitignore
# Jekyll
_site/
.jekyll-cache/
.jekyll-metadata
.sass-cache/

# Ruby
*.gem
*.rbc
.bundle/
vendor/

# System
.DS_Store
Thumbs.db

# Editor
.vscode/
*.swp
*.swo
*~

# Environment
.env
.env.*

# Node modules
node_modules/

# Add these for reports/temp organization:
/temp/
temp/
**/temp/

# Claude settings
.claude/settings.local.json

# Don't ignore reports directory
!reports/
!reports/**
```

### Claude Code Settings (.claude Directory)

The `.claude` directory contains Claude Code configuration files with specific version control rules:

#### Version Controlled Files (commit these):
- `.claude/settings.json` - Shared team settings for hooks, tools, and environment
- `.claude/commands/*.md` - Custom slash commands available to all team members
- `.claude/hooks/*.sh` - Hook scripts for automated validations and actions

#### Ignored Files (do NOT commit):
- `.claude/settings.local.json` - Personal preferences and local overrides
- Any `*.local.json` files - Personal configuration not meant for sharing

**Important Notes:**
- Claude Code automatically adds `.claude/settings.local.json` to `.gitignore`
- The shared `settings.json` should contain team-wide standards
- Personal preferences or experimental settings belong in `settings.local.json`
- Hook scripts in `.claude/hooks/` should be executable (`chmod +x`)

## Agent Delegation & Tool Execution

### ⚠️ MANDATORY: Always Delegate to Specialists & Execute in Parallel

**When specialized agents are available, you MUST use them instead of attempting tasks yourself.**

**When performing multiple operations, send all tool calls (including Task calls for agent delegation) in a single message to execute them concurrently for optimal performance.**

#### Why Agent Delegation Matters:
- Specialists have deeper, more focused knowledge
- They're aware of edge cases and subtle bugs
- They follow established patterns and best practices
- They can provide more comprehensive solutions

#### Key Principles:
- **Agent Delegation**: Always check if a specialized agent exists for your task domain
- **Complex Problems**: Delegate to domain experts, use diagnostic agents when scope is unclear
- **Multiple Agents**: Send multiple Task tool calls in a single message to delegate to specialists in parallel
- **DEFAULT TO PARALLEL**: Unless you have a specific reason why operations MUST be sequential (output of A required for input of B), always execute multiple tools simultaneously
- **Plan Upfront**: Think "What information do I need to fully answer this question?" Then execute all searches together

#### Critical: Always Use Parallel Tool Calls

**Err on the side of maximizing parallel tool calls rather than running sequentially.**

**IMPORTANT: Send all tool calls in a single message to execute them in parallel.**

**These cases MUST use parallel tool calls:**
- Searching for different patterns (imports, usage, definitions)
- Multiple grep searches with different regex patterns
- Reading multiple files or searching different directories
- Combining Glob with Grep for comprehensive results
- Searching for multiple independent concepts
- Any information gathering where you know upfront what you're looking for
- Agent delegations with multiple Task calls to different specialists

**Sequential calls ONLY when:**
You genuinely REQUIRE the output of one tool to determine the usage of the next tool.

**Planning Approach:**
1. Before making tool calls, think: "What information do I need to fully answer this question?"
2. Send all tool calls in a single message to execute them in parallel
3. Execute all those searches together rather than waiting for each result
4. Most of the time, parallel tool calls can be used rather than sequential

**Performance Impact:** Parallel tool execution is 3-5x faster than sequential calls, significantly improving user experience.

**Remember:** This is not just an optimization—it's the expected behavior. Both delegation and parallel execution are requirements, not suggestions.

## Common Tasks

### Adding a New Page

1. Create a new `.md` file in the root directory
2. Add front matter with layout and title
3. Write content using pure markdown
4. Test locally with `mise run dev`
5. Commit and push to deploy

Example:
```markdown
---
layout: default
title: About Us
---

# About ZenCash

Content goes here...
```

### Styling Changes

1. Open `assets/css/tailwind.css`
2. Add or modify CSS rules
3. Use CSS selectors to target generated HTML
4. Test in both light and dark modes
5. Verify in development server

### Updating Layout

1. Edit files in `_layouts/` or `_includes/`
2. Use Liquid templating syntax
3. Keep structure minimal
4. Test with `mise run dev`

### Deployment

Simply push to `main` branch - GitHub Actions handles the rest automatically.
