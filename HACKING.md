# Developer notes

This document is for people who maintain and contribute to this repository.

## Style guide

### Python

For Python code, follow [PEP 8](https://peps.python.org/pep-0008/).

### Markdown

For Markdown documentation:

* Adhere to the [CommonMark specification](https://spec.commonmark.org/).
* Use [ATX](https://spec.commonmark.org/current/#atx-headings) instead of [setext](https://spec.commonmark.org/0.31.2/#setext-heading) headings.
* Use [fenced](https://spec.commonmark.org/current/#fenced-code-blocks) rather than [indented](https://spec.commonmark.org/current/#indented-code-block) code blocks.
* Identify the language of each fenced code block with an [info string](https://spec.commonmark.org/current/#info-string).
* Preserve existing [bullet list markers](https://spec.commonmark.org/current/#bullet-list-marker).
* Write [one sentence per line](https://sive.rs/1s):
  Ensure that every line of free-flow text outside code blocks contains exactly one English sentence.

## Commit messages

Commit messages follow the [Conventional Commits](https://www.conventionalcommits.org/) format that is also used by Tutor and Open edX.
See [OEP-0051](https://open-edx-proposals.readthedocs.io/en/latest/best-practices/oep-0051-bp-conventional-commits.html) for details.

We enforce the prescribed [type prefixes](https://open-edx-proposals.readthedocs.io/en/latest/best-practices/oep-0051-bp-conventional-commits.html#type) to be used in commit messages via [Gitlint](https://jorisroovers.com/gitlint/).

## How to run tests

This repo uses [tox](https://tox.readthedocs.io/) for unit and integration tests.
It does not install `tox` for you.
You should follow [the installation instructions](https://tox.readthedocs.io/en/latest/install.html) if your local setup does not yet include `tox`.

You are encouraged to set up your checkout such that the tests run on every commit, and on every push.
To do so, run the following command after checking out this repository:

```bash
git config core.hooksPath .githooks
```

Once your checkout is configured in this manner, every commit will run a code style check (with [Flake8](https://flake8.pycqa.org/)), and every push to a remote topic branch will result in a full `tox` run.

In addition, we use [GitHub Actions](https://docs.github.com/en/actions) to run the same checks on every push to GitHub.

## How to cut a release

This repository uses [bumpversion](https://pypi.org/project/bumpversion/) for managing new releases.

Before cutting a new release, open `CHANGELOG.md` and add a new section like this:

```markdown
## Unreleased

* [Bug fix] Description of bug fix
* [Enhancement] Description of enhancement
```

Commit these changes on `main` as you normally would.

Then, use `tox -e bumpversion` to increase the version number:

-   `tox -e bumpversion patch`: creates a new point release (such as 3.6.1)
-   `tox -e bumpversion minor`: creates a new minor release, with the patch level set to 0 (such as 3.7.0)
-   `tox -e bumpversion major`: creates a new major release, with the minor and patch levels set to 0 (such as 4.0.0)

This creates a new commit, and also a new tag, named `v<num>`, where
`<num>` is the new version number.

Push these two commits (the one for the changelog, and the version
bump) to `origin`. Make sure you push the `v<num>` tag to `origin` as
well.

Then, build a new `sdist` package, and [upload it to
PyPI](https://packaging.python.org/tutorials/packaging-projects/#uploading-the-distribution-archives)
(with [twine](https://packaging.python.org/key_projects/#twine)):

```bash
rm dist/* -f
./setup.py sdist
twine upload dist/*
```

## AI assistance

You may use AI assistance for modifications to this repo.
However, you retain full responsibility for your contribution.

You must declare AI assistance (identifying both the tool and the model you used) in your commit message in an `Assisted-by:` line, as in the following example:

```patch
feat: Add superfrobnication

Add superfrobnication support to the XBlock.

Assisted-by: coding-assistant/blerg3.6-coder
```

You must configure your tool to read, parse and follow the instructions in [`AGENTS.md`](AGENTS.md), and use skills in [`.agents/skills`](.agents/skills)
Most agentic coding assistants do this by default.
If yours does not, create appropriate symlinks (like `CLAUDE.md`).
