# Less is more: How to reduce code size and make it easier to read.

I am super lazy. You can't imagine how lazy I am. I am so lazy that I someimes
work all day and all night, just to make a tiny thing simpler.

Some examples on how to simplify software.

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

Avoid to use synonyms. Use well the same well defined terms again and again.

Old:
```
program_types = models.ManyToManyField(ProjectType, verbose_name=_('Project Types'))
```

Why two words: "program" and "project"?

Less synonyms:
```
project_types = models.ManyToManyField(ProjectType, verbose_name=_('Project Types'))
```

## Avoid I18N

If your native language is not english, and you develop an application which
will be usued solely by people of your mother tongue, and it is clear that in the near
future no other language is required, then avoid I18N (internationalization).

If needed you can refactor your code later and add other languages.

First make your current customers happy. 




