---
title: "Beginning of Quarter"
layout: default
parent: Playbooks
nav_order: 1
---

# Beginning of Quarter

This playbook runs through everything you need to do to set up the course infrastructure at the start of the quarter. It is a long document, **but the goal is not to read through all of this in one sitting - split it up and take breaks!**

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

This is a (medium-sized) guide that explains how to set up a new copy of the course website. This is doable in one sitting, and probably takes under 1-2 hours.

### Why: Copying, not Forking

**We do *not* suggest forking the previous course website directly**. This may seem strange! Quickly, here's why:

1. you will keep an (irrelevant) git history, and may inherit irrelevant files
2. you won't gain upstream benefits from improvements to [Just the Class](https://github.com/kevinlin1/just-the-class) (and [Just the Docs](https://just-the-docs.com))

Instead, the suggested general workflow looks like this:

1. create a new repository [using Just the Class as a template](https://github.com/kevinlin1/just-the-class/generate)
2. then, copy over the relevant code changes, which are to:
  - `_config.yml`
  - very minor styling
  - new `lecture` and `calendar` features
3. finally, update the content
4. deploy your changes!

We'll detail that process in the next few subsections.

{: .important }

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
{% raw %}# Sample workflow for building and deploying a Jekyll site to GitHub Pages
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
        uses: actions/deploy-pages@v1{% endraw %}
```

Once you commit this file, head to the GitHub Repository. In Settings > Pages > Build and deployment > Source, select GitHub Actions.

Now, in subsequent pushes to `main`, the site should automatically build and deploy to GitHub Pages; the settings menu we were in above will show you the status of the last build, as well as the URL. Nice!

## Bootstrapping the First Project

This section explains how to bootstrap the first project by setting up all of the related grading and template infrastructure.

It's a bit involved; it likely takes between 1-3 hours to figure everything out, depending on how readable everything is!

### Prerequisites: Preparing Carey's Solution

Before we start, we'll assume that you have both the spec and Carey's solution to Project 1. The solution itself is usually broken down into:

- evergreen project infrastructure, which usually includes:
  - `intbase.py`: a file that defines the ABC that students subclass, as well as enums and constants for various parts of the program
  - `bparser.py`: a file that parses programs for students. We first introduced this in S2023.
- the "main" code, which is similar to what a student would submit:
  - an `interpreterv*.py`: the entrypoint for the interpreter
  - any supporting files the interpreter imports (e.g. `env_v1.py`)

Before you continue, we suggest processing all of these files by:

- using a code formatter like [black](https://pypi.org/project/black/) to enforce a consistent code style across all the files
- resolving open lints (by either fixing or ignoring the problem) using [pylint](https://pypi.org/project/pylint/)
- checking for any typos + unused methods/classes/files
- a good heuristic for knowing when you're done: if running black + pylint produces no errors (this is easy to do with the VSCode extensions!)

Keeping the code consistent is helpful to reduce noise in `git` diffs, and also makes for a more seamless (and best practices-first) student experience.

### Template Files: Starter Repo

First, we'll create a *starter repository* for students to clone. For project 1, this should just contain the evergreen project infrastructure; in later projects, we'll add solutions for prior ones (so students can start from a good base).

1. in the [GitHub organization](https://github.com/UCLA-CS-131), create a new repository titled `your-quarter-project-starter` (ex `spring-23-project-starter`)
  - the repository should be public
  - pick a Python `.gitignore`
  - traditionally, we've picked this repository to be unlicensed, but it's up to you!
2. add the evergreen project infrastructure (`intbase.py` and optionally `bparser.py`)
3. flesh out the `README` to explain how to use the starter. You can use the [`spring-23-project-starter`](https://github.com/UCLA-CS-131/spring-23-project-starter/tree/p1) as a reference
4. push these changes with a descriptive commit message; then, draft a new GitHub release targeting the main branch

The [`p1` tag of the `spring-23-project-starter`](https://github.com/UCLA-CS-131/spring-23-project-starter/tree/p1) is a good reference for the end state of this subsection!

### Private Autograder Setup

Next, we'll set up the private autograder (which you'll use to grade the students code). We've set up a repository called the [`minimal-autograder-template`](https://github.com/UCLA-CS-131/minimal-autograder-template), which gives you a good starting point.

To set up the private autograder,

1. set up a **private** repository for the autograder to live in. This can either be the existing `autograder-experimental` repository (wipe the previous files), or a new *private* repository.
2. copy in the code from `minimal-autograder-template`
3. resolve each of the `# TODO` comments in `tester.py` (you may need to refer to the spec to answer such questions)
4. replace the `intbase.py` and `bparser.py` with the new versions in this quarter
  - if this quarter does not have a parser, remove `bparser.py` entirely
5. replace the tests in `v1/tests` with equivalent valid ones for project 1
  - the input and expected output of each test case can be found as a comment at the bottom of each test case file

At this point, the autograder *should* be fully functioning. Test it by:

1. Adding Carey's solution files, i.e. `interpreterv1.py` and everything it imports
2. Run `python3 tester.py 1`
3. The output *should* be a successful run with `2/2` test cases passing, like so:

```sh
$ python3 tester.py 1
Running 2 tests...
Running v1/tests/test_print_string.brewin...  PASSED
Running v1/fails/test_if.brewin...            PASSED
2/2 tests passed.
Total Score:    100.00%
```

Before proceeding, *make sure* that this is the correct output! You may need to do some extra debugging/code changes if things have changed between projects.

Once this is ready, let's commit our changes! First, edit the `.gitignore` so that project solutions will *never* be committed. Here's an example of one in `autograder-experimental`:

```
*.zip
*.json
.DS_Store
__pycache__

# solutions
interpreter*
env*
func*
tokenize*
class*
object*
type*
```

Now, you're ready to add and commit your changes.

```sh
$ git add .
$ git commit -m "Pre-project 1 scaffolding."
$ git push
```

### Deploying to Gradescope

A `make` command packages up the project to deploy to Gradescope, and explicitly only includes relevant files. If one has been added (an infrastructure file, *not* a solution file), edit the `Makefile` to include it.

```
v1: clean run_autograder setup.sh tester.py harness.py bparser.py intbase.py v1
	zip -r grader.zip run_autograder setup.sh tester.py harness.py bparser.py intbase.py v1
```

{: .note }

There's a circular dependency since `v1` is both a task and a folder. Matt never got around to resolving this, but it's a very short fix!

Now, we can run `make v1` to generate a `grader.zip`.

```sh
$ make v1
...
$ ls | grep "grader.zip"
grader.zip
```

This is the final deliverable that you need! You can now [create an autograded assignment in Gradescope](https://gradescope-autograders.readthedocs.io/en/latest/getting_started/). When you're asked for a `.zip` file, upload the `grader.zip`. Set the release date to be far in the future, so students don't see the assignment.

Once you've finished uploading, run a test with your own copy of `interpreterv*.py`, just to make sure that everything works smoothly! The autograder should return a result within five seconds, with a 2/2 score.

If you've made any changes, make sure to commit a working build, so other TAs can reproduce it if necessary. And that's it; now, other TAs can iterate on the autograder using the instructions in the [project playbook]({% link playbooks/project.md %}).

### Public Autograder Setup

{: .note }

If you haven't decided which test cases will be public yet, feel free to add a subset of public test cases. Just keep in mind: you can always add more, but you can't remove them (they're on the internet forever)!

We provide students with a public version of the autograder that contains all of the relevant portions to grading (i.e. the local test runner, the interface for test cases, timeout code) but does not contain private test cases or the irrelevant pieces (Gradescope-uploading infrastructure). In this subsection, we'll quickly make this for Project 1.

To do this,

1. Create a public repository titled `your-quarter-autograder`, e.g. `spring-23-autograder`
  - you can choose the Python `.gitignore` and the MIT License
2. Copy over from the private autograder:
  - the test runners: `tester.py` and `harness.py`
  - `intbase.py`
  - the structure of the `v1` folder
  - `bparser.py` (if it exists)
3. Replace the `v1` folder with only the subset of test cases that you'd like to be public; update the `tester.py` to only register those test cases (within `generate_test_suite_v1()`)
  - usually, we add between 10%-20% of the private test cases; keep the names the same!
  - good candidates include test cases already listed in the spec
4. Update the `README` with instructions on how to use the autograder. The [`spring-23-autograder`](https://github.com/UCLA-CS-131/spring-23-autograder) is a good starting point.
5. **Double-check**:
  - the repository should not contain any private test cases or solution code!
  - pasting in a working solution (i.e. `interpreterv1.py` + what it imports) and running `python3 tester.py 1` should result in all test cases being passed
6. Commit and push the code; tag it with a release on GitHub

That's it! You've now wrapped up all the autograder setup. What's left is barista!

## Setting Up Barista

We'll now scaffold out the necessary items to get Barista up and running. To do this, you will need a working solution for P1.

### Prerequisite: Ownership

Unlike the other infrastructure pieces, there are two questions about ownership:

1. where should the code be kept? We can't make it fully public, since it contains solution code for the project.
2. which [Fly](https://fly.io) instance should be used? You may or may not have access to the original one at `barista.fly.dev`.

We suggest:

1. a private repository that holds the `barista` code, which forks the public one
  - or alternatively, keep a local copy of the git repository that you *never* push
2. create a new Fly app with a new domain (e.g. `barista-s23.fly.dev`) that you fully control
  - or alternatively, deploying to the same Fly instance (which requires a credentials handoff)

This guide assumes that you are doing the numbered suggestions, but the same ideas work if the alternatives are taken; they're just slightly trickier.

### Repo Setup

"Fork" the existing [barista repo](https://github.com/UCLA-CS-131/barista) to a private one. We say "fork" since real forks can't be private; so, you probably just want to make a new private repository and copy all the contents over. **Make sure to copy over the dotfiles, i.e. `.github/workflows`!**

Make sure you can run the code in your private copy (see: [Getting Started]({% link getting-started.md %})).

### Backend

First, we'll update the backend to handle the new version. The steps are as follows:

1. create a new folder `interpreters/YOUR_QUARTER` (e.g. `interpreters/s23`)
2. add a working solution. In `interpreters/YOUR_QUARTER`,
  - copy in the `interpreterv*.py` and imported files from the current project solution
  - **important**: change all relative imports (in all new files) to specify the current directory.
    - `import classv3` should become `from . import classv3`
    - `from classv3 import ClassDef` should become `from .classv3 import ClassDef`
    - **Your code will not work without this!**
3. add an executor. Create `interpreters/YOUR_QUARTER/executor.py` with the code block below.
4. finally, add a Flask endpoint for the API. Edit `app.py` to include the function with the code block below.

```py
# Step 3: Add an executor to interpreters/YOUR_QUARTER/executor.py
from . import interpreterv1

def run(version, raw_program, stdin=None):
    match version:
        case "1":
            interpreter = interpreterv1
        case _:
            raise ValueError("Invalid version; expected one of 1")
```

```py
# Step 4: Add an endpoint to app.py; replace s23 with your quarter
from interpreters.s23 import executor as executors23

# ...

@app.post("/s23")
def s23():
    return parse_and_run(executors23)
```

### Frontend

At this point, you should be able to ping the new endpoint and test its availability. However, we can also test this by setting up the frontend.


1. create `src/constants/YOUR_QUARTER.ts` with the contents in the code block below (replacing `s23` with your quarter where relevant)
  - replace the `defaultProgram` key with a valid program for P1
2. edit `src/constants/index.ts` to import and register your new "quarter", shown in the code block below.
3. hard-refresh your React app.
  - the new default UI should include your newly-created version
  - the default program should be what's in `defaultProgram`
  - when you hit `"Run"`, the program should evaluate and display the correct item in the output

```js
// Step 1: create src/constants/YOUR_QUARTER.ts
export const S23_VERSIONS = [
  { version: "1", quarter: "s23", title: "brewin", defaultProgram: "(print 'hello world')" },
]
```

```js
// Step 2: edit src/constants/index.ts
import { S23_VERSIONS } from "./s23"; // edit S23_VERSIONS to the name from step 1, and ./s23 to the file path from step 1

// ...

const DEFAULT_VERSION = S23_VERSIONS[0]; // edit S23_VERSIONS to the name from step 1

export {
  // ..
  S23_VERSIONS, // edit S23_VERSIONS to the name from step 1
};
```

{. note }

Want to add code highlighting? See [Deep Dive: Barista]({% link advanced/barista.md %})

### Deploy

**Before** you commit your changes and push them, you should test with a local deploy first!

You should:

1. [Install `flyctl`](https://fly.io/docs/hands-on/install-flyctl/)
2. [Log in to Fly](https://fly.io/docs/getting-started/log-in-to-fly/)
3. Delete the current `fly.toml`
4. Follow the [rest of the Fly Python tutorial](https://fly.io/docs/languages-and-frameworks/python/#configure-the-app-for-fly), skipping the part about the `Procfile` (there's an existing `Dockerfile`)
  - when asked to name your app, give it something like `barista-YOUR-QUARTER` (e.g. `barista-s23`) - this becomes your URL!

This should give you a working Fly app - which you can visit! Verify that everything works properly (e.g. submitting and running code).

Now, **don't push your commits yet**. You'll want to do the last step!

### Setting up Continuous Deploys

{: .note }

GitHub Actions allows 2000 minutes per month of CI time. This is pretty generous (each Fly build takes about 2 minutes), but be careful!


We'll now set up Fly to automatically deploy for each commit to `main`. The `barsita` repository already has a workflow set up for this; you just need to get an API token and set the GitHub environment variable.

To do this, follow steps 4, 5, and 6 of [Fly's tutorial](https://fly.io/docs/app-guides/continuous-deployment-with-github-actions). You don't need to do the other steps since they're already set up!

Now, you can finally commit and push your changes. You should see an Actions run on the commit (which will look like [this run on the public repo](https://github.com/UCLA-CS-131/barista/actions/runs/5416229605/jobs/10025430812)). If you could deploy properly locally, it's pretty likely that the CD version will also work.

Once it's updated, take a look at the deployed URL (e.g. `barista-s23.fly.dev`); it should still work as intended.

And that's it - everything is now properly setup!
