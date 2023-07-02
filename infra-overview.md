---
title: Infrastructure Overview
layout: default
nav_order: 1
---

# Infrastructure Overview

A one-stop shop that explains all the infrastructure for the course.

{:toc}

- dummy item

{::options toc_levels="2..4" /}

{: .note }
Interested in the *why* of our infrastructure? Take a look at the [Philosophy]({% link advanced/philosophy.md %})!

## Course Website

Our course website is slightly more complicated than plain HTML, CSS, and JavaScript -- but not by much! We use a type of software called a **static site generator**. In a nutshell, this lets you write code in a different language that *generates* HTML, CSS, and JS. Once you compile the site, the resulting object is just HTML, CSS, and JS that can be served any way that you'd like.

The course website (and, the website you're reading this on) is built using a static site generator called [Jekyll](https://jekyllrb.com/). In particular, we're using a Jekyll theme called [Just the Docs](https://just-the-docs.com/). This provides all of the styling for the website, generates reusable components like the sidebar, and provides neat compile-time features like code highlighting, search, and section navigation.

The course website additionally uses [Just the Class](https://github.com/kevinlin1/just-the-class), a theme made by [Kevin Lin](https://kevinl.info/) at UW that adds some class-related functionality to Just the Docs, such as the weekly schedule.

Jekyll lets you use [Markdown](https://en.wikipedia.org/wiki/Markdown) to write content, which gets converted to HTML at compile-time. In addition, the [Liquid](https://shopify.github.io/liquid/) templating language allows you to use primitives like conditionals and loops, though we won't use them frequently in the website.

Jekyll is written in a programming language called [Ruby](https://www.ruby-lang.org/en/), which you'll need to run Jekyll locally. However, you don't need to know Ruby to use Jekyll.

The site is automatically updated through [GitHub Actions](https://github.com/features/actions), which is a form of **continuous integration** and **continuous deployment**. After every commit to the `main` branch of the repository, an action will automatically build and deploy the site to the GitHub Pages URL. In other words, you don't need to manage the deployment!

## Autograder


## Barista

{: .note }
Want to learn more about Barista? See the [Deep Dive: Barista]({% link advanced/barista.md %}) page.

Barista is a newer addition to our course offering. In a nutshell, it is a simple online runtime for Brewin, the language that students implement throughout the quarter's course projects. Students can write arbitrary Brewin programs and run them against a canonical Brewin interpreter(historically, Carey's implementation). Barista should provide the correct output in all cases of defined behaviour: in either semantically correct Brewin programs, or in the few well-defined error states. In contrast, undefined behaviour is left open to the implementation (which we often display as a `RuntimeError`).

There are three components to Barista:

1. a Python runtime that runs Brewin code. Since Carey's interpreter is written in Python, the runtime itself is a trivial wrapper over the interpreter.
2. a [React](https://react.dev/)-based frontend that provides a simple interface to use the interpreter. It manages some client state (current Brewin version, past submissions, etc.) and sends JSON payloads to the backend.
3. a small [Flask](https://flask.palletsprojects.com/) server that receives API requests from the frontend and forwards them on to the runtime. In our architecture, the Flask server also serves the HTML file for the frontend itself.

Developing on Barista requires some non-trivial knowledge of full-stack web development. However, *maintaining* Barista is relatively simple; TAs only need to copy-paste in new versions of a canonical interpreter and write a handful of lines of Python code (for the backend) and JSON (for the frontend).
