---
title: "Writing a New Lecture Note"
layout: default
parent: Playbooks
nav_order: 3
---

# Writing a New Lecture Note

This playbook runs through how you should add a new lecture note to the course website.

{: .important }

This playbook assumes you've already completed the [Getting Started]({{site.baseurl}}/getting-started/) guide.

{: .no_toc }

## Table of Contents

{:toc}

- dummy item

{::options toc_levels="2..4" /}

## Using a Previous Note

{: .note }

This is *completely optional*; you can write new notes from scratch!

## Creating a New Page

We suggest writing lecture notes in Markdown. Create a new markdown file in the `lectures/` folder of the course website.

```
$ touch lectures/01.md
```

By convention, the names of the file are the two-digit lecture number. This gives you nice default sort ordering.

## Adding the Front Matter

Now, we'll add some **front matter** to the markdown file. This is necessary information for Jekyll and Just the Docs to render the page properly.

```yml
---
# required by the website
title: "Lecture 01"    # the rendered title in the sidebar / page
layout: lecture        # used by Jekyll to generate CSS
parent: Lecture Notes  # used by Jekyll to generate navigation

# the following is rendered in the note header as strings

# required lecture metadata
week: 1
lecture_date: 2023-04-03
author: Matt Wang

# optional, if you used a previous lecture
original_author: Matt Wang
originally_written: 2022-09-26
---
```

Do not change the `layout` and `parent` keys! However, you should adjust everything else to match the information for your new lecture note. And, don't forget the opening and closing `---` and `---`!

## Table of Contents

Just the Docs supports automatically generating a table of contents for the lecture note, [similar to the ToC for this page](#table-of-contents). You can generate it with this liquid snippet, which should go *under* the second `---` in the front matter:

```md
{% raw %}{: .no_toc }

## Table of Contents

{:toc}

- filler item

{::options toc_levels="2..4" /}{% endraw %}
```

To briefly explain this snippet:

- `{: .no_toc }` annotates the `## Table of Contents` header to *not* show up in the table of contents
- `{:toc}` generates a table of contents in-place. But, it requires a filler item to denote the type of the list, which is the `- dummy item` on the next line
- `{::options toc_levels="2..4" /}` tells the generator to only use heading levels `2` to `4` to generate the table of contents; in other words, to use `##`, `###`, and `####` (but skip `#`)

## Writing the Content


### Tip: Callouts

### Tip: Images

### Tip: Code Highlighting

### Tip: Hidden/Toggleable Content

### Tip: Extra CSS

## Publishing the Lecture Note
