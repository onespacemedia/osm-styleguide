# Model style

General form:

```
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

```
title = models.CharField(
    max_length=100,
    help_text=(
        'This is helpful text, but there is a problem because it spans more '
        'than one line'
    ),
)
```

This makes it harder to search for distinctive strings; a `grep` for "more than one line" will not return the file in which the above field lives.
