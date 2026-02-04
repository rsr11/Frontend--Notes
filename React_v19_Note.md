# React v19 Notes 
React is very amazing framework for frontend and very easy to use but, there was always a problem with it, the SEO, that alone created many problem for developers, without a SEO that a website without marketing power.



## hooks 
- useForm
- useAction
- useTransition \
  A common use case in React apps is to perform a data mutation and then update state in response. For example, when a user submits a form to change their name, you will make an API request, and then handle the response. In the past, you would need to handle pending states, errors, optimistic updates, and sequential requests manually.
```
  function UpdateName({}) {
  const [name, setName] = useState("");
  const [error, setError] = useState(null);
  const [isPending, startTransition] = useTransition();

  const handleSubmit = () => {
    startTransition(async () => {
      const error = await updateName(name);
      if (error) {
        setError(error);
        return;
      } 
      redirect("/path");
    })
  };

  return (
    <div>
      <input value={name} onChange={(event) => setName(event.target.value)} />
      <button onClick={handleSubmit} disabled={isPending}>
        Update
      </button>
      {error && <p>{error}</p>}
    </div>
  );
}
```
<br>

## there is note in this buddy
the asyn function that uses useTransition are called action.
Actions automatically manage submitting data for you:
- Pending state: Actions provide a pending state that starts at the beginning of a request and automatically resets when the final state update is committed.
- Optimistic updates: Actions support the new useOptimistic hook so you can show users instant feedback while the requests are submitting.
- Error handling: Actions provide error handling so you can display Error Boundaries when a request fails, and revert optimistic updates to their original value automatically.
- Forms: <form> elements now support passing functions to the action and formAction props. Passing functions to the action props use Actions by default and reset the form automatically after submission.

## Newly Introduce features 
- Concurrent rendering \
Concurrent rendering is an enhancement in React that allows React to work on multiple tasks at once, improving performance and user experience. It allows React to prioritize important updates without blocking the main thread, ensuring that user interactions like typing or clicking happen smoothly.

``` import React, { useState, Suspense } from 'react';

const SlowComponent = React.lazy(() => import('./SlowComponent'));

function App() {
    const [showComponent, setShowComponent] = useState(false);

    return (
        <div>
            <button onClick={() => setShowComponent(!showComponent)}>Toggle Component</button>
            <Suspense fallback={<div>Loading...</div>}>
                {showComponent && <SlowComponent />}
            </Suspense>
        </div>
    );
}

export default App;
```

<br>

- Automatic batching \
Automatic batching groups multiple state updates into a single render cycle, improving performance by reducing the number of renders. This means that if multiple state updates happen simultaneously (such as in different event handlers), React will batch them together into one render, preventing unnecessary renders.
```
import React, { useState } from 'react';

function App() {
    const [count, setCount] = useState(0);
    const [text, setText] = useState('');

    const handleClick = () => {
        setCount(count + 1);
        setText('React');
    };

    return (
        <div>
            <button onClick={handleClick}>Update</button>
            <div>Count: {count}</div>
            <div>Text: {text}</div>
        </div>
    );
}

export default App;
```
<br>

- Suspence enhancement \
React 19 enhances the Suspense API to improve handling asynchronous data fetching and server-side rendering (SSR). Suspense helps React manage loading states by allowing components to "wait" for data to load and display fallback UI while the content is being fetched.

```
import React, { Suspense } from 'react';

const fetchData = () => new Promise(resolve => setTimeout(() => resolve('Fetched Data'), 2000));
const DataComponent = React.lazy(() => fetchData().then(() => import('./DataComponent')));

function App() {
    return (
        <div>
            <Suspense fallback={<div>Loading...</div>}>
                <DataComponent />
            </Suspense>
        </div>
    );
}

export default App;
```
<br>

- Error boundary \
Error Boundaries in React 19 have been enhanced to handle errors more gracefully, especially when working with Server Components and Suspense. React now provides better error messaging and allows us to catch errors within the component tree and display fallback UI instead of crashing the app.
```
import React, { Component } from 'react';

class ErrorBoundary extends Component {
    state = { hasError: false };

    static getDerivedStateFromError() {
        return { hasError: true };
    }

    componentDidCatch(error, info) {
        console.log(error, info);
    }

    render() {
        if (this.state.hasError) {
            return <h1>Something went wrong.</h1>;
        }

        return this.props.children;
    }
}

function BuggyComponent() {
    throw new Error('Oops, something went wrong!');
    return <div>Good Component</div>;
}

function App() {
    return (
        <ErrorBoundary>
            <BuggyComponent />
        </ErrorBoundary>
    );
}

export default App;
```
<br>

- React compiler \
React 19 introduces the React Compiler, a tool that significantly optimizes the compilation process of JSX and JavaScript. This improvement helps speed up the build process, reduces runtime overhead, and improves the performance of React applications.

