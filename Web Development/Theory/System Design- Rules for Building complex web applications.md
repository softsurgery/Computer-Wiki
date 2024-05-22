[Medium - System Design](https://codesensei.medium.com/system-design-how-to-design-a-complex-web-application-657c3798c532)

A lot of my students, as they go through interviews in their career, they often get questions about how they would design an app that does X, Y, and Z. Based on X, Y, Z, they would need to have good reasons about which programming language they would use, which framework they would use, which database they would use, which micro-services they would create, if there will be APIs and if so what language will be used to create the APIs, and if there would be any front-end frameworks and if so what they would use.

I’ve thought about this question a lot and also how I could pass down my knowledge to my students most effectively.

If someone asked how I would design a web application when I was very early in my career, I probably wouldn’t be able to give a strong argument about why we should go with programming language A, framework B, database C, have micro-services that are D, E, and F, and where we use Amazon Services G, H, I, and also use front-end frameworks/libraries J, K, L. My answer would have been, I would use X, Y, and Z, because that’s what I know…

Now, after learning, mastering, and teaching people over various programming languages (PHP, Javascript, C#, Python, Ruby, Java, Swift, Objective C) and having exposure to multiple frameworks (CodeIgniter, Laravel, Zend, Express, Socket.io, .NET MVC, .NET Core, Flask, Django, Sinatra, Rails, Spring, React, Angular, Ember, Meteor, Vue.js, jQuery, Underscore, Bootstrap, Foundation, etc) as well as having experienced with more tools for collaborating and pushing codes to the production (e.g. Amazon AWS, Microsoft Azure, Amazon S3, Github tools to streamline team collaboration and deployment, continuous integration and continuous delivery including use of Jenkins, Dockers, etc), I do have more of a strong opinion on how any system should be architected.

There is certainly a logic that happens in my mind when I think about how I would architect a system. It felt more like gut feeling and from my experience but I never really tried to create a logic flow on how I decide these answers.

So one Friday morning, I sat down, opened up my Windows laptop (which has Visio), and tried to lay out what happens in my brain when I think about System Design. After about 5 hours, reviewing the flow again to see if I missed anything, I finally came up with a 5 page work-flow that captured my decision making process!

It felt weird that what seemed to be an intuition (based on my experience) could actually be transcribed to fit in a 5 page…. It was great (that I could now give someone a PDF to have them follow to see what I would do) but also a bit sad as what others will now learn from these 5 pages, in minutes, is what took me years to learn…

Anyhow, in this flow, I answered questions such a

- when I would use Python
- if the app has real time component, which language I would use and why
- my views on light framework vs a heavy framework
- my opinion about how continuous integration, continuous delivery and continuous deployment should be set up and which git hooks it should be tied to
- my opinion about when the team should use a github flow (simple) vs a git flow (complex)
- my opinion about various MVC frameworks and which one to use when
- my opinion about how to design micro-services and how many micro-services I would create for any project
- where I would store data, when I would use a relational vs non-relational database, and where I would store them.
- importance of asking questions in the discovery stage
- importance of not over-engineering the system and my rule of thumbs for determining whether the project should use heavy front-end frameworks such as Angular or React
- ideal team size for each app/micro-service
- how to bucket different services available in the cloud and how to utilize them depending on whether you have traditional web servers or you’re using containers (Docker or other types)
- how a tool like Jenkins can be used for continuous integration/delivery/deployment

The final output for my process is below. For the key principle, I’ve highlighted them in yellow.

You can also watch me explaining step by step my thought process for the items in the doc:

## Key things to remember

1. Ask a lot of questions to better understand what you’re asked to build. Don’t assume. Always ask and validate.
2. Smaller projects is better (where you have no more than 5–10 engineers working on each app/micro-service)
3. Don’t use light-weight framework to build something that will require months of development. Use it for short week-end projects. Similarly, don’t use a heavy framework when building a simple service.
4. Keep your database small/light. Use file serves such as Amazon S3 to store large files.
5. Know how to scale up/down number of servers/containers yourself but also know that there are services that orchestrate this (e.g. Elastic Bean Stalk, Kubernetes, services that manage Kubernetes, etc)
6. Creating multiple micro-services is a trend right now. This is good but you can also go over board and go to the extreme. Any extreme hurts productivity and how fast things can be built. My rule of thumb is usually how many engineers will be working on it and for how long. For example, if a micro-service can be built in 2 hours by a single engineer, and imagine your project has 100 engineers, you don’t want to have 50,000 micro-services as your engineers will be overwhelmed. Similarly, you also don’t want to have just 2–5 micro-services either as that means 20–50 engineers per micro-service. My rule of thumb is to keep each micro-service to the point where it does not exceed 5–10 FT back-end engineers working on it. This does not however mean that you could have a well designed micro-service that was built by a few engineers and where it doesn’t require a lot of maintenance. Use balance in all things but just know that more micro-services you create, more effort it will take for your engineers to understand all the different micro-services for your project and it could slow down the entire project (with sheer increasing number of micro-services that are added to your project).
7. My personal preference is not to build something in C# or Java MVC frameworks unless my developers are only familiar with these frameworks and already love them. There is a higher learning curve for these languages/frameworks and I honestly think there is a reason why the open source community have shifted away from these two languages and frameworks. They are not bad languages…. In fact, I love C# and Java as a language but to quickly build something and deploy, it is just not as easy and light-weight as other options.
8. Usually the bottleneck is with the database operations. It is almost never the programming language.
9. Using a heavy-weight framework is good for building an enterprise level applications that is complex. It does introduce additional load on the system so expect that there will be a bit of performance sacrifice for using a large framework. Again, don’t over-engineer and use a heavy-weight framework when you don’t need to. Using a heavy-weight system for something very light is analogous to having 1000 folders to store just a few files. Use a light-weight framework (non MVC) if your app/micro-service needs to only perform a few specific functions. Not only will this consume less CPU/RAM for your server/container (saving you money) but it will also give you performance gain as your server/container doesn’t have to load all the heavy-framework before processing your code.
10. Focus on the principles (why we do things) instead of just the how’s. If you don’t know what principles are being followed, when things change, you will be stuck in the old and incorrect ways. Understand the principles and have the principles drive you to the right actions.
11. Keep learning. How servers and containers are orchestrated/managed is constantly evolving. Each cloud service also offers their unique service for managing servers/containers.
12. Try not to be overwhelmed. All complex projects can be broken down to simpler components. All technologies that exist in the world break down to the core building blocks of software and if you had enough time, you could also create any of these technologies. Use other technologies, as this would save you time, but don’t get to the point where you’re always relying on other people’s code/libraries/services to get your project working. Avoid being a ‘typist’ who only follows tutorials/walk-throughs to piece together a project. Have the confidence and the ability to create whatever you can think of using fundamental building blocks of software.