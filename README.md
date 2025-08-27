## Personal Notes: Software Engineering & Architecture

This repository contains personal notes on software engineering, software architecture, cloud infrastructure, and programming. Notes are lightweight, living documents with minimal ceremony, organized with YAML frontmatter and an index for quick navigation.

### How this repo is organized

- Categories: Architecture, Cloud & AWS, Kubernetes & Containers, Event-Driven & Streaming, Programming, Operations & Observability
- Each note has YAML frontmatter with: title, category, tags, description, status
- Central navigation lives in `INDEX.md`
- New notes should start from `templates/note-template.md`

### Conventions

- Keep notes small and focused. Prefer linking related notes rather than merging everything into one.
- Use descriptive tags to make search and grouping easier.
- Prefer adding examples, diagrams, and links over long prose.

### Frontmatter schema

```yaml
---
title: Short descriptive title
category: One of [Architecture, Cloud & AWS, Kubernetes & Containers, Event-Driven & Streaming, Programming, Operations & Observability]
tags: [tag1, tag2]
description: One-liner about what the note covers
status: notes # or draft, wip
---
```

### Start here

- Browse the index: `INDEX.md`
- Create a new note from the template: `templates/note-template.md`

### Tips

- Use your editor’s search across `title`, `tags`, and content.
- Add cross-links between related notes using relative paths, e.g. `[Microservices](./Architecture/microservices.md)`.


