- [React guidelines and best practices](#react-guidelines-and-best-practices)
  - [Naming and file conventions](#naming-and-file-conventions)
    - [Components should use `.jsx` or `.tsx` file extension](#components-should-use-jsx-or-tsx-file-extension)
    - [Custom hooks should use `.js` or `.ts` file extension](#custom-hooks-should-use-js-or-ts-file-extension)
    - [Component file names and component Names Should Use PascalCase](#component-file-names-and-component-names-should-use-pascalcase)
    - [Custom hooks files names and hook names should use camelCase](#component-names-and-custom-hooks-should-be-the-same-as-the-file-names)
    - [Component names and custom hooks should be the same as the file names](#component-names-and-custom-hooks-should-be-the-same-as-the-file-names)
    - [Component prop names should use camelcase](#component-prop-names-should-use-camelcase)
    - [Component variables, handlers, `usestate` state and setters should use camelCase](#component-variables-handlers-usestate-state-and-setters-should-use-camelcase)
  - [Code Standards](#code-standards)
    - [Single component or hook per file](#single-component-or-hook-per-file)
    - [Prefer functional to class components](#prefer-functional-to-class-components)
    - [Export components using named export](#export-components-using-named-export)
    - [Always destructure props](#always-destructure-props)
    - [Avoid using DOM or predefined component prop names for different purposes](#avoid-using-dom-or-predefined-component-prop-names-for-different-purposes)
    - [Favor small, single-purpose components](#favor-small-single-purpose-components)
    - [Separate business logic from UI](#separate-business-logic-from-ui)
    - [Omit the value of the prop when it is explicitly `true`](#omit-the-value-of-the-prop-when-it-is-explicitly-true)
    - [Always use double quotes (") for JSX attributes](#always-use-double-quotes--for-jsx-attributes)
    - [Self-close components without children](#self-close-components-without-children)
    - [Don't extract components without purpose](#dont-extract-components-without-a-purpose)
    - [Prefer the logical `&&` operator for conditional rendering, but make sure the left operand is always boolean](#prefer-the-logical--operator-for-conditional-rendering-but-make-sure-the-left-operand-is-always-boolean)
    - [Avoid nested ternary operators](#avoid-nested-ternary-operators)
    - [Extract repetitive markup](#extract-repetitive-markup)
    - [Pass objects instead of primitives](#pass-objects-instead-of-primitives)
    - [Avoid spreading props](#avoid-spreading-props)
    - [Favor multiple smaller `useeffect` calls to a single big one](#favor-multiple-smaller-useeffect-calls-to-a-single-big-one)
    - [Follow the "Rules of Hooks"](#follow-the-rules-of-hooks)
    - [Pass all hook dependencies](#pass-all-hook-dependencies)
  - [React with Typescript](#react-with-typescript)
    - [Use the component name as a prefix followed by `Props` for all component interfaces](#use-the-component-name-as-a-prefix-followed-by-props-for-all-component-interfaces)
    - [Keep component props interface in the component file](#keep-component-props-interface-in-the-component-file)
  - [Testing](#testing)
    - [Follow the `Arrange Act Assert` test pattern](#follow-the-arrange-act-assert-test-pattern)
    - [Don't rely on snapshot tests](#dont-rely-on-snapshot-tests)

# React guidelines and best practices

## Naming and file conventions

### Components should use `.jsx` or `.tsx` file extension

```jsx
// ❌
UserCard.js, UserProfile.ts

// ✅ 
UserCard.jsx, UserProfile.tsx
```

> This preference can be enforced with an existing ESLint rule in your project: [react/jsx-filename-extension](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-filename-extension.md)

### Custom Hooks should use `.js` or `.ts` file extension

```jsx
// ❌
useUserData.jsx, useFetch.tsx

// ✅ 
useUserData.js, useFetch.ts
```

### Component file names and component names should use pascalCase

```jsx
// ❌ Wrong use of camelCase and snake_case
profilePicture.jsx, Profile_Picture.tsx

// ✅ Using PascalCase
ProfilePicture.jsx, ProfilePicture.tsx
```

### Custom hooks files names and hook names should use camelCase

```jsx
// ❌ Wrong use of PascalCase and snake_case
UseUserData.js, use_user_data.js


// ✅ Using camelCase
useUserData.js, useLocalStorage.ts
```

### Component names and custom hooks should be the same as the file names

That way it becomes immediately clear to anyone looking at the file structure what components they can expect to find in which files.  It improves searchability within the codebase.
Promotes consistency and aids in debugging, as stack traces will reference the component name, which, if it matches the filename, makes tracing the source of errors more straightforward.

```jsx
// ❌ Avoid using different names for file and component/custom hook names

// UserProfile.jsx
const User = ({ user }) => {
  ...
}

// ✅ Use the same file and component names

// UserProfile.jsx
const UserProfile = ({ user }) => {
...
}
```

### Component prop names should use camelCase

```jsx
// ❌ Avoid using PascalCase or UPPERCASE
<UserProfile
  Name={user.name}
  AVATAR={user.avatar}
  has_connections={hasConnections}
/>

// ✅ Use camelCase
<UserProfile
  name={user.name}
  avatar={user.avatar}
  hasConnections={hasConnections}
/>
```

> This preference can be enforced with an existing ESLint rule in your project using the [camelcase](https://eslint.org/docs/latest/rules/camelcase) rule.

### Component variables, handlers, `usestate` state and setters should use camelCase

```jsx
// ❌ Avoid using PascalCase or UPPERCASE
const UserProfile = ({ user }) => {
  const [IsActive, SetIsActive] = useState(null);
  const AVATAR = user.avatar;
  
  const HandleUserActivation = () => {...}
  
  return (
    ...
  ) 
}

// ✅ Use camelCase
const UserProfile = ({ user }) => {
  const [isActive, setIsActive] = useState(null);
  const avatar = user.avatar;

  const handleUserActivation = () => {...}

  return (
    ...
  )
}
```

## Code standards

### Single component or hook per file

Following the rule of a single component or hook per file simplifies understanding, testing, and maintaining the code by isolating individual components or hooks, making them easier to manage.
That way, each file's purpose is clear and truly modular.

```jsx
// ❌ Multiple components in a single file

// Layout.jsx
import React from 'react';

const Header = () => <header>Header Content</header>;

const Footer = () => <footer>Footer Content</footer>;

export const Layout = ({ children }) => (
  <>
    <Header />
    {children}
    <Footer />
  </>
)

// ✅ Every component is in a separate file

// Header.jsx
export const Header = () => {
  return <header>Header Content</header>;
};

// Footer.jsx
export const Footer = () => {
  return <footer>Footer Content</footer>;
};

// Layout.jsx
export const Layout = ({ children }) => (
  <>
    <Header />
    {children}
    <Footer />
  </>
)
```

### Prefer functional to class components

Functional components are favored over class components due to their simplicity and the introduction of Hooks, which allow for using state and lifecycle features without classes.
They often result in cleaner, less verbose code and have advantages in performance due to reduced memory usage and better optimization potential.

```jsx
// ❌ Verbose class component
class Welcome extends React.Component {
  state = { name: 'John' };

  handleChange = (event) => {
    this.setState({ name: event.target.value });
  }

  render() {
    return (
      <div>
        <h1>Hello, {this.state.name}</h1>
        <input type="text" value={this.state.name} onChange={this.handleChange} />
      </div>
    );
  }
}

// ✅ Simple and clean functional component
const Welcome = () => {
  const [name, setName] = useState('John');

  return (
    <div>
      <h1>Hello, {name}</h1>
      <input type="text" value={name} onChange={e => setName(e.target.value)} />
    </div>
  );
}
```

> This preference can be enforced using the following ESLint plugin: [eslint-plugin-react-prefer-function-component](https://github.com/tatethurston/eslint-plugin-react-prefer-function-component)

### Export components using named export

Using named exports allows for explicit and consistent importing of components, which can make the code easier to follow. It also facilitates tree-shaking in modern bundlers, allowing for an efficient final bundle by potentially eliminating unused code.

```jsx
// ❌ Inconsistent component name

// UserProfile.js
const UserProfile = () => {
  // component code
}

export default UserProfile;

// In another file
import User from './UserProfile';

// ✅ Consistent name throughout the application

// UserProfile.js
export const UserProfile = () => {
  // component code
}

// In another file
import { UserProfile } from './UserProfile';

```

> This preference can be enforced using the [eslint-plugin-import](https://github.com/import-js/eslint-plugin-import) ESLint plugin and the [import/no-default-export](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-default-export.md) rule.

### Always destructure props

Destructuring props in components enhances code readability by making it clear which properties are being used, reduces repetition by eliminating the need for the `props.` prefix, and simplifies the assignment of default values.
It allows for a neater and more intuitive way to extract multiple properties from the props object passed to a function. Additionally, destructuring can make the code cleaner and more maintainable by providing a clear view of the component's API surface at a glance.

```jsx
// ❌ Repetitive props and verbose assignment of default values
const Greeting = (props) => {
  const name = props.name || 'User';
  const role = props.role || 'Guest';

  return <h1>Hello, {name}! Your role is {role}.</h1>;
}

// ✅ Easy to grasp props with clean assignment of default values 
const Greeting = ({ name = 'User', role = 'Guest' }) => {
  return <h1>Hello, {name}! Your role is {role}.</h1>;
}

```

> This preference can be enforced with an existing ESLint rule in your project: [react/destructuring-assignment](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/destructuring-assignment.md)

### Avoid using dom or predefined component prop names for different purposes

People expect props like style and className to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

```jsx
// ❌ Avoid using DOM component prop names
<Header style="fancy" />
<Header className="fancy" />

// ✅ Use custom prop names
<MyComponent variant="fancy" />
```

### Favor small, single-purpose components

Writing small, single-purpose components enhances modularity, making the codebase easier to maintain and scale as it grows. It also improves reusability, allowing developers to mix and match components like building blocks to create new features with less code.
Small components facilitate easier testing and debugging since each component's functionality can be isolated and verified independently.

```jsx
// ❌ Large, multi-purpose component
const UserProfile = props => {
  // ... lots of other logic ...

  const handleFriendRequest = () => {
    // Friend request logic
  };

  return (
    <div>
      <h1>User Profile</h1>
      <img src={props.user.imageUrl} alt="user profile" />
      <p>Name: {props.user.name}</p>
      <p>Email: {props.user.email}</p>
      {/* ... lots more user details ... */}
      <button onClick={handleFriendRequest}>Add Friend</button>
      {/* ... maybe even more unrelated functionality */}
    </div>
  );
};

// ✅ Small, single-purpose components

// UseProfile.jsx
const UserProfile = ({ user }) => (
  <div>
    <h1>User Profile</h1>
    <ProfileImage imageUrl={user.imageUrl} />
    <ProfileDetails name={user.name} email={user.email} />
    <AddFriendButton onAddFriend={() => console.log('Add friend clicked')} />
  </div>
);

// ProfileImage.jsx
const ProfileImage = ({ imageUrl }) => <img src={imageUrl} alt="user profile" />;

// ProfileDetails.jsx
const ProfileDetails = ({ name, email }) => (
  <div>
    <p>Name: {name}</p>
    <p>Email: {email}</p>
  </div>
);

// AddFriendButton.jsx
const AddFriendButton = ({ onAddFriend }) => (
  <button onClick={onAddFriend}>Add Friend</button>
);
```

### Separate business logic from ui

React components often encapsulate both UI and business logic, which is appropriate for UI-related behavior but can lead to issues when handling domain-specific operations like API calls and user input validation.
To manage these operations more efficiently and keep components clean, it's beneficial to extract such logic into custom hooks. Custom hooks integrate smoothly with a component's lifecycle and help in keeping component code focused on the UI by abstracting complex operations.
They also enable components to receive only the necessary data in the needed format, streamlining the interaction with other hooks and external APIs.

```jsx
// ❌ Logic mixed with component
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Mixing data fetching logic with component logic
    fetch(`https://api.example.com/users/${userId}`)
      .then((response) => response.json())
      .then((data) => setUser(data))
      .catch((error) => console.error(error));
  }, [userId]);

  // ... UI rendering with user data ...

  return user ? <div>{user.name}</div> : <div>Loading...</div>;
}


// ✅ Logic separated from the UI using custom hook

// useUserData.jsx
const useUserData = (userId) => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Extracted data fetching logic
    fetch(`https://api.example.com/users/${userId}`)
      .then((response) => response.json())
      .then((data) => {
        setUser(data)
        setLoading()
      })
      .catch((error) => console.error(error));
  }, [userId]);

  return user;
}

// UserProfile.jsx
const UserProfile = ({ userId }) => {
  const user = useUserData(userId);

  // ... UI rendering with user data ...

  return user ? <div>{user.name}</div> : <div>Loading...</div>;
}

```

### Omit the value of the prop when it is explicitly `true`

Omitting the value of a prop when it's true simplifies the JSX markup, making it more concise, as the mere presence of the prop name indicates a true value. This shorthand mirrors the convention in HTML where boolean attributes can be written without a value.
It also helps in reducing boilerplate code, allowing developers to write cleaner and more declarative components.

```jsx
// ❌ Verbose way of setting the prop value to true
<Video autoplay={true} />

// ✅ The more clear shorthand syntax
<Video autoplay />
```

> This preference can be enforced with an existing ESLint rule in your project using the [react/jsx-boolean-value](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md) rule.

### Always use double quotes (") for JSX attributes

Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

```jsx
// ❌ Avoid using single quotes
<User name='John' />

// ✅ Use double quotes
<User name="John" />
```

> This preference can be enforced with an existing ESLint rule in your project using the [@stylistic](https://eslint.style) plugin and the [@stylistic/js/jsx-quotes](https://eslint.style/rules/js/jsx-quotes) rule.
> If you use ESLint v8.52.0 or lower, use [jsx-quotes](https://eslint.org/docs/latest/rules/jsx-quotes)

### Self-close components without children

Self-closing components without children in JSX lead to cleaner code.
It also aligns with HTML syntax for void elements, offering a visual cue for the component's role as a standalone entity without the need for additional closing tags.

```jsx
// ❌ Avoid using non-self-closing component syntax
<UserProfile user={user}></UserProfile>

// ✅ Self-close components without children
<UserProfile user={user} />
```

> This preference can be enforced with an existing ESLint rule in your project using the [react/self-closing-comp](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md) rule.

### Don't extract components without a purpose

Extracting components without a clear purpose can lead to unnecessary abstraction, making the codebase more difficult to navigate and understand due to excessive componentization.
It can also negatively impact performance if the overhead of component creation doesn't provide a tangible benefit like reusability or separation of concerns.

```jsx

// ❌ Unnecessary component extraction without a clear purpose
const UserName = ({ name }) => <span>{name}</span>;

const UserProfile = ({ user }) => (
  <div>
    <h1>User Profile for <UserName name={user.name} /></h1> {/* Unnecessary wrapping */}
    <p>Email: {user.email}</p>
    <p>Location: {user.location}</p>
  </div>
);

// ✅ Direct usage without unnecessary component 
const UserProfile = ({ user }) => (
  <div>
    <h1>User Profile for <span>{user.name}</span></h1> {/* Directly using the span */}
    <p>Email: {user.email}</p>
    <p>Location: {user.location}</p>
  </div>
);
```

### Prefer the logical `&&` operator for conditional rendering, but make sure the left operand is always boolean

Using the logical `&&` operator for conditional rendering allows components to render only if a certain condition is true.
However, ensuring the left operand is boolean is essential, as non-boolean values (like objects or arrays) can be mistakenly rendered if they are truthy, leading to potential bugs or unexpected UI outputs.

```jsx
// ❌ Non-boolean feft operand can lead to unexpected outputs
const UserGreetings = ({ user }) => {
  const userDetails = user && {
    // ...some user details
  };

  return (
    <div>
      {userDetails && <p>Welcome, {user.name}!</p>} {/* This might render the userDetails object if it's truthy */}
      {userDetails && <p>Email: {user.email}</p>}
    </div>
  );
};

// ✅ Make sure the left operand is converted to boolean
const UserGreetings = ({ user }) => {
  const isLoggedIn = !!user; // Convert user existence to a boolean

  return (
    <div>
      {isLoggedIn && <p>Welcome, {user.name}!</p>} {/* Safe conditional rendering */}
      {isLoggedIn && <p>Email: {user.email}</p>}
    </div>
  );
};

```

### Avoid nested ternary operators

Nested ternary operators can make code extremely difficult to read and understand, leading to a maintenance challenge where deciphering the conditional logic becomes as complex as writing it.
Overusing ternary nesting can also lead to errors as it becomes easier to lose track of the conditions and the corresponding outcomes, which undermines the benefits of using this concise syntax for simple conditional rendering or assignment.
In these cases, it's better to separate the conditional logic.

```jsx
// ❌ Nested ternary operator
const WelcomeMessage = ({ user, isLoggedIn }) => (
  <div>
    {isLoggedIn ? user ? <h1>Welcome back, {user.name}!</h1> : <h1>Welcome, guest!</h1> : null}
  </div>
);

// ✅ Separated conditional logic
const WelcomeMessage = ({ user, isLoggedIn }) => {
  let message;

  if (isLoggedIn) {
    message = user ? <h1>Welcome back, {user.name}!</h1> : <h1>Welcome, guest!</h1>;
  } else {
    message = null;
  }

  return <div>{message}</div>;
};
```

> This preference can be enforced with an existing ESLint rule in your project using the [no-nested-ternary](https://eslint.org/docs/latest/rules/no-nested-ternary) rule.

### Extract repetitive markup

Extracting repetitive markup into a separate, reusable component helps to keep the code DRY (Don't Repeat Yourself), which simplifies updates and maintenance. It also enhances the readability of the codebase and can make it easier to manage and identify patterns or errors in the markup.
It's better to create a separate component and if needed, use a configurator object. This way we only define the markup once, and we change it in a single place.

```jsx
// ❌ Repetative markup
const Filters = () => {
  const handleFilterClick = (genre) => {...};

  return (
    <>
      <p>Book Genres</p>
      <ul>
        <li>
          <div onClick={() => handleFilterClick("filterFiction")}>
            Fiction
          </div>
        </li>
        <li>
          <div onClick={() => handleFilterClick("filterClassics")}>
            Classics
          </div>
        </li>
      </ul>
    </>
  );
}

// ✅ Extracted configuration and component
const FilterGenre = ({ genre, onFilterClick }) => (
  <div onClick={() => onFilterClick(genre.identifier)}>
    {genre.name}
  </div>
)

const Filters = () => {
  const handleFilterClick = (genre) => {...};
  
  return (
    <>
      <p>Book Genres</p>
      <ul>
        {GENRES.map((genre) => (
          <li>
            <FilterGenre genre={genre} onFilterClick={handleFilterClick} />
          </li>
        ))}
      </ul>
    </>
  );
}

const GENRES = [
  {
    identifier: "filterFiction",
    name: 'Fiction',
  },
  {
    identifier: "filterClassics",
    name: 'Classics',
  }
]
```

### Pass objects instead of primitives

Passing objects instead of primitives as props can encapsulate multiple related values into a single prop, reducing the overall number of props passed to a component and simplifying the component's API. This can make component usage and prop management more straightforward,
as related values are grouped together, making the data flow easier to trace and understand. Additionally, it can improve component reusability and refactoring by treating prop values as structured data, which can adapt better to changes in the component's implementation or usage.

```jsx
// ❌ Passsing every user value as a separate prop
<UserProfile
  name={user.name}
  email={user.email}
  biography={user.bio}
  avatarUrl={user.avatarUrl}
/>

// ✅ Passing a single prop with all user info
<UserProfile user={user}/>
```

### Avoid spreading props

Spreading props in React can lead to unexpected behavior if components receive props that they shouldn't, such as invalid HTML attributes, wrong or modified handlers and data, making the code harder to debug and potentially causing security vulnerabilities.
It also hides which props are being used, reducing the readability of the component and making it more difficult for future developers to understand which props are required or optional.

```jsx
// ❌ It's unclear what the article props are and open to wrong props passed
<Article category={category} author={author} {...article} />


// ✅ All props are visible
<Article
  author={author}
  category={category}
  title={article.title}
  url={article.url}
  content={article.content}
/>
```

> This preference can be enforced with an existing ESLint rule in your project using the [react/jsx-props-no-spreading](https://github.com/jsx-eslint/eslint-plugin-react/blob/master/docs/rules/jsx-props-no-spreading.md) rule.

### Favor multiple smaller `useeffect` calls to a single big one

Using multiple smaller `useEffect` calls allows you to isolate different side effects that do not depend on each other, leading to better organization and separation of concerns.
It also makes your component logic easier to follow and debug, as each effect is responsible for a specific behavior or set of related behaviors.
Additionally, it can improve performance, as React can optimize re-running effects only when specific related props or state have changed, rather than re-running a larger effect with broader dependencies.

```jsx
// ❌ useEffect with multiple responsibilities
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Fetch user data
    fetchUserById(userId).then(setUser);

    // Set up subscriptions or event listeners
    const handleResize = () => {
      // Handle the resize event
    };
    window.addEventListener('resize', handleResize);

    // Cleanup function for effect
    return () => window.removeEventListener('resize', handleResize);
  }, [userId]); // Runs for both user fetching and resize events, which is not optimal

  // ... component logic ...

  return (
    // ... JSX ...
  );
};

// ✅ separate useEffects with clear responsibilities
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);

  // Effect for fetching user data
  useEffect(() => {
    fetchUserById(userId).then(setUser);
  }, [userId]); // Only re-runs when userId changes

  // Separate effect for setting up subscriptions or event listeners
  useEffect(() => {
    const handleResize = () => {
      // Handle the resize event
    };

    window.addEventListener('resize', handleResize);

    // Cleanup function for this effect
    return () => window.removeEventListener('resize', handleResize);
  }, []); // Empty array means this effect runs once on mount and on unmount

  // ... component logic ...

  return (
    // ... JSX ...
  );
};
```

### Follow the "Rules of Hooks"

The three rules of hooks are:

- Hooks can only be called inside React function components.
- Hooks can only be called at the top level of a component.
- Hooks cannot be conditional.

Following the rules of hooks ensures that components have a stable and predictable execution by allowing React to correctly track and manage hook-related state and effects.

```jsx

// ❌ Conditionally calling the hook
const MyComponent = ({ condition }) => {
  const [name, setName] = useState('');
  
  if (name !== '') {
    useEffect(function persistForm() {
      localStorage.setItem('formData', name);
    });
  }
  
  // rest of component logic
};

// ✅ Following the rules of hooks
const MyComponent = ({ condition }) => {
  const [name, setName] = useState('');

  useEffect(function persistForm() {
    // 👍 We're not breaking the first rule anymore
    if (name !== '') {
      localStorage.setItem('formData', name);
    }
  });

  // rest of component logic
};
```

> This preference can be enforced with an existing ESLint rule in your project using the [eslint-plugin-react-hooks](https://github.com/facebook/react/tree/main/packages/eslint-plugin-react-hooks) plugin and the `react-hooks/rules-of-hooks` rule.

### Pass all hook dependencies

Passing all dependencies to hooks like `useEffect`, `useMemo` and others, is crucial for ensuring that the hook correctly reflects the state and props it depends on, preventing bugs related to stale closures.
It guarantees that the effect runs exactly when it should—after any of its dependencies change—aligning the component's behavior with React's design principles for hooks and state management.

```jsx
// ❌ the useEffect has a missing dependency, which could lead to bugs
const Timer = ({ someProp }) => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      // Missing dependency on 'someProp'
      setCount((c) => c + someProp);
    }, 1000);

    return () => clearInterval(interval);
  }, []); // 'someProp' is not included as a dependency

  return <h1>{count}</h1>;
}

// ✅ All dependencies included, ensuring that the effect reruns 
// and the count state is up to date
const Timer = ({ someProp }) => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      // 'someProp' is now correctly listed as a dependency
      setCount((c) => c + someProp);
    }, 1000);

    return () => clearInterval(interval);
  }, [someProp]); // 'someProp' is included in the dependency array

  return <h1>{count}</h1>;
}
```

> This preference can be enforced with an existing ESLint rule in your project using the [eslint-plugin-react-hooks](https://github.com/facebook/react/tree/main/packages/eslint-plugin-react-hooks) plugin and the `react-hooks/exhaustive-deps` rule.

## React with Typescript

### Use the component name as a prefix followed by `Props` for all component interfaces

```tsx
// ❌ Not clear whether this is a props interface or not
interface User {
  username: string;
  email: string;
  avatar: string;
}

// ✅ The interface is properly named
interface UserProfileProps {
  username: string;
  email: string;
  avatar: string;
}
```

### Keep component props interface in the component file

Keeping a component's props interface within the same file as the component ensures a clear and immediate reference for developers.

```tsx
// ❌ Passing the interface from another file 
import { UserFormProps } from './types'

const UserForm = ({ username, email }: UserFormProps) => {
  // rest of component logic
}

// ✅ The interface next to the component makes it easier to grasp
interface UserFormProps {
  username: string;
  email: string;
}

const UserForm = ({ username, email }: UserFormProps) => {
  // rest of component logic
}
```

> For more general TypeScript guidelines check the [Typescript guidelines and best practices](./typescript.md)

## Testing

### Follow the `Arrange Act Assert` test pattern

The Arrange-Act-Assert (AAA) pattern provides a clear structure for writing test cases. By consistently following this pattern, tests become more understandable and easier to modify, ensuring that each test is focused on a specific action and outcome.

```jsx
// ❌ Test without clear Arrange-Act-Assert structure
test('increments counter', () => {
  render(<Counter />);
  fireEvent.click(screen.getByText('Increment')); // Action without separation
  expect(screen.getByRole('contentinfo')).toHaveTextContent('1'); // Assertion mixed with arragement
});

// ✅ Test with clear Arrange-Act-Assert structure
test('increments counter', () => {
  // Arrange
  const { getByText, getByRole } = render(<Counter />);
  const counterButton = getByText('Increment');
  const contentInfo = getByRole('contentinfo')

  // Act
  fireEvent.click(counterButton);

  // Assert
  expect(contentInfo).toHaveTextContent('1');
});
```

### Don't rely on snapshot tests

Snapshot testing in React, while useful for catching unexpected changes, can often result in wasted time as developers may habitually update snapshots without thoroughly examining the changes.
They are not a substitute for explicit assertions that clarify what is being tested and provide specific information about failures.
Although snapshot tests can act as a good sanity check to indicate affected parts by code changes, they should not replace thorough, intentional tests that validate component behavior.
