# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **white-labelable Jekyll documentation template** for organizations establishing open source program guidelines. It provides a turnkey solution for documenting OSS best practices, publishing checklists, naming conventions, maintainer duties, and CI/CD automation patterns.

The template is based on guidelines originally developed by Kontent.ai, generalized for universal use by any organization.

## Repository Structure

- `/docs/` - All documentation content and Jekyll site configuration
  - `_config.yml` - Jekyll site configuration with organization/branding blocks
  - `_config.yml.template` - Template version with placeholders for new adopters
  - `_includes/` - Jekyll custom includes (head_custom, nav_footer_custom)
  - `_sass/` - Custom SASS styling
    - `color_schemes/` - Custom dark/light themes (custom_dark.scss, custom_light.scss)
    - `custom/` - Custom style overrides
  - `assets/` - Images, logos, and other static assets
    - `logo.svg` - Placeholder logo (replace with org logo)
    - `favicon.svg` - Placeholder favicon (replace with org icon)
    - `README.md` - Asset replacement guide
  - `ci-and-automation/` - CI/CD guidelines for different tech stacks (PHP, Ruby, Java, Swift, JavaScript, .NET)
  - `examples/` - Example template files
    - `example-sdk-guidelines.md` - SDK development template
    - `example-integration-guidelines.md` - Integration patterns template
    - `example-governance.md` - Governance structure template
  - Core documentation files:
    - `index.md` - Landing page
    - `Checklist-for-publishing-a-new-OS-project.md` - Publishing checklist
    - `Naming-conventions.md` - Repository and package naming
    - `Duties-of-a-Repository-Maintainer.md` - Maintainer responsibilities
    - `SETUP.md` - Deployment guide
    - `CUSTOMIZATION.md` - Advanced customization guide
    - `TEMPLATE_GUIDE.md` - Philosophy and design decisions
- `/PRD.md` - Product Requirements Document
- `/README.md` - Template introduction and quick start
- `cSpell.json` - Spell checking configuration for markdown files
- `.github/workflows/spellchecking.yml` - GitHub Actions workflow for automated spell checking
- `.github/CODEOWNERS` - Template for code ownership

## Development Commands

### Running the Jekyll site locally

```bash
cd docs
bundle install
bundle exec jekyll serve
```

The site will be available at `http://localhost:4000`.

### Spell checking

Run spell check on all markdown files:

```bash
cspell --config ./cSpell.json "**/*.md"
```

The spell checker is configured via `cSpell.json`:
- Add recognized words to the `words` array
- Add words to ignore to the `ignoreWords` array
- Patterns to ignore are defined in `ignoreRegExpList`

## Architecture and Key Concepts

### Configuration-Driven White-Labeling

The template uses a centralized configuration system in `docs/_config.yml`:

**Organization Block**: Defines organization identity
- `name` - Full organization name
- `shortname` - Lowercase identifier for package names
- `email` - Contact email for OSS program
- `github_org` - GitHub organization slug
- `website` - Company website
- `docs_url` - Product documentation URL (optional)

**Branding Block**: Controls visual appearance
- `logo_path` - Path to organization logo
- `favicon_path` - Path to favicon
- `color_scheme` - Theme selection (custom_dark or custom_light)
- `primary_color` - Main brand color (hex)
- `secondary_color` - Accent color (hex)
- `accent_color` - Dark UI elements color (hex)

**Social Block**: Optional social media links
- `twitter` - Twitter/X handle
- `discord_url` - Discord server URL
- `stackoverflow_tag` - Stack Overflow tag

### Liquid Templating

Content files use Liquid variables to reference configuration:
- `{{ site.organization.name }}` - Organization name
- `{{ site.organization.shortname }}` - Short identifier
- `{{ site.organization.email }}` - Contact email
- `{{ site.organization.github_org }}` - GitHub organization
- `{{ site.branding.primary_color }}` - Brand colors
- etc.

### Jekyll Configuration

The site uses:
- **Theme**: `just-the-docs/just-the-docs` (remote theme)
- **Color scheme**: Custom themes (custom_dark, custom_light)
- **Markdown**: GitHub Flavored Markdown (GFM)
- **Edit links**: Enabled, pointing to main branch

