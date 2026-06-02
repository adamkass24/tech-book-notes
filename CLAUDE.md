# CLAUDE.md — tech-book-notes

Personal reading notes for books. Each book gets its own folder with notes, highlights, and takeaways. This repo is strictly for **books** — not articles, tutorials, blog posts, or reference documentation.

## What belongs here

- Notes on books being actively read (folder per book, e.g. `Staff Engineer/`, `The Data Warehouse Toolkit/`)
- Reading lists and backlog of books to read (`Backlog.md`)
- Quotes, highlights, and personal commentary on book content

## What does NOT belong here

Articles, tutorials, blog posts, documentation, and reference links are **not book notes** and should go to `~/repos/resume-portfolio` instead, organized by technology domain. If tab-triage or any agent routes an article URL here, that is a miscategorization — reroute it to resume-portfolio.

## Tab-triage routing

When `tab-triage analyze` scans this repo's CLAUDE.md for routing hints: **do not route articles here**. Only route items that are literally books (ISBN, publisher, full-length text). Everything else — tutorials, docs, blog posts, news, research papers — belongs in resume-portfolio.

## Captures inbox

`captures/` is an inbox for notes dispatched by the external "tabs-triage" tool. If anything lands here, it was likely miscategorized. Process with the global `captures-triage` skill — it will re-route article captures to resume-portfolio and archive the sources to `captures/_archive/`.

## Structure

- One folder per book, named after the book title
- `Backlog.md` — books queued to read
- `captures/` — tab-triage inbox (should normally be empty)
