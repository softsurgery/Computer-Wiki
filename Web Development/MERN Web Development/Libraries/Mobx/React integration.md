[Mobx Documentation](https://mobx.js.org/react-integration.html#react-integration)

Usage:

```jsx
import { observer } from "mobx-react-lite" // Or "mobx-react".

const MyComponent = observer(props => ReactElement)
```

While MobX works independently from React, they are most commonly used together. 

`observer` is provided by a separate React bindings package you choose [during installation](https://mobx.js.org/installation.html#installation). In this example, we're going to use the more lightweight [`mobx-react-lite` package](https://github.com/mobxjs/mobx/tree/main/packages/mobx-react-lite).

```jsx
import React from "react"
import ReactDOM from "react-dom"
import { makeAutoObservable } from "mobx"
import { observer } from "mobx-react-lite"

class Timer {
    secondsPassed = 0

    constructor() {
        makeAutoObservable(this)
    }

    increaseTimer() {
        this.secondsPassed += 1
    }
}

const myTimer = new Timer()

// A function component wrapped with `observer` will react
// to any future change in an observable it used before.
const TimerView = observer(({ timer }) => <span>Seconds passed: {timer.secondsPassed}</span>)

ReactDOM.render(<TimerView timer={myTimer} />, document.body)

setInterval(() => {
    myTimer.increaseTimer()
}, 1000)
```

The `observer` HoC (Higher Order Components) automatically subscribes React components to _any observables_ that are used _during rendering_. As a result, components will automatically re-render when relevant observables change. It also makes sure that components don't re-render when there are _no relevant_ changes. So, observables that are accessible by the component, but not actually read, won't ever cause a re-render.

In practice this makes MobX applications very well optimized out of the box and they typically don't need any additional code to prevent excessive rendering.

For `observer` to work, it doesn't matter _how_ the observables arrive in the component, only that they are read. Reading observables deeply is fine, complex expression like `todos[0].author.displayName` work out of the box. This makes the subscription mechanism much more precise and efficient compared to other frameworks in which data dependencies have to be declared explicitly or be pre-computed (e.g. selectors).

## Local and external state

There is great flexibility in how state is organized, since it doesn't matter (technically that is) which observables we read or where observables originated from. The examples below demonstrate different patterns on how external and local observable state can be used in components wrapped with `observer`.

### Using external state in `observer` components :

## using props
Observables can be passed into components as props (as in the example above):

```jsx
import { observer } from "mobx-react-lite"
const myTimer = new Timer() // See the Timer definition above.
const TimerView = observer(({ timer }) => <span>Seconds passed: {timer.secondsPassed}</span>)
// Pass myTimer as a prop.
ReactDOM.render(<TimerView timer={myTimer} />, document.body)
```

## using global variables

Since it doesn't matter _how_ we got the reference to an observable, we can consume observables from outer scopes directly (including from imports, etc.):

```jsx
const myTimer = new Timer() // See the Timer definition above.
// No props, `myTimer` is directly consumed from the closure.
const TimerView = observer(() => <span>Seconds passed: {myTimer.secondsPassed}</span>)
ReactDOM.render(<TimerView />, document.body)
```

Using observables directly works very well, but since this typically introduces module state, this pattern might complicate unit testing. Instead, we recommend using React Context instead.

## using React context

[React Context](https://reactjs.org/docs/context.html) is a great mechanism to share observables with an entire subtree:

```jsx
import {observer} from 'mobx-react-lite'
import {createContext, useContext} from "react"
const TimerContext = createContext<Timer>()const TimerView = observer(() => {    
	// Grab the timer from the context.
	const timer = useContext(TimerContext) 
	// See the Timer definition above.    
	return (        
		<span>Seconds passed: {timer.secondsPassed}</span>
)})
ReactDOM.render(    
	<TimerContext.Provider value={new Timer()}>        
	<TimerView /> </TimerContext.Provider>,
	document.body
)
```

Note that we don't recommend ever replacing the `value` of a `Provider` with a different one. Using MobX, there should be no need for that, since the observable that is shared can be updated itself.

### Using local observable state in `observer` components

Since observables used by `observer` can come from anywhere, they can be local state as well. Again, different options are available for us.

## `useState` with observable class

The simplest way to use local observable state is to store a reference to an observable class with `useState`. Note that, since we typically don't want to replace the reference, we totally ignore the updater function returned by `useState`:

```jsx
import { observer } from "mobx-react-lite"
import { useState } from "react"
const TimerView = observer(() => {
	const [timer] = useState(() => new Timer())// See the Timer definition above.
	return <span>Seconds passed: {timer.secondsPassed}</span>
})
ReactDOM.render(<TimerView />, document.body)
```

If you want to automatically update the timer like we did in the original example, `useEffect` could be used in typical React fashion:

```jsx
useEffect(() => {
	const handle = setInterval(() => {
		timer.increaseTimer()    
	}, 1000)    
	return () => {clearInterval(handle)}
}, [timer])
```

## `useState` with local observable object

As stated before, instead of using classes, it is possible to directly create observable objects. We can leverage [observable](https://mobx.js.org/observable-state.html#observable) for that:

```jsx
import { observer } from "mobx-react-lite"
import { observable } from "mobx"
import { useState } from "react"
const TimerView = observer(() => {
const [timer] = useState(() => observable({
		secondsPassed: 0, increaseTimer() {
			this.secondsPassed++
		}
	})
	) 
	return <span>Seconds passed: {timer.secondsPassed}</span>
})
ReactDOM.render(<TimerView />, document.body)
```

## `useLocalObservable` hook

The combination `const [store] = useState(() => observable({ /* something */}))` is quite common. To make this pattern simpler the [`useLocalObservable`](https://github.com/mobxjs/mobx-react#uselocalobservable-hook) hook is exposed from `mobx-react-lite` package, making it possible to simplify the earlier example to:

```jsx
import { observer, useLocalObservable } from "mobx-react-lite"
import { useState } from "react"
const TimerView = observer(() => {    
	const timer = useLocalObservable(() => ({        
		secondsPassed: 0,increaseTimer() {            
			this.secondsPassed++        
		}    
	}))    
	return <span>Seconds passed: {timer.secondsPassed}</span>
})
ReactDOM.render(<TimerView />, document.body)
```
### You might not need locally observable state

In general, we recommend to not resort to MobX observables for local component state too quickly, as this can theoretically lock you out of some features of React's Suspense mechanism. As a rule of thumb, use MobX observables when the state captures domain data that is shared among components (including children). Such as todo items, users, bookings, etc.

State that only captures UI state, like loading state, selections, etc, might be better served by the [`useState` hook](https://reactjs.org/docs/hooks-state.html), since this will allow you to leverage React suspense features in the future.

Using observables inside React components adds value as soon as they are either 
1) deep, 
2) have computed values or 
3) are shared with other `observer` components.

## Always read observables inside `observer` components

You might be wondering, when do I apply `observer`? The rule of thumb is: _apply `observer` to all components that read observable data_.

`observer` only enhances the component you are decorating, not the components called by it. So usually all your components should be wrapped by `observer`. Don't worry, this is not inefficient. On the contrary, more `observer` components make rendering more efficient as updates become more fine-grained.

#### Tip: Grab values from objects as late as possible

`observer` works best if you pass object references around as long as possible, and only read their properties inside the `observer` based components that are going to render them into the DOM / low-level components. In other words, `observer` reacts to the fact that you 'dereference' a value from an object.

In the above example, the `TimerView` component would **not** react to future changes if it was defined as follows, because the `.secondsPassed` is not read inside the `observer` component, but outside, and is hence _not_ tracked:

```jsx
const TimerView = observer(({ secondsPassed }) => <span>Seconds passed: {secondsPassed}</span>)
React.render(<TimerView secondsPassed={myTimer.secondsPassed} />, document.body)
```

Note that this is a different mindset from other libraries like `react-redux`, where it is a good practice to dereference early and pass primitives down, to better leverage memoization. If the problem is not entirely clear, make sure to check out the [Understanding reactivity](https://mobx.js.org/understanding-reactivity.html) section.
### Don't pass observables into components that aren't `observer`

Components wrapped with `observer` _only_ subscribe to observables used during their _own_ rendering of the component. So if observable objects / arrays / maps are passed to child components, those have to be wrapped with `observer` as well. This is also true for any callback based components.

If you want to pass observables to a component that isn't an `observer`, either because it is a third-party component, or because you want to keep that component MobX agnostic, you will have to [convert the observables to plain JavaScript values or structures](https://mobx.js.org/observable-state.html#converting-observables-back-to-vanilla-javascript-collections) before passing them on.

To elaborate on the above, take the following example observable `todo` object, a `TodoView` component (observer) and an imaginary `GridRow` component that takes a column / value mapping, but which isn't an `observer`:

```javascript
class Todo {
    title = "test"
    done = true

    constructor() {
        makeAutoObservable(this)
    }
}

const TodoView = observer(({ todo }: { todo: Todo }) =>
   // WRONG: GridRow won't pick up changes in todo.title / todo.done
   //        since it isn't an observer.
   return <GridRow data={todo} />

   // CORRECT: let `TodoView` detect relevant changes in `todo`,
   //          and pass plain data down.
   return <GridRow data={{
       title: todo.title,
       done: todo.done
   }} />

   // CORRECT: using `toJS` works as well, but being explicit is typically better.
   return <GridRow data={toJS(todo)} />
)
```

### Callback components might require `<Observer>`

Imagine the same example, where `GridRow` takes an `onRender` callback instead. Since `onRender` is part of the rendering cycle of `GridRow`, rather than `TodoView`'s render (even though that is where it syntactically appears), we have to make sure that the callback component uses an `observer` component. Or, we can create an in-line anonymous observer using [`<Observer />`](https://github.com/mobxjs/mobx-react#observer):

```javascript
const TodoView = observer(({ todo }: { todo: Todo }) => {
    // WRONG: GridRow.onRender won't pick up changes in todo.title / todo.done
    //        since it isn't an observer.
    return <GridRow onRender={() => <td>{todo.title}</td>} />

    // CORRECT: wrap the callback rendering in Observer to be able to detect changes.
    return <GridRow onRender={() => <Observer>{() => <td>{todo.title}</td>}</Observer>} />
})
```