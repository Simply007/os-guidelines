---
layout: default
title: Examples
nav_order: 9
has_children: true
---

# Example Templates

This section contains customizable templates for common documentation needs in open source projects. These examples serve as starting points that you can adapt to your organization's specific requirements.

## Available Templates

### [SDK Development Guidelines](examples/example-sdk-guidelines.md)

A comprehensive template for documenting SDK development requirements. This template covers:

- Expected API functionality and endpoints
- Backward compatibility guidelines
- Data types and response formats
- HTTP headers and rate limiting
- Error handling patterns
- Testing requirements
- Documentation standards
- Naming conventions
- Publishing requirements

**When to use**: If your organization provides an API and wants to encourage community SDK development, customize this template with your API's specific requirements and publish it as official guidelines.

---

## How to Use These Templates

1. **Copy the template** to a new file in your documentation
2. **Read through all sections** and customize placeholders (marked with notes)
3. **Remove template notes** (the blue note boxes) before publishing
4. **Add your specifics**: API endpoints, data types, rate limits, etc.
5. **Update navigation**: Adjust `nav_order` and `parent` in front matter
6. **Test locally**: Run `bundle exec jekyll serve` to preview
7. **Commit and publish**: Push to GitHub to deploy via GitHub Pages

## Contributing New Templates

Have a template that would benefit other organizations? We welcome contributions:

1. Ensure the template is **generic and reusable** (not specific to your product)
2. Include clear **placeholder notes** explaining what needs customization
3. Provide **examples** using Liquid variables like `{{ site.organization.name }}`
4. Test the template with **sample data** to ensure it works
5. **Submit a pull request** to the template repository

See our [CONTRIBUTING.md](../CONTRIBUTING.md) for detailed contribution guidelines.

---

## Requested Templates

Looking for a template we don't have yet? Open an issue with the `template-request` label. Popular requests include:

- API versioning and deprecation policy
- Contributor license agreement (CLA) process
- Security vulnerability disclosure
- Product integration guidelines
- Code of conduct customization guide

We prioritize templates based on community demand and general applicability.

---

## Template Philosophy

All templates in this section follow these principles:

**Generic over specific**: Templates should be applicable to most organizations, not tied to specific products or technologies.

**Comprehensive yet concise**: Cover all important topics without overwhelming adopters with unnecessary detail.

**Customization-friendly**: Include clear notes about what needs customization and provide examples using Liquid variables.

**Best practices by default**: Recommend industry-standard practices unless there's a good reason to deviate.

**Progressive disclosure**: Start with required elements, mark optional sections clearly.

For more on template design philosophy, see [TEMPLATE_GUIDE.md](TEMPLATE_GUIDE.md).
