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

Then work through the two decisions the template calls out.

### 1. Keep the `mecmcp-*` ignore if the repo consumes mecmcp

All six mecmcp-family server repos carry it. The mecmcp crates are pinned by
git tag and moved by hand in a `chore/mecmcp-<version>` PR; Dependabot advances
the git ref past the tag, which cargo then refuses —

```
failed to select a version for the requirement mecmcp-server = ^0.23.0
```

— failing the **whole** cargo run, so unrelated crate updates disappear with it.
The symptom is "Dependabot stopped opening cargo PRs", which does not look like
a version-pin problem. Remove the ignore only in a repo that does not consume
mecmcp.

### 2. Delete the `docker` block unless there is a Dockerfile

```bash
git ls-files | grep -i dockerfile
```

Nothing returned means delete the block. A `docker` ecosystem pointing at a
directory with no Dockerfile produces no updates **and no error** — inert config
that reads as coverage. `mecmcp` is deliberately cargo + github-actions only for
this reason. If a Dockerfile does exist, set `directory` to the directory holding
it (`/` when it is at the repo root).

## Verify it actually went live

Committing the file is not evidence that Dependabot read it. After it lands on
the default branch, confirm the jobs appear:

```bash
gh run list --repo <owner>/<repo> --branch main --limit 10
```

You are looking for runs named `cargo in /.`, `github_actions in /.` and, where
applicable, `docker in /.`. Those appearing is the proof; the file existing is not.

If they never appear:

- Confirm the path is exactly `.github/dependabot.yml` — not `dependabot.yml` at
  the repo root, and not under a subdirectory.
- Check GitHub's own parse result at
  `https://github.com/<owner>/<repo>/network/updates`. A malformed or rejected
  config is reported there, and that is the authoritative answer — GitHub
  validates against its own schema, so a file that a local YAML parser accepts
  can still be rejected (an unknown key, or an ecosystem name that is not on
  GitHub's supported list).

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
