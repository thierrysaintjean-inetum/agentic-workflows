---
name: update-github-info
description: Propose source-linked updates to Mona's GitHub Info website.
intent: Keep Mona's GitHub Info website useful with accurate, source-linked GitHub updates.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
tools:
  bash: false
  edit: true
  web-fetch: true
network:
  allowed:
    - github.blog
    - github.com
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

Read `notes/mona-notes.md` and `site/content/github-info.md`. Then use web-fetch to read both:

- https://github.blog/latest/
- https://github.blog/changelog/

Identify recent, verified updates that help developers learn GitHub faster and fit the site's existing themes. Keep proposed summaries short and practical. Cite each selected update with a direct link to its GitHub Blog or Changelog source.

Update only `site/content/github-info.md`. Preserve unrelated content and its existing structure. Do not invent details, repeat items already covered, or make speculative edits. If there is no useful new information or the file would not change, call `noop` with a brief reason. If either required source cannot be fetched or its contents cannot be verified, do not edit the page; call `report_incomplete` with the unavailable source and reason.

When there is a meaningful change, open one non-draft pull request targeting `main` and request review from Mona. Never write directly to `main`.
