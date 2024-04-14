[Django Models](https://www.w3schools.com/django/django_models.php)

We will use the Python interpreter (Python shell) to add some members to it.

To open a Python shell, type this command:

```bash
python manage.py shell
```

Now we are in the shell, the result should be something like this:

```bash
Python 3.9.2 (tags/v3.9.2:1a79785, Feb 19 2021, 13:44:55) [MSC v.1928 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>>
```

At the bottom, after the three `>>>` write the following:

```python
>>> from members.models import Member
```

Hit [enter] and write this to look at the empty Member table:

```python
>>> Member.objects.all()
```

This should give you an empty QuerySet object, like this:

```python
<QuerySet []>
```

A QuerySet is a collection of data from a database.

Add a record to the table, by executing these two lines:

```python
>>> member = Member(firstname='Emil', lastname='Refsnes')
>>> member.save()
```

Execute this command to see if the Member table got a member:

```python
>>> Member.objects.all().values()
```

Hopefully, the result will look like this:

```python
<QuerySet [{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes'}]>
```