Example usage
=============

The value of this recipe is that its part can be referenced from other parts
using the ${...} syntax. The recipe mirrors the current environment variables
into its section, so that e.g. ${env:USER} will give the current user.

In addition to whatever is in the environment, `UID` and `GID` will be set to
the effective user id and group id as reported by Python's `os` module.

Let's look at how this works:

    >>> write('buildout.cfg',
    ... """
    ... [buildout]
    ... parts = env
    ...
    ... [env]
    ... recipe = gocept.recipe.env
    ... """)

This configuration references an environment variable called
`buildout-test-info`. Lets set it so we know its value:

    >>> import os
    >>> os.environ['buildout-test-info'] = '42'

Running the buildout gives us::

    >>> print 'start', system(buildout) # doctest:+ELLIPSIS
    start...
    Installing env.
    <BLANKLINE>

And the `installed.cfg` recorded the corresponding environment value:

    >>> cat('.installed.cfg')
    [buildout]
    ...
    [env]
    ...
    GID = ...
    ...
    UID = ...
    ...
    buildout-test-info = 42
    ...
