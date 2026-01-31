---
layout: default
title: Setup Guide
nav_order: 2
---

# Setup Guide

This guide walks you through deploying the Open Source Guidelines template to your organization's GitHub Pages in under 15 minutes.

## Prerequisites

Before you begin, ensure you have:

- **GitHub Account** with permissions to create repositories in your organization
- **Git** installed locally (for local testing)
- **Ruby 2.7+** and **Bundler** (only needed for local testing)
- **Text Editor** for editing YAML configuration files

## Quick Deployment (15 minutes)

### Step 1: Create Your Repository (2 minutes)

1. Click the **"Use this template"** button at the top of this repository
2. Choose your organization as the owner
3. Name your repository (e.g., `os-guidelines`, `open-source-docs`)
4. Set visibility to **Public** (required for GitHub Pages on free plans)
5. Click **"Create repository from template"**

### Step 2: Configure Your Organization (5 minutes)

1. Navigate to `docs/_config.yml` in your new repository
2. Click the **pencil icon** to edit the file
3. **First, configure the site URL settings** (see [URL Configuration](#url-configuration) below for details):

```yaml
# For GitHub Pages project site (username.github.io/repo-name/)
baseurl: "/your-repo-name"              # Replace with YOUR repository name
url: "https://username.github.io"       # Replace with YOUR GitHub username/org

# For custom domain at root, use:
# baseurl: ""
# url: "https://yourdomain.com"
```

4. Update the `organization` block with your details:

```yaml
organization:
  name: "Acme Corporation"                    # Your full organization name
  shortname: "acme"                           # Used in package names (lowercase, no spaces)
  email: "opensource@acme.com"                # Your OSS program contact email
  github_org: "acme-corp"                     # Your GitHub organization slug
  website: "https://acme.com"                 # Your main company website
  docs_url: "https://docs.acme.com"           # (Optional) Product documentation URL
```

5. Update the `branding` block:

```yaml
branding:
  logo_path: "assets/logo.svg"                # Path to your logo (see Step 3)
  favicon_path: "assets/favicon.svg"          # Path to your favicon (see Step 3)
  color_scheme: "custom_dark"                 # Options: custom_dark, custom_light
  primary_color: "#FF6B35"                    # Your main brand color (hex)
  secondary_color: "#004E89"                  # Your accent color (hex)
  accent_color: "#1A1A1D"                     # Dark backgrounds/borders (hex)
```

6. (Optional) Update the `social` block:

```yaml
social:
  twitter: "acmecorp"                         # Twitter/X handle (without @)
  discord_url: "https://discord.gg/abc123"    # Discord server invite URL
  stackoverflow_tag: "acme"                   # Stack Overflow tag
```

7. **Commit changes** directly to the `main` branch

### Step 3: Replace Placeholder Assets (5 minutes)

The template includes placeholder logo and favicon files. Replace them with your organization's branding:

#### Option A: Upload via GitHub Web Interface

1. Navigate to `docs/assets/` in your repository
2. Click **"Add file" > "Upload files"**
3. Upload your logo file (recommended: SVG format, 200x48px)
4. Name it `logo.svg` (or update `branding.logo_path` in `_config.yml`)
5. Upload your favicon file (recommended: SVG format, 48x48px)
6. Name it `favicon.svg` (or update `branding.favicon_path` in `_config.yml`)
7. **Commit changes**

#### Option B: Using Git Locally

```bash
# Clone your repository
git clone https://github.com/your-org/your-repo.git
cd your-repo

# Replace placeholder assets
cp /path/to/your-logo.svg docs/assets/logo.svg
cp /path/to/your-favicon.svg docs/assets/favicon.svg

# Commit and push
git add docs/assets/
git commit -m "Add organization branding assets"
git push origin main
```

#### Asset Specifications

- **Logo**: Recommended 200x48px (or 4:1 aspect ratio), SVG or PNG
- **Favicon**: Recommended 48x48px (square), SVG or PNG
- See [docs/assets/README.md](assets/README.md) for detailed specifications

### Step 4: Enable GitHub Pages (2 minutes)

1. Go to your repository's **Settings** tab
2. Scroll to **"Pages"** in the left sidebar
3. Under **"Build and deployment"**:
   - Source: **Deploy from a branch**
   - Branch: **`main`**
   - Folder: **`/docs`**
4. Click **"Save"**
5. Wait 1-2 minutes for the initial build

### Step 5: Verify Deployment (1 minute)

1. Refresh the Settings > Pages page
2. You'll see: **"Your site is live at `https://[your-org].github.io/[repo-name]`"**
3. Click the URL to visit your documentation site
4. Verify:
   - Your organization name appears in the header
   - Your logo displays correctly
   - Navigation works
   - Search functionality works

**Congratulations!** Your open source guidelines are now live.

---

## URL Configuration

### Understanding baseurl

The `baseurl` setting is **critical** for GitHub Pages to load assets (CSS, JavaScript, images) correctly. Different deployment scenarios require different configurations:

#### Scenario 1: GitHub Pages Project Site (Most Common)

Your site URL: `https://username.github.io/repo-name/`

**Configuration:**
```yaml
baseurl: "/repo-name"                    # Your repository name with leading slash
url: "https://username.github.io"        # Your GitHub Pages domain
```

**Example:**
- Repository: `acme-corp/os-guidelines`
- Site URL: `https://acme-corp.github.io/os-guidelines/`
- Configuration:
  ```yaml
  baseurl: "/os-guidelines"
  url: "https://acme-corp.github.io"
  ```

#### Scenario 2: GitHub Pages User/Organization Site

Your site URL: `https://username.github.io/` (repository named `username.github.io`)

**Configuration:**
```yaml
baseurl: ""                              # Empty for root deployment
url: "https://username.github.io"        # Your GitHub Pages domain
```

#### Scenario 3: Custom Domain at Root

Your site URL: `https://opensource.acme.com/`

**Configuration:**
```yaml
baseurl: ""                              # Empty for root deployment
url: "https://opensource.acme.com"       # Your custom domain
```

#### Scenario 4: Custom Domain with Path

Your site URL: `https://acme.com/docs/`

**Configuration:**
```yaml
baseurl: "/docs"                         # Path with leading slash
url: "https://acme.com"                  # Domain without path
```

### Why baseurl Matters

The Jekyll theme uses `{{ site.baseurl }}` to generate URLs for assets:
- CSS files: `{{ site.baseurl }}/assets/css/...`
- JavaScript: `{{ site.baseurl }}/assets/js/...`
- Images: `{{ site.baseurl }}/assets/...`

**Incorrect baseurl causes 404 errors** for these assets, breaking styling and functionality.

### Verifying Your Configuration

After deploying, check your browser console (F12):
- ✅ **No 404 errors**: Configuration is correct
- ❌ **404 errors for CSS/JS**: Check your baseurl setting

**Common mistake**: Empty baseurl on a project site causes assets to load from wrong location:
- Wrong: `https://username.github.io/assets/...` (404)
- Correct: `https://username.github.io/repo-name/assets/...` (200)

---

## Local Testing (Optional)

### Local Development with baseurl

When testing locally, you have two options:

#### Option A: Override baseurl for Local Testing (Recommended)

```bash
cd docs
bundle install

# Override baseurl to empty for local development
bundle exec jekyll serve --baseurl ""

# Site available at: http://localhost:4000/
```

This works because localhost serves from root, not a subdirectory.

#### Option B: Test with Production baseurl

```bash
cd docs
bundle install

# Use production baseurl (from _config.yml)
bundle exec jekyll serve

# Site available at: http://localhost:4000/your-repo-name/
```

This simulates production environment and helps catch URL issues before deployment.

#### Option C: Use Development Config File

Create `docs/_config_dev.yml`:

```yaml
# Development overrides
baseurl: ""
url: "http://localhost:4000"
```

Then serve with both configs:

```bash
bundle exec jekyll serve --config _config.yml,_config_dev.yml

# Site available at: http://localhost:4000/
```

The second config file overrides settings from the first.

---

## Local Testing (Detailed)

To preview changes locally before pushing to GitHub:

### Install Dependencies

```bash
# Navigate to the docs directory
cd docs

# Install Ruby dependencies
bundle install
```

### Run Jekyll Locally

```bash
# Start the Jekyll development server
bundle exec jekyll serve

# Site will be available at http://localhost:4000
```

Jekyll will watch for file changes and automatically rebuild. Press `Ctrl+C` to stop the server.

### Common Local Testing Issues

**Issue**: `bundle: command not found`
**Solution**: Install Bundler: `gem install bundler`

**Issue**: `Ruby version too old`
**Solution**: Install Ruby 2.7+ using [rbenv](https://github.com/rbenv/rbenv) or [RVM](https://rvm.io/)

**Issue**: `Could not find gem 'github-pages'`
**Solution**: Run `bundle install` in the `docs/` directory

---

## Configuration Reference

### Organization Block

| Field | Required | Description | Example |
|-------|----------|-------------|---------|
| `name` | Yes | Full organization name | `"Acme Corporation"` |
| `shortname` | Yes | Lowercase identifier for package names | `"acme"` |
| `email` | Yes | Open source program contact email | `"opensource@acme.com"` |
| `github_org` | Yes | GitHub organization slug | `"acme-corp"` |
| `website` | Yes | Main company website | `"https://acme.com"` |
| `docs_url` | No | Product documentation URL | `"https://docs.acme.com"` |

### Branding Block

| Field | Required | Description | Example |
|-------|----------|-------------|---------|
| `logo_path` | Yes | Path to logo (relative to `docs/`) | `"assets/logo.svg"` |
| `favicon_path` | Yes | Path to favicon (relative to `docs/`) | `"assets/favicon.svg"` |
| `color_scheme` | Yes | Theme to use | `"custom_dark"` or `"custom_light"` |
| `primary_color` | Yes | Main brand color (hex) | `"#FF6B35"` |
| `secondary_color` | Yes | Accent color (hex) | `"#004E89"` |
| `accent_color` | Yes | Dark UI elements (hex) | `"#1A1A1D"` |

### Social Block

| Field | Required | Description | Example |
|-------|----------|-------------|---------|
| `twitter` | No | Twitter/X handle (without @) | `"acmecorp"` |
| `discord_url` | No | Discord server invite URL | `"https://discord.gg/abc123"` |
| `stackoverflow_tag` | No | Stack Overflow tag | `"acme"` |

---

## Custom Domain (Optional)

To use a custom domain (e.g., `opensource.acme.com`):

1. Add a `CNAME` file to `docs/` containing your custom domain:
   ```
   opensource.acme.com
   ```

2. In your DNS provider, add a CNAME record:
   - **Name**: `opensource` (or your subdomain)
   - **Value**: `[your-org].github.io`
   - **TTL**: 3600 (or default)

3. In GitHub Settings > Pages:
   - Enter your custom domain in the **"Custom domain"** field
   - Wait for DNS check to complete
   - Enable **"Enforce HTTPS"** (recommended)

**Note**: DNS propagation can take 24-48 hours.

---

## Troubleshooting

### Site Not Building

**Symptom**: GitHub Pages shows "Build failed" or site doesn't update

**Solutions**:
1. Check the **Actions** tab for build errors
2. Verify `_config.yml` syntax (use a [YAML validator](https://www.yamllint.com/))
3. Ensure `/docs` folder exists and contains `_config.yml`
4. Check that branch is set to `main` in Settings > Pages

### Logo/Favicon Not Displaying

**Symptom**: Broken image icons or 404 errors

**Solutions**:
1. Verify files exist at the paths specified in `_config.yml`
2. Check file names match exactly (case-sensitive)
3. Ensure image files are committed to the repository
4. Clear browser cache and hard refresh (Ctrl+Shift+R / Cmd+Shift+R)

### Colors Not Updating

**Symptom**: Changes to `primary_color` don't appear on site

**Solutions**:
1. Ensure you're editing `docs/_config.yml` (not root `_config.yml`)
2. Wait 2-3 minutes for GitHub Pages to rebuild
3. Clear browser cache
4. Check that `color_scheme` matches an existing scheme file

### Search Not Working

**Symptom**: Search box doesn't return results

**Solutions**:
1. Wait for initial site build to complete (search index generation)
2. Verify `search_enabled: true` in `_config.yml`
3. Check browser console for JavaScript errors
4. Try rebuilding site (push a small change to trigger rebuild)

### Liquid Syntax Errors

**Symptom**: `{% raw %}{{ site.organization.name }}{% endraw %}` appears as literal text

**Solutions**:
1. Ensure you're using double curly braces: `{% raw %}{{ }}{% endraw %}` (not single `{ }`)
2. Check variable name matches `_config.yml` exactly
3. Verify indentation in YAML (spaces, not tabs)

---

## Next Steps

Now that your site is deployed:

1. **Review Content**: Read through all pages and remove/customize sections that don't apply to your organization
2. **Add Custom Pages**: See [CUSTOMIZATION.md](CUSTOMIZATION.md) for instructions on adding organization-specific guidelines
3. **Customize Styling**: Learn how to adjust colors and themes in [CUSTOMIZATION.md](CUSTOMIZATION.md)
4. **Set Up Spellchecking**: Configure `cSpell.json` to include your organization's technical terms
5. **Invite Contributors**: Add maintainers to your repository to keep documentation updated

---

## Support

- **Documentation Issues**: [Report on GitHub](../../issues)
- **Feature Requests**: [GitHub Discussions](../../discussions)
- **Template Questions**: See [TEMPLATE_GUIDE.md](TEMPLATE_GUIDE.md)
- **Jekyll Help**: [Jekyll Documentation](https://jekyllrb.com/docs/)
- **Just the Docs Theme**: [Theme Documentation](https://just-the-docs.com/)
