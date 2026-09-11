# Dependabot configuration template

**This file is NOT automatically inherited by repositories.**

GitHub does not support inheriting `dependabot.yml` from a `.github` repository.
Each repository must have its own `.github/dependabot.yml` file to enable
Dependabot version updates.

## Usage

Copy `dependabot.yml` from this directory into your repository's `.github/`
directory:

```bash
cp dependabot/dependabot.yml .github/dependabot.yml
```

Then customize the `ignore` list and package ecosystems as needed for your
repository.

## What gets inherited vs what doesn't

GitHub automatically inherits these community health files from this `.github`
repository:

- ✅ CODE_OF_CONDUCT.md
- ✅ CONTRIBUTING.md
- ✅ SECURITY.md
- ✅ FUNDING.yml
- ✅ Issue/PR templates

But these must be in each repository:

- ❌ `dependabot.yml` (this template)
- ❌ LICENSE files
- ❌ Repository-specific CI/CD workflows

For more information, see [Creating a default community health file](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file).

## Account-level settings

To enable Dependabot alerts automatically for new repositories:

1. Go to https://github.com/settings/security_analysis
2. Under "Dependabot alerts", check "Automatically enable for new repositories"

Note: This only enables *alerts* for new repos. Version updates still require
the `dependabot.yml` file.
