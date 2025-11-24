# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static site serving as a website for ZenCash, a crypto trading platform. The site uses Tailwind CSS for styling and is built with a minimalist, dark theme design featuring a unique markdown-inspired aesthetic with monospace typography and a red/white/black color scheme.

## Codebase Structure

```
/home/user/zencash.ro/
├── .github/workflows/
│   └── deploy.yml              # GitHub Actions CI/CD pipeline
├── _includes/                  # Reusable HTML components (Liquid templates)
│   ├── footer.html            # Site footer with auto copyright year
│   └── header.html            # Site header with navigation
├── _layouts/                   # Page templates
│   └── default.html           # Main layout (header + content + footer)
├── assets/
│   └── css/
│       └── tailwind.css       # Tailwind imports + custom CSS (processed by Jekyll)
├── .gitignore                  # Excludes _site/, .jekyll-cache/, etc.
├── .ruby-version               # Ruby 3.3.1
├── 404.md                      # Custom error page
├── CLAUDE.md                   # This file - AI assistant instructions
├── Gemfile                     # Ruby dependencies
├── Gemfile.lock                # Locked dependency versions
├── README.md                   # Project documentation
├── _config.yml                 # Jekyll configuration
├── index.md                    # Homepage content
└── mise.toml                   # Build automation tasks
```

### Key Files and Their Purposes

| File | Purpose | Key Details |
|------|---------|-------------|
| `_config.yml` | Jekyll site configuration | Defines plugins, metadata, build settings |
| `_layouts/default.html` | Main page template | Flexbox layout with header/content/footer |
| `_includes/header.html` | Site header component | Contains site title and navigation |
| `_includes/footer.html` | Site footer component | Auto-updating copyright with current year |
| `assets/css/tailwind.css` | Main stylesheet | Tailwind imports + custom CSS with Jekyll front matter |
| `index.md` | Homepage content | Markdown content using default layout |
| `404.md` | Error page | Permalink set to /404.html |
| `mise.toml` | Build tool config | Defines install, dev, build tasks |
| `.github/workflows/deploy.yml` | CI/CD pipeline | Builds and deploys to GitHub Pages |

## Development Workflow

### Local Development Commands

- `mise run install` - Install/update Ruby dependencies (runs automatically before dev/build)
- `mise run dev` - Start Jekyll development server with live reload and auto-open browser
- `mise run build` - Build Jekyll site for production (outputs to `_site/`)
- `mise run lazygit` - Open git UI for version control

**Alternative (direct Jekyll):**
```bash
bundle exec jekyll serve --livereload --open-url  # Development
bundle exec jekyll build                          # Production
```

### Development Server

