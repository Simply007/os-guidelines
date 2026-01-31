---
layout: default
title: Home
nav_order: 1
---

# Open Source Guidelines Template

Welcome! This is a comprehensive template for organizations looking to establish clear, professional guidelines for their open source projects. Whether you're building SDKs, developer tools, or integrations, this template provides the structure and best practices to help your community contributors succeed.

## What's Included

This template provides ready-to-use documentation covering:

- **[Publishing Checklist](./Checklist-for-publishing-a-new-OS-project.md)** - Pre-launch requirements for community profiles, testing, CI/CD, and branch protection
- **[Naming Conventions](./Naming-conventions.md)** - Consistent naming patterns for repositories, packages, and projects
- **[Maintainer Duties](./Duties-of-a-Repository-Maintainer.md)** - Clear responsibilities for repository maintainers
- **[CI/CD Guides](./ci-and-automation/ci-and-automation.md)** - Technology-specific automation guidelines for .NET, Java, JavaScript, PHP, Ruby, and Swift
- **[Example Templates](./examples/)** - SDK, integration, and governance guideline templates

## Getting Started

1. **Setup** - Follow the [SETUP.md](./SETUP.md) guide to customize this template for {{ site.organization.name }}
2. **Customize** - Check [CUSTOMIZATION.md](./CUSTOMIZATION.md) for advanced configuration options
3. **Examples** - Review [template examples](./examples/) to see how to structure your guidelines

## About This Template

This template is based on the open source guidelines developed by [Kontent.ai](https://github.com/kontent-ai/os-guidelines), refined through years of maintaining SDKs and tools across multiple technology stacks. We've generalized these guidelines so any organization can adopt and adapt them.

The documentation uses Liquid templating to reference your organization's name ({{ site.organization.name }}), contact email ({{ site.organization.email }}), and other configurable values throughout. This makes it easy to white-label the entire site by editing a single configuration file.

## Important: This is a Template

**This template requires customization before use.** The default configuration contains placeholder values like "Your Organization" and "opensource@yourorg.com" that must be updated to reflect your organization. All documentation pages contain references that need to be adapted to your specific products, services, and policies.

Start by reviewing the [SETUP.md](./SETUP.md) guide to configure your organization's details, branding, and social media links.

---

### Need Help?

Questions about this template? Found something that needs improvement? Contact us at {{ site.organization.email }} or contribute directly to the [template repository](https://github.com/{{ site.organization.github_org }}/os-guidelines).
