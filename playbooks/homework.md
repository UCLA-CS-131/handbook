---
title: "Homework Lifecycle"
layout: default
parent: Playbooks
nav_order: 3
---

# Homework Lifecycle

This playbook runs through everything you need to do to manage a homework, from setup to sending out grades. It *does not* cover how to write a good homework; that'll be up to you and Carey to decide!

{: .important }

This playbook assumes you've already completed the [Getting Started]({% link getting-started.md %}) guide.

{: .no_toc }

## Table of Contents

{:toc}

- dummy item

{::options toc_levels="2..4" /}

## Preparing the Homework

There are two deliverables in a homework:

1. a PDF of the homework questions; this is what the students will use to do the homework
2. an annotated PDF of the homework questions *with* answers; this is what students will use to check their answers (after submission) and revise for exams

In general, exporting a PDF from Google Docs (or similar) is straightforward. Here are a handful of things to look out for:

- check for typos! commonly, mistakes are made when *numbering* the problems.
- make sure the questions and solutions docs are in sync; sometimes, the solutions are out of date

## Publishing the Homework

### Gradescope

On Gradescope, create a new assignment; select the "Homework / Problem Set" type. Then, upload the PDF from the previous section as the template.

Following the rest of the wizard is pretty stragithforward; some callouts:

- **important: check "Allow students to view and download the template"**. This *isn't* enabled by default, which means students won't be able to see the homework!
- when creating the outline, we *highly* recommend creating an "effort" question and pre-making the rubric; this lets students automatically select the pages to submit under that question
- double-check the due date *and* the late due date

Set the release date to whenever the homework should be visible to students. You can optionally send an announcement via Bruinlearn that the homework is available.

### Course Website

Update the entry in `homeworks.md` with the correct Gradescope link and due date.

## Grading the Homework

Some tips as you grade the homework:

- the "grade by course section" UI is helpful!
- familiarize yourself with [Gradescope's keyboard shortcuts](https://help.gradescope.com/article/5uxa8ht1a2-instructor-assignment-grade-submissions#interaction_tips); you might find particularly helpful
  - `1`, `2`, `3`, ... which apply that rubric item
  - `z`, which skips to the next ungraded homework
  - right arrow, which moves to the next page

Optional but highly suggested:

- keep track of what problems students are struggling with; this is a great heads-up for material that you should cover in discussion!
- when possible, provide feedback on each homework
  - a simple "good job!" goes a long way :)
  - identifying common mistakes (e.g. confusing templates and generics) can help resolve misunderstandings early on

{: .note }

Homework feedback can have a big impact! Here's a motivating piece of feedback from Matt's S23 course evals:

> Something unexpected that really impacted my outlook on this class was getting comments on my homework. You have no idea how much better my day became when I read those comments, even if those comments were mostly outlining mistakes I made. Just knowing that a person was looking at my work (especially work that doesn't always have an objective answer like the projects) was huge motivation to try harder.
>
> -- anonymous student

## Releasing Grades and Solutions

### Grades (on Gradescope)

To publish grades, click the **Publish Grades** button in the "Review Grades" section. We recommend doing this as soon as at least one TA is done grading the homeworks.

Grades can be published at any time; if the homework is partially graded, students with ungraded homeworks will not see any score until it is graded.

Typically, we do not send an email to students that their homework has been graded.

### Solutions (on course website)

1. create a *public* homework solutions file in the Shared Drive
2. update the entry in `homeworks.md` with the link to the solutions doc
