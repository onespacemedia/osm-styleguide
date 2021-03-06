# Introduction

As OSM moves forward and grows as a reputable design agency, it’s imperative to make sure as a team we are consistently producing work that makes us unique and stand out from the crowd. This styleguide will help developers achieve consistency and the highest standard of work as project size and frequency increase.

As the projects we work on become larger in scale and longer-running, it is very important that we work in a unified way and with a ever growing team with different specialities and abilities. It is necessary to keep the process streamlined in order to:

- Keep everything maintainable.
- Keep code transparent, sane, and readable.
- Keep everything scalable.

# Editing this style guide

For optimal results, edit this document with help from [docsify](https://docsify.js.org/#/quickstart). To start:

- Run `npm i docsify-cli -g`
- Clone the `styleguide` repository into your Workspace folder
- Run `docsify init ./styleguide`, then `docsify serve styleguide`

A web server will be spawned on port 3000. As you edit this README.md it will automatically refresh on the front-end.

# Guidelines for any language

## Consistency

To quote [PEP 8](https://www.python.org/dev/peps/pep-0008/),

> A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is the most important.

Because our rules evolved over time, many older projects will not conform to them. If you find yourself revisiting an older project, it is more important to stay consistent with the rest of that project than it is to enforce these rules.

There is one exception to this: *Do not escalate CSS nesting specificity wars.* This harms maintainability in the future. If you possibly can, create a new, non-conflicting class for any new components.

## Legibility vs cleverness & conciseness

Your code may be picked up by another developer at any time, or you might be called upon to work on a project on which you worked many months ago. Consequently, legibility is important; it reduces the overhead of getting involved (or re-involved) with a project.

If there is a choice between a clever and concise way of writing something, and a less clever and more verbose way of writing something to the same effect, you should tend towards the latter.

## Commenting

Comment anything that someone fresh to the project might not understand immediately. Don't write comments for the sake of writing comments. There is no need for this:

```python
# Initialise the counter variable
counter = 0
# Loop over the items.
for item in some_list:
    # Add 1 to counter
    counter = counter + 1
```

The purpose of comments is not to describe what code does. They are to help other developers understand your code.

Anything particularly clever should have be commented (though you may want to see if you can write it in a less clever way). "Magic" numbers and behaviour should have a comment. Anything done to work around strange bugs should be commented. Anything which would cause another developer to say "why did they do this instead of X" (for values of X being some way that you would have tried or did try) should be commented.

If you know your code is going to be tricky, the [rubber duck effect](https://blog.codinghorror.com/rubber-duck-problem-solving/) suggests that you should write your comments *first* - the act of describing a problem often makes it easier to solve!

## Variable names

Keystrokes are abundant. While not enforced, your variable names should be at least a single English (or technical) word. A positive Python example of what you should do:

```
class ThingyListView(ListView):
    def get_queryset(self):
        queryset = super().get_queryset()
        queryset = queryset.filter(page__page=self.request.pages.current)
        return queryset

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['categories'] = Category.objects.all()
        return context
```

It would be tempting to shorten `context` and `queryset` to `ctx` and `qs`. This would make your code harder to reason about.

Variable names should be in accord with the most common community-accepted standard for whatever language you are using. For example, we use function_names_with_underscores in Python, because this is what the Python community has largely agreed to use. We use camelCase in JavaScript, because this is what the language's standard library and the browser DOM use.

# CSS guidelines

Most of these rules are enforced by [Stylelint](https://stylelint.io/), both in pre-commit hooks and in CI.

## General formatting rules

- Indent blocks with two spaces
- A space before the opening curly brace ({)
- Properties and values on the same line
- Always have a space after the colon (:)
- Opening brace on the same line as our last selector ({);
- Declarations on a new line including after the first opening curly brace ({);
- Closing curly brace on a new line (});
- Trailing semicolon at the end of each declaration (;).
- Always end a CSS file with an empty line.
- Properties should always be lowercase.
- Shorten colour values - use #fff rather than #ffffff, for example.

## Class naming conventions

We largely follow the [Enduring CSS](http://ecss.io/) convention, which is very similar in philosophy (differing mainly in syntactical details) to the [Block, Element, Modifier](http://getbem.com/naming/) convention. Class names will take one of three forms.

1. `namespace-Block`
2. `namespace-Block-modifier`
2. `namespace-Block_Element`
3. `namespace-Block_Element-modifier`

Here, `namespace` is a two-to-four letter namespace. This could be a distinct set of types of different components, or it could be For example, for things related to a site's header, this might be namespaced under `hd-`. Different kinds of 'cards' would be namespaced `crd-`. Different kinds of modular sections would be `sec-`.

A `block` is a distinctive component with standalone meaning. Its first letter should always be capitalised. If you are struggling to find a name for it that doesn't fit in a single word, use `CondensedTitleCase`.

An `element` is something which only ever exists within the context of a `block`.

A `modifier` describes a *minor* variation of a particular block or element.

An extremely simple example follows. Here is our HTML structure:


```css
.sec-Section {
  /* This is a standard section on a page, in cases where most or all sections
  on a page have similar layout and spacing rules. The `.sec` namespace is
  short for "sections", for pages that are built out of distinct, logically
  separate sections. */
  padding-top: 4vr;
  padding-bottom: @padding-top;

  background-color: #fff;
}

.sec-Section-grey {
  /* This follows all the same rules as `.sec-Section` above, with one tiny
  modification: its background colour is grey. */
  background-color: var(--Color_Grey-light);
}

.sec-Section_Inner {
  /* This is the inner part of a section. This doesn't have any applicability
  outside of a section. (Of course, containers are used elsewhere on a site,
  but we do not use utility classes.) */
  @include Grid_Container;
}

.sec-Section_Header {
  /* This defines the header for a section, with rules for how it will be
  spaced from the content. */
  margin-bottom: 2vr;

  text-align: center;
}

.sec-Section_Title {
  /* A section title which lives inside the above component. Its role is
  logically distinct, and separable, from being part of the section header -
  we define centre-aligned text and _Header above, not here. All this does is
  describe a standard font that will be used on all (or most) section titles.
  */
  @include Font_24-32;
}

.sec-Section_Body {
  /* This defines the 'body' of a section. Note that we don't actually have
  any assumptions about what that content might be! It might be a WYSIWYG
  block, or it might be a list of things, or it might be a picture, - don't
  make assumptions! More than likely whatever sits inside .sec-Section_Content
  will be namespaced differently. For example, if it was a list of news
  articles, we'd probably want to have something in the .lst- namespace. A
  list of news articles is a list of news articles that could be
  used in all sorts of ways, not just in something built with the section
  builder. */
}
```

## Selectors

Do not have duplicate selectors. You should only need to look in one block to see all the properties and states of any given component. It is okay for a single block to have more than one selector, even when that selector has its own dedicated block elsewhere.

If your block has more than one selector, you should place each selector on a new line:

```css
.frm-Form_Input,
.frm-Form_Textarea {
  @include Font_14-20;

  width: 100%;

  border: 1px solid var(--Color_Border);
  border-radius: 2px;
}
```

### Specificity

As much as possible, you should always target a single class name, and as much as possible this should not be dependent on its context.

### Modifier classes

Modifier classes should not be nested if they target the same element. Prefer this:

```css
.crd-Card {
  background-color: #fff;
}

.crd-Card-transparent {
  background-color: transparent;
}
```

Rather than this:

```css
.crd-Card {
  background-color: #fff;

  &.crd-Card-transparent {
    background-color: transparent;
  }
}
```

This keeps the specificity to a single class name.

If you need to modify the attributes of a card based on the type or state of one of its ancestors, you should usually nest the parent selector inside the child element's definition block, rather than nesting the child element in the parent's definition block.


```css
.crd-Card_Body {
  color: var(--Color_Body);

  .crd-Card:hover & {
    color: var(--Color_Blue);
  }
}
```

In this case, it means you only need to look at one CSS block to see all the possible states for `.crd-Card`.

### Targeting adjacent adjacent elements

Generally, you should avoid `+` to target adjacent elements, when those adjacent elements use the same class name, and the parent element only contains (or can be made to only contain) elements of that class name selector. Take this example:

```
<ul class="lst-Stacked">
  <li class="lst-Stacked_Item">...</li>
  <li class="lst-Stacked_Item">...</li>
  <li class="lst-Stacked_Item">...</li>
</ul>
```

The correct way to target adjacent elements (in this example, to put a margin between them) would be this:

```css
.lst-Stacked {
  margin-top: 1vr;

  &:first-child {
    margin-top: 0;
  }
}
```

Instead of this:

```css
.lst-Stacked {
  & + & {
    margin-top: 1vr;
  }
}
```


### !important

Avoid `!important`. There are rare exceptions, such as overriding other `!important` declarations added by third-party code, or inline styles added by the same.

### IDs as selectors

Don't target IDs in the form `#selector`, as it has [infinitely more specificity](https://css-tricks.com/specifics-on-css-specificity/) than class names.

If you are forced to target an ID (such as with elements injected by third-party code), target `*[id="foo"]` instead.

## Grouping properties

Properties are grouped for readability, with an empty line separating adjacent groups, in this order:

```css
 .nsp-Component_ChildNode {
    @include and @apply declarations;

    content properties;

    position properties;

    flex properties;

    display, width, height, margin and padding properties;

    font and text properties;

    background border and color properties;

    animation and transition properties;
  }
```

For further details, see the [grouping configuration](https://github.com/onespacemedia/project-template/blob/develop/%7B%7Bcookiecutter.repo_name%7D%7D/package.json#L208) in the project template.

## Commenting

With the increasing complexity of Onespacemedia projects it is increasingly important to comment your CSS. Trying to understand your own code is difficult enough. Someone inheriting the project will find it more difficult than that.

There should always be a heading comment at the top of a .css file that includes the component name and the namespace selector:

```css
/*
|--------------------------------------------------------------------------
| Component
|--------------------------------------------------------------------------
| @namespace: nsp-
|
*/
```

Then comments after should be grouped and order in the same way the HTML is ordered, keeping everything together in small sections in the file:

```css
/*
|--------------------------------------------------------------------------
| Header
|--------------------------------------------------------------------------
*/

/*
|--------------------------------------------------------------------------
| Body
|--------------------------------------------------------------------------
*/

/*
|--------------------------------------------------------------------------
| Footer
|--------------------------------------------------------------------------
*/
```

If you use "magic" numbers in your CSS, add a comment above it explaining why you are using those numbers.

Comment any strange or clever CSS. Sometimes cross-browser fixes will require unusual rules that would not make any sense to a first-time observer, who might see it as a target for refactoring.

If you must add a `/* stylelint-disable */` comment to your CSS to bypass linting, it is often good form to write an explanatory comment about why it is disabled immediately above the line, to avoid the tendency to `/* stylelint-disable */` whenever a linting error is hit (for reasons both valid and invalid).

## Vertical rhythm and the `vr` unit

We use [postcss-lh](https://github.com/jameskolce/postcss-lh) to give us a custom CSS unit based on the root line height of the document (typically, 24px). We've renamed `postcss-lh`'s default `lh` unit to `vr`, for [vertical rhythm](http://webtypography.net/2.2.2).

You can and should use these units for spacing and sizing where appropriate. Always use whole numbers or halves (e.g. `0.5vr`. Where you can't round up to one of these without severe visual deviation from the design, don't use `vr`. Assuming `1vr` is 24 pixels, 6 pixels should be expressed as `6px`, not `0.25vr`.

## Other units

Avoid `em`; there are only very rare cases where this unit is necessary.

You never need to use `rem` - our build system translates `px` into `rem` units (via [postcss-pxtorem](https://github.com/cuth/postcss-pxtorem)), so just use `px` instead.

Otherwise, you should feel free to use whatever other unit is necessary to get a pleasing result.

Grid gutters and other horizontal _margins_ should be in `px`.

Vertical margins and padding should usually be specified in `vr` or `px`.

For percentages, make liberal use of `calc`, especially for things that follow a site's grid. To specify a width of 5 columns out of twelve, minus the grid gutter, use `width: calc(5 /  12 * 100% - var(--Grid_Gutter))`. It is much easier to understand the intent in this example at a quick glance than `width: calc(41.66% - var(--Grid_Gutter))`.

## Vendor prefixes

Generally, you don't need to add vendor prefixes to property names and values; [Autoprefixer](https://github.com/postcss/autoprefixer) handles this for you in our build system.

It is occasionally necessary to use vendor prefixes to work around weird cross-browser bugs, and in the vanishingly small number of cases that Autoprefixer does not work. In both these cases, you should add a comment to explain why you are using vendor prefixes.

## Using `z-index`

Do not start `z-index` wars, and don't escalate them if you see them in existing projects.

It's tempting to add an extremely high `z-index` for things that you think absolutely must be on top of all other elements of a site. This often manifests itself as `z-index: 99999`. And then someone notices something that needs to be on top of _that_, and adds an extra digit to make it `z-index: 999999`. Then later, on a retainer task a few months down the line, someone adds an extra digit to _that_...

Generally, if you find yourself using more than low-single-digit z-indices on anything other than a global component like a sticky header, you are in a `z-index` war. If you are using numbers greater than 100, you are definitely in a `z-index` war.

If you can, use DOM ordering and `position: relative` with no z-index if something has to sit above another thing within a component; you will be fine if you place the component later in the DOM at the same or higher level.

If you can't, use the lowest possible `z-index` you can. Within the same component, 1 is usually sufficient.

# HTML guidelines

In these examples, class names are omitted for readability.

## General formatting rules

Tag names, attributes, and attribute values should be in lowercase where possible.

Attribute values should be quoted in "double quotes"

Where an element starts on a new line to a parent element, it should be indented by two spaces. Elements that are rendered as blocks (whether they are block-level elements in the specification or made to render that way in CSS) should usually start on a new line. Avoid doing this if it will cause an ungrammatical space in text, though.

For self-closing tags that do not have textual content, omit closing tags where the HTML5 specification permits it. Avoid the XML form. This is fine:

```html
<link rel="stylesheet" href="/static/css/style.css">
```

In cases where the HTML5 specification permits omitting an end tag, but the element has textual content, you should not omit the end tag. `<li>` and `<p>`, for example, may have their end tags omitted in the specification, but it's easier to spot the scope of an element with the explicit closing tag in place:

```html
<ul>
  <li>List item!</li>
</ul>

<p>A paragraph of text!</p>

<div>While this div, being a block-level element, would implicitly have closed the above P element if its end tag was omitted, the scope of the above element is more obvious with the explicit closing tag.</div>
```

An empty line between two HTML elements at the same indentation level is optional. Use your judgement.

Use the short form for attributes like `checked` on `<input>` and `selected` on `<option>`:

```html
<select>
  <option selected value="1">I'm the default option</option>
  <option value="2">I'm not</option>
</select>
```

## Comments

It is rarely useful for comments to be in the rendered HTML; you should usually use template-syntax `{# comments #}`, rather than `<!-- comments -->`. Similarly, for inline JavaScript, use template syntax instead of JavaScript `// comments` or `/* comments */`.

If Jinja/Django template comments in inline JS confuse your syntax highlighter, you can do something like this:

```
/* {# This comment is hidden from the rendered source, and also won't confuse syntax highlighters! #} */
```


## Jinja and Django template styles

### Spacing

Always put a space between the delimiters of a 'print' or of a template tag, and its name. This makes it much more readable:

```
{# Prints should have spaces inside the curlies. #}
<h1>Hello, {{ name }}!</h1>

<ul>
  {# Tags and statements should have a space before and after the percent. #}
  {% for animal in animals %}
    <li>{{ animal }}</li>
  {% endfor %}
</ul>
```

### Indentation

Indentation should be with two spaces.

You should not have more than one consecutive empty line.

A statement block, which is to say any tag that has a corresponding `end` tag, should have its contents indented and on a new line if it contains any substantial amount of content.

It's fine to use a one liner to override a short string, as we quite often want to do with `{% block %}`:

```
{% extends 'base.html' %}

{% block title %}I'm a mere short title{% endblock %}

{% block body_class %}body-IAmAcceptableToo{% endblock %}

{% block content %}
  <p>As I contain a block-level HTML element and more than a trivial quantity of text, I start a new line and a new indentation level.</p>
{% endblock %}
````

There are times you should feel free to not do this to avoid unnecessary whitespace. For example, we can't put the `if` on its own line here because it would cause an ungrammatical space before the comma:

```
Filed under {% for category in categories %}{{ category }}{% if not forloop.last %}, {% endif %}{% endfor %}
```

Block-level tags, which is to say those with a corresponding `end` tag, should usually have an empty line between them and any other items at the same indentation level.

```
{% block content %}
  <h1>{{ person.name }}</h1>

  {% if person.nickname %}
    <p>{{ person.nickname }}</p>
  {% endif %}

  {% with person.pet_set.all() as animals %}
    {% if animals %} {# No empty line as it starts a new indentation level. #}
      <h2>Pets</h2>

      <ul>
        {% for animal in animals %}
          <li>{{ animal }}</li>
        {% endfor %}
      </ul>
    {% endif %}
  {% endwith %}
{% endblock %}
```

Blocks should not be padded with an empty line. Do this:

```
{% if some_condition %}
  <p>This is good!</p>
{% endif %}
```

Rather than this:

```
{% if some_condition %}

  <p>Don't do this!</p>

{% endif %}
```

### Jinja2 `set` and `with`

You should usually prefer `{% with %}`, because it avoids bugs caused by scope pollution.

It's okay to use `{% set %}` _near the top of a file_ to create global shortcuts, such as this common case:

```
{% set content = pages.current.content %}
```

`{% set %}` and `{% with %}` assignments should have a space before and after the equals, just as one would in the generally-accepted Python style:

```
{% with articles = get_news_articles() %}
  ...
{% endwith %}
```

As well as being consistent with Python assignments, it means that constructs like this don't look weird:

```
{% with value1, value = get_some_tuple() %}
  ...
{% endwith %}
```

But as with generally accepted Python practice, keyword arguments passed to a function should not:

```
{% with articles = get_news_articles(count=5) %}
  ...
{% endwith %}
```

Function parameters should have a space after each comma, but never before:

```
{% with articles = get_news_articles(featured=True, count=5) %}
  ...
{% endwith %}
```

### Jinja2 macros

If your macro takes many arguments, consider whether you can consolidate content-related ones into a single argument.

An example might be a macro called like this, to render a card for a news article:

```
{{ cards.card(
  article.title,
  article.summary,
  article.image,
  article.get_absolute_url(),
  modifier_parameter=True
) }}
```

In this case, consider whether you could build an `as_card` method on your article model...

```python
def as_card(self):
  return {
    'title': self.title,
    'summary': self.summary,
    'image': self.image,
    'url': self.get_absolute_url(),
  }
```

...and modify your `card` macro to take a single first parameter for the card's content:


```
{{ cards.card(article.as_card(), modifier_parameter) }}
```

This has a bonus of moving excessive logic out of templates into Python, where anything but the most simple logic is much cleaner.

## Using SVGs

Where possible, SVGs should be inline in HTML rather than included with `<img>` or applied with a `background-image`. This permits targeting SVG paths in CSS for things like hover effects and animations.

For frequently-used icons, make liberal use of spriting to reduce the size of the document. Examples of sprite usage can be found in our [project template](https://github.com/onespacemedia/project-template).

With a few rare exceptions, SVGs should contain only `path` elements, and you should be able to target the visible parts of the path with `fill` alone (rather than strokes). This gives a single target for CSS, and a single CSS property for styling it. Ask a designer to convert the SVG for you if you don't know how to convert it yourself.

When inlining SVGs, be careful with SVGs exported by Adobe Illustrator and Sketch. Apart from being vastly bigger than necessary, they almost invariably cause SVGs with identical IDs, identical class names, and inline `<style>` blocks. This often causes an SVG to apply its styles to paths in a different SVG earlier in the document.

[SVGOMG](https://jakearchibald.github.io/svgomg/) is a useful tool for cleaning up SVG files; once you've worked out a good set of options you will find it does the right thing the vast majority of the time.

1. Remove the `<?xml` declaration.
2. Remove all attributes from the root `svg` element except `viewBox`.
3. Remove the entire `<defs>` block if one is present.
4. Remove any `id` attributes.
5. Remove any `g` groups.
6. Remove any explicit `fill=` attributes on paths.
7. Replace class names with ones conforming to our naming conventions (or for SVGs with a single colour, like most icons, remove class names altogether)
8. Use CSS (within the standard frontend build system, not in the SVG itself) to style the SVGs, if necessary.

# JavaScript guidelines

We conform to the [Standard Style](https://standardjs.com/). In all semi-recent projects, this is enforced by the build system. To condense the Standard authors' summary:

- Two space for indentation
- Single quotes for strings, except to avoid escaping
- No unused variables
- No semicolons for statement termination
- Put a space after keywords and function names
- Use `===` instead of `==`
- Always prefix browser globals with `window`, except `document` and `navigator`

The one exception we've made to Standard's rules is that unused variables are a warning during development, not an error - unused variables should not make their way into production code.

Read the [complete set of rules](https://standardjs.com/rules.html) on the Standard site for more.

## ES6 features

These rules apply for scripts processed by our frontend build system, which are transpiled to universally-browser-friendly ES5. In the rare cases where inline JavaScript is used in a document, you should avoid ES6 features, as they are not universally and consistently implemented in all the browsers we support.

Always use `let` or `const`, never `var`. Always use `const` for locals which are not reassigned.

Use ES6 fat-arrow functions for anonymous functions whenever possible, which is almost always.

```javascript
window.addEventListener('DOMContentLoaded', () => {
  doDomContentLoadedStuff()
})
```

Use object-literal shorthand where you can:


```javascript
// In cases where our variable (or constant) names are the same as the names
// of our object keys, we can do this:
const animals = {
  cow,
  dog
}
```

Use ES6 string templates instead of ES5 concatenation:

```javascript
const animal = 'cat'
const animalPlural = `${animal}s`
```

Use ES6 `for...of` loops, rather than ES5 `for` when traversing arrays:

```javascript
for (const animal of animals) {
  animal.makeNoise()
}
```

# Python guidelines

If in doubt, follow [PEP 8](https://www.python.org/dev/peps/pep-0008/).

## String quotes

Prefer 'single quotes' over "double quotes".

If a Python file consistently uses double quotes, then stay consistent with that file by using double quotes for new code within it.

If a file uses a mixture of single and double quotes, use single quotes for new code, but feel free to convert it to use single quotes consistently.

For quoting text that contains an apostrophe, use double quotes. This example would permit grepping for the string "don't":

```python
have_bugs = models.BooleanField(
    default=False,
    help_text="Don't check this!"
)
```

## Line lengths

Try to keep lines to a maximum of 79 characters.

This is a suggestion, not a rule. If limiting the line length would result in uglier code, then ignore it.

## Multi-line constructs

The last closing parenthesis, bracket, or brace should line up with the indentation level of the first line of the construct. For example, this is good:

```python
ANIMAL_CHOICES = [
    (1, 'Cat'),
    (2, 'Dog'),
    (3, 'Horse'),
]
```

Don't do this:

```python
ANIMAL_CHOICES = [
    (1, 'Cat'),
    (2, 'Dog'),
    (3, 'Horse'),
    ]
```

Multi-line array, dictionary, and tuples should have a dangling comma on the last item. Multi-line function calls may optionally have them too, though this isn't always possible (it's a syntax error in Python 2.x if your last argument is of the form `**kwargs`).

For function calls (including class instantiation), if your declaration goes across more than one line, then the first argument should be on a new line:

```python
objects = queryset.filter(
    is_online=True,
    page=self.request.pages.current,
)
```

## Import ordering

Use [isort's default ordering](http://timothycrosley.github.io/isort/). Group imports thusly:

1. Imports from `__future__`
2. Python standard library
3. Third-party 'globally' installed packages (in our case, always installed in a virtual environment)
4. Current project
5. Explicit local imports (relative imports)

Use isort's "Grid" option for multi-line outputs.

Project-local imports should usually be relative imports. Imports from further up the directory structure should come first.

```
from ...utils.views import FilteredListView
from ..other_app.models import Taxonomy
from .models import Article, Category
```

## String formatting

For Python 3.6 projects (all new projects, and all projects since late 2017), you should use [f-strings](https://www.python.org/dev/peps/pep-0498/) to format values into a string.

```python
class Animal(models.Model):

    # ....

    def __str__(self):
        return f'{self.name}, the {self.species}'
```

As well as being more concise, bad string formats are a _compile-time error_, so they will be caught as soon as your program starts. `.format` and the `%` operator will raise an exception at *runtime*, where they might go unnoticed for a while.

## Django model style

This is the general form of model fields:

```python
class StyleGuide(models.Model):

    title = models.CharField(
        max_length=100,
        blank=True,
        null=True,
    )
```

There should be a new line before the first argument to a field's constructor, and each argument should be on its own line. This makes diffs easier to read when arguments are added or deleted.

There should be a dangling comma after the last argument. Again, this makes diffs easier to read (because adding an argument to the end does not affect neighbouring lines).

String literals that are displayed to users, such as the `help_text` for a model field, should not be broken across lines, regardless of their length. Don't do something like this:

```python
title = models.CharField(
    max_length=100,
    help_text=(
        'This is helpful text, but there is a problem because it spans more '
        'than one line'
    ),
)
```

This makes it harder to search for distinctive strings; a `grep` for "more than one line" will not return the file in which the above field lives. Do this instead:

```python
title = models.CharField(
    max_length=100,
    help_text='This is helpful text, and because it is all on one line you can grep for any part of this string.',
)
```

However, it is rare to `grep` across sentence boundaries. So if you need to avoid very large line lengths, this is fine:

```python
title = models.CharField(
    max_length=100,
    help_text=(
      'This is a multi-sentence help text that we are breaking across multiple lines. '
      'It is unlikely that someone will be searching for, in this case, "multiple lines. It is". '
      'So this does not harm greppability in practice.'
    )
)
```

All models must define a `__str__` method in Python 3 projects, or `__unicode__` for Python 2.

All models should define an `ordering` attribute in their `Meta` class. Postgresql, our preferred database, will return rows in an ["unspecified"](https://www.postgresql.org/docs/9.1/static/queries-order.html) order if you do not. Don't rely on primary key ordering to give date-added-based ordering; this is not guaranteed. Implement a "date added" field and have explicit ordering on that column.

### About various field types

Don't use a very large `max_length` for `CharField`. Generally, unless there are good technical reasons to do otherwise, don't supply a `max_length` of more than 300. If you need more characters than that, use a `TextField`.

A `ForeignKey`, `ManyToManyField`, etc, should usually use the string form for specifying the model:

```python
page = models.ForeignKey(
    'pages.Page',
)

articles = models.ManyToManyField(
    'news.Article',
)
```

This avoids problems with recursive imports across apps.

### Model ordering

Order model attributes thusly:

1. Static class variables other than database fields (e.g., the common `urlconf = 'projectname.apps.app_name.urls'` for CMS `ContentBase` derivatives)
2. Database fields
3. The `Meta` class
4. Double-underscore-prefixed special methods, e.g. `__unicode__`/`__str__`
5. Overrides of standard model methods (e.g. `save()`), in alphabetical order
6. All other model methods, in alphabetical order

## Admin usability

### App and model naming

Always ensure that the plural form of a model name makes sense. By default, Django's admin will pluralise 'Category' as 'Categorys', for example. Use `verbose_name_plural` to correct this:

```python

class Category(models.Model):
    title = models.CharField(
        max_length=100,
    )

    class Meta:
        verbose_name_plural = 'categories'
```


Where appropriate, supply an appropriate `verbose_name` to ensure that capitalisation is correct for both model names and field names, including appropriate capitalisation for brand names. A model called `Faq` should have a `verbose_name` of 'FAQ'. A 'linkedin_url' field should have a `verbose_name` of 'LinkedIn URL', to avoid the default admin rendering of 'Linkedin url'.

Similarly, where required, ensure that app names have an appropriate `AppConfig` to ensure that app names render grammatically in the admin.

### Help text

Always supply a `help_text` for a field if you think it will be useful. Don't supply a `help_text` if the field name is entirely self-explanatory.

Help text should be concise, grammatically-correct full sentences with punctuation. Avoid fluffy wording, poor spelling & grammar, and sentence fragments.

```python
label = models.CharField(
    max_length=100,
    # Good example of a help_text.
    help_text="This is never shown on the front end of the site; it's to help you identify this component in the admin.",
    # Don't use sentence fragments like this:
    # help_text="Identifies this component in the admin.",
    # Or this, which has fluffy wording and adds no more information than the field name:
    # help_text="Please enter a label for this component"
)
```

### Capitalisation

Use sentence case for capitalisation of app names, field names, fieldsets, and help texts.

Avoid Title Case.

A model's `verbose_name` should usually be in lower case.

```python
class VideoCategory(models.Model):
    title = models.CharField(
        max_length=100,
    )

    class Meta:
        verbose_name = 'category'
        verbose_name_plural = 'categories'
```

The Django admin will automatically capitalise the first letter of the `verbose_name` where appropriate. Providing a `verbose_name` of "Category" in this example would cause the name to be capitalised when its name is used mid-sentence, which is incorrect as 'Category' is not a proper noun.

In the case below, capitalising the 'T' in 'Twitter' is appropriate as it is a proper noun:

```python
class TwitterAccount(models.Model):
    username = models.CharField(
        max_length=100,
    )

    class Meta:
        verbose_name = 'Twitter account'
```

## Django views

Always use [class-based views](https://docs.djangoproject.com/en/1.11/topics/class-based-views/). We have a nice [tutorial](http://www.onespacemedia.com/news/2014/feb/5/getting-started-generic-class-based-views-django/).

View classes should have their attributes in this order:

1. Static class attributes
2. Double-underscore methods (`__init__` and friends)
3. Methods inherited from Django generic views, in alphabetical order
4. Custom methods, in alphabetical order

# Git usage

## Keep commits specific

Code commits should be as small as reasonably possible, and should contain specific changes described accurately in the commit message.  Do not use generic commit messages such as "Made changes" or "Text tweak". Be descriptive!

Some tools will allow you to make changes without a commit message. Never do this.

If you have made multiple changes use the interactive commit system (`git commit -p`) and cherry-pick the feature-specific code. Do not group multiple changes together in the same commit.

## Squashing & merging branches

Squashing & merging is good when it does not remove any interesting history. Don't squash & merge if your branch history is long - it is likely that the history would be useful to other developers.

It's fine to squash commits if, for example, you made one commit and then a few others to fix linting (as often happens on our [project template](https://github.com/onespacemedia/project-template)), because this history is not very important. On the other hand, if someone wants to look at the version history to understand how or why a certain bit of code was added, they may be upset to find it's a 10,000-line squashed commit with, say, the message "Write CRM system".

## Things you should avoid

There is almost never a good reason for force-pushing (with the `-f` option on command-line `git`).

You should normally avoid `--no-verify` to bypass pre-commit and pre-push hooks. Perhaps every developer knows the moment their one-line change that they were sure could not possibly break the build did, in fact, break the build.
