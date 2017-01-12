# Consistency

To quote PEP8,

> A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is the most important.

As the current set of rules evolved over time, many older projects will not be conformant with them. If you find yourself revisiting an older project. it is more important to stay consistent with the rest of that project than it is to enforce these rules.

There is one exception to this: *Do not escalate CSS nesting specifity wars.* This harms maintainability further down the line. If you possibly can, create a new, non-conflicting class for any new components that you add.
