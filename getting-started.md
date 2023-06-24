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

First, **please read the [Why]({{site.baseurl}}/#why) and [Infrastructure Overview]({{site.baseurl}}/infra-overview)** pages.

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

In this section, we'll install the [Ruby](https://www.ruby-lang.org/en/) programming language, and verify that it (and Bundler, its package manager) is properly installed.


## Setup: Autograder

## Setup: Barista
