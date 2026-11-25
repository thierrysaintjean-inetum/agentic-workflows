---
name: update-github-info
description: Propose source-linked updates to Mona's GitHub Info website.
intent: Keep Mona's GitHub Info website useful with accurate, source-linked GitHub updates.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
model: copilot/gpt-5
tools:
  bash: true
  edit: true
  web-fetch: null
  cli-proxy: false
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
engine:
  id: copilot
  copilot-sdk: true
safe-outputs:
  create-pull-request:
    title-prefix: "Update GitHub Info: "
    base-branch: main
    reviewers:
      - mona
    draft: false
    allowed-files:
      - site/content/github-info.md
---

# Update GitHub Info

Read `notes/mona-notes.md` and `site/content/github-info.md`. Fetch each source below with `curl --fail --location --silent --show-error --allow-url <host> <url>`, using the matching hostname from `network.allowed`. Do not use the native web-fetch tool; it is unavailable in Copilot API proxy mode.

- https://github.blog/latest/
- https://github.blog/changelog/
- https://awesome-copilot.github.com/workflows/

Identify recent, verified updates that help developers learn GitHub faster and fit the site's existing themes. Keep proposed summaries short and practical. Cite each selected update with a direct link to its original GitHub Blog, Changelog, or Awesome Copilot source.

Update only `site/content/github-info.md`. Preserve unrelated content and its existing structure. Do not invent details, repeat items already covered, or make speculative edits. If there is no useful new information or the file would not change, call `noop` with a brief reason. If either required source cannot be fetched or its contents cannot be verified, do not edit the page; call `report_incomplete` with the unavailable source and reason.

When there is a meaningful change, open one non-draft pull request targeting `main` and request review from Mona. Never write directly to `main`.
