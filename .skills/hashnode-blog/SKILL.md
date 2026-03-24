---
name: hashnode-blog
description: >
  Use this skill whenever the user wants to write, create, or publish a blog post to Hashnode via GitHub.
  Triggers include: "write a blog", "create a blog post", "new post for Hashnode", "publish to Hashnode",
  "write an article", "create a draft blog", "add blog to GitHub", or any request to generate content
  for a Hashnode-connected GitHub repository. This skill guides the user through collecting required blog
  metadata (title, description, tags, content) in a chatbot-style one-at-a-time flow, then produces a
  properly formatted Hashnode-compatible markdown file and commits it to the linked GitHub repo.
  ALWAYS use this skill for Hashnode blog creation — do not skip it even for simple blog requests.
---

# Hashnode Blog Skill

This skill creates Hashnode-compatible markdown blog posts and commits them to a GitHub repo that is already synced with Hashnode. The post is marked as a **draft** so the user can review, add images, and publish manually.

---

## Workflow Overview

1. **Collect metadata** — ask the user questions one at a time (chatbot style)
2. **Validate** — all required fields must be present; never create the file if any are missing
3. **Generate** — write the markdown file with proper Hashnode frontmatter
4. **Commit & push** — `git add`, `git commit`, `git push` to the linked repo

---

## Step 1 – Collect Metadata (One Question at a Time)

Ask these questions **sequentially**, waiting for a real answer before moving on. Do NOT batch all questions at once. If the user skips or gives a blank answer, gently re-ask — these fields are all **required** and the blog cannot be created without them.

| # | Question to ask the user | Field | Notes |
|---|---|---|---|
| 1 | "What's the **title** of your blog post?" | `title` | Required |
| 2 | "Give me a short **description** (subtitle) — one or two sentences." | `subtitle` | Required |
| 3 | "What **tags** should this post have? (up to 5, comma-separated slugs e.g. `javascript, nodejs, webdev`)" | `tags` | Required — see valid tags note below |
| 4 | "Now paste or describe the **content** of the blog post. You can give me notes, bullet points, or the full text — I'll write it up." | `content` | Required |
| 5 | "Is this post part of a **series**? If yes, give me the series slug (e.g. `my-series`). If no, just say no." | `seriesSlug` | Optional |
| 6 | "Should I **generate a table of contents** for this post? (yes/no)" | `enableToc` | Optional, default false |
| 7 | "What is your **Hashnode publication domain**? (e.g. `yourname.hashnode.dev` or your custom domain)" | `domain` | Required |

> **Tags note:** Tags must be valid Hashnode tag slugs (lowercase, hyphenated). Common ones: `javascript`, `typescript`, `python`, `nodejs`, `reactjs`, `nextjs`, `webdev`, `programming`, `tutorial`, `beginners`. If unsure, remind the user they can find a full list at https://github.com/Hashnode/support/blob/main/misc/tags.json

---

## Step 2 – Validate

Before writing any file, confirm all **required** fields are present and non-empty:
- `title` ✓
- `subtitle` ✓  
- `tags` (at least one) ✓
- `content` ✓
- `domain` ✓

If any required field is missing, ask for it again. **Do not proceed** until all are collected.

---

## Step 3 – Generate the Markdown File

### File Naming

Generate a slug from the title:
- Lowercase all characters
- Replace spaces and special characters with hyphens
- Remove consecutive hyphens
- Example: "Getting Started with Next.js" → `getting-started-with-nextjs`

Filename: `<slug>.md`

> **Important:** Hashnode requires markdown files to be in the **root directory** of the repo, not in a subfolder.

### Frontmatter Template

```markdown
---
title: <title>
subtitle: <subtitle>
slug: <slug>
tags: <tag1, tag2, tag3>
domain: <domain>
saveAsDraft: true
enableToc: <true|false>
seriesSlug: <seriesSlug or leave blank>
cover: 
seoTitle: <title>
seoDescription: <subtitle>
---
```

Leave `cover:` blank — the user will add the image manually before publishing.

### Content Writing

After the frontmatter, write the blog content. If the user gave notes or bullet points, expand them into a well-structured, engaging blog post with:
- A strong intro paragraph
- Clear H2/H3 headings for sections
- Code blocks where relevant (use fenced triple backticks with language hint)
- A closing summary or call-to-action

Keep the tone friendly, technically accurate, and developer-focused.

---

## Step 4 – Write and Commit

### Write the file

Write the markdown file to the root of the git repository. First, detect the repo root:

```bash
git rev-parse --show-toplevel
```

Then write the file there. **Do not place it in a subfolder.**

### Commit and push

```bash
cd <repo-root>
git add <filename>.md
git commit -m "draft: <title>"
git push
```

If `git push` fails (e.g. no upstream set), try:
```bash
git push --set-upstream origin main
# or
git push --set-upstream origin master
```

If it still fails, tell the user what went wrong and provide the manual commands.

---

## Step 5 – Confirm to User

Once committed and pushed, tell the user:

> ✅ **Blog draft created!** The file `<filename>.md` has been committed and pushed to GitHub. Hashnode will pick it up automatically as a **draft** — you can find it in your Drafts section on the Hashnode dashboard. From there you can:
> - Add a **cover image** (upload to Hashnode CDN via https://hashnode.com/uploader, then paste the URL into the `cover:` field)
> - Review and edit the content
> - Publish when ready

---

## Edge Cases

- **User wants to update an existing post**: If the slug already exists in the repo, editing the same file will update the post on Hashnode. Inform the user.
- **Series slug wrong**: If the series slug is incorrect, Hashnode will ignore it silently. Tell the user to check their series slug in the Hashnode dashboard → Series section.
- **Max 10 file changes per commit**: Hashnode only processes up to 10 changed files per commit. Keep commits focused.
- **No git repo found**: If `git rev-parse` fails, tell the user: "I couldn't find a git repository here. Please run this from inside your Hashnode-linked GitHub repo folder."
- **Content too short**: If the user gives very minimal content (under 3 sentences), ask: "Would you like me to expand this into a full blog post, or keep it as-is?"

---

## Reference

Full Hashnode frontmatter fields are documented in `references/frontmatter.md`.
