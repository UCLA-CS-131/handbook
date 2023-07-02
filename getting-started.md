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
$ git clone git@github.com:UCLA-CS-131/spring-23.git
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

## Setup: Barista
