---
title: "Writing a New Lecture Note"
layout: default
parent: Playbooks
nav_order: 3
---

# Writing a New Lecture Note

This playbook runs through how you should add a new lecture note to the course website. There are no additional prerequisites outside of the getting started setup.

{: .important }

This playbook assumes you've already completed the [Getting Started]({% link getting-started.md %}) guide.

{: .no_toc }

## Table of Contents

{:toc}

- dummy item

{::options toc_levels="2..4" /}

## Using a Previous Note

{: .note }

This is *completely optional*; you can write new notes from scratch!

Past TAs have written lecture notes for previous iterations of the course! **While they won't match exactly**, they're a useful base to get started from.

To use a previous lecture note (e.g. from [spring-23](https://github.com/UCLA-CS-131/spring-23)), you can copy parts or all of the corresponding markdown file in `lectures/`, e.g. [`lectures/01.md`](https://github.com/UCLA-CS-131/spring-23/blob/main/lectures/01.md). If you do this, **make sure to update the front matter accordingly**!

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

The rest of the content for the lesson plan should go *after* the table of contents. Ideally, you should use levels of headings (starting at 2, i.e. `##`) to split up the lecture content.

{: .note }

Already finished your note? Head to the [publishing section](#publishing-the-lecture-note).

The next few subsections offer some tips about using Jekyll and Just the Docs/Class. If you're not familiar with Markdown, we suggest you look at GitHub's [markdown tutorial](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).
### Tip: Callouts

Just the Docs has a feature called [callouts](https://just-the-docs.com/docs/ui-components/callouts/) -- mimicking the common UI element. Our course website has the suggested colors, which look like this:

{: .important }

This is a *really critical* concept for the course!

{: .note }

This next section won't be covered on the exam.

{: .new }

This is a new part of the course!

{: .warning }

If you don't follow this step, you won't be able to run Haskell programs on your computer.

This is just generated from Markdown! The code for the above is exactly:

```md
{% raw %}{: .important }

This is a *really critical* concept for the course!

{: .note }

This next section won't be covered on the exam.

{: .new }

This is a new part of the course!

{: .warning }

If you don't follow this step, you won't be able to run Haskell programs on your computer.{% endraw %}
```

Each of the `{: .x }` is applying the `x` CSS class to the following block; this is the same as writing:

```html
{% raw %}<p class="note">
This next section won't be covered on the exam.
</p>{% endraw %}
```

Under the hood, the callouts are configured in the `_config.yml` file for the course website; you can edit them to add new callouts, change colors, etc.

### Tip: Images

You can upload images (such as diagrams or slides) to flesh out the lecture note! Here is the course website logo:

![the number 131]({{site.baseurl}}/assets/images/logo.png)

Which is rendered by the following markdown:

```md
![the number 131]({% raw %}{{site.baseurl}}{% endraw %}/assets/images/logo.png)
```

**Please add [*alt text*](https://developer.mozilla.org/en-US/docs/Web/API/HTMLImageElement/alt) to your image!**

Out of convention, images should go in `assets/images`. The `{% raw %}{{site.baseurl}}{% endraw %}` is necessary for the link to resolve properly.

### Tip: Code Highlighting

Jekyll uses the [Rouge](https://github.com/rouge-ruby/rouge) syntax highlighter to *automatically* highlight your code at compile-time! Since lecture notes often feature code from various languages, code highlighting is especially important.

Here are a few examples:

```py
# Different types of equality in Python
fav = 'pizza'
a = f'I <3 {fav}!'
b = f'I <3 {fav}!'
c = a

if a == b:
  print('Both objects have same value!')
if c is a:
  print('c and a refer to the same obj')
if a is not b:
  print('a and b refer to diff. objs')
```

```hs
--- searching over a binary tree in Haskell
data Tree =
  Nil |
  Node String Tree Tree

search Nil val = False
search (Node curval left right) val
 | val == curval = True
 | val < curval = search left val
 | otherwise = search right val
```

```prolog
% an implementation of member in Prolog
is_member(X,[X|Tail]).
is_member(Y,[Head|Tail]) :- is_member(Y,Tail).
```

Here is the corresponding Markdown code:

{% highlight markdown %}
```py
# Different types of equality in Python
fav = 'pizza'
a = f'I <3 {fav}!'
b = f'I <3 {fav}!'
c = a

if a == b:
  print('Both objects have same value!')
if c is a:
  print('c and a refer to the same obj')
if a is not b:
  print('a and b refer to diff. objs')
```

```hs
--- searching over a binary tree in Haskell
data Tree =
  Nil |
  Node String Tree Tree

search Nil val = False
search (Node curval left right) val
 | val == curval = True
 | val < curval = search left val
 | otherwise = search right val
```

```prolog
% an implementation of member in Prolog
is_member(X,[X|Tail]).
is_member(Y,[Head|Tail]) :- is_member(Y,Tail).
```
{% endhighlight %}

The [Rouge wiki](https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers) has a list of supported languages; virtually all the ones covered in the course are available. Past iterations of the lecture notes have featured C++, Java, Kotlin, Swift, Rust, Go, JavaScript, and many others!

### Tip: Hidden/Toggleable Content

You can use the `<details>` and `<summary>` HTML elements to create hidden-by-default, collapsible content! Here's an example:

> When you create a new template, e.g., `vector<string>`, does the compiler ensure the templated code is type-safe? If so, how?

<details markdown="0"><summary>Answer</summary>

Yes - since the compiler basically generates a concrete version of the function/class with the specified type, and the compiles it as it would any other class, type safety is guaranteed!
</details>

The markdown + HTML code for the above is:

```html
When you create a new template, e.g., `vector<string>`, does the compiler ensure the templated code is type-safe? If so, how?

<details markdown="0"><summary>Answer</summary>

Yes - since the compiler basically generates a concrete version of the function/class with the specified type, and the compiles it as it would any other class, type safety is guaranteed!
</details>
```

Note a couple of things:

- the `<details>` tag wraps all of the content, *including* the `<summary>` tag
- the `<details>` tag has the `markdown="0"` attribute, which is necessary for it to render as HTML

## Publishing the Lecture Note

{: .note }

Before you publish the lecture note, we suggest you spellcheck it! One common CLI spellchecker is [`aspell`](http://aspell.net/); many editor extensions are also very popular.

Once you've confirmed that the lecture notes renders as-desired locally, publishing it is very simple! All you need to do is commit and push:

```sh
$ git add lectures/01.md
$ git commit -m "Adds lecture 1 notes"
$ git push
```

Assuming that there are no merge conflicts, this should immediately be reflected on the website repo's `main` branch. Among other things, it should start a GitHub Actions run called "Deploy Jekyll site to Pages". If this succeeds, then the lecture note has been successfuly published -- you should see the chnages on the course website.

## Appendix: Advanced Customization

You might find markdown too limiting for ceratin use-cases. That's totally okay! You can extend the existing capability in a few ways; the two easy ways are:

1. adding styling via custom CSS
2. rendering an HTML element(s)

Implicitly, we've already used both of these in our tips.

### Extra CSS

The `{: .x }` syntax adds a CSS class (in this case, one named `x`) to the immediately following Markdown block. With this, you can:

- add Just the Doc's [utility CSS classes](https://just-the-docs.com/docs/utilities), which help with things like colors, typography, and responsive layout
- write your own CSS class. To do this, go to `_sass/custom/custom.scss`, and add your CSS code at the end of the file. You can then use this class across the website!

See [Tip: Callouts](#tip-callouts) for this in action.

### Extra HTML

Jekyll's default markdown parser is [kramdown](https://kramdown.gettalong.org/). To render HTML, the tl;dr is: you can add any HTML with the `markdown="0"` attribute; this will make it render as "raw" HTML. See [Tip: Hidden/Toggleable Content](#tip-hiddentoggleable-content) for this in action.

There's some more depth to how kramdown parses HTML. For more, [refer to their docs on HTML syntax](https://kramdown.gettalong.org/syntax.html#html-blocks).

### Other Options

You may want to do even more! Since the website is served as HTML, CSS, and JS, you can do anything you'd be able to do in another website! A non-exhaustive list includes:

- writing JavaScript to make the lecture note more interactive
- embedding another website or widget with an `<iframe>`
- linking in a WebAssembly resource
- changing the layout of the lecture note (through `_layouts/lecture.html`)

None of these require extraordinarily-special effort or knowledge; they mostly work with Jekyll as you'd expect.

You may also want to extend upon the compile-time capability of Jekyll, which can be done by [writing a plugin](https://jekyllrb.com/docs/plugins/) (using Ruby). Since this requires non-trivial Ruby knowledge, it's probably out of scope of the TAs; but, it'd be awesome if you gave this a shot! If you do, make sure to document it so that other TAs can maintain your work.
