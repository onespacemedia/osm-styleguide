# Model style

General form:

```python
class StyleGuide(models.Model):

    title = models.CharField(
        max_length=100,
        blank=True,
        null=True,
    )
```

There should be a new line before the first argument to a field, and each argument goes on a new line. This makes diffs easier to read when new fields are added.

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

# About various field types

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

# Model ordering

Order model attributes thusly:

* Static class variables (e.g., the common `urlconf = 'projectname.apps.news.urls'`)
* Database fields
* The `Meta` class
* Double-underscore-prefixed special methods, e.g. `__unicode__`/`__str__`
* Overrides of standard model methods (e.g. `save()`).
* All other model methods, in alphabetical order
