---
title: CyanFox-Docs Usage
permalink: /docs/cyanfox-docs/usage
previous: /docs/cyanfox-docs/installation
previous_title: Installation
---

# Usage

- <a href="/docs/cyanfox-docs/" wire:navigate>Introduction</a>
- <a href="/docs/cyanfox-docs/installation" wire:navigate>Installation</a>
- <a href="/docs/cyanfox-docs/usage" wire:navigate>Usage</a>

To create a new page, create a new markdown file in the `pages/en` or `pages/de` directory.

The file should start with the following front matter:

```markdown
---
title: Page Title
permalink: /docs/page-title
previous: /docs/previous-page
previous_title: Previous Page
next: /docs/next-page
next_title: Next Page
---

# Your Page Content
```

The `title` field is required and will be used as the page title.
The `permalink` field is required and will be used as the page URL.
The `previous` and `next` fields are optional and will be used to create navigation links to the previous and next pages.
The `previous_title` and `next_title` fields are optional and will be used as the text for the previous and next links.
