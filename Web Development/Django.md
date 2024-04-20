Django is a free and open-source, Python-based web framework that follows the model–template–views architectural pattern. It is maintained by the Django Software Foundation, an independent organization established in the US as a 501 non-profit.

## W3Schools :
---
[Django Tutorial](https://www.w3schools.com/django/index.php)
###### What is Django?
* Django is a Python framework that makes it easier to create web sites using Python.
* Django takes care of the difficult stuff so that you can concentrate on building your web applications.
* Django emphasizes reusability of components, also referred to as DRY (Don't Repeat Yourself), and comes with ready-to-use features like login system, database connection and CRUD operations (Create Read Update Delete).
* Django is especially helpful for database driven websites.
###### How does Django Work?
* Django follows the MVT design pattern (Model View Template).
- Model - The data you want to present, usually data from a database.
- View - A request handler that returns the relevant template and content - based on the request from the user.
- Template - A text file (like an HTML file) containing the layout of the web page, with logic on how to display the data.
###### So, What is Going On?
* When you have installed Django and created your first Django web application, and the browser requests the URL, this is basically what happens:
1. Django receives the URL, checks the `urls.py` file, and calls the view that matches the URL.
2. The view, located in `views.py`, checks for relevant models.
3. The models are imported from the `models.py` file.
4. The view then sends the data to a specified template in the `template` folder.
5. The template contains HTML and Django tags, and with the data it returns finished HTML content back to the browser.
* Django can do a lot more than this, but this is basically what you will learn in this tutorial, and are the basic steps in a simple web application made with Django.
###### Django History
* Django was invented by Lawrence Journal-World in 2003, to meet the short deadlines in the newspaper and at the same time meeting the demands of experienced web developers.
* Initial release to the public was in July 2005.
* Latest version of Django is 4.0.3 (March 2022).
--- 
# Basics
* [[Getting Started]]
* [[Create Project]]
* [[Create App]]
* [[Views]]
* [[URLs]]
* [[Templates]]
* [[Models]]
* [[Insert Data]]
* [[Update Data]]
* [[Delete Data]]
* [[Update Model]]
# Additional Resources
---
* [[Define Relationships Between Models]]
* [[Implement JWT]]
* [[Swagger UI]]