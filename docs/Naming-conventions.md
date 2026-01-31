---
layout: default
title: Naming Conventions
nav_order: 6
---

# Naming conventions

## GitHub Repositories

### Repository name:

* projects under the [{{ site.organization.name }} Github organization](https://github.com/{{ site.organization.shortname }}/) don't need any further mention of the product in their name
* If a {{ site.organization.name }} related repository is managed in an external organization, use pattern `{{ site.organization.shortname }}-<project-name>` or at least provide `{{ site.organization.shortname }}` somewhere in the repository name (i.e. `sourcebit-source-{{ site.organization.shortname }}`)

### `<project-name>` guidelines:

* Use a broad-to-specific convention to keep similar projects grouped together (e.g. delivery-sdk-js + delivery-sdk-net)

### Tagging

If you want to, you can mark your repository by specific [GitHub topics](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/classifying-your-repository-with-topics). This ensures, you get to the topic listing and your repository will be much more accessible.

> Feel free to use multiple topics.
> Use any other appropriate tags for the repo. (e.g. cli, dotnet, plugin, etc.)

* [{{ site.organization.shortname }}](https://github.com/topics/{{ site.organization.shortname }}) - General topic used in all repositories related to {{ site.organization.name }}.
    * [{{ site.organization.shortname }}-integration](https://github.com/topics/{{ site.organization.shortname }}-integration) - Repository helps to integrate with other services (see [Integrations info](./Integrations.md) for more).
    * [{{ site.organization.shortname }}-tool](https://github.com/topics/{{ site.organization.shortname }}-tool) Repository helps with the tooling around {{ site.organization.name }} (SDKs, CLIs, Generators, Mini apps using API etc.).
    * [{{ site.organization.shortname }}-sample](https://github.com/topics/{{ site.organization.shortname }}-sample) Repository is showcasing the usage of {{ site.organization.name }}.

## Code

### Namespaces

We stick to [Microsoft's conventions](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/names-of-namespaces) `<Company>.(<Product>|<Technology>)[.<Feature>][.<Subnamespace>]`. Some examples:

  * `<Company>.<Product>.*` for projects under the [{{ site.organization.name }} Github organization](https://github.com/{{ site.organization.shortname }}/)
  * `<YourCompany>.<Product>.*` for any externally maintained {{ site.organization.name }} projects
  * `<Company>.<Technology>*` for non-product related code. Eg. `<Company>.AspNetCore.Http` if the code is related to `Microsoft.AspNetCore.Http`.

### Packages

* Package names should omit mentions of the technology stack, assuming it can be derived from the package manager (for example `@{{ site.organization.shortname }}/delivery-sdk-js` package hosted on npm should lose the `-js` suffix)
* When it makes sense, use organization prefixes (e.g. @{{ site.organization.shortname }}/deliver-sdk-js)
