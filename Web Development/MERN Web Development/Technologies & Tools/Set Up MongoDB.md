[How to get started with MongoDB in 10 minutes](https://www.freecodecamp.org/news/learn-mongodb-a4ce205e7739/)

MongoDB is a rich document-oriented NoSQL database.

If you are a complete beginner to NoSQL, I recommend you to have a look [NoSQL](https://www.notion.so/Say-NO-to-SQL-1f898636dc5c4aa9a5a7b5b2762490e6?pvs=21).

# **Configuration :**

In order to work with MongoDB, first you need to install MongoDB on your computer. To do this, visit [the official download center](https://www.mongodb.com/download-center/community) and download the version for your specific OS.

After downloading MongoDB community server setup, you’ll go through a ‘next after next’ installation process. Once done, head over to the C drive in which you have installed MongoDB. Go to program files and select the MongoDB directory.

```
C: -> Program Files -> MongoDB -> Server -> 4.0(version) -> bin
```

In the bin directory, you’ll find an interesting couple of executable files.

- mongod : stands for “Mongo Daemon”. mongod is a background process used by MongoDB. The main purpose of mongod is to manage all the MongoDB server tasks. For instance, accepting requests, responding to client, and memory management.
- mongo : is a command line shell that can interact with the client (for example, system administrators and developers).

Now let’s see how we can get this server up and running. To do that on Windows, first you need to create a couple of directories in your C drive. Open up your command prompt inside your C drive and do the following:

```powershell
C:\> mkdir data/db
C:\> cd data
C:\> mkdir db
```

The purpose of these directories is MongoDB requires a folder to store all data. MongoDB’s default data directory path is `/data/db` on the drive. Therefore, it is necessary that we provide those directories like so.

If you start the MongoDB server without those directories, you’ll probably see this following error: trying to start mongodb server without \data\db directories

After creating those two files, head over again to the bin folder you have in your mongodb directory and open up your shell inside it. Run the following command: `mongod`

Voilà! Now our MongoDB server is up and running! ?

In order to work with this server, we need a mediator. So open another command window inside the bind folder and run the following command: `mongo`

After running this command, navigate to the shell which we ran mongod command (which is our server). You’ll see a ‘connection accepted’ message at the end. That means our installation and configuration is successful!

Just simply run in the mongo shell: `db`

initially you have a db called ‘test’

### **Setting up Environment Variables :**

To save time, you can set up your environment variables. In Windows, this is done by following the menus below:

```
Advanced System Settings -> Environment Variables -> Path(Under System Variables) -> Edit
```

Simply copy the path of our bin folder and hit OK! In my case it’s `C:\Program Files\MongoDB\Server\4.0\bin`

Now you’re all set!

# **Working with MongoDB :**

There’s a bunch of GUIs (Graphical User Interface) to work with MongoDB server such as MongoDB Compass, Studio 3T and so on.

They provide a graphical interface so you can easily work with your database and perform queries instead of using a shell and typing queries manually.