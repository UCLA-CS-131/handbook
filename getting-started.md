---
title: Getting Started & Setup
layout: default
nav_order: 2
---

# Getting Started & Setup

Are you a new (or returning) TA? Start here!

{: .no_toc }

## Table of Contents

{:toc}

- dummy item

{::options toc_levels="2..4" /}

## Required (and not required) knowledge

First, **please read the [Why]({% link index.md %}#why) and [Infrastructure Overview]({% link infra-overview.md %})** pages.

The rest of this guide assumes that you know:

- how to use [git](https://git-scm.com/) and [GitHub](https://github.com/) as a version control system
- how to use your local shell (either something `sh`-like on macOS/Linux/WSL, or Powershell on Windows)
- basic proficiency in [Python](https://www.python.org/) (for the autograder + barista) and [Markdown](https://en.wikipedia.org/wiki/Markdown) for the course website

In contrast, this guide will explain the technologies needed to run the course website, autograder, and barista. This includes:

- how to install [Ruby](https://www.ruby-lang.org/en/) and [Jekyll](https://jekyllrb.com/) to build the course website
- how to use [GitHub Actions](https://github.com/features/actions) and [GitHub Pages](https://pages.github.com/) to deploy the course website
- how to locally run the autograder, and how to deploy it to [Gradescope](https://gradescope.com) for students to use
- how to install [Node.js](https://nodejs.org/en) and install Python dependencies with [pip](https://pypi.org/project/pip/), to locally run and build barista
- how to deploy new copies of barista with [Fly](https://fly.io/)

There are some things the guide *won't* explain because they're not necessary to maintain the course infrastructure, but could help with extending or debugging it. This includes:

- how to *program* with [Liquid](https://shopify.github.io/liquid/), Jekyll's templating language; generally, you only need to do things in the Markdown world with occasional Jekyll tags
- how to *program* with [Ruby](https://www.ruby-lang.org/en/), which powers Jekyll; you likely don't need to go this "low-level" into the static site generator
- how to use [Docker](https://www.docker.com/), which
  - builds the image the autograder runs on; this is abstracted away from you by Gradescope
  - builds the image barista runs on; this has been done for you already
- how to use [React](https://react.dev/) and [Flask](https://flask.palletsprojects.com/), which power barista's frontend and backend respectively

## Setup: Course Website

### Prerequisite: Installing Ruby

{: .warning }

You may already have a system Ruby installed. If it is out of date, **you still need to do this section**; at minimum, you need to be on Ruby 3.

In this section, we'll install the [Ruby](https://www.ruby-lang.org/en/) programming language, and verify that it (and [Bundler](https://bundler.io/), its package manager) is properly installed.

To install Ruby, we highly recommend using a version manager. The most popular ones are [rbenv](https://github.com/rbenv/rbenv) and [rvm](https://rvm.io/), which work on macOS/Linux/WSL. On stock Windows, [RubyInstaller](https://rubyinstaller.org/) is the de facto best choice.

Once you've properly set up your version manager, you should install a recent version of Ruby. In almost all cases, the latest stable version should suffice; note that you *must* install Ruby `>= 3`.

You can verify that your installation succeeds with the `-v` command:

```sh
$ ruby -v
ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [arm64-darwin21]
```

Ruby's default package manager is called Bundler. If you installed Ruby using the above method, it should be linked into your PATH by default:

```sh
$ bundler -v
Bundler version 2.3.26
```

If it isn't, try reinstalling Ruby.

### Clone and Install

First, let's clone the repository. Replace the URL with the relevant course website repo:

```sh
git clone git@github.com:UCLA-CS-131/spring-23.git
```

Every time you clone the repository, you'll need to install all the Ruby dependencies with Bundler:

```sh
$ cd spring-23
$ bundle
```

If this fails, one of two things likely happened:

- your Ruby installation is broken; reinstall Ruby with the above steps
- there's an issue with your *platform* (typically on Windows and non-standard Linux distros)

### Serving

Once you have a local copy of the website, running a local copy is pretty simple; ex, for this website itself:

```sh
$ bundle exec jekyll serve
Configuration file: /Users/matt/code/handbook/_config.yml
            Source: /Users/matt/code/handbook
       Destination: /Users/matt/code/handbook/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.479 seconds.
 Auto-regeneration: enabled for '/Users/matt/code/handbook'
    Server address: http://127.0.0.1:4000/handbook/
  Server running... press ctrl-c to stop.
```

You can now visit a copy of the website at [http://127.0.0.1:4000/handbook/]. By default, Jekyll comes with auto-regeneration; after editing any files in the site, Jekyll will rebuild your site. To view your changes, all you'll need to do is refresh the page!


## Setup: Autograder

### Prerequisite: Installing Python

{: .warning }

You may already have a system Python installed. If it is out of date, **you still need to do this section**; at minimum, you need to be on Python 3.9.

Please [install Python 3](https://www.python.org/downloads/). You can verify its success with:


```sh
python3 --version
```

(unlike Barista, the autograder has no dependencies and doesn't use a `venv`, so it's not necessary to verify `pip` and `venv`)

### Prerequisite: Installing `make`

Most systems already come with [`make`](https://www.gnu.org/software/make/), a utility that helps generate various files from other source files. We use `make` to generate the final Gradescope `.zip` bundle.

You can verify if `make` already exists with

```sh
$ make -v
GNU Make 3.81
Copyright (C) 2006  Free Software Foundation, Inc.
This is free software; see the source for copying conditions.
...
```

To install `make` if it doesn't exist, follow the instructions for [GNU `make`](https://www.gnu.org/software/make/) (on macOS/Linux/WSL) or [GnuWin](https://gnuwin32.sourceforge.net/install.html) for Windows.

### Clone, Test, Deploy

{: .important }

This section uses a *private* repository that is only accessible to members of the [GitHub Organization](https://github.com/UCLA-CS-131).

First, let's clone the repository. We're intentionally using `--depth=1` to avoid unnecessary history from past quarters.

```sh
git clone --depth=1 git@github.com:UCLA-CS-131/autograder-experimental.git
```

Enter the folder:

```sh
cd autograder-experimental
```

You can now do a variety of actions, including running the autograder locally:

```sh
$ python3 tester.py 3 # grading version 3 of the project
...
40/40 tests passed.
Total Score:    100.00%
```

Or, package the autograder for deployment to Gradescope via a `grader.zip`. *This only packages a select few files, listed in the `Makefile`*.

```sh
$ ls | grep "grader.zip"

$ make v1
...
$ ls | grep "grader.zip"
grader.zip
```

## Setup: Barista

### Prerequisite: Installing Python

{: .warning }

You may already have a system Python installed. If it is out of date, **you still need to do this section**; at minimum, you need to be on Python 3.9.

Please [install Python 3](https://www.python.org/downloads/). You must have a working Python installation with access to [`pip`](https://pypi.org/project/pip/) and [`venv`](https://docs.python.org/3/library/venv.html). You can verify all three with:

```sh
python3 --version
```

```sh
pip3 --version
```

```sh
python3 -m venv -h
```

{: .note }

On some installations, these will be under `python` and `pip` instead of `python3` and `pip3` respectively.

If any of these commands fail, you'll want to reinstall Python.

### Prerequisite: Installing Node

{: .warning }

You may already have Node installed. If it is out of date, **you still need to do this section**; at minimum, you need to be on Node 16.

In this section, we'll install the [Node.js](https://nodejs.org/) JS runtime, and verify that it (and [npm](https://www.npmjs.com/), its package manager) is properly installed.

To install Node, we highly recommend using a version manager. The most popular one is [`nvm`](https://github.com/nvm-sh/nvm), which works on macOS/Linux/WSL. On stock Windows, [`nvm-windows`](https://github.com/coreybutler/nvm-windows) is the equivalent.

You can verify that Node and `npm` are properly installed with the following commands:


```sh
node -v
```

```sh
npm -v
```

### Clone and Install

First, let's clone the repository.

```sh
git clone git@github.com:UCLA-CS-131/barista.git
```

Next, we can install all of the Node dependencies using `npm`:

```sh
$ cd barista
$ npm install
```

Next, we'll set up a [`venv`](https://docs.python.org/3/library/venv.html). This lets us create an isolated and reproducible Python environment to run the backend.

```sh
$ python3 -m venv venv
$ source venv/bin/activate
$ pip install -r requirements.txt
```

This last command installs all the relevant Python packages using `pip` *within* the `venv`.

### Serving

We need to both serve the frontend and the backend. First, we can run the Flask backend in dev mode:

```sh
$ source venv/bin/activate # activate the venv
$ npm run start-api

> barista@0.1.0 start-api
> venv/bin/flask run --port 8000

 * Serving Flask app 'app.py'
 * Debug mode: on
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:8000
Press CTRL+C to quit

```

This opens the Flask server at [http://127.0.0.1:8000](http://127.0.0.1:8000); you can ping it locally without the frontend.

{: .note }

Every time you open a new terminal to serve the app, you need to re-activate the `venv`; if not, you won't have access to the packages you installed earlier!

Then, we can serve the frontend with the traditional node workflow:

```sh
$ npm start
Compiled successfully!

You can now view barista in the browser.

  http://localhost:3000

Note that the development build is not optimized.
To create a production build, use npm run build.

webpack compiled successfully
No issues found.

```

This should automatically open the React app at [http://localhost:3000](http://localhost:3000).

### Optional: Fly (for Deploys)

The current iteration of barista is deployed with [Fly](https://fly.io/), a Heroku-like PaaS. Matt has set up a GitHub Action that automatically deploys on each commit; but, there are often times where you'll want to deploy an instance *without* publically committing on GitHub (e.g. in the middle of a project).

The sketch of the steps is as follows:

1. [Sign up for a Fly account](https://fly.io/app/sign-up) - we recommend using GitHub SSO
2. Install [`flyctl`](https://fly.io/docs/hands-on/install-flyctl/), Fly's CLI for their platform
3. Run `fly deploy` within the repository

One of the reasons that Matt chose Fly is that the process is *extremely* smooth. This means that you should be able to spin up arbitrary barista instances very quickly!
