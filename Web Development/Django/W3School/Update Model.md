[Django Update Model](https://www.w3schools.com/django/django_update_model.php)

## Add Fields in the Model :

To add a field to a table after it is created, open the`models.py` file, and make your changes:

`project/members/models.py`:

```python
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField()
  joined_date = models.DateField()
```

As you can see, we want to add `phone` and`joined_date` to our Member Model.

This is a change in the Model's structure, and therefor we have to make a migration to tell Django that it has to update the database:

```bash
python manage.py makemigrations members
```

Which, in my case, will result in a prompt, because I try to add fields that are not allowed to be null, to a table that already contains records.

As you can see, Django asks if I want to provide the fields with a specific value, or if I want to stop the migration and fix it in the model:

```bash
python manage.py makemigrations members
You are trying to add a non-nullable field 'joined_date' to members without a default;
we can't do that (the database needs something to populate existing rows).
Please select a fix:
 1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
 2) Quit, and let me add a default in models.py
Select an option:
```

I will select option 2, and open the `models.py` file again and allow NULL values for the two new fields:

`project/members/models.py`:

```python
from django.db import models

class Member(models.Model):
  firstname = models.CharField(max_length=255)
  lastname = models.CharField(max_length=255)
  phone = models.IntegerField(null=True)
  joined_date = models.DateField(null=True)

```

And make the migration once again:

```bash
python manage.py makemigrations members
```

Which will result in this:

```bash
Migrations for 'members':
  members\\migrations\\0002_member_joined_date_member_phone.py
    - Add field 
  joined_date to member
    - Add field 
  phone to member
```

Run the migrate command:

```bash
python manage.py migrate
```

Which will result in this output:

```bash
Operations to perform: Apply all migrations: admin, auth, contenttypes, members, sessions
Running migrations:
  Applying members.0002_member_joined_date_member_phone... OK
(myworld) C:\\Users\\*Your Name*\\myworld\\my_tennis_club>
```

## Insert Data :

We can insert data to the two new fields with the same approach as we did in the [Update Model](https://www.notion.so/Update-Model-a0ae86aa28834a079b0c7aacd17f163e?pvs=21) :

First we enter the Python Shell:

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

At the bottom, after the three `>>>` write the following (and hit [enter] for each line):

```python
>>> from members.models import Member
>>> x = Member.objects.all()[0]
>>> x.phone = 5551234
>>> x.joined_date = '2022-01-05'
>>> x.save()
```

This will insert a phone number and a date in the Member Model, at least for the first record, the four remaining records will for now be left empty.

Execute this command to see if the Member table got updated:

```python
>>> Membeobjects.all().values()
```

The result should look like this:

```python
<QuerySet [
{'id': 1, 'firstname': 'Emil', 'lastname': 'Refsnes', 'phone': 5551234, 'joined_date': datetime.date(2022, 1, 5)},
{'id': 2, 'firstname': 'Tobias', 'lastname': 'Refsnes', 'phone': None, 'joined_date': None},
{'id': 3, 'firstname': 'Linus', 'lastname': 'Refsnes', 'phone': None, 'joined_date': None},
{'id': 4, 'firstname': 'Lene', 'lastname': 'Refsnes', 'phone': None, 'joined_date': None},
{'id': 5, 'firstname': 'Stalikken', 'lastname': 'Refsnes', 'phone': None, 'joined_date': None}]>
```