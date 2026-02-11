# Open Source Guidelines Template

A turnkey Jekyll documentation template for establishing professional open source program guidelines. Deploy comprehensive OSS best practices to your organization in under 15 minutes.

## Features

- **Configuration-Driven White-Labeling**: Customize organization name, colors, and logos in one YAML file
- **Pre-Written Best Practices**: Publishing checklists, naming conventions, and maintainer duties based on proven patterns
- **Tech-Stack CI/CD Guides**: Ready-to-use automation guidelines for .NET, Java, JavaScript, PHP, Ruby, and Swift
- **Professional Design**: Modern, responsive, accessible documentation site using Just the Docs theme
- **GitHub Pages Ready**: Zero-config deployment to your organization's GitHub Pages
- **Automated Spellchecking**: Built-in CI workflow ensures documentation quality

## Quick Start

1. **Use this template** - Click "Use this template" button to create your repository
2. **Configure your organization** - Edit `docs/_config.yml` with your org details
3. **Replace placeholder assets** - Add your logo to `docs/assets/logo.svg` and favicon to `docs/assets/favicon.svg`
4. **Enable GitHub Pages** - Go to Settings > Pages > Deploy from branch `main` > Folder `/docs`
5. **Visit your site** - `https://[your-org].github.io/[repo-name]`

See [SETUP.md](docs/SETUP.md) for detailed deployment instructions.

## What's Included

- Publishing checklist for new open source projects
- Repository naming conventions
- Maintainer responsibilities and duties
- CI/CD automation guides for 6+ tech stacks
- Automated spellchecking workflow
- Customizable dark/light themes
- Example templates for SDK, integration, and governance guidelines

## Documentation

- **[SETUP.md](docs/SETUP.md)** - Step-by-step deployment guide
- **[CUSTOMIZATION.md](docs/CUSTOMIZATION.md)** - Advanced theming and content customization
- **[TEMPLATE_GUIDE.md](docs/TEMPLATE_GUIDE.md)** - Design philosophy and content decisions
- **[PRD.md](PRD.md)** - Complete product requirements and roadmap

## Customization Levels

| Time Investment | What You Get |
|----------------|--------------|
| **5 minutes** | Quick rebrand (change colors in config) |
| **15 minutes** | Full rebrand (add logo, update org details) |
| **1-3 hours** | Custom content (add organization-specific pages) |
| **3+ hours** | Advanced theming (custom SCSS, layouts) |

## Spellchecking

This template includes automated spellchecking via GitHub Actions. To run spellcheck locally:

```bash
cspell --config ./cSpell.json "**/*.md"
```

Configure spellcheck behavior in [cSpell.json](./cSpell.json):
- Add recognized words to the `words` array
- Add words to ignore to the `ignoreWords` array
- Customize patterns to ignore in `ignoreRegExpList`

## Attribution

This template is based on open source guidelines originally developed by me @Simply007 for [Kontent.ai](https://github.com/kontent-ai/os-guidelines). Content has been generalized and adapted for universal use.

## License

- **Code**: MIT License
- **Documentation**: CC BY 4.0

See [LICENSE](LICENSE) for details.

## Contributing

Contributions are welcome! Please see our [contributing guidelines](CONTRIBUTING.md) for details on how to submit improvements to this template.

## Support

- Report issues: [GitHub Issues](../../issues)
- Request features: [GitHub Discussions](../../discussions)
- Ask questions: [GitHub Discussions](../../discussions)
