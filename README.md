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

"else" is not needed. Less is more:
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

# Avoid useless comments

```
# FOO_CHOICE_BAR
# FOO_CHOICE_BLUE
# FOO_CHOICE_BAZ
if foo_type in [
    constants.FOO_CHOICE_BAR,
    constants.FOO_CHOICE_BLUE,
    constants.FOO_CHOICE_BAZ,
]:
    do_something()
```

These three comments above the "if" are not needed. Less is more.

# Less ways to get config values

Old:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': os.environ.get('POSTGRES_DB', 'foobar'),
        'USER': os.environ.get('POSTGRES_USER', 'foobar'),
        'PASSWORD': os.environ.get('POSTGRES_PASSWORD', 'foobar'),
        'HOST': os.environ.get('POSTGRES_HOST', '127.0.0.1'),
        'PORT': os.environ.get('POSTGRES_PORT', '5432'),
    },
}
```
Above config uses the environment variables. But if the env var is not available, then a default value is provided.


New:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': os.environ['POSTGRES_DB'],
        'USER': os.environ['POSTGRES_USER'],
        'PASSWORD': os.environ['POSTGRES_PASSWORD'],
        'HOST': os.environ['POSTGRES_HOST'],
        'PORT': os.environ['POSTGRES_PORT'],
    },
}
```

I prefer this once, since it is conditionless. There is no fallback which gives you wrong values in the case that the system has missing env vars.

If there are less ways to get config values, then the system is simpler and easier to handle.

# Use extend() to add a list.

Old:
```
valid_integers = [1, 2, 3]
for my_int in config.get_valid_integers():
    valid_integers.append(my_int)
```

New:
```
valid_integers = [1, 2, 3]
valid_integers.extend(config.get_valid_integers())
```

# Superfluous comment

```
    # pathlength checking protected field
    path_length_checking_file = ProtectedPathlengthCheckingField(
        max_length=100,
        upload_to='pathlength/',
        blank=True)
        ```
```

The comment does not provide any value. It is not needed.

# "response" is enough.

```
def test_foo_form(client):
    form_response = client.get(reverse('foo_form'))
    assert ...
    ....

    profile_response = client.post(reverse('foo_form'), data)
    assert ...
```

I prefer this, because it is enough. I think this makes reading the code easier.

The pattern `response = client....()` is common. It is simpler, if you always call the
response "response". You usualy work on one response after the other. You don't deal
with two responses in parallel.

```
def test_foo_form(client):
    response = client.get(reverse('foo_form'))
    assert ...
    ....

    response = client.post(reverse('foo_form'), data)
    assert ...
```



    
# Creating emails: html body is enough, plain-text not needed

I think it is enough to send the text/html without a text/plain alternative.

Docs how to do this with Django are at the bottom of this part [Sending alternative content types](https://docs.djangoproject.com/en/4.0/topics/email/#sending-alternative-content-types)


Today all mail clients show the html body of an email. 
