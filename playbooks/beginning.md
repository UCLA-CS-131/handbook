---
title: "Beginning of Quarter"
layout: default
parent: Playbooks
nav_order: 1
---

# Beginning of Quarter

This playbook runs through everything you need to do to set up the course infrastructure at the start of the quarter.

{: .important }

This playbook assumes you've already completed the [Getting Started]({% link getting-started.md %}) guide.

{: .no_toc }

## Table of Contents

{:toc}

- dummy item

{::options toc_levels="2..4" /}

## External Services

The course uses a handful of external services, some of which you'll need Carey's help to do. Before you proceed, you should:

- ask Carey to set up the class's [Bruinlearn](https://bruinlearn.ucla.edu/); we only use this for course announcements + directing students to the course website.
- set up the [Gradescope](gradescope.com) instance for this class. **This should be doable through Bruinlearn's Gradescope tab.**
- set up a class discussion forum. We've used [CampusWire](https://campuswire.com/) in the past, but you can use whatever option you'd prefer; we suggest [avoiding Piazza due to privacy concerns](https://stanforddaily.com/2020/10/04/concerned-with-piazzas-data-privacy-management-some-professors-look-to-alternative-discussion-forums/).
- make sure that all TAs (including you!) have access to the [class's GitHub Organization](https://github.com/UCLA-CS-131). Carey is an admin and can invite people.
- the TA shared drive. This is *different* from a normal "shared folder". It will contain all public and private materials for the course. Ask Carey for access.

## Setting Up the Course Website

### Why: Copying, not Forking

**We do *not* suggest forking the previous course website directly**. This may seem strange! Quickly, here's why:

1. you will keep an (irrelevant) git history, and may inherit irrelevant files
2. you won't gain upstream benefits from improvements to [Just the Class](https://github.com/kevinlin1/just-the-class)

Instead, the suggested general workflow looks like this:

1. create a new repository [using Just the Class as a template](https://github.com/kevinlin1/just-the-class/generate)
2. then, copy over the relevant code changes, which are to:
  - `_config.yml`
  - very minor styling
  - new `lecture` and `calendar` features
3. finally, update the content
4. deploy your changes!

We'll detail that process in the next few subsections.

{:. important }

You can, of course, just fork the old website and make the changes. But, we really don't recommend this!

### New Template and Code Changes

First, create a new Just the Class template; [GitHub has a wizard for this](https://github.com/kevinlin1/just-the-class/generate). Place it in the `UCLA-CS-131` group, and title it `quarter-year` (e.g. `fall-22`, `spring-23`).

#### `_config.yml` Changes

The `_config.yml` defines various variables that affect how the site is built and rendered. You should update these to match CS 131, including:

- `title`, `tagline`, `description`, `author` - all self-explanatory strings
- `baseurl` - this should be the subpath of your repo, e.g. `/spring-23`
- `url` - this should always be `https://ucla-cs-131.github.io/`
- `aux_links` and `nav_external_links` - see below

In addition to editing the existing `_config.yml` values, we've enabled a few more Just the Docs features; add these to the bottom of `_config.yml`:

```yml
# Adds a "Back to Top" link
back_to_top: true
back_to_top_text: "Back to top"

# Adds the "Edit this page on GitHub" link to the footer of each page
gh_edit_link: true
gh_edit_link_text: "Edit this page on GitHub"
gh_edit_repository: "https://github.com/UCLA-CS-131/spring-23" # NOTE: update this to your new repo!
gh_edit_branch: "main"
gh_edit_view_mode: "tree"

# Callouts let you add vibrant UI elements to notes
# via markdown code like {: .note }
callouts_level: quiet
callouts:
  important:
    title: Important
    color: yellow
  new:
    title: New
    color: green
  note:
    title: Note
    color: blue
  warning:
    title: Warning
    color: red

# Mermaid is a JS library that renders figures from markdown.
# You may want to update this version; pick one from https://cdn.jsdelivr.net/npm/mermaid/
mermaid:
  version: "9.1.7"
```

#### CSS Change(s)

Currently, there's only one CSS change we make from the stock Just the Class distribution.

In `_sass/custom/custom.scss`, add the following class to restrict the size of the title in the navbar:

```css
.site-title {
  font-size: 20px !important;
}
```

#### New Collection: `lecture`

We've created a new layout called `lecture` that styles the lecture notes. Create a new file, `_layouts/lecture.html`, with the following content:

```html
{% raw %}---
layout: page
---

<h1>{{page.title}}</h1>
<p>{{page.lecture_date}} | Week {{page.week}} | edited by {{page.author}}</p>
{% if page.original_author and page.originally_written %}
<p>(originally written {{page.originally_written}} by {{page.original_author}})</p>
{% endif %}

{{content}}{% endraw %}
```

You can then create an accompanying `lectures.md` that renders these all in a list:

```md
{% raw %}---
layout: page
title: Lecture Notes
description: Lecture Notes written by the TAs!
has_children: true
---

# Lecture Notes

These lecture notes are written by the TAs. They aim to supplement and expand upon the course slides.

{% for lecture in site.lectures %}

- [{{lecture.title}}]({{site.baseurl}}{{ lecture.url }})

{% endfor %}{% endraw %}
```

#### Adjusted Course Calendar

Matt has written some code that gives the calendar page filtering capabilities. To do this, change `calendar.md` to the following:

```html
{% raw %}---
layout: page
title: Course Calendar
description: Listing of course modules and topics.
---

# Course Calendar

<form id="filter-form">
  Filter by:
  <input type="checkbox" id="Lecture" name="Lecture" value="Lecture" checked />
  <label for="Lecture">Lecture</label>&nbsp;
  <input type="checkbox" id="Section" name="Section" value="Section" checked />
  <label for="Section">Section</label>&nbsp;
  <input type="checkbox" id="Posted" name="Posted" value="Posted" checked />
  <label for="Posted">Posted</label>&nbsp;
  <input type="checkbox" id="Due" name="Due" value="Due" checked />
  <label for="Due">Due</label>&nbsp;
  <input type="checkbox" id="Exam" name="Exam" value="Exam" checked />
  <label for="Exam">Exam</label>&nbsp;
</form>

*[Matt](https://matthewwang.me) says*: yes, the filtering leaves the day even if there's nothing there. Working on it!


{% for module in site.modules %}
{{ module }}
{% endfor %}

<script>
const LABELS = ['Lecture', 'Section', 'Posted', 'Due', 'Exam'];
const modules = document.getElementsByClassName('module');
const moduleDls = [...modules].map(module => module.children[0]);
const items = [...moduleDls].map(module => module.children);

const ddApplyClasses = (children, classDecider) => {
  for (const child of children){
    if (child.nodeName != 'DD') {
      continue;
    }
    child.className = ''; // necessary ... for some reason?
    const newClass = classDecider(child);
    child.className = newClass;
  }
}

const ddPredicateBuilder = (labels) => {
  return (ddNode) => {
    return labels
      // this toUpperCase is a hack to only catch the label
      .map(label => ddNode.innerText.includes(label.toUpperCase()))
      .some(v => v)
  }
}

const ddClassDecider = (predicate) => {
  return (ddNode) => predicate(ddNode) ? '' : 'd-none'
}

const rerenderItems = () => {
  const activeLabels =
    LABELS
      .map(label => document.getElementById(label).checked ? label : '')
      .filter(label => label !== '');

  for (const item of items) {
    ddApplyClasses(item, ddClassDecider(ddPredicateBuilder(activeLabels)))
  }
}

const filterForm = document.getElementById('filter-form');
filterForm.addEventListener('change', rerenderItems);
</script>{% endraw %}
```

### Updating Course Information

Beyond the above code changes, you are likely going to want to update all of the site's content. Here's a semi-comprehensive list of where you need to look:

- top-level content pages that are just Markdown:
  - `homeworks.md`
  - `practice.md` (which should ... probably be renamed)
    - you should also copy the relevant files in `assets/exams`!
  - `projects.md`
  - `syllabus.md`
- components and pages that are generated from other data
  - course staff, generated from `_staffers`
  - weekly schedlule, generated from `_schedules`
  - course calendar, generated from `_modules`
  - the sidebar external links, generated from `_config.yml`

More of how to do this is explained in the [Misc. Website Updates playbook]({% link playbooks/update-website.md %}).

### Deploying the Site

Deploying the site is very simple! We'll do so with [GitHub Actions](https://github.com/features/actions) and [GitHub Pages](https://pages.github.com/).

First, we'll create a GitHub Action to build and deploy our Jekyll site. This is relatively standard as far as Actions go (so there should be lots of support if this fails!).

Create a new file at `.github/workflows/pages.yml` with:

```yml
# Sample workflow for building and deploying a Jekyll site to GitHub Pages
name: Deploy Jekyll site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow one concurrent deployment
concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          bundler-cache: true # runs 'bundle install' and caches installed gems automatically
          cache-version: 0 # Increment this number if you need to re-download cached gems
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v1
      - run: bundle exec jekyll build --baseurl ${{ steps.pages.outputs.base_path }} # defaults output to '/_site'
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1 # This will automatically upload an artifact from the '/_site' directory

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```

Once you commit this file, head to the GitHub Repository. In Settings > Pages > Build and deployment > Source, select GitHub Actions.

Now, in subsequent pushes to `main`, the site should automatically build and deploy to GitHub Pages; the settings menu we were in above will show you the status of the last build, as well as the URL. Nice!

## Bootstrapping the First Project

### Template Files
### Autograder Setup

### Deploying to Gradescope

### Student-Facing Code

### Updating Barista
