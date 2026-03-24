# Hashnode GitHub Frontmatter – Full Reference

Source: https://docs.hashnode.com/blogs/blog-dashboard/github/frontmatter-breakdown

## Required Fields

| Field | Type | Example | Notes |
|-------|------|---------|-------|
| `title` | String | `Getting Started with Next.js` | Title of the post |
| `slug` | String | `getting-started-with-nextjs` | URL slug — must be unique. Updating slug updates the post URL |
| `tags` | String (comma-separated) | `nextjs, reactjs, webdev` | Up to 5 tag slugs. Find valid tags at https://github.com/Hashnode/support/blob/main/misc/tags.json |
| `domain` | String | `yourname.hashnode.dev` | Your Hashnode publication domain (custom or hashnode.dev subdomain) |

## Optional Fields

| Field | Type | Default | Notes |
|-------|------|---------|-------|
| `subtitle` | String | — | Short description shown under the title |
| `cover` | String (URL) | — | Cover image URL. Must be uploaded to Hashnode CDN first via https://hashnode.com/uploader |
| `saveAsDraft` | Boolean | `false` | Set `true` to save as draft instead of publishing |
| `enableToc` | Boolean | `false` | Auto-generate a table of contents |
| `seriesSlug` | String | — | Slug of the series to add this post to |
| `seoTitle` | String | — | SEO title (defaults to post title if omitted) |
| `seoDescription` | String | — | SEO description |
| `canonical` | String (URL) | — | Canonical URL if post is cross-posted from another platform |
| `hideFromHashnodeCommunity` | Boolean | `false` | Hide from Hashnode's public feed (still on your blog) |
| `disableComments` | Boolean | `false` | Turn off comments for this post |
| `publishAs` | String | — | Team publications only — publish on behalf of another member |
| `ignorePost` | Boolean | `false` | If `true`, Hashnode ignores this file entirely |

## Full Example Frontmatter

```yaml
---
title: Getting Started with Next.js
subtitle: A beginner-friendly guide to building apps with Next.js 14
slug: getting-started-with-nextjs
tags: nextjs, reactjs, javascript, webdev, tutorial
domain: yourname.hashnode.dev
saveAsDraft: true
enableToc: true
seriesSlug: nextjs-series
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681132538878/itnaYF1h-.png
seoTitle: Getting Started with Next.js – Complete Guide
seoDescription: A beginner-friendly guide to building apps with Next.js 14
canonical:
hideFromHashnodeCommunity: false
disableComments: false
---
```

## Slug Rules

- If you use the slug of an existing post → **the existing post is updated**
- If you change only the slug → post URL is updated
- If you change only the file path → post is updated
- If you change **both** path and slug → treated as a **new post**

## Important Constraints

- Markdown files must be in the **root directory** of the repo (not in subfolders)
- Maximum of **10 file changes** are processed per commit
- `saveAsDraft: true` → appears in Drafts section; removing this field will auto-publish the post
- Newsletter and post-scheduling are **not supported** via GitHub integration
- To delete a post, use the Hashnode dashboard (Articles and drafts section) — deleting the file does NOT delete the post
