---
layout: default
title: Customization Guide
nav_order: 3
---

# Customization Guide

This guide covers advanced customization beyond basic setup. Learn how to add custom content, modify themes, and extend the template to fit your organization's needs.

## Table of Contents

1. [Adding New Pages](#adding-new-pages)
2. [Organizing Content](#organizing-content)
3. [Theming and Colors](#theming-and-colors)
4. [Custom Components](#custom-components)
5. [Spell Checking Configuration](#spell-checking-configuration)
6. [CI/CD Workflow Customization](#cicd-workflow-customization)

---

## Adding New Pages

### Create a Basic Page

1. Create a new `.md` file in the `docs/` directory:

```markdown
---
layout: default
title: Security Policy
nav_order: 10
---

# Security Policy

Your content here...
```

2. Commit and push the file
3. The page will automatically appear in the navigation

### Front Matter Options

**Required fields:**
- `layout: default` - Uses the default page layout
- `title: "Page Title"` - Displayed in navigation and page header

**Optional fields:**
- `nav_order: 10` - Controls navigation position (lower numbers appear first)
- `nav_exclude: true` - Hides page from navigation (still accessible via URL)
- `parent: "Parent Page Title"` - Creates nested navigation
- `permalink: /custom-url/` - Custom URL for the page
- `has_children: true` - Indicates page has child pages
- `has_toc: false` - Disables table of contents on page

### Example: Nested Navigation

**Parent page** (`docs/ci-cd-guides.md`):
```yaml
---
layout: default
title: CI/CD Guides
nav_order: 5
has_children: true
---
```

**Child page** (`docs/github-actions-guide.md`):
```yaml
---
layout: default
title: GitHub Actions
parent: CI/CD Guides
nav_order: 1
---
```

### Using Liquid Variables

Reference organization configuration in your content:

```markdown
Welcome to {{ site.organization.name }}'s open source guidelines!

Contact us at {{ site.organization.email }} for questions.

All our packages are published under the `@{{ site.organization.shortname }}/` scope.
```

**Available variables:**
- `{{ site.organization.name }}` - Full organization name
- `{{ site.organization.shortname }}` - Short identifier
- `{{ site.organization.email }}` - Contact email
- `{{ site.organization.github_org }}` - GitHub organization
- `{{ site.organization.website }}` - Company website
- `{{ site.organization.docs_url }}` - Documentation URL (if set)
- `{{ site.branding.primary_color }}` - Primary brand color
- `{{ site.url }}` - Site URL
- `{{ page.title }}` - Current page title
- `{{ page.url }}` - Current page URL

---

## Organizing Content

### Directory Structure

```
docs/
├── _config.yml           # Site configuration
├── index.md              # Home page
├── getting-started.md    # Root-level pages
├── naming-conventions.md
├── ci-automation/        # Subdirectory for related pages
│   ├── ci-automation.md  # Parent page (has_children: true)
│   ├── php-guide.md      # Child page
│   └── ruby-guide.md
└── examples/             # Example templates
    └── example-sdk.md
```

### Best Practices

**Navigation organization:**
- Use `nav_order` to control sidebar sequence
- Group related pages in subdirectories
- Keep directory depth ≤ 2 levels for clarity

**File naming:**
- Use kebab-case: `my-page-name.md`
- Be descriptive: `javascript-ci-cd-guide.md` not `js.md`
- Match title when possible

**Content organization:**
- Start with high-level overview pages
- Link to detailed guides from overviews
- Use consistent heading structure (H1 for title, H2 for sections)

---

## Theming and Colors

### Quick Color Changes

Edit `docs/_config.yml` branding section:

```yaml
branding:
  color_scheme: "custom_dark"       # or "custom_light"
  primary_color: "#FF6B35"          # Main brand color
  secondary_color: "#004E89"        # Accent color
  accent_color: "#1A1A1D"           # Dark UI elements
```

Changes take effect on next GitHub Pages build (2-3 minutes).

### Choosing Colors

**Primary color** - Used for:
- Links
- Button backgrounds
- Active navigation items
- Code highlights

**Secondary color** - Used for:
- Hover states
- Secondary buttons
- Badges and labels

**Accent color** - Used for:
- Dark backgrounds
- Borders
- Shadows

**Accessibility tips:**
- Ensure sufficient contrast (WCAG AA: 4.5:1 for text)
- Test with [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- Preview in light/dark modes

### Advanced SCSS Customization

#### Modify Color Scheme Files

Edit `docs/_sass/color_schemes/custom_dark.scss`:

```scss
// Brand colors (configure in _config.yml)
$primary-color: #FF6B35 !default;
$secondary-color: #004E89 !default;
$accent-color: #1A1A1D !default;

// Semantic color assignments
$link-color: $primary-color;
$btn-primary-bg: $secondary-color;
$code-background-color: $accent-color;

// Custom overrides
$body-background-color: #0d1117;
$body-text-color: #c9d1d9;
$nav-background-color: #161b22;
```

**Available SCSS variables** (from Just the Docs):
- `$body-background-color` - Page background
- `$body-text-color` - Main text color
- `$link-color` - Link color
- `$btn-primary-bg` - Primary button background
- `$code-background-color` - Code block background
- `$border-color` - Border color for elements
- `$table-background-color` - Table backgrounds

See [Just the Docs customization docs](https://just-the-docs.com/docs/customization/) for full variable list.

#### Add Custom Styles

Edit `docs/_sass/custom/custom.scss`:

```scss
// Import your color scheme
@import "./color_schemes/{{ site.color_scheme }}";

// Custom component styles
.custom-callout {
  background-color: rgba($primary-color, 0.1);
  border-left: 4px solid $primary-color;
  padding: 1rem;
  margin: 1rem 0;
}

// Modify existing elements
.site-title {
  font-size: 1.5rem !important;
  font-weight: 700;
}

.site-footer {
  border-top: 2px solid $primary-color;
}

// Custom button styles
.btn-custom {
  background-color: $secondary-color;
  color: white;

  &:hover {
    background-color: darken($secondary-color, 10%);
  }
}
```

### Creating a Custom Theme

To create a new color scheme (e.g., "high-contrast"):

1. Copy an existing scheme:
   ```bash
   cp docs/_sass/color_schemes/custom_dark.scss \
      docs/_sass/color_schemes/high_contrast.scss
   ```

2. Edit the new file with your colors

3. Update `docs/_config.yml`:
   ```yaml
   branding:
     color_scheme: "high_contrast"
   ```

4. Test locally before pushing

---

## Custom Components

### Callout Boxes

**Default callout** (using kramdown):
```markdown
{: .note }
This is an important note for readers.

{: .warning }
This is a warning about potential issues.
```

**Custom callout with colors**:

Add to `docs/_sass/custom/custom.scss`:
```scss
.custom-note {
  background-color: rgba($secondary-color, 0.1);
  border-left: 4px solid $secondary-color;
  padding: 1rem;
  margin: 1rem 0;
  border-radius: 4px;

  &::before {
    content: "ℹ️ Note: ";
    font-weight: 700;
  }
}
```

Use in markdown:
```markdown
{: .custom-note }
This appears as a styled callout box.
```

### Code Blocks with Titles

```markdown
**`config.yml` - Configuration example:**
```yaml
organization:
  name: "Acme Corp"
```
```

### Custom Includes

Create reusable content snippets:

**File**: `docs/_includes/contact-banner.html`
```html
<div class="contact-banner">
  <p>
    Questions? Contact us at
    <a href="mailto:{{ site.organization.email }}">
      {{ site.organization.email }}
    </a>
  </p>
</div>
```

**Usage in markdown**:
```liquid
{% raw %}{% include contact-banner.html %}{% endraw %}
```

### Navigation Footer

Edit `docs/_includes/nav_footer_custom.html`:

```html
<footer class="site-footer">
  <p>
    <a href="{{ site.organization.website }}">{{ site.organization.name }}</a>
    {% if site.organization.docs_url %}
      | <a href="{{ site.organization.docs_url }}">Documentation</a>
    {% endif %}
    | <a href="{{ site.url }}/CONTRIBUTING">Contribute</a>
  </p>
</footer>
```

---

## Spell Checking Configuration

### Adding Technical Terms

Edit `cSpell.json` to add words used in your documentation:

```json
{
  "words": [
    "yourorg",
    "customterm",
    "techstack"
  ]
}
```

**When to add words:**
- Technical terms specific to your organization
- Product names
- Programming language keywords (if not already in cSpell dictionaries)
- Acronyms used in your documentation

### Ignoring Words

For words that appear but shouldn't trigger suggestions:

```json
{
  "ignoreWords": [
    "FIXME",
    "TODO"
  ]
}
```

### Ignoring Patterns

To ignore specific markdown patterns:

```json
{
  "ignoreRegExpList": [
    "/`[^`]*`/g",                    // Inline code
    "/```[\\s\\S]*?```/g",           // Code blocks
    "/\\(http[^)]+\\)/g",            // URLs in links
    "/^---[\\s\\S]*?---/gm"          // YAML front matter
  ]
}
```

### Running Spellcheck Locally

```bash
# Check all markdown files
cspell --config ./cSpell.json "**/*.md"

# Check specific file
cspell --config ./cSpell.json docs/my-page.md

# Show only errors (no file names)
cspell --config ./cSpell.json "**/*.md" --no-progress --no-summary
```

### Disabling Spellcheck for a Section

Use HTML comments in markdown:

```markdown
Regular text is checked.

<!-- cspell:disable -->
This text is ignored: foobar baz qux
<!-- cspell:enable -->

Back to normal checking.
```

---

## CI/CD Workflow Customization

### Modifying Spellcheck Workflow

Edit `.github/workflows/spellchecking.yml`:

**Run on additional events:**
```yaml
on:
  pull_request:
  push:
    branches: [main]    # Add this to check on push
```

**Change Node.js version:**
```yaml
- uses: actions/setup-node@v3
  with:
    node-version: 20    # Update version
```

**Add cSpell options:**
```yaml
- run: cspell --config ./cSpell.json "**/*.md" --no-must-find-files
```

### Adding Link Checking

Create `.github/workflows/link-check.yml`:

```yaml
name: Link Check
on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sunday
  workflow_dispatch:      # Manual trigger

jobs:
  link-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Check links
        uses: gaurav-nelson/github-action-markdown-link-check@v1
        with:
          config-file: '.github/link-check-config.json'
```

**Config file** (`.github/link-check-config.json`):
```json
{
  "ignorePatterns": [
    {
      "pattern": "^http://localhost"
    }
  ],
  "timeout": "10s",
  "retryOn429": true
}
```

### Adding Build Validation

Create `.github/workflows/jekyll-build.yml`:

```yaml
name: Jekyll Build Test
on: [pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: '3.1'
          bundler-cache: true
          working-directory: docs

      - name: Build Jekyll site
        run: |
          cd docs
          bundle exec jekyll build
        env:
          JEKYLL_ENV: production
```

---

## Advanced Customization Examples

### Custom Landing Page Layout

Create `docs/_layouts/home.html`:

{% raw %}
```html
---
layout: default
---

<div class="home-hero">
  <h1>{{ site.organization.name }} Open Source Guidelines</h1>
  <p class="lead">{{ site.description }}</p>

  <div class="cta-buttons">
    <a href="/SETUP" class="btn btn-primary">
      Get Started
    </a>
    <a href="https://github.com/{{ site.organization.github_org }}" class="btn btn-secondary">
      View on GitHub
    </a>
  </div>
</div>

{{ content }}

<style>
.home-hero {
  text-align: center;
  padding: 4rem 2rem;
  background: linear-gradient(135deg,
    {{ site.branding.primary_color }},
    {{ site.branding.secondary_color }}
  );
  color: white;
  margin: -2rem -2rem 2rem -2rem;
}

.home-hero .lead {
  font-size: 1.25rem;
  margin: 1rem 0 2rem;
}

.cta-buttons {
  display: flex;
  gap: 1rem;
  justify-content: center;
}
</style>
```
{% endraw %}

Use in `docs/index.md`:
```yaml
---
layout: home
title: Home
nav_exclude: true
---
```

### Organization-Wide Search

To add a custom search scope, edit `docs/_config.yml`:

```yaml
search:
  button: true
  heading_level: 2
  previews: 3
  preview_words_before: 5
  preview_words_after: 10
  tokenizer_separator: /[\s/]+/
```

---

## Testing Your Customizations

### Local Testing Checklist

Before pushing changes:

- [ ] Run `bundle exec jekyll serve` and review site locally
- [ ] Test navigation on mobile viewport (browser DevTools)
- [ ] Verify all custom Liquid variables render correctly
- [ ] Run spellcheck: `cspell --config ./cSpell.json "**/*.md"`
- [ ] Check for console errors in browser DevTools
- [ ] Test search functionality with custom content
- [ ] Verify custom colors have sufficient contrast

### Troubleshooting Custom Styles

**Styles not applying:**
1. Check SCSS syntax (use [SCSS validator](https://www.sassmeister.com/))
2. Clear browser cache (hard refresh: Ctrl+Shift+R)
3. Verify file is in `docs/_sass/custom/` directory
4. Check Jekyll build output for SCSS errors

**Liquid variables not rendering:**
1. Verify variable name matches `_config.yml` exactly
2. Check YAML indentation (use spaces, not tabs)
3. Ensure variable is defined before using in templates

---

## Getting Help

- **Jekyll Issues**: [Jekyll Documentation](https://jekyllrb.com/docs/)
- **Theme Issues**: [Just the Docs Docs](https://just-the-docs.com/)
- **Template Issues**: [GitHub Issues](../../issues)
- **Custom Requests**: [GitHub Discussions](../../discussions)

## Further Reading

- [TEMPLATE_GUIDE.md](TEMPLATE_GUIDE.md) - Design philosophy and decisions
- [Just the Docs Customization](https://just-the-docs.com/docs/customization/)
- [Jekyll Liquid Syntax](https://jekyllrb.com/docs/liquid/)
- [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/)
