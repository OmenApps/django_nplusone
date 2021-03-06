# django_nplusone
Discover possible N+1 queries in Django ORM at runtime

The objective of this library is to help you discover any N+1s in your code at development time. Enable logging of 
warnings by following instructions shown in Usage section. Run your Django app as usual and you will see warning logs 
of any possible N+1s if found.

## Installation

Install the package from PyPI using `pip` as following

```
pip install django_nplusone
```

## Usage

Once package is installed, you can register the package in your `settings.py` as:

```
import nplusone

if DEBUG:
    nplusone.show_nplusones()

```

This should start logging possible N+1s warnings using your logger configuration. The library uses standard python 
`logging` module and uses logger by the name of `nplusone`.

## Ignore specific warnings

You can ignore warning for a specific statement by ending the statement with `# NO-NPLUSONE`.

## Test

You can test the N+1 warning logging by putting some code as shown below somewhere in your application code that you 
know will get executed. Say you have a model `User` with a foreign key field `org` to `Organization` model.

```
user = User.objects.all()[0]
print(user.org)
```

You should see a log message with a message that looks like this:

```
... Possible N+1 for model: User, field: org, relationship: ForwardManyToOne, file: <?>, function: <?>, line: <?>, statement: <?>
```

where `<?>` will have correct values for the fields derived from call stack pointing you to the statement that has
possible N+1
