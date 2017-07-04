# Jinja and Django template styles

## Spacing

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

## Indentation

Indentation should be with two spaces.

You should not have more than one consecutive empty line.

A statement block, which is to say any tag that has a corresponding `end` tag, should have its contents indented and on a new line if it contains any substantial amount of content.

It's fine to use a one liner to override a short string, as we quite often want to do with `{% block %}`:

```
{% extends 'base.html' %}

{% block title %}I'm a mere short title{% endblock %}

{% block body_class %}body-IAmAcceptableToo{% endblock %}

{% block content %}
  <p>As I contain a block-level HTML element and more than a trivial quantity
  of text, I start a new line and a new indentation level.</p>
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

An empty line between two HTML elements at the same indentation level is optional. In the above case, there's one between our `<h2>` and `<ul>` because they have obviously distinct roles.

Blocks should never be padded with an empty line:

```
{% if some_condition %}

  <p>Don't do this!</p>

{% endif %}
```

## Jinja2 `set` and `with`

`{% set %}` and `{% with %}` assignments should have a space before and after the equals, just as one would in the generally-accepted Python style:

```
{% set articles = get_news_articles() %}
```

As well as being consistent with Python assignments, it means that constructs like this don't look weird:

```
{% set value1, value = get_some_tuple() %}
```

But as with Python, keyword arguments passed to a function should not:

```
{% set articles = get_news_articles(count=5) %}
```

Function parameters should have a space after each comma, but never before:

```
{% set articles = get_news_articles(featured=True, count=5) %}
```

## Macros

If your macro takes a lot of arguments, consider whether you can consolidate them into a single one.

An example might be a macro called like this, to render a card for a news article:

```
{{ cards.card(
  article.title,
  article.summary,
  article.image,
  article.get_absolute_url(),
  "some modifier"
) }}
```

In this case, consider whether you could build an `as_card` method on your article model...

```
def as_card(self):
  return {
    "title": self.title,
    "summary": self.summary,
    "image": self.image,
    "url": self.get_absolute_url(),
  }
```

...and modify your `card` macro to take a single first parameter for the card's content:

```
{{ cards.card(article.as_card(), "modifier") }}
```

This has a bonus of moving any logic that may be required out of the template language and in to Python, where anything but the most simple logic will be much cleaner than it is in the template language.
