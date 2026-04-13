# Single Source of Truth

## What It Is
Ensuring every piece of data in your application has exactly one source of truth, avoiding the duplication of props into local state.

## Why It Matters
Duplicating state ("syncing" props to state) is a major source of bugs. It often leads to "stale UI" where one version of the data updates but the other doesn't, resulting in data inconsistency and difficult-to-track state conflicts.

## The Wrong Way

```jsx
// Syncing props to state creates two sources of truth
const UserProfile = ({ user }) => {
  const [name, setName] = useState(user.name); // WRONG: Prop duplicated in state
  const [email, setEmail] = useState(user.email); // WRONG: Prop duplicated in state

  useEffect(() => {
    // Boilerplate to keep state in sync - error prone!
    setName(user.name);
  }, [user.name]);

  return (
    <View>
      <TextInput value={name} onChangeText={setName} />
    </View>
  );
};
```

**What's wrong here:**
The `user` object from the parent is the source of truth, but `UserProfile` creates its own local copy. If the parent's `user` updates, the child might lag, or if the user submits the form, they might be submitting a "stale" version of the state.

## The Right Way

```jsx
// Single source of truth with controlled component or key resetting
const UserProfile = ({ user, onUpdate }) => {
  // Option A: Lift state up (recommended)
  // Parent manages the user object, child just calls onUpdate

  // Option B: If local editing is needed, reset local state when ID changes
  return (
    <UserForm key={user.id} initialUser={user} onUpdate={onUpdate} />
  );
};

const UserForm = ({ initialUser, onUpdate }) => {
  const [user, setUser] = useState(initialUser);

  const handleSubmit = () => {
    onUpdate(user);
  };
  
  // ... local edits happen here
};
```

**What changed:**
1. Use the `key` prop on the child component. When the `user.id` changes, React will completely unmount and re-mount the child, resetting its state to the new `initialUser` without any `useEffect` syncing.
2. In many cases, it's better to "Lift State Up" and have the child be a purely controlled component using props from the parent.

## Additional Context
If you find yourself using `useEffect` solely to sync a prop to a local state, you are violating the single source of truth principle. Use the `key` resetting pattern instead for a cleaner, more predictable UI.

## Related Rules
- `react-state-minimize`: Both focus on keeping state management simple and efficient.
- `navigation-native-navigators`: Shared state across screens often relies on stable navigation parameters.
