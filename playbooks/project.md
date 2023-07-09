---
title: "Project Lifecycle"
layout: default
parent: Playbooks
nav_order: 2
---

# Project Lifecycle

This playbook runs through everything you need to do to manage a project, from setup to sending out grades.

{: .important }

This playbook assumes you've already completed the [Getting Started]({% link getting-started.md %}) guide, and that the [autograder has been set up]({% link playbooks/beginning.md %}).

{: .no_toc }

## Table of Contents

{:toc}

- dummy item

{::options toc_levels="2..4" /}

## Preparation

{: .important }

This section assumes that you have some sort of working solution for the project. Don't proceed without one!

### Autograder Setup (code and files)

First, we'll scaffold out the directory structure for test cases.

Create a folder `v*` that has its own folders `tests` and `fails`, where `*` is the version of the project that you're running. Ex, for `v3`:

```sh
$ mkdir v3
$ mkdir v3/tests
$ mkdir v3/fails
```

The test cases live within `tests/` and `fails/`. Each test case is comprised of a source code file (`.brewin`), an expected output (`.exp`), and optionally standard input to pass to the interpreter (`.in`). Let's make our first test case:

```sh
$ echo "hello world" > v3/tests/hello_world.exp
$ echo "print('hello world')" > v3/tests/hello_world.brewin
```

Test cases are based on file names; make sure they match!

Now, we'll set up the code needed to load a new project version into the autograder. This is relatively simple!

In `tester.py`, create a function called `generate_test_suite_v*`. Ex, for `v3`:

```py
def generate_test_suite_v3():
  """wrapper for generate_test_suite for v3"""
  tests = ["hello_world"] # list of tests with expected output for v3
  fails = []              # list of tests with expected errors for v3

  return __generate_test_suite(3, tests, fails) # change this 3 to whatever version you're running!
```

Note that in the `tests` list, we added a `hello_world`; the extensions for the test are inferred.

Then, add this to the `match` statement in the `main` function.

```py
async def main():
  """main entrypoint: argparses, delegates to test scaffold, suite generator, gradescope output"""
  # ...

  match version:
    case "1":
      tests = generate_test_suite_v1()
    case "2":
      tests = generate_test_suite_v2()
    case "3":
      tests = generate_test_suite_v3()
    case _:
      raise ValueError("Unsupported version; expect one of 1,2,3")

```

Finally, you'll need an interpreter to test against! Grab your own (or one from Carey). You can now run the autograder with `python3 tester.py *`, where `*` is the version you specified in the match statement.

The output depends on the error. For example, if the interpreter gave the wrong answer:

```sh
$ python3 tester.py 3
Running 1 tests...
Running v3/tests/hello_world.brewin...
Expected output:
['hello world']

Actual output:
['5']
 FAILED
0/1 tests passed.
Total Score:      0.00%
```

In contrast, if it was correct:

```sh
$ python3 tester.py 3
Running 1 tests...
Running v3/tests/hello_world.brewin...  PASSED
1/1 tests passed.
Total Score:    100.00%
```

Great! The autograder is all set up. This is a great time to commit our work:

```sh
$ git add .
$ git commit -m "Scaffolding Project 3"
$ git push
```

Now, let's add some test cases!

### Writing Test Cases

The next step is to write test cases - a lot of them! Some quick suggestions:

