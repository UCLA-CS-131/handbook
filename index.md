---
title: Home
layout: home
nav_order: 0
---

# CS 131 TA Handbook

This is a TA handbook for UCLA COM SCI 131. The idea is that a new TA could read this website and know how to run the infrastructure for the class, without much prior experience! This currently covers the course website, autograder, and barista - as well as some of the processes behind typical course administration.

If you're a TA for the class, you might want to:

1. read through the [infrastructure overview]({% link infra-overview.md %})
2. get yourself [all set up]({% link getting-started.md %}) to administer the course
3. read through the various [playbooks]({% link playbooks/index.md %}) on:
  - [beginning of quarter housekeeping]({% link playbooks/beginning.md %}), including creating the website and setting up all project-related things
  - managing a [project]({% link playbooks/project.md %}), [homework]({% link playbooks/homework.md %}), or [lecture note]({% link playbooks/lecture-note.md %}) from start to finish
  - learn how to [update other areas of the website]({% link playbooks/update-website.md %})

You might also be interested in some of the (optional but context-providing) [advanced topics]({% link advanced/index.md %}).

If you're not a TA for CS 131, welcome - we'd always love an extra set of eyes. And, no matter who you are, contributions are welcome - please [let us know on GitHub](https://github.com/UCLA-CS-131/handbook) if you see any problems; PRs are even more welcome!

## Why?

Medium-length answers to some meta-questions; the [Advanced: Philosophy]({% link advanced/philosophy.md %}) page has more long-form content.

### Why is this website so *long*?

Two answers! On a shallow level, there's a lot to talk about :)

But, on a deeper level, we wanted this handbook to make as few assumptions as possible about incoming TAs. Undergraduate computer science curricula vary across institutions, which means there isn't one "standard background" for any new TA. This is especially important given that a significant portion of course infrastructure is related to software engineering skills that aren't frequently taught in curricula. We do assume some knowledge of Python, git, and using the command line - but beyond that, aim to explain as much about new tooling as possible.

TAs experienced with our tooling can skim or skip sections of this handbook. Our goal is to make it comprehensive for *all* TAs.

### Why is there so much infrastructure for this course?

In some ways, there isn't! At many other schools - particularly at large public schools that serve thousands of students - there's a wealth of existing open-source course infrastructure for other functions (e.g. office hour queues and guided assignments).

However, it's true that this is not the norm at UCLA CS. The motivation for each piece of infrastructure is directly tied to student needs and feedback.

Transparent grading (and autograding) was a top 3 concern for *every* departmental town hall between 2019 and 2022 inclusive. This motivated the use of Gradescope and the live remote autograder; the clonable local autograder framework and sample test cases; and, most recently, barista. Our new framework helps students catch errors like incorrectly named files, wrong line endings, and other issues that are othorgonal to class learning objectives. It also encourages students to learn test-driven development while letting them focus on the actual skill (writing effective tests) rather than matching parity with a closed-source grader.

Both in departmental town halls and prior course feedback, students indicated that they'd like more online, centralized, and open access resources. The open-source course website supports a variety of students, from auditors to propsective students to past students who want to brush up on material. The focus on lecture notes and course topics makes it simpler for students to revise for exams, and focus on critically thinking during the lecture (rather than typing every code snippet).

Of course, this infrastructure requires maintenance. Which leads into...

### Why does this website exist?

Two reasons! First, the narrow one: quite a bit of this course infrastructure was built and managed by [Matt](https://matthewwang.me/) in the first two iterations of the course; a side effect is that much of it remained in his head and relied on his skillset. We want to make sure that future TAs can still use the course infrastructure (and benefit students), even though he's not around as a TA anymore. In a sense, this website *is* a handoff document.

However, that's not the complete story. The much broader reason is that there is little institutional knowledge passed between TAs for many classes at UCLA CS. For some classes, it's not uncommon for TAs to rewrite autograders every quarter *for the same assignment*, which seems like avoidable duplicate work! This means TAs can't spend as much time on student interaction or pedagogical work. Thus, the goal of this website is to make course infrastructure as seamless as possible, so TAs can focus on what they're needed for most! As this website gets improved over iterations of the course, it acts as a form of digital institutional knowledge; TAs will be able to learn from past experiences.

There are some nice related benefits. First, it's easier to see *what* the responsibilities of TAs are in this class, which can help with transparent hiring. It also provides students a level of transparency into the inner workings of their course, which can even motivate student-driven infrastructure improvements. Big picture, we're hopeful that this can contribute to the broader spheres of institutional knowledge within CS education, both at UCLA and beyond.

## Licensing and Attribution

This repository's code is licensed under the [MIT License]. You are generally free to reuse or extend upon this code as you see fit; just include the original copy of the license.

This repository's content is licensed under the [CC-BY SA 4.0 License]. This provides restrictions on reuse -- in particular, you must attribute the content and provide transformations under the same license as the original.

The initial bootstrap for this repository was written by [Matthew Wang](https://matthewwang.me/), who authored the course infrastructure and TA'd it the first two quarters it was offered by Carey (Fall '22, Spring '23).

[MIT License]: https://en.wikipedia.org/wiki/MIT_License
[CC-BY SA 4.0 License]: https://creativecommons.org/licenses/by-sa/4.0/
