# Less is more: How to reduce code size and make it easier to read.

Some examples on how to simplify code.

## An empty list is no special condition

Old:
```
if mylist:
    for item in mylist:
        ... do something with item
```

If you are sure that mylist is not `None`, then this is simpler:

```
for item in mylist:
    ... do something with item
```

## Less Synonyms

Old:
```
program_types = models.ManyToManyField(ProjectType, verbose_name=_('Project Types'))
```

Why two words: "program" and "project"?

Less synonyms:
```
project_types = models.ManyToManyField(ProjectType, verbose_name=_('Project Types'))
```





