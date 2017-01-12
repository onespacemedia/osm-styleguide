# Python code style

If in doubt, [PEP 8](https://www.python.org/dev/peps/pep-0008/).

## Quotes

Prefer 'single quotes' over "double quotes".

If a Python file consistently uses double quotes, then stay consistent with the file by using double quotes for new code.

If a file uses a mixture of single and double quotes, use single quotes for new code, but feel free to convert it to use single quotes consistently.

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

Multi-line array, dictionary, and tuples should have a dangling comma on the last item. Multi-line function calls may optionally have them too, though this isn't always possible (it's a syntax error if your last argument is of the form `**kwargs`).

For function calls (including class instantiation), if your declaration goes across more than one line, then the first argument should be on a new line:

```python
objects = queryset.filter(
    is_online=True,
    page=self.request.pages.current,
)
```

## Imports

Use [isort's default ordering](http://timothycrosley.github.io/isort/).