- make sure to test each item in the spec! A good way to do this is to read through the table of contents, and add test cases for each category.
- try to provide non-overlapping test cases (between features), which can be a good way to give partial credit. For example, if a project requires generics and error handling, try having some cases handle *just* generics, some *just* error handling, and maybe a handful that mix the few!
- test case names are public to students - pick ones that are reasonably helpful (but don't leak the entire test case)

As you add test cases, you should "register" them in `generate_test_suite_v*`.

Test cases that test for errors (`fails/`) work very similarly to output test cases; the output should be the stringified error message you expect from the `.get_error_type_and_line()` method. For example (in a case with line numbers disabled):

```sh
$ echo "ErrorType.TYPE_ERROR" > v3/fails/plus.exp
$ echo "print('3' + 4)" > v3/fails/plus.brewin
```

You can read the comparison code for this in `tester.py`'s `run_test_case()` method.

Before you deploy to Gradescope, this is a great time to commit your work!

```sh
$ git add .
$ git commit -m "Added 50 test cases to Project 3"
$ git push
```

### Deploying to Gradescope

First, we'll make two tiny code adjustments. In the `Makefile`, add a line for your version; you can copy the one below, and change the three `v3`s:

```
v3: clean run_autograder setup.sh tester.py harness.py bparser.py intbase.py v3
	zip -r grader.zip run_autograder setup.sh tester.py harness.py bparser.py intbase.py v3
```

{: .note }

There's a circular dependency since `v3` is both a task and a folder. Matt never got around to resolving this, but it's a very short fix!

Next, in `run_autograder`, change the version of the `python3` command to match what you're deploying!

```sh
# changes this depending on the assignment!
PROD=True python3.11 tester.py 3
```

Now, you should be able to run `make v3` to get a `grader.zip` file:

```sh
$ make v3
...
$ ls | grep "grader.zip"
grader.zip
```

This is the final deliverable that you need! You can now [create an autograded assignment in Gradescope](https://gradescope-autograders.readthedocs.io/en/latest/getting_started/). When you're asked for a `.zip` file, upload the `grader.zip`.

Once you've finished uploading, run a test with your own copy of `interpreterv*.py`, just to make sure that everything works smoothly! The autograder should return a result within five seconds.

Great job! Make sure to commit a working build, so other TAs can reproduce it if necessary.

```sh
$ git add .
$ git commit -m "Make Project 3 deployable"
$ git push
```

## Releasing to Students

{: .important }

This section assumes that you have some sort of working solution for the *previous* and current projects. Don't proceed without them!

### Updating the Project Starter

The project starter is what we give students to bootstrap each project. The contents of the project starter are:

- always: evergreen project infrastructure, which usually includes:
  - `intbase.py`: a file that defines the ABC that students subclass, as well as enums and constants for various parts of the program
  - `bparser.py`: a file that parses programs for students. We first introduced this in S2023.
- after Project 1: a solution to the *previous* project, which includes:
  - an `interpreterv*.py`: the entrypoint for the interpreter
  - any supporting files the interpreter imports (e.g. `env_v2.py`)

To update the starter,

1. review the previous project solution *prior* to publishing. This should involve:
  - using a code formatter like [black](https://pypi.org/project/black/) to enforce a consistent code style across all the files
  - resolving open lints (by either fixing or ignoring the problem) using [pylint](https://pypi.org/project/pylint/)
  - checking for any typos + unused methods/classes/files
  - a good heuristic for knowing when you're done: if running black + pylint produces no errors (this is easy to do with the VSCode extensions!)
2. verify that the provided solution *passes all of the test cases* for the previous project.
3. commit any updates to the evergreen project infrastructure. Since these changes are uncommon, include a commit message that explains what happened!
4. commit the new solution to the previous project. In particular, **this commit should not overwrite any existing files**; it should only create new ones!
5. push all the relevant changes and create a new release on GitHub, tagging the `main` branch

Great! You're now ready to update the public autograder.

### Updating the Public Autograder

The public autograder is a functionally equivalent subset of the autograder we use, except:

- it doesn't have all of the test cases
- it doesn't include irrelevant items for students, i.e. the Gradescope update packager

To update the public autograder,

1. migrate any updates made to the project infrastructure in [Updating the Project Starter](#updating-the-project-starter); make a commit with a descriptive message
2. then, repeat the steps in [Autograder Setup (code and files)](#autograder-setup-code-and-files) in the local autograder
3. register the public test cases you'd like students to see, using the steps in [Autograder Setup (code and files)](#autograder-setup-code-and-files); make a commit with a descriptive message
  - usually, we add between 10%-20% of the private test cases; keep the names the same!
  - good candidates include test cases already listed in the spec
4. verify that the provided solution *passes all of the test cases* for the current project (but **do not commit it**)
5. update the `README` for the repository; make a commit with a descriptive message
6. push all the relevant changes and create a new release on GitHub, tagging the `main` branch

If you need to deviate from these instructions, it likely means that something is wrong with the private autograder; make sure to keep those in sync!

### Deploying to Barista

{: .note }

It's up to the TAs to figure out how to version-control barista without publically publishing the solution to the project. We'll assume that you're keeping a private repository.

Now, we'll deploy a working interpreter for the current project to barista, so students can verify that their test cases are well-formed.

To update barista,

1. (if not already): run through [Getting Started: Barista]({% link getting-started.md %}#setup-barista), which includes getting access to the [Fly](https://fly.io) instance
2. add a working solution. In `interpreters/YOUR_QUARTER`,
    - copy in the `interpreterv*.py` and imported files from the current project solution
    - **important**: change all relative imports (in all new files) to specify the current directory.
        - `import classv3` should become `from . import classv3`
        - `from classv3 import ClassDef` should become `from .classv3 import ClassDef`
        - **Your code will not work without this!**
3. add a new *executor*, which will run the project solution in the backend. In `interpreters/YOUR_QUARTER/executor.py`, import the new `interpreterv*.py`, and add a `case` statement that sets `interpreter` to it. (example below)
4. register the new version in the frontend. In `src/constants/YOUR_QUARTER.ts`, add a new object to the `export const YOUR_QUARTER_VERSIONS` array. (example below)
  - `version` should match the value in the `run` function you made earlier
  - `quarter` should match the values for previous versions/your quarter
  - `title` is what's rendered in the language selection dropdown
  - `defaultProgram` is the first program loaded if the language is the default language. Pick a concise example from the public test cases. Use backticks to create multi-line strings.
  - optional: `highlighter`; see [Deep Dive: Barista]({% link advanced/barista.md %}) for more information.
5. change the `DEFAULT_VERSION` in `src/constants/index.ts` to be the index of the new object you added in step 4.
6. test your change locally first! Using the instructions in [Getting Started: Barista]({% link getting-started.md %}#setup-barista), serve a local copy of both the backend and frontend. Verify that the "run!" button does what's intended!
7. once you've verified this works, deploy a new copy with `fly deploy` (and/or committing to your private repository)
8. verify that the newly-deployed app works *online* (i.e. at the `fly.dev` domain)

Phew! That was a lot, but now all the course infrastructure is ready for students to work on. Great job!

```py
# executor from step 3
from . import interpreterv1
from . import interpreterv2
from . import interpreterv3

def run(version, raw_program, stdin=None):
    match version:
        case "1":
            interpreter = interpreterv1
        case "2":
            interpreter = interpreterv2
        case "3":
            interpreter = interpreterv3
        case _:
            raise ValueError("Invalid version; expected one of {1,2,3}")
```

```js
// version from step 4
export const S23_VERSIONS = [
  // ...
  {
    version: "3",
    quarter: "s23",
    title: "brewin##",
    defaultProgram: "print('hello world')",
  },
];
```


Commit [`debe7e7`](https://github.com/UCLA-CS-131/barista/commit/debe7e7d55778c1fa1979bf5ba6106059c1f0d7c#diff-a7d5c79d9bfe2064aa48a56bc7a3ba38ebe5c0dc8482a686122b75f73ca7b4d6) is a worked example of the above steps for P2/P3 in S2023.

### Release Spec & Gradescope

The final step is to update the course website. In `projects.md`, add a new heading and list all the relevant details. Here's a template for your convenience:

```md
## Project 3

Project 3 has been released! It is due at **11:59 PM PT on June 4th**. Some links that you'll find helpful:

- [Project 3 Spec](https://docs.google.com/document/d/1YqSGkY4lE5nr-u27TQ-C8vd7f21SQA-qHL1aZf0ye4s/edit?usp=sharing)
- [Project Autograder](https://github.com/UCLA-CS-131/spring-23-autograder) - **includes test cases!!**
- [Project Starter Template](https://github.com/UCLA-CS-131/spring-23-project-starter)
- [Gradescope Submission](https://www.gradescope.com/courses/529662/assignments/2906325)
- [Barista](https://barista.fly.dev/)
```

If you haven't already, make the assignment public on Gradescope.

Once this is done, you're good to send an email out to students! Great job!

## Mid-Project Maintenance

### Updating the Autograder

Updating the private autograder is simple:

1. make the necessary changes
2. run `make v*` (e.g. `make v3`) to generate a `grader.zip`
3. on Gradescope, [replace the current autograder](https://gradescope-autograders.readthedocs.io/en/latest/getting_started/)
4. optionally; re-run all students' submissions (this is a good idea if a test case is bugged)

To update the public one, refer to [Updating the Public Autograder](#updating-the-public-autograder); make sure to pick a descriptive commit / PR message!

In either case, it's good etiquette to let the students know that a change has been made (via announcement).

### Changing Student Submissions on Gradescope

Students may want to activate an earlier submission to be their "final grade"; typically, this is related to the late penalty. Depending on access control settings, they may be able to "activate" their desired solution; however, you can also do so:

1. Go to "Assignments"
2. Click on the assignment (ex Project 3)
3. Go to "Manage Submissions"
4. Click on the student
5. Click on "Submission History"
6. Select "Activate" on the desired submission

## Releasing Grades

There are three items to do:

1. Release solutions to the project: do this by [Updating the Project Starter](#updating-the-project-starter)
2. Release the private test cases: do this by adding them to the [Public Autograder](#updating-the-public-autograder) (i.e. copy over the new test cases to `tests/` and `fails/`, and register them in `tester.py`)
3. Release the actual grades through Gradescope

Releasing grades is less important for the project, since students already know their grade. However, to see the final breakdown (as well as summary statistics like the mean and median), you need to hit "Publish Grades" in the "Review Grades" section of the assignment on Gradescope. Optionally, you can send students an email letting them know that their grades are in.

Typically, we do not handle late penalties within Gradescope; those are applied at the end of the quarter.
