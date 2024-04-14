[FreeCodeCamp](https://www.freecodecamp.org/news/django-model-relationships/)

[Django](https://www.djangoproject.com/) is a free and open-source web framework written in Python. It helps with rapid web development and provides out-of-the-box web security.  
  
Websites must be able to store and retrieve data from databases. Django makes provisions for this. By default, Django operates a Relational Database Management System.

> A relational database is a type of database that stores and provides access to data points that are related to one another. Relational databases are based on the relational model, an intuitive, straightforward way of representing data in tables. In a relational database, each row in the table is a record with a unique ID called the key. [(Source: Oracle Cloud)](https://www.oracle.com/ng/database/what-is-a-relational-database/#:~:text=The%20software%20used%20to%20store,storage%2C%20access%2C%20and%20performance.)

A major advantage of a Relational Database Management System is being able to define the types of relationships between the different data held in the different tables of your database.

This tutorial shows you how you can define the relationships between your Django [models](https://docs.djangoproject.com/en/3.2/topics/db/models/). To get the most out of this tutorial, you should have a basic understanding of the Django web framework, especially Django file structure and [models](https://docs.djangoproject.com/en/3.2/topics/db/models/).

## **Different Types of Model Relationships in Django**

Each [model](https://docs.djangoproject.com/en/3.2/topics/db/models/) in a [Django](https://www.djangoproject.com/) application represents a database table. This means that you can define the kind of relationship you want between the different models in your Django application.

Django supports three main types of relationships between its models. They're as follows:

### **One-to-One Relationship**

A ****one-to-one**** relationship means that a record in one table relates to a single record in another table.

An instance of this is if you have a Django model that defines users. This model can then have a one-to-one relationship with another Django model, which defines users' profiles. In this scenario, a user can have only one profile and a profile can be associated with only one user.

Django provides you with `OneToOneField`****,**** which helps define a one-to-one relationship between two different models.

The following code shows you how you can define a ****one-to-one**** relationship in Django. Go to your `<app>/models.py` and write the following code:

```python
from django.db import models

class User(models.Model):
    name = models.CharField(max_length=50)

    def __str__(self):
        return str(self.name)

class Profile(models.Model):
    user = models.OneToOneField(
        User,
        on_delete=models.PROTECT,
        primary_key=True,
    )
    language = models.CharField(max_length=50)
    email = models.EmailField(max_length=70,blank=True,unique=True)

    def __str__(self):
        return str(self.email)
```

Let's see what's going on in the above code:

1. Line one imports the sub-module `models` from the `django.db` module.
2. Line three defines a Django model named `User`.
3. Line four defines a `name` property within the `User` model, and the keyword `CharField` is how you define text with a limited number of characters.
4. Line six and seven is a special method within the `User` model and returns a string representation of the `User` model that includes the `name` property.
5. Line nine defines a Django model named `Profile`.
6. Line ten defines a ****one-to-one**** relationship between the `Profile` model and the `User` model using the `OneToOneField` keyword.
7. Lines eleven and twelve define the properties `language` and `email` within the `Profile` model.
8. Line twelve and thirteen is a special method within the `Profile` model and returns a string representation of the `Profile` model that includes the `email` property.

### **Many-to-one Relationship**

In a ****many-to-one**** relationship, a record in one table relates to multiple records in another table. Some resources refer to a ****many-to-one**** relationship as a __one-to-many__ relationship. These two mean the same thing.

An example of a one to many relationship is the relationship between an author and their published books. While an author can have more than one book to their name, it's less common to find a book with more than one author. You can see this kind of relationship as a parent-child relationship.

A diagram showing a one-to-many relationship between an Author model and a Book model.

Django provides you with `ForeignKey`, which helps define a ****many-to-one**** relationship between two different models.

The following code shows you how you can define a ****many-to-one**** relationship in Django. Go to your `<app>/models.py` and write the following code:

```python
from django.db import models

class Author(models.Model): 
    name = models.CharField(max_length=50, blank=False, unique=True)

    def __str__(self):
        return str(self.name)
    
class Book(models.Model):
    author = models.ForeignKey(
        Author,
        on_delete=models.PROTECT,
        blank=False
    )

    title = models.CharField(max_length=100)

    def __str__(self):
        return self.title
```

This code is similar to the code in the first example. However, in line ten here, the `ForeignKey` keyword is what you use to define a many-to-one relationship between two Django models. In this case, the current model will have a ****many-to-one**** relationship with the `Author` model.

### **Many-to-Many Relationship**

In a ****many-to-many**** relationship, multiple records in one table relate to many records in another table. For instance, you may have a collection model in your application, in which one collection has many books within it. In the same vein, a book may belong to several collections.

The following diagram illustrates a ****many-to-many**** relationship:

![diagram indicating many-to-many relationship](https://www.freecodecamp.org/news/content/images/2023/03/Untitled-drawing--3-.png)

A diagram showing a many-to-many relationship between a Collection model and a Book model.

Django provides you with a [`ManyToManyField`](https://docs.djangoproject.com/en/4.1/ref/models/fields/#django.db.models.ManyToManyField) which helps define a ****many-to-many**** relationship between two different models.

The following code shows you how you can define a many-to-many relationship in Django. Go to your `<app>/models.py` and enter the write the following code:

```python
from django.db import models

class Collection(models.Model):
  name = models.CharField(max_length=50)
  
  def __str__(self):
        return str(self.name)
    
class Book(models.Model):
  collection = models.ManyToManyField(Collection)
  
  title = models.CharField(max_length=100)
  
  def __str__(self):
        return str(self.title)
```

In this code sample, the `ManyToManyField` keyword in line 10 is what you use to define a ****many-to-many**** relationship between the `Collection` and `Book` models.

## **Conclusion**

Although this tutorial explains the basics of Django model relationships to help you get a basic understanding, real-life projects can get more complicated.

The complexity of your Django model relationships depends on your application's complexity. So before you build your applications, it's important to plan out your database relationships. Doing this saves you lots of time and effort compared to finding problems and backtracking later.

---

![Damilola Oladele](https://www.freecodecamp.org/news/content/images/size/w60/2023/03/damilola_compressed.jpeg)

[Damilola Oladele](https://www.freecodecamp.org/news/author/damilola_oladele/)

Damilola Oladele is a self-motivated software engineer. He has a background in law and property management. He is passionate about using new technologies to provide web-based solutions.

---

If you read this far, thank the author to show them you care.

Learn to code for free. freeCodeCamp's open source curriculum has helped more than 40,000 people get jobs as developers. [Get started](https://www.freecodecamp.org/learn/)

freeCodeCamp is a donor-supported tax-exempt 501(c)(3) charity organization (United States Federal Tax Identification Number: 82-0779546)

Our mission: to help people learn to code for free. We accomplish this by creating thousands of videos, articles, and interactive coding lessons - all freely available to the public.

Donations to freeCodeCamp go toward our education initiatives, and help pay for servers, services, and staff.

You can [make a tax-deductible donation here](https://www.freecodecamp.org/donate/).

## Trending Guides

- [Date Formatting in JS](https://www.freecodecamp.org/news/how-to-format-a-date-with-javascript-date-formatting-in-js/)
- [Java Iterator Hashmap](https://www.freecodecamp.org/news/java-iterator-hashmap-how-to-iterate-through-a-hashmap-with-a-loop/)
- [Cancel a Merge in Git](https://www.freecodecamp.org/news/git-abort-merge-how-to-cancel-a-merge-in-git/)
- [What is a Linked List?](https://www.freecodecamp.org/news/what-is-a-linked-list-types-and-examples/)
- [Install Java in Ubuntu](https://www.freecodecamp.org/news/how-to-install-java-in-ubuntu/)
- [Python Ternary Operator](https://www.freecodecamp.org/news/python-tenary-operator/)
- [Full Stack Career Guide](https://www.freecodecamp.org/news/full-stack-engineer-career-guide/)
- [Python Sort Dict by Key](https://www.freecodecamp.org/news/python-sort-dictionary-by-key/)
- [Smart Quotes Copy/Paste](https://www.freecodecamp.org/news/smart-quotes-single-quote-and-double-quotation-mark-for-copy-paste/)
- [JavaScript Array Length](https://www.freecodecamp.org/news/javascript-array-length-tutorial/)
- [Sets in Python](https://www.freecodecamp.org/news/how-to-use-sets-in-python/)
- [Kotlin vs Java](https://www.freecodecamp.org/news/kotlin-vs-java/)
- [SQL Temp Table](https://www.freecodecamp.org/news/sql-temp-table-how-to-create-a-temporary-sql-table/)
- [HTML Form Basics](https://www.freecodecamp.org/news/how-to-use-html-forms/)
- [Comments in YAML](https://www.freecodecamp.org/news/how-to-add-a-multiline-comment-in-yaml/)
- [Pandas Count Rows](https://www.freecodecamp.org/news/pandas-count-rows-how-to-get-the-number-of-rows-in-a-dataframe/)
- [Python End Program](https://www.freecodecamp.org/news/python-end-program-how-to-exit-a-python-program-in-terminal/)
- [Python XOR Operator](https://www.freecodecamp.org/news/xor-py-how-the-python-xor-operator-works/)
- [Python Dict Has Key](https://www.freecodecamp.org/news/how-to-check-if-a-key-exists-in-a-dictionary-in-python/)
- [Python List to String](https://www.freecodecamp.org/news/python-list-to-string-how-to-convert-lists-in-python/)
- [Exit Function in Python](https://www.freecodecamp.org/news/python-exit-how-to-use-an-exit-function-in-python-to-stop-a-program/)
- [String to Array in Java](https://www.freecodecamp.org/news/string-to-array-in-java-how-to-convert-a-string-to-an-array-in-java/)
- [Python Import from File](https://www.freecodecamp.org/news/python-import-from-file-importing-local-files-in-python/)
- [Parse a String in Python](https://www.freecodecamp.org/news/how-to-parse-a-string-in-python/)
- [Python Merge Dictionaries](https://www.freecodecamp.org/news/python-merge-dictionaries-merging-two-dicts-in-python/)
- [Copy a Directory in Linux](https://www.freecodecamp.org/news/how-to-copy-a-directory-in-linux-with-the-cp-command/)
- [Reactive Programming Guide](https://www.freecodecamp.org/news/reactive-programming-beginner-guide/)
- [Center Text Vertically CSS](https://www.freecodecamp.org/news/how-to-center-text-vertically-with-css/)
- [What’s a Greedy Algorithm?](https://www.freecodecamp.org/news/greedy-algorithms/)
- [Edit Commit Messages in Git](https://www.freecodecamp.org/news/how-to-edit-git-commit-messages-with-git-amend/)

## Mobile App

- [![Download on the App Store](https://cdn.freecodecamp.org/platform/universal/apple-store-badge.svg)](https://apps.apple.com/us/app/freecodecamp/id6446908151?itsct=apps_box_link&itscg=30200)
- [![Get it on Google Play](https://cdn.freecodecamp.org/platform/universal/google-play-badge.svg)](https://play.google.com/store/apps/details?id=org.freecodecamp)

[About](https://www.freecodecamp.org/news/about/) [Alumni Network](https://www.linkedin.com/school/free-code-camp/people/) [Open Source](https://github.com/freeCodeCamp/) [Shop](https://www.freecodecamp.org/news/shop/) [Support](https://www.freecodecamp.org/news/support/) [Sponsors](https://www.freecodecamp.org/news/sponsors/) [Academic Honesty](https://www.freecodecamp.org/news/academic-honesty-policy/) [Code of Conduct](https://www.freecodecamp.org/news/code-of-conduct/) [Privacy Policy](https://www.freecodecamp.org/news/privacy-policy/) [Terms of Service](https://www.freecodecamp.org/news/terms-of-service/) [Copyright Policy](https://www.freecodecamp.org/news/copyright-policy/)