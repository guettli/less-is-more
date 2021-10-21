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

## Avoid PDF creation

Creating HTML is fun and easy. Browser support is great today and many people can help, if you have a question.

That's different if you want to create PDF.

Creating one output format (HTML) is enough.

Save time, money and energy and avoid creating PDF files.


## less code, easier to read

Old code:
```
def get_foo_capacity_base(self, foo, estimated=False):
    if self.from_date_year:
        try:
            current_foo_amount = get_current_foo_amount(foo, self.from_date_year, estimated)
            capacity = FooCapacity.objects.get(foo=foo, year=self.from_date_year).capacity
            return moneyfmt(capacity - current_foo_amount)
        except FooCapacity.DoesNotExist:
            return '<a href="{}?foo={}&year={}">Add</a>'.format(
                reverse('admin:activity_FooCapacity_add'),
                foo,
                self.from_date_year)
    else:
        current_year = timezone.now().year
        try:
            current_foo_amount = get_current_foo_amount(foo, current_year, estimated)
            capacity = FooCapacity.objects.get(foo=foo, year=current_year).capacity
            return moneyfmt(capacity - current_foo_amount)
        except FooCapacity.DoesNotExist:
            return '<a href="{}?foo={}&year={}">Add</a>'.format(
                reverse('admin:activity_FooCapacity_add'),
                foo,
                current_year)
```

New version:
```
def get_foo_capacity_base(self, foo, estimated=False):
    year = self.from_date_year
    if not year:
        year = timezone.now().year
    try:
        current_foo_amount = get_current_foo_amount(foo, year, estimated)
        capacity = FooCapacity.objects.get(foo=foo, year=year).capacity
        return moneyfmt(capacity - current_foo_amount)
    except FooCapacity.DoesNotExist:
        return '<a href="{}?foo={}&year={}">Add</a>'.format(
            reverse('admin:activity_FooCapacity_add'),
            foo,
            year)
            ```

# "else" not needed

Old:
```
def foo(...):
    if bar == BAR_TYPE_X:
        return ...
    elif bar == BAR_TYPE_Y:
        return ...
    elif bar == BAR_TYPE_Z:
        return ...
    else:
        return ...
```

"else" is not needed:
Simpler:
```
def foo(...):
    if bar == BAR_TYPE_X:
        return ...
    if bar == BAR_TYPE_Y:
        return ...
    if bar == BAR_TYPE_Z:
        return ...
    return ...
```
