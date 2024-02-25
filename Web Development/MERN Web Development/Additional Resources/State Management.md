[Why State Management is Important For React Apps](https://medium.com/bb-tutorials-and-thoughts/why-state-management-is-important-for-react-apps-e72a36d81edd)

When it comes to software engineering it’s all about the data. Databases store the data, server-side technologies like Java, .NET, etc move data around and from server to client, Frontend frameworks use that data and show that to the user. All the tools and frameworks that we use to make the data move efficiently. For example, it’s very easy to use one of the available database engines such as MySql, Oracle, PostgreSQL, etc, and create a table and put data in it but, designing the data so that you query the database efficiently and without performance bottlenecks are important.

> If you want to make quick decisions in the project: know Your data well.
> 
> 
> *If you want to make your apps efficient: Design your data well.*
> 

Managing your data in frontend frameworks is equally important as databases. You could avoid a number of API calls if you manage your data well.

# **How Data Flows Without State Management:**

In React applications, we usually maintain the data in the services. As the number of features increases, the number of services increases which means data management becomes difficult. Without state management data is everywhere. You can’t really have a single source of truth for your data this kind of setup is very difficult to maintain, and it makes your development process slow.

# **How Data Flows with State Management:**

When you have state management in place data actually flows from your app to state and vice versa. You know exactly where your data is. These state management tools also give you a point-in-time snapshot of the entire data. In that way, you know exactly where your data is and that makes your development faster.

For example, you have some features in your app as below. State management can maintain the same structure as a database, and you can select the slice of data whenever you want in your app.

# ***Prerequisites:***

You need to understand certain concepts before you consider State Management tools such as NGRX. The following concepts are important because the entire state management setup depends on these.

- **Functional Programming**
- **Reactive Programming**
- **RXJS**
- **Typescript**
- **JavaScript ES6 and ESNext Features**

## **Functional Programming:**

Functional programming is a part of declarative programming. Functions in JavaScript are first-class citizens which mean functions are data and you can save, retrieve, and pass these functions throughout your application just like variables.

There are some core concepts for the programming to be functional. Here are those.

***Immutability:*** Immutability means unchangeable. In functional programming, you can’t change the data and it never changes. If you want to mutate or change the data, you have to copy the data and change the copied version and use it.

***Pure Functions:*** Pure Functions are the functions that always take one or more arguments and computes on arguments and return data or functions. It doesn’t have side effects such as setting a global state, changing the application state and it always treats arguments as immutable data.

***Data Transformations:*** We are talking a lot about immutability so how do we change data if the data is immutable. As discussed above, we always produce transformed copies of the original instead of directly changing the original data.

***Higher-Order Functions:*** Higher-order functions are the functions that take functions as an argument or return functions or sometimes they do both. These higher-order functions can manipulate other functions.

***Recursion:*** Recursion is a technique where the function calls itself until a certain condition is met. It’s better to use Recursion instead of loops whenever possible. You have to be careful with this since browsers can’t handle too much recursion and throw errors.

***Composition:*** Combining all the smaller functions into larger functions and eventually, you would get an application. This is called composition.

## **Reactive Programming:**

Functional programming is a part of declarative programming. It works with data streams and subscribes to the data and does something whenever that data changes. All the data and data-related events are done by data streams such as Events, messages, API calls, etc.

## **RXJS:**

RxJS is a library for composing asynchronous and event-based programs by using observable sequences. It provides one core type, the Observable, satellite types (Observer, Schedulers, Subjects) and operators inspired by Array (map, filter, reduce, every, etc.) to allow handling asynchronous events as collections.

## **Typescript**

TypeScript is a superset of JavaScript. You can write your React application in Typescript nowadays.

## **JavaScript ES6 and ESNext Features**

There are so many latest features in JavaScript. All these features are released over the years named ES6, ES7, ESNext, etc. You need to understand commonly used features.

# **How Redux Works**

Redux is a state management library for React Applications. When your application gets bigger and bigger communication becomes difficult to handle. Redux provides unidirectional data flow and a single source of truth for the entire app.

Components that aware of the store are called smart components and Components that aren’t aware of the store are called dumb components. If you look at the following diagram, all the smart components send the data to the store and receive data from the store promoting unidirectional data flow for the entire app.

Refreshing or reloading the page will lose the entire state of the application. That’s when localstorage comes into the picture. The whole state of the app is serialized and saved into localstorage just before the reloading the page and the entire state is deserialized from the localstorage and reinitialize the state of the app. This is called rehydrating the store.

Sometimes we have to make API calls to fetch the data for the app. Whenever the store needs data from the backend API, it uses thunk API to make an API call and fetch the data and update the store.

# **Other Libraries:**

There are other libraries for state management of React apps. Redux has been there for a while and the following are the recent additions. You can click on these and find more on their websites. I will write an article comparing all these in the coming articles.

- [Redux Toolkit (Management of Redux easier)](https://redux-toolkit.js.org/)
- [Redux-Saga](https://redux-saga.js.org/)
- [Mobx](https://mobx.js.org/react-integration.html)

# **Advantages:**

- You have a single source of truth for the data in your app.
- Data flows in one direction.
- Accessing the data whenever and wherever you want facilitates the speed of your application development.
- You could avoid 100s of API calls if you design it well. This would take a lot of load off from the backend server.
- Automatically sync with local storage. This makes you retain the data whenever you refresh the page.

# **Disadvantages**

- You have to design it carefully otherwise the whole setup becomes clumsy and unmaintainable.
- Another dependency this means your app bundle size might increase.

# **Summary**

- Managing your data is very important in frontend applications for better performance and efficiency.
- If you want to make quick decisions in the project, you need to know the data well.
- If you want to make an efficient app you need to design data well.
- Data is scattered everywhere without state management.
- State management libraries facilitate one-way data flow in the applications.
- You need to understand certain concepts before trying your first state management libraries such as functional programming, reactive programming, RXJS, typescript, and ESNext features.
- We have three libraries to consider for state management: React-Redux, Redux-saga, and Mobx.

# **Conclusion**

Don’t use it in every app and you need to make a decision based on several factors such as team, app size, learning curve, etc. Use state management based on your apps and design them carefully so that they can be extensible and maintainable. I will write posts about how to use these three libraries with example projects.