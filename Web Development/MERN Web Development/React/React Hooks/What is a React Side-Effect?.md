
A React side-effect occurs when we use something that is outside the scope of React.js in our React components e.g. Web APIs like localStorage.

What do I mean by outside of the React scope?

It means not part of the React framework, for example, the **localStorage** in your browser. Local Storage is a Web API and not part of the React universe.

On a side note, localStorage is an essential tool for building web applications. You can read more about it at the official MDN website:
## Window.localStorage - Web APIs | MDN

```
The keys and the values stored with localStorage are always in the UTF-16 string format, which uses two bytes per…
```

When we use React with any of the **Browser’s API** such as the **localStorage**, we are creating side-effects.

For example, if we run this code, we are creating a side-effect by storing some value in localStorage.

```jsx
useEffect(() => {  
 localStorage.setItem('some key', true);  
}, []);
```

Another example of creating side-effects is using native DOM methods instead of methods included in React:

```jsx
useEffect(() => {  
 document.getElementById("overlay").style.display = "block";  
}, []);
```

The main thing we should be concerned with is whether we can manage these side-effects effectively.

**What do you mean by managing the side-effects effectively?**

I mean whether or not we can effectively keep track of the changes in the side effects and whether we can manage the side-effects in a single place or a single component within our frontend application.
# Conclusion :
- **When we talk about side effects in the context of React.js, we are referring to anything that is outside the scope of React**
- **So calling any native Web APIs will be considered as a side effect as it’s not within the React universe**
- **Making a HTTPS request to an external API is another example of a side effect and the list goes on…**
- **We usually manage React side effects inside the useEffect hook (part of the React Hooks API)**