### Content Organization

Documentation is organized by topic:
- **Core Guidelines**: Publishing checklist, naming conventions, maintainer duties
- **CI/CD Automation**: Tech-stack-specific guides (generalized from Kontent.ai originals)
- **Examples**: Template files for SDK, integration, and governance documentation
- **Setup Documentation**: SETUP.md, CUSTOMIZATION.md, TEMPLATE_GUIDE.md

### Spell Checking System

Automated spell checking runs on pull requests via GitHub Actions. The workflow:
1. Checks out code
2. Installs Node.js 18
3. Installs cspell globally
4. Runs cspell against all markdown files

Common technical terms are defined in `cSpell.json`.

## Working with the Template

### For Template Users (Organizations Adopting This)

1. **Initial Setup** (see SETUP.md):
   - Copy `_config.yml.template` to `_config.yml`
   - Replace PLACEHOLDER_* values with your organization details
   - Replace `docs/assets/logo.svg` and `docs/assets/favicon.svg`
   - Enable GitHub Pages

2. **Content Customization** (see CUSTOMIZATION.md):
   - Add organization-specific pages
   - Customize CI/CD guides for your tech stack
   - Modify color schemes if needed
   - Add custom components

3. **Maintenance**:
   - Update `cSpell.json` with your technical terms
   - Review and update guidelines periodically
   - Keep Jekyll dependencies up to date

### For Template Maintainers (This Repository)

1. **Adding New Documentation**:
   - Create `.md` files in `docs/` or subdirectories
   - Add YAML front matter with `layout: default` and `nav_order`
   - Use Liquid variables for organization-specific content
   - Add new technical terms to `cSpell.json`

2. **Updating CI/CD Guides**:
   - Keep technical patterns and workflows current
   - Maintain generic examples (avoid org-specific references)
   - Test workflows with latest GitHub Actions versions

3. **Theme Customization**:
   - Color scheme files: `docs/_sass/color_schemes/custom_*.scss`
   - Custom overrides: `docs/_sass/custom/custom.scss`
   - Custom includes: `docs/_includes/*_custom.html`

4. **Testing Changes**:
   - Local Jekyll build: `cd docs && bundle exec jekyll serve`
   - Spell check: `cspell --config ./cSpell.json "**/*.md"`
   - Verify Liquid variables render correctly
   - Check mobile responsiveness

## Design Principles

1. **Minimal Configuration, Maximum Impact**: Single config file controls all branding
2. **Generalization**: Content applicable to any organization, not product-specific
3. **Progressive Disclosure**: Quick start → standard setup → advanced customization
4. **Proven Practices**: Based on real-world Kontent.ai OSS program experience

## Attribution

This template is based on open source guidelines originally developed by [Kontent.ai](https://github.com/kontent-ai/os-guidelines). Content has been generalized and adapted for universal use under the MIT License.

## Repository Governance

- **Main branch**: `main`
- **Code owners**: See `.github/CODEOWNERS` (template, needs customization)
- **Issues**: Use GitHub Issues for bugs and feature requests
- **Discussions**: Use GitHub Discussions for questions and ideas

## Key Files for AI Assistants

When making changes, these files are most commonly modified:

**Configuration:**
- `docs/_config.yml` - Primary configuration
- `cSpell.json` - Spell check dictionary

**Core Content:**
- `docs/Checklist-for-publishing-a-new-OS-project.md`
- `docs/Naming-conventions.md`
- `docs/Duties-of-a-Repository-Maintainer.md`

**Documentation:**
- `docs/SETUP.md` - Setup instructions
- `docs/CUSTOMIZATION.md` - Customization guide
- `docs/TEMPLATE_GUIDE.md` - Design philosophy
- `README.md` - Template introduction
- `PRD.md` - Product requirements

**Styling:**
- `docs/_sass/color_schemes/custom_dark.scss`
- `docs/_sass/color_schemes/custom_light.scss`
- `docs/_sass/custom/custom.scss`

**Assets:**
- `docs/assets/logo.svg` - Placeholder logo
- `docs/assets/favicon.svg` - Placeholder favicon
