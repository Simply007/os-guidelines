---
layout: default
title: Template Guide
nav_order: 4
---

# Template Guide

This guide explains the philosophy, design decisions, and best practices behind the Open Source Guidelines template. Whether you're evaluating the template or looking to contribute improvements, this document will help you understand how it works and why.

## Table of Contents

1. [Design Philosophy](#design-philosophy)
2. [What Changed from Kontent.ai Original](#what-changed-from-kontentai-original)
3. [Customization Levels](#customization-levels)
4. [Best Practices for Adopters](#best-practices-for-adopters)
5. [Contributing to the Template](#contributing-to-the-template)

---

## Design Philosophy

### Core Principles

The Open Source Guidelines template is built on three fundamental principles:

**1. Minimal Configuration, Maximum Impact**

The template should work out-of-the-box with minimal setup. Organizations should be able to deploy a professional-looking documentation site within 15 minutes by only editing a single configuration file (`_config.yml`).

**2. Generalization Over Specificity**

All content is written to be product-agnostic and framework-agnostic. The template focuses on universal open source best practices rather than implementation-specific details. This ensures the guidelines remain relevant across different types of organizations, products, and technology stacks.

**3. Progressive Disclosure**

The template supports multiple levels of customization, from basic branding (5 minutes) to complete content overhaul (days). This allows organizations to start simple and gradually customize as needed, without being overwhelmed by options upfront.

### Technical Architecture Decisions

**Jekyll + Just the Docs**

We chose Jekyll with the Just the Docs theme for several reasons:

- **Zero-cost hosting** via GitHub Pages (no server setup required)
- **Built-in search** powered by lunr.js (no external service dependencies)
- **Markdown-first** content authoring (familiar to developers)
- **Git-based workflow** (version control, pull requests, collaboration)
- **Liquid templating** for variable substitution (organization name, branding)
- **SCSS styling** for easy theme customization without JavaScript

**Configuration-Driven Branding**

Rather than requiring file edits throughout the codebase, all organization-specific information is centralized in `docs/_config.yml`. Liquid variables like `{{ site.organization.name }}` allow content to dynamically adapt to each organization.

**Modular Content Structure**

Documentation is organized into discrete, independent pages rather than a monolithic guide. This allows organizations to:
- Remove pages that don't apply
- Add custom pages without restructuring
- Reorder navigation via `nav_order` front matter
- Create nested hierarchies with parent/child relationships

---

## What Changed from Kontent.ai Original

The template is derived from Kontent.ai's internal developer resources site. Here's what changed during the templatization process:

### Files Removed (7 product-specific pages)

These pages were tightly coupled to Kontent.ai's specific product and couldn't be meaningfully generalized:

1. **Guidelines-for-Kontent.ai-related-tools.md** - Specific to Kontent.ai's Delivery API
2. **Guidelines-for-SDK-developers.md** - API contract documentation (product-specific)
3. **Guidelines-for-GitHub-permissions-in-Kontent-ai-organization.md** - Internal permissions policy
4. **New-product-feature.md** - Internal new feature workflow
5. **Related-Resources.md** - Links to Kontent.ai-specific resources
6. **integrations/** directory - Custom element and integration guidelines for Kontent.ai platform
7. **examples/custom-element-sample-css/** - Product-specific code samples

**Replacement**: A generic `docs/examples/example-sdk-guidelines.md` template is provided that organizations can customize for their own API requirements.

### Files Kept and Generalized (3 core guidelines)

These pages contain universal open source best practices and were adapted for any organization:

1. **Checklist-for-publishing-a-new-OS-project.md**
   - Removed: Kontent.ai-specific references, internal approval processes
   - Added: Liquid variables for organization name, generic examples
   - Result: Universal checklist for publishing open source projects

2. **Naming-conventions.md**
   - Removed: Kontent.ai naming patterns, product-specific examples
   - Added: Placeholder examples using `{{ site.organization.shortname }}`
   - Result: Template for establishing organizational naming standards

3. **Duties-of-a-Repository-Maintainer.md**
   - Removed: References to internal teams, specific tools
   - Added: Generic maintenance best practices
   - Result: Role definition applicable to any organization

### CI/CD Documentation (8 tech-specific guides)

These files were kept with minimal changes because they contain universal CI/CD patterns:

- **ci-and-automation/ci-and-automation.md** - Overview page
- **ci-and-automation/dotnet-GitHub-Actions-Guidelines.md** - .NET CI/CD patterns
- **ci-and-automation/Java-GitHub-Actions-Guidelines.md** - Java CI/CD patterns
- **ci-and-automation/JavaScript-GitHub-Actions-Guidelines.md** - JavaScript CI/CD patterns
- **ci-and-automation/PHP-GitHub-Actions-Guidelines.md** - PHP CI/CD patterns
- **ci-and-automation/Ruby-GitHub-Actions-Guidelines.md** - Ruby CI/CD patterns
- **ci-and-automation/Swift-GitHub-Actions-Guidelines.md** - Swift CI/CD patterns

**Changes**: Replaced package names and organization references with Liquid variables.

### New Template Documentation (4 meta-guides)

These files explain how to use the template itself:

1. **README.md** (root) - Template introduction and quick start
2. **docs/SETUP.md** - Deployment guide with step-by-step instructions
3. **docs/CUSTOMIZATION.md** - Advanced customization guide
4. **docs/TEMPLATE_GUIDE.md** - This file (philosophy and design decisions)

### Configuration System (New)

The original Kontent.ai site used hardcoded values throughout. The template introduces a centralized configuration system:

**`docs/_config.yml` additions:**
```yaml
organization:
  name: "Your Organization"
  shortname: "yourorg"
  email: "opensource@yourorg.com"
  github_org: "your-org"
  website: "https://yourorg.com"
  docs_url: "https://docs.yourorg.com"

branding:
  logo_path: "assets/logo.svg"
  favicon_path: "assets/favicon.svg"
  color_scheme: "custom_dark"
  primary_color: "#FF6B35"
  secondary_color: "#004E89"
  accent_color: "#1A1A1D"

social:
  twitter: "yourorg"
  discord_url: "https://discord.gg/..."
  stackoverflow_tag: "yourorg"
```

All content pages use these variables via Liquid templating:
```liquid
{{ site.organization.name }}
{{ site.organization.email }}
{{ site.branding.primary_color }}
```

---

## Customization Levels

The template supports four levels of customization, each building on the previous:

### Level 1: Minimal (5 minutes)

**Goal**: Deploy the site with your organization's name and branding.

**Steps**:
1. Edit `docs/_config.yml` organization block
2. Replace `docs/assets/logo.svg` and `docs/assets/favicon.svg`
3. Enable GitHub Pages

**Result**: A live documentation site with your branding but generic content.

**Best for**: Quick evaluation, proof-of-concept, internal documentation.

### Level 2: Standard (15 minutes)

**Goal**: Add social links and customize colors to match your brand.

**Steps**:
1. Complete Level 1
2. Update `branding.primary_color`, `secondary_color`, `accent_color` in `_config.yml`
3. Add `social` block with Twitter, Discord, Stack Overflow links
4. Test color contrast for accessibility

**Result**: Fully branded site ready for public use with generic guidelines.

**Best for**: Most organizations starting an open source program.

### Level 3: Content Customization (1-3 hours)

**Goal**: Tailor guidelines to your organization's specific needs.

**Steps**:
1. Complete Level 2
2. Review all documentation pages:
   - Remove sections that don't apply
   - Add organization-specific policies
   - Update examples with your naming conventions
3. Add custom pages:
   - Security policy
   - Code of conduct
   - Contribution guidelines
4. Update `cSpell.json` with your technical terms
5. Customize CI/CD guides for your tech stack

**Result**: Guidelines that reflect your organization's open source practices.

**Best for**: Organizations with established open source processes, specific compliance requirements, or unique workflows.

### Level 4: Advanced (3+ hours)

**Goal**: Deep customization of theme, layout, and functionality.

**Steps**:
1. Complete Level 3
2. Modify SCSS files:
   - Create custom color schemes in `docs/_sass/color_schemes/`
   - Add custom styles in `docs/_sass/custom/custom.scss`
3. Create custom layouts:
   - Add templates in `docs/_layouts/`
   - Create custom includes in `docs/_includes/`
4. Extend functionality:
   - Add new GitHub Actions workflows
   - Implement custom search configurations
   - Create interactive components

**Result**: A fully customized documentation site with unique design and features.

**Best for**: Organizations with design resources, specific branding requirements, or advanced documentation needs.

---

## Best Practices for Adopters

### Before You Deploy

**1. Review All Content**

Don't blindly deploy the template. Read through each page and ask:
- Does this guideline apply to our organization?
- Do we have additional requirements to add?
- Are the examples relevant to our tech stack?

**2. Plan Your Customization Level**

Decide upfront how much you'll customize:
- **Minimal**: Good for initial launch, iterate later
- **Standard**: Recommended starting point for most organizations
- **Content**: Do this before publicizing the site externally
- **Advanced**: Only if you have design resources and specific needs

**3. Set Up Local Testing**

Install Ruby and Bundler to test changes locally before pushing:
```bash
cd docs
bundle install
bundle exec jekyll serve
```

This prevents broken deployments and allows rapid iteration.

### Content Management

**1. Start with What You Have**

Rather than removing guidelines you don't follow yet, keep them as aspirational goals. Use comments like:

```markdown
> **Note**: Our organization is working toward implementing this guideline. Current status: [In Progress / Planned]
```

**2. Link to External Policies**

If you already have policies elsewhere, link to them rather than duplicating:

```markdown
For our organization's security policy, see [Security Policy](https://yourorg.com/security).
```

**3. Use Versioning for Major Changes**

If you make significant updates to guidelines, consider versioning:
- Add a "Last updated" date to pages
- Use git tags for major guideline revisions
- Document breaking changes in commit messages

**4. Keep Examples Generic**

When adding examples, use placeholders:
- `yourorg` instead of actual organization name
- `your-repo` instead of specific repository
- `your-package` instead of package names

This makes guidelines easier to understand and follow.

### Maintenance

**1. Assign Ownership**

Designate a team or individual responsible for:
- Reviewing pull requests to documentation
- Updating guidelines as practices evolve
- Responding to questions and feedback

Add this to `docs/_config.yml`:
```yaml
maintainers:
  - name: "DevRel Team"
    email: "devrel@yourorg.com"
```

**2. Schedule Regular Reviews**

Set a recurring calendar reminder to review documentation:
- **Quarterly**: Check for outdated links, update examples
- **Annually**: Comprehensive review of all guidelines
- **After major changes**: Update affected documentation immediately

**3. Enable Spell Checking**

The template includes automated spell checking via GitHub Actions. Keep `cSpell.json` updated:
- Add new technical terms as they arise
- Remove obsolete terms when deprecating features
- Review spell check failures in pull requests

**4. Monitor Usage**

Track how developers use the documentation:
- Enable Google Analytics (add tracking ID to `_config.yml`)
- Review GitHub Pages traffic in repository Insights
- Solicit feedback via GitHub Discussions or internal surveys

### Branding Consistency

**1. Asset Guidelines**

Maintain brand consistency with proper assets:
- **Logo**: Use vector format (SVG) when possible
- **Favicon**: Provide multiple sizes (16x16, 32x32, 48x48, 64x64)
- **Colors**: Document color codes in your brand guidelines
- **Typography**: Ensure font usage matches brand standards

**2. Color Accessibility**

Test color combinations for accessibility:
- Use [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
- Target WCAG AA (4.5:1 ratio for normal text)
- Test with color blindness simulators

**3. Multi-Theme Support**

Consider offering both light and dark themes:
```yaml
branding:
  color_scheme: "custom_dark"  # or "custom_light"
```

Users appreciate having a choice based on their environment and preferences.

### Content Strategy

**1. Progressive Disclosure**

Structure content from general to specific:
- Overview page with high-level guidelines
- Detailed guides for specific scenarios
- Reference documentation for technical details

**2. Consistent Terminology**

Define key terms upfront and use them consistently:
- SDK vs. library vs. package
- Repository vs. project vs. codebase
- Maintainer vs. contributor vs. owner

**3. Clear Call-to-Actions**

Every page should have a clear next step:
- "Learn more in [Next Topic]"
- "Need help? Contact us at [email]"
- "Ready to publish? See [Checklist]"

**4. Link Liberally**

Connect related concepts with inline links:
- Link to other pages in your documentation
- Link to external resources (GitHub docs, Jekyll docs)
- Link to your product documentation when relevant

---

## Contributing to the Template

If you've made improvements to the template that could benefit others, we encourage you to contribute back.

### What Makes a Good Contribution

**Generic, Not Specific**

Contributions should be applicable to most organizations:
- Universal best practices (good)
- Product-specific workflows (not suitable)

**Well-Documented**

Include clear explanations:
- What problem does this solve?
- How do organizations use this feature?
- Are there configuration options?

**Backward Compatible**

Don't break existing adopters:
- Additive changes (new pages, features) are great
- Breaking changes (removing pages, changing config structure) require discussion

### Types of Contributions

**Content Improvements**
- Fix typos, grammatical errors, unclear explanations
- Add missing best practices
- Improve examples for clarity

**New Guidelines**
- Additional tech stack CI/CD guides (Go, Rust, Python, etc.)
- New topic pages (security, accessibility, licensing)
- Template variations for different documentation needs

**Technical Enhancements**
- Improved styling and accessibility
- New Jekyll features or plugins
- Better GitHub Actions workflows

**Documentation**
- Expand SETUP.md, CUSTOMIZATION.md, or this guide
- Add troubleshooting tips
- Create video tutorials or walkthroughs

### Contribution Process

1. **Open an issue first** - Discuss significant changes before implementing
2. **Fork the repository** - Make changes in your fork
3. **Test locally** - Ensure Jekyll builds without errors
4. **Submit a pull request** - Reference the issue, explain your changes
5. **Respond to feedback** - Address review comments promptly

### Code of Conduct

We follow the [Contributor Covenant Code of Conduct](https://www.contributor-covenant.org/). Be respectful, inclusive, and constructive in all interactions.

---

## Design Decisions Deep Dive

### Why Jekyll over Other Static Site Generators?

**Alternatives considered**: Hugo, Gatsby, Docusaurus, MkDocs

**Why Jekyll won**:
- Native GitHub Pages support (zero configuration)
- Mature theme ecosystem (Just the Docs is excellent)
- Ruby is widely available (comes pre-installed on macOS)
- Liquid templating is simple and powerful
- No JavaScript build step required

**Trade-offs**:
- Slower build times than Hugo (not an issue for documentation sites)
- Ruby dependency (vs. Go, Node.js)
- Less modern than Gatsby/Docusaurus (but simpler)

### Why Just the Docs Theme?

**Alternatives considered**: Minimal Mistakes, Primer, Tale

**Why Just the Docs won**:
- Built-in search (no external dependencies)
- Excellent navigation system (parent/child, ordering)
- Clean, professional design
- Highly customizable via SCSS
- Active maintenance and community

**Trade-offs**:
- Less visual flair than some themes (but more professional)
- Limited layout variations (but consistent experience)

### Why Centralized Configuration?

**Alternative approach**: Hardcoded values, separate data files

**Why centralized config won**:
- Single source of truth for organization info
- Clear onboarding (edit one file to deploy)
- Easy to validate (YAML linting)
- Familiar to Jekyll users

**Trade-offs**:
- Large `_config.yml` file (but well-organized)
- Requires Jekyll restart for changes (acceptable)

### Why Generic Content?

**Alternative approach**: Provide multiple variations (SaaS, Enterprise, etc.)

**Why generic won**:
- Easier to maintain (single codebase)
- Forces universal best practices
- Reduces decision fatigue for adopters

**Trade-offs**:
- Less immediately applicable (requires customization)
- More work for adopters to tailor content

---

## Template Evolution

### Version History

**v1.0** (Initial release)
- Converted from Kontent.ai internal docs
- Added configuration system
- Created setup and customization guides

**Future Plans**:
- Additional CI/CD guides (Python, Go, Rust)
- GitHub Actions workflow templates
- Interactive configuration wizard
- Example implementations gallery

### Feedback and Improvements

We continuously improve the template based on adopter feedback. Common requests:

**Most requested features**:
1. More CI/CD tech stack guides
2. Localization/internationalization support
3. Additional theme variations
4. Integration with external tools (Notion, Confluence)

**Known limitations**:
1. Jekyll build time on large sites (workaround: use incremental builds)
2. Search quality on very large sites (consider external search service)
3. Limited mobile optimization (theme improvement needed)

### Staying Up-to-Date

If you've deployed the template, here's how to stay current:

**Watch the Repository**
- Click "Watch" on the template repo
- Choose "Custom" > "Releases" to get notified of new versions

**Review Changelogs**
- Check release notes before updating
- Test updates in a separate branch first

**Selective Updates**
- You don't need to adopt every change
- Cherry-pick improvements relevant to your organization
- Maintain your customizations

---

## Conclusion

The Open Source Guidelines template is designed to be flexible, maintainable, and professional. By understanding its design philosophy and best practices, you can effectively deploy and customize it for your organization.

Key takeaways:
- Start with minimal customization, iterate based on feedback
- Keep content generic but add organization-specific details where needed
- Maintain documentation as your open source program evolves
- Contribute improvements back to help the broader community

Questions or feedback? Open an issue or discussion on the template repository.

---

## Further Reading

- [SETUP.md](SETUP.md) - Step-by-step deployment guide
- [CUSTOMIZATION.md](CUSTOMIZATION.md) - Advanced customization techniques
- [Jekyll Documentation](https://jekyllrb.com/docs/) - Learn more about Jekyll
- [Just the Docs Documentation](https://just-the-docs.com/) - Theme customization reference