- **Port:** Default is 4000 (http://localhost:4000)
- **Live Reload:** Automatically refreshes browser on file changes
- **Auto-Open:** Browser opens automatically with `mise run dev`
- **Watch Mode:** Monitors all files except those in `exclude` list

## Jekyll Configuration

### Site Metadata (_config.yml)

- **Title:** ZenCash
- **Description:** "Crypto trading platform"
- **URL:** https://zencash.ro
- **Baseurl:** None (deployed to root)
- **Markdown:** kramdown processor
- **Permalinks:** Pretty URLs (removes .html extensions)

### Jekyll Plugins

The site uses these plugins (configured in Gemfile):

1. **`jekyll-tailwindcss`** - Integrates Tailwind CSS with Jekyll's build pipeline
   - Processes CSS through Tailwind's JIT compiler
   - Enables Tailwind utility classes in HTML/layouts

2. **`jekyll-seo-tag`** - Generates SEO metadata tags
   - Usage: `{% seo %}` in layout `<head>`
   - Auto-generates meta tags, Open Graph, Twitter Cards

3. **`jekyll-sitemap`** - Generates sitemap.xml automatically
   - No configuration needed
   - Updates on each build

### Excluded from Build

These files/directories are excluded from Jekyll processing:
- `Gemfile`, `Gemfile.lock`
- `mise.toml`
- `README.md`
- `vendor/`, `.bundle/`
- `.git/`, `.ruby-lsp/`
- `node_modules/`

## Design System

### Color Scheme

**Light Theme (default):**
- Background: White/light gray
- Text: Dark gray (#333, #666)
- Accent: Red (#dc2626)

**Dark Theme (prefers-color-scheme: dark):**
- Background: Pure black (#000000)
- Text: White/gray (#ffffff, #cccccc)
- Accent: Bright red (#ff0000)
- Glassmorphism effects with rgba overlays

### Typography

- **Font Family:** Courier New, monospace (throughout entire site)
- **Design Philosophy:** Markdown-inspired visual aesthetic
- All headings and text use monospace for consistent developer-focused look

### Custom CSS Styling Patterns

All custom styling is in `assets/css/tailwind.css` using pure CSS with pseudo-elements:

**Headings:** Markdown-style prefixes using `::before`
- `h1` → `# ` prefix
- `h2` → `## ` prefix
- `h3` → `### ` prefix
- `h4` → `#### ` prefix

**Lists:** Custom bullet points
- `ul li` → `* ` prefix (instead of default bullets)

**Links:** Bracketed style
- `a` → `[text]` appearance using `::before` and `::after`

**Code:** Backtick-wrapped
- `code` → `` `text` `` appearance using pseudo-elements
- Background color highlighting

**Transitions:** Smooth color transitions (0.3s ease) for theme switching

### Tailwind CSS Integration

- **Import Method:** Using `@import` directives in Jekyll-processed CSS
  ```css
  @import "tailwindcss/preflight";  // Reset styles
  @import "tailwindcss/utilities";  // Utility classes
  ```
- **Custom Classes:** Defined in same file after imports
- **Processing:** Jekyll processes CSS file (requires front matter delimiters `---`)

## Component Architecture

### Layout System

**Default Layout (`_layouts/default.html`):**
- HTML5 boilerplate with responsive viewport
- Flexbox-based structure for sticky footer
- Three sections:
  1. Header (included)
  2. Main content (centered, flex-grow)
  3. Footer (included)

**Key CSS Classes:**
```html
<body class="min-h-screen flex flex-col">
  <header>{% include header.html %}</header>
  <main class="flex-grow flex items-center justify-center">
    <div class="container mx-auto px-4 py-8">
      <div class="text-center">
        {{ content }}
      </div>
    </div>
  </main>
  <footer>{% include footer.html %}</footer>
</body>
```

### Include Components

**Header (`_includes/header.html`):**
- Background: `bg-zen-gray`
- Bottom border: `border-b border-gray-800`
- Contains site title linked to homepage
- Uses `{{ site.title }}` Liquid variable

**Footer (`_includes/footer.html`):**
- Background: `bg-zen-gray`
- Top border: `border-t border-gray-800`
- Auto-updating copyright: `{{ 'now' | date: "%Y" }}`
- Sticky footer via flexbox in parent layout

## Code Style Guidelines

### Critical Rules (MUST FOLLOW)

1. **NO HTML IN MARKDOWN**
   - NEVER add ANY HTML tags to .md files
   - Use pure markdown syntax only
   - All styling is handled via CSS, not inline HTML

2. **CSS-ONLY STYLING**
   - All visual styling must be done in `assets/css/tailwind.css`
   - Use CSS selectors and pseudo-elements (::before, ::after)
   - Style the generated HTML markup, don't modify markdown

3. **TAILWIND SYSTEM**
   - Use Tailwind utility classes in layouts/includes
   - Don't write raw CSS for things Tailwind provides
   - Custom CSS should extend Tailwind, not replace it

4. **JEKYLL + TAILWIND PIPELINE**
   - CSS files need Jekyll front matter (`---`) to be processed
   - Use `@import` for Tailwind, not `@tailwind` directives
   - Let Jekyll process CSS before serving/building

### Best Practices

- **Layouts:** Keep minimal and focused on structure
- **Content:** Separate content (markdown) from presentation (layouts)
- **Includes:** Use for reusable components (header, footer, etc.)
- **Liquid Variables:** Use `{{ site.* }}` for configuration values
- **Front Matter:** Always include in content files (layout, title, etc.)

### Naming Conventions

- **Layouts:** `_layouts/*.html` (lowercase, descriptive)
- **Includes:** `_includes/*.html` (lowercase, descriptive)
- **Pages:** `*.md` in root or subdirectories
- **Assets:** `assets/{type}/*` (css, js, images)
- **Config:** Root-level `_config.yml`

## Content Management

### Creating New Pages

1. Create `{pagename}.md` in root or subdirectory
2. Add front matter:
   ```yaml
   ---
   layout: default
   title: Page Title
   ---
   ```
3. Write content in pure markdown
4. Page will be available at `/pagename/` (pretty URLs)

### Front Matter Options

- `layout:` - Which layout to use (default, custom)
- `title:` - Page title (used in SEO, browser tab)
- `permalink:` - Custom URL (optional, overrides default)
- `exclude:` - Set to true to exclude from site build

### Special Pages

**404 Page:**
- Must have `permalink: /404.html`
- GitHub Pages serves this for not-found errors
- Uses same layout as other pages

## Build and Deployment

### Local Build Process

1. **Install Dependencies:** `mise run install` or `bundle install`
2. **Development:** `mise run dev` (starts server with live reload)
3. **Production Build:** `mise run build` (outputs to `_site/`)

### CI/CD Pipeline (GitHub Actions)

**Trigger:**
- Push to `main` branch
- Manual workflow dispatch

**Build Steps:**
1. Checkout code (actions/checkout@v4)
2. Setup Ruby 3.3 with bundler cache
3. Configure GitHub Pages
4. Build Jekyll site with production environment
5. Upload build artifact

**Deploy Steps:**
1. Deploy artifact to GitHub Pages
2. Output deployment URL

**Configuration:**
- Workflow file: `.github/workflows/deploy.yml`
- Concurrency: Single "pages" group (cancels in-progress)
- Permissions: contents:read, pages:write, id-token:write

### Deployment URL

- Production: https://zencash.ro
- GitHub Pages: https://{username}.github.io/zencash.ro/ (if baseurl configured)

## Technical Patterns

### Liquid Templating

- **Variables:** `{{ site.title }}`, `{{ page.title }}`
- **Tags:** `{% include header.html %}`, `{% seo %}`
- **Filters:** `{{ 'now' | date: "%Y" }}`

### Jekyll Processing

- Files with front matter (`---`) are processed
- CSS files need front matter to use Liquid or imports
- Markdown files are converted to HTML using kramdown

### CSS Custom Properties

```css
:root {
  --bg-color: #ffffff;
  --text-color: #333333;
  --accent-color: #dc2626;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg-color: #000000;
    --text-color: #ffffff;
    --accent-color: #ff0000;
  }
}
```

### Responsive Design

- Mobile-first approach
- Tailwind responsive utilities (`sm:`, `md:`, `lg:`, `xl:`)
- Viewport meta tag for proper mobile rendering
- Container with responsive padding

## Common Tasks

### Adding a New Section to Homepage

1. Edit `index.md`
2. Use markdown syntax (##, lists, links)
3. Do NOT add HTML tags
4. Styling will be applied automatically via CSS

### Modifying Site-wide Styling

1. Edit `assets/css/tailwind.css`
2. Add/modify CSS rules or custom properties
3. Use CSS selectors and pseudo-elements
4. Test in both light and dark modes

### Changing Site Metadata

1. Edit `_config.yml`
2. Update title, description, url, etc.
3. Restart Jekyll server for config changes to take effect

### Adding a Plugin

1. Add to `Gemfile` in plugins group:
   ```ruby
   group :jekyll_plugins do
     gem "jekyll-new-plugin"
   end
   ```
2. Add to `_config.yml` plugins list:
   ```yaml
   plugins:
     - jekyll-new-plugin
   ```
3. Run `bundle install`
4. Restart Jekyll server

## Troubleshooting

### Jekyll Server Won't Start

- Check Ruby version: `ruby --version` (should be 3.3.1)
- Reinstall dependencies: `bundle install`
- Clear cache: `rm -rf .jekyll-cache _site`
- Check for port conflicts (default 4000)

### CSS Changes Not Appearing

- Ensure CSS file has front matter (`---` at top)
- Hard refresh browser (Cmd+Shift+R / Ctrl+Shift+R)
- Clear Jekyll cache and rebuild
- Check browser console for CSS errors

### Build Fails on GitHub Actions

- Check workflow run logs in GitHub Actions tab
- Ensure Gemfile.lock is committed
- Verify Ruby version matches between local and CI
- Check for missing front matter in content files

### Styling Not Applied to Markdown Elements

- Verify CSS selectors target correct elements
- Check that markdown is being processed (view page source)
- Ensure Tailwind preflight is imported
- Test CSS specificity and selector order

## Repository Information

- **Git Repository:** Yes (git initialized)
- **Platform:** Linux 4.4.0
- **Ruby Version:** 3.3.1
- **Jekyll Version:** Latest (via Gemfile)
- **Bundler Version:** 2.3.3

## Additional Notes

### Current Limitations

- No blog/posts functionality (no `_posts/` directory)
- No collections configured
- No JavaScript (purely static HTML/CSS)
- No images currently in use

### Future Expansion Possibilities

- Add blog with `_posts/` directory
- Create custom collections for crypto data
- Add JavaScript for interactive features
- Implement search functionality
- Add image optimization pipeline
- Multi-language support with i18n

## Resources

- Jekyll Documentation: https://jekyllrb.com/docs/
- Tailwind CSS: https://tailwindcss.com/docs/
- Liquid Templating: https://shopify.github.io/liquid/
- GitHub Pages: https://docs.github.com/en/pages
- Mise Documentation: https://mise.jdx.dev/
