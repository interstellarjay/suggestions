# Conventions Discussion


## Boilerplate
### Discussion: Boilerplate for templates
#### Markdown file in the project root?
To help remove choices of how to do things, a file named `boilerplate.md` in the root of the project with the standard way of doing things. 

With this file, we can copy and paste code as a template for:
  + Functional components
  + Unit tests
  + Redux (actions, reducers, selectors)
  + Styled components with Storybook stories
  + Variables
  + Services
  + Classes 


A template in the project root encourages developers to help each other by adding or improving templates when they have solved a problem. This speeds up production and helps ensure high quality of code for everyone.

--- 
## Code Conventions
### Discussion: Code style
#### American English vs British English?
+ American English is preferred
  + `initialize` or `init`
  + `optimize`
  + `randomize`
#### Functions vs Classes?
+ Functions not classes.
#### Quotation marks?
+ Suggest use of `'`
#### Variables?
+ `let` and `const` variables defined with `camelCase`.
+ `const` return values variables defined with `SNAKE_CASE`.
#### Functions?
+ Pure functions defined with `function myFunction (param:paramType)`.
+ Async functions defined with `async function (param:paramType)`.
+ To help other coders understand the logic of the app, declare a const when returning `true`, `false` or an `number` from a function.

```typescript
const USER_ACCEPTED = true;
const USER_REJECTED = false;

function isUserAuthorised (user:IUser) {
    if (user.validated === true){
        return USER_ACCEPTED;
    }
    return USER_REJECTED;
}
```
#### Operators?
+ Use strict evaluation `===` and `!==`.
+ Don't use `>=` or `<=` it is never needed, recommend `>`, `<`, or `===`.
+ Don't use `else`, it is never needed `if (...)` is ok.
+ Don't use of ternary operator `a == b ? x : y` as it does not explicitly explain the context. Instead use `if (...)` statements.

```tsx
if (LOADING) {
    return (
        <div>Loading...</div>
    )
}
return (
    <div>
        <PageContent />
    </div>
)
```
---
## Styled Components
### Discussion: Styled components with [Storybook](https://auth0-cosmos.now.sh/sandbox/)?
Sometimes it can become difficult to understand all the working parts of an app. Building a visual component library can help us create ready-made isolated blocks of functionality that can be easily reused.

Storybook helps developers focus on building everything as shareable components. This speeds up the approval of component QA and helps developers and designers work together. 

A story file is written for each component, this creates a library of built components privately accessible via the browser. This helps showcase the props and functionality of what each component does.

```javascript
import React from 'react';
import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';

// Import the component
import MyComponent from './MyComponent/MyComponent';

// Widget Component
storiesOf('Widget', module)
    .add('Default props', () => (
        <Widget
            title='Contact Us'
            onClick={action('clicked')}
        />
    )
    .add('Without a title', () => (
        <Widget
            onClick={action('clicked')}
        />
    )
```

This helps frontend developers create smaller, isolated commits of high quality pieces of code that can be tested by everyone.

### Discussion: Extending Luna?
A story file is written for each component, this creates a library of built components privately accessible via the browser. This helps showcase the props and functionality of what each component does.


This helps frontend developers create smaller, isolated commits of high quality pieces of code that can be tested by everyone.

---
## React, Redux & TypeScript 
### Discussion: Prop types defined with `interface`?
The interface syntax is much shorter than Prop Types syntax and helps promote good use of TypeScript. 

Using interfaces helps to avoid rewriting prop types definitions again and again. In the example below, we can always use the `IAddress` when we want one of our props to be an adress. 

```ts
/* Define or import complex prop types as interfaces */
interface IAddress {
    buildingname?: string; // optional field
    buildingnumber: string;
    street: string;
    town: string;
    county: string;
    country: string;
    postcode: string;
}
/* Define our component's props */
interface IUserProps {
    address: IAddress;
    age: number;
    name: string;
}
/* Our component */
const Profile:React.SFC<IPerson> {
    return (
        <div>
            ...
        </div>
    )
}
```

### Discussion: Redux hooks pattern? 
Using the hooks provided by the [react-redux](https://react-redux.js.org/next/api/hooks) bindings we can handle global state simply.
+ `useSelector` removes the need for container components and `mapStateToProps` boilerplate
+ `useDispatch` removes the need for container components and `mapDispatchToProps` boilerplate

In `redux/actions/Actions.ts` :
```ts
const INCREMENT_COUNTER = 'INCREMENT_COUNTER';
const DECREMENT_COUNTER = 'DECREMENT_COUNTER';
const RESET_COUNTER     = 'RESET_COUNTER';

export function incrementCounter(payload:number) {
    return {
        type: INCREMENT_COUNTER,
        payload
    }
}
export function decrementCounter(payload:number) {
    return {
        type: DECREMENT_COUNTER,
        payload
    }
}
export function resetCounter(payload:number) {
    return {
        type: RESET_COUNTER,
        payload
    }
}
```

In `components/MyComponent/MyComponent.tsx`:
```tsx
import * from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { incrementCounter } from '../redux/actions/Actions';

export const CounterComponent = (props) => {
    const dispatch  = useDispatch(); 
    const count     = useSelector(state => state.count);
    return (
        <div>
            <button onClick={() => incrementCounter(payload)}>
                {'Counter value is: ' + count}
            </button>
        </div>
    )
}
```

### Discussion: Only optimising when bottlenecked?
React is extremely fast, and making use of simple structured pages, pagination and not overfetching will enable the performance to remain high. However if performance *were sing the `{ useCallback }` function we can optimise performise with [memoisation](https://medium.com/@sdolidze/react-hooks-memoization-99a9a91c8853)

To optimise rendering of child components we can memoize functions  following patern is required for 
```ts
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
