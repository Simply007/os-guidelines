# Assets Directory

This directory contains branding assets for your documentation site. Replace the placeholder files with your organization's logo and favicon to customize the site's appearance.

## Quick Start

Replace these two files to brand your documentation site:

1. **`logo.svg`** - Displayed in the site header and navigation
2. **`favicon.svg`** - Browser tab icon

After replacing the files, commit and push your changes. GitHub Pages will rebuild the site automatically (2-3 minutes).

---

## Asset Specifications

### Logo (`logo.svg`)

**Purpose**: Main brand identity shown in the site header.

**Recommended specifications**:
- **Format**: SVG (preferred) or PNG
- **Dimensions**: 200px width × 48px height (4:1 aspect ratio)
- **Aspect ratios**: 4:1 to 5:1 work well
- **File size**: Keep under 50 KB for fast loading
- **Background**: Transparent or solid (matches your color scheme)
- **Colors**: Use your primary brand color or full-color logo

**Where it appears**:
- Site header (top-left corner)
- Navigation sidebar (desktop view)
- Mobile menu (collapsed nav)

**Design tips**:
- Use a horizontal logo variant (vertical logos don't fit well)
- Ensure good contrast against the header background color
- Test visibility in both light and dark modes if supporting both
- Include your organization name in the logo for recognition
- Avoid very thin lines or small text (may not render well at small sizes)

**File naming**:
- Default: `logo.svg`
- Custom: Update `branding.logo_path` in `docs/_config.yml`

**SVG optimization**:
```bash
# Install SVGO (optional, for optimization)
npm install -g svgo

# Optimize your SVG
svgo logo.svg -o logo-optimized.svg
```

---

### Favicon (`favicon.svg`)

**Purpose**: Small icon shown in browser tabs, bookmarks, and shortcuts.

**Recommended specifications**:
- **Format**: SVG (preferred) or PNG
- **Dimensions**: 48px × 48px (square)
- **Aspect ratio**: 1:1 (square)
- **File size**: Keep under 10 KB
- **Background**: Transparent or solid color
- **Colors**: Simple, high-contrast design

**Where it appears**:
- Browser tab (next to page title)
- Bookmark bar
- History
- Mobile home screen shortcuts

**Design tips**:
- Use a simplified version of your logo (icon only, no text)
- Ensure it's recognizable at 16x16px (smallest size)
- Use solid shapes and bold lines (avoid fine details)
- Test on light and dark browser themes
- Consider using your brand's icon mark or monogram

**File naming**:
- Default: `favicon.svg`
- Custom: Update `branding.favicon_path` in `docs/_config.yml`

**Multi-size support** (optional, advanced):

For maximum compatibility, you can provide multiple favicon sizes:

1. Create a `favicon.ico` with multiple sizes embedded:
   ```bash
   # Using ImageMagick
   convert favicon-16.png favicon-32.png favicon-48.png favicon-64.png favicon.ico
   ```

2. Update HTML in `docs/_includes/head_custom.html`:
   ```html
   <link rel="icon" type="image/svg+xml" href="{{ '/assets/favicon.svg' | relative_url }}">
   <link rel="icon" type="image/png" sizes="32x32" href="{{ '/assets/favicon-32.png' | relative_url }}">
   <link rel="icon" type="image/png" sizes="16x16" href="{{ '/assets/favicon-16.png' | relative_url }}">
   <link rel="shortcut icon" href="{{ '/assets/favicon.ico' | relative_url }}">
   ```

---

## File Formats

### SVG (Recommended)

**Advantages**:
- Scalable to any size without quality loss
- Small file size
- Supports transparency
- Can be styled with CSS (color changes)
- Works on all modern browsers

**Best for**:
- Logo with text
- Icon-based favicons
- Responsive designs

**Browser support**: All modern browsers (IE 11+ for favicons)

**Example SVG structure**:
```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 48">
  <rect fill="#FF6B35" width="200" height="48" rx="4"/>
  <text x="100" y="30" font-family="Arial" font-size="24"
        fill="white" text-anchor="middle">Your Org</text>
</svg>
```

### PNG (Alternative)

**Advantages**:
- Universal browser support (including old browsers)
- Good for photographic logos
- Supports transparency

**Best for**:
- Complex gradients
- Photographic elements
- Maximum compatibility

**Requirements**:
- Use transparent background (unless your design requires solid)
- Export at 2x resolution for retina displays (e.g., 400×96 for logo)
- Optimize file size with tools like [TinyPNG](https://tinypng.com/)

**Export settings**:
- Color mode: RGB (not CMYK)
- Bit depth: 24-bit with alpha channel (PNG-24)
- Compression: Maximum (lossless)

---

## How to Replace Assets

### Method 1: GitHub Web Interface (Easiest)

1. Navigate to your repository on GitHub
2. Go to `docs/assets/` directory
3. Click **"Add file" > "Upload files"**
4. Drag and drop your `logo.svg` and `favicon.svg`
5. Scroll down and click **"Commit changes"**
6. Wait 2-3 minutes for GitHub Pages to rebuild

### Method 2: Git Command Line

```bash
# Clone your repository (if not already cloned)
git clone https://github.com/your-org/your-repo.git
cd your-repo

# Copy your assets
cp /path/to/your-logo.svg docs/assets/logo.svg
cp /path/to/your-favicon.svg docs/assets/favicon.svg

# Commit and push
git add docs/assets/
git commit -m "Add organization branding assets"
git push origin main
```

### Method 3: GitHub Desktop

1. Open your repository in GitHub Desktop
2. Navigate to `docs/assets/` in your file system
3. Replace `logo.svg` and `favicon.svg` with your files
4. Return to GitHub Desktop (changes will appear)
5. Add commit message: "Add organization branding assets"
6. Click **"Commit to main"**
7. Click **"Push origin"**

---

## Custom Asset Paths

If you want to use different file names or organize assets differently:

### Using Custom Paths

Edit `docs/_config.yml`:

```yaml
branding:
  logo_path: "assets/images/company-logo.svg"
  favicon_path: "assets/icons/site-icon.svg"
```

### Using External URLs

You can also reference assets from external URLs (CDN, corporate website):

```yaml
branding:
  logo_path: "https://cdn.yourorg.com/logo.svg"
  favicon_path: "https://cdn.yourorg.com/favicon.svg"
```

**Note**: External URLs may slow down page load if the external server is slow.

---

## Design Recommendations

### Color and Contrast

**Logo contrast**:
- Test logo visibility against header background
- Aim for WCAG AA contrast ratio (4.5:1 minimum)
- Consider providing separate light/dark logo variants

**Checking contrast**:
1. Use [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
2. Test with browser dev tools (disable colors, check grayscale)
3. Verify in both light and dark modes (if supporting both)

### Accessibility

**Alt text**: Logo alt text is set in `docs/_config.yml`:
```yaml
organization:
  name: "Your Organization"  # Used as alt text
```

**Semantic HTML**: The logo uses proper semantic markup:
```html
<a href="/" class="site-title">
  <img src="{{ logo_path }}" alt="{{ site.organization.name }}">
</a>
```

**Keyboard navigation**: Logo is accessible via keyboard (Tab key).

### Performance

**Optimizing SVG files**:

Remove unnecessary metadata from SVG files:
```bash
# Using SVGO
svgo logo.svg --multipass --pretty
```

**Optimizing PNG files**:

Compress PNG without visible quality loss:
```bash
# Using ImageMagick
convert logo.png -strip -quality 90 logo-optimized.png

# Or use online tools:
# - TinyPNG: https://tinypng.com/
# - Squoosh: https://squoosh.app/
```

**Lazy loading** (optional):

For below-the-fold images, add lazy loading:
```html
<img src="image.svg" alt="Description" loading="lazy">
```

---

## Common Issues and Solutions

### Logo Not Displaying

**Symptom**: Broken image icon in header

**Solutions**:
1. Verify file exists at `docs/assets/logo.svg`
2. Check file name matches exactly (case-sensitive)
3. Confirm file is committed and pushed to GitHub
4. Wait 2-3 minutes for GitHub Pages rebuild
5. Clear browser cache (Ctrl+Shift+R / Cmd+Shift+R)
6. Check `branding.logo_path` in `_config.yml` matches file location

### Logo Too Large or Too Small

**Symptom**: Logo doesn't fit well in header

**Solutions**:
1. Resize your logo to recommended dimensions (200×48px)
2. Adjust CSS in `docs/_sass/custom/custom.scss`:
   ```scss
   .site-title img {
     max-width: 200px;
     max-height: 48px;
   }
   ```

### Favicon Not Updating

**Symptom**: Old favicon still appears after replacement

**Solutions**:
1. Clear browser cache (favicon is aggressively cached)
2. Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)
3. Try incognito/private browsing window
4. Check browser dev tools > Network tab (should show 200 status)
5. Wait up to 24 hours for browser cache to expire

### SVG Not Rendering

**Symptom**: SVG appears as broken image or doesn't display

**Solutions**:
1. Validate SVG syntax with [SVG Validator](https://validator.w3.org/)
2. Ensure SVG has proper XML declaration:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 48">
   ```
3. Remove Adobe Illustrator metadata (use SVGO)
4. Convert to PNG as fallback if issues persist

### Poor Contrast

**Symptom**: Logo hard to see against background

**Solutions**:
1. Adjust logo colors to match theme
2. Add stroke/outline to logo shapes
3. Change header background color in `_config.yml`:
   ```yaml
   branding:
     accent_color: "#1A1A1D"  # Darker background
   ```
4. Create separate light/dark logo variants

### File Size Too Large

**Symptom**: Slow page load, large repository size

**Solutions**:
1. Optimize SVG with SVGO (can reduce size 50-80%)
2. Compress PNG with TinyPNG or ImageMagick
3. Consider using fewer colors
4. Remove embedded fonts from SVG (use web fonts instead)
5. Use external CDN for assets instead of repository storage

---

## Asset Creation Tools

### Vector Graphics (SVG)

**Professional tools**:
- Adobe Illustrator (industry standard)
- Sketch (Mac only, popular for web design)
- Figma (web-based, free tier available)
- Affinity Designer (one-time purchase)

**Free alternatives**:
- Inkscape (open source, cross-platform)
- Vectr (simple, web-based)
- Boxy SVG (Chrome extension)

### Raster Graphics (PNG)

**Professional tools**:
- Adobe Photoshop
- Affinity Photo

**Free alternatives**:
- GIMP (open source, cross-platform)
- Photopea (web-based Photoshop alternative)
- Paint.NET (Windows only)

### Online Favicon Generators

If you don't have design tools, use online generators:

- [RealFaviconGenerator](https://realfavicongenerator.net/) - Comprehensive favicon generator
- [Favicon.io](https://favicon.io/) - Simple text-to-favicon tool
- [Canva](https://www.canva.com/) - Logo and icon creator (free tier)

---

## Advanced Customization

### Animated Logo (SVG)

Add CSS animations to your SVG logo:

```html
<!-- docs/assets/logo.svg -->
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 48">
  <style>
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
    .animated {
      animation: fadeIn 1s ease-in;
    }
  </style>
  <rect class="animated" fill="#FF6B35" width="200" height="48" rx="4"/>
</svg>
```

### Responsive Logo Variants

Use different logos for desktop vs. mobile:

```html
<!-- docs/_includes/head_custom.html -->
<picture>
  <source media="(max-width: 768px)" srcset="{{ '/assets/logo-mobile.svg' | relative_url }}">
  <img src="{{ '/assets/logo.svg' | relative_url }}" alt="{{ site.organization.name }}">
</picture>
```

### Dark Mode Logo Switching

Automatically switch logo based on user's color scheme:

```scss
// docs/_sass/custom/custom.scss
@media (prefers-color-scheme: dark) {
  .site-title img {
    content: url('/assets/logo-dark.svg');
  }
}

@media (prefers-color-scheme: light) {
  .site-title img {
    content: url('/assets/logo-light.svg');
  }
}
```

---

## Additional Assets

You can add other assets to this directory as needed:

### Screenshots and Diagrams

```
docs/assets/
├── screenshots/
│   ├── setup-step-1.png
│   ├── setup-step-2.png
│   └── setup-step-3.png
└── diagrams/
    ├── architecture.svg
    └── workflow.svg
```

Reference in markdown:
```markdown
![Setup Step 1](assets/screenshots/setup-step-1.png)
```

### Icons and Graphics

```
docs/assets/
└── icons/
    ├── github.svg
    ├── twitter.svg
    └── discord.svg
```

### Custom Fonts (Optional)

```
docs/assets/
└── fonts/
    ├── custom-font.woff2
    └── custom-font.woff
```

Load in `docs/_sass/custom/custom.scss`:
```scss
@font-face {
  font-family: 'CustomFont';
  src: url('/assets/fonts/custom-font.woff2') format('woff2');
  font-weight: normal;
  font-style: normal;
}

body {
  font-family: 'CustomFont', sans-serif;
}
```

---

## Getting Help

**Asset design questions**:
- [Figma Community](https://www.figma.com/community) - Free design resources
- [r/logodesign](https://www.reddit.com/r/logodesign/) - Logo feedback community
- [Dribbble](https://dribbble.com/) - Design inspiration

**Technical issues**:
- [Jekyll Documentation](https://jekyllrb.com/docs/assets/) - Asset handling in Jekyll
- [Just the Docs](https://just-the-docs.com/) - Theme documentation
- [GitHub Issues](../../issues) - Report template-specific issues

**Need professional help**:
- Hire a designer on Fiverr, Upwork, or 99designs
- Contact your organization's marketing/design team
- Engage a design agency for comprehensive branding

---

## Checklist

Before finalizing your assets:

- [ ] Logo is 200×48px (or similar 4:1 aspect ratio)
- [ ] Favicon is 48×48px (square)
- [ ] Both files are optimized (small file size)
- [ ] Logo has good contrast against header background
- [ ] Favicon is recognizable at 16×16px
- [ ] SVG files are valid and render correctly
- [ ] Files are committed and pushed to GitHub
- [ ] Site rebuilds successfully (check Actions tab)
- [ ] Assets display correctly on live site
- [ ] Tested on desktop and mobile
- [ ] Tested in multiple browsers (Chrome, Firefox, Safari)
- [ ] Favicon appears in browser tab

---

**Last updated**: 2026-01-31
