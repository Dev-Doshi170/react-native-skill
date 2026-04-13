# Minimize State Usage

## What It Is
Reducing the amount of local state and avoiding storing data in state that can be derived from other state/props or stored in `useRef`.

## Why It Matters
Every state update triggers a re-render of the component and its children. Excessive or duplicated state leads to "cascading re-renders," causing sluggish UI, input lag, and excessive battery drain.

## The Wrong Way

```jsx
// Storing UI values and calculated data in state
const SearchScreen = () => {
  const [searchText, setSearchText] = useState('');
  const [results, setResults] = useState([]);
  const [resultCount, setResultCount] = useState(0); // DERIVED STATE
  const [isTyping, setIsTyping] = useState(false); // UI STATE

  useEffect(() => {
    setResultCount(results.length);
  }, [results]);

  const handleScroll = (e) => {
    // Storing scroll position in state just to check if at top
    setScrollY(e.nativeEvent.contentOffset.y); 
  };
  
  // ...
};
```

**What's wrong here:**
1. `resultCount` is derived from `results.length` and doesn't need its own state. 
2. Updating `scrollY` in state during every scroll event (60+ times per second) forces the entire screen to re-render constantly.

## The Right Way

```jsx
const SearchScreen = () => {
  const [searchText, setSearchText] = useState('');
  const [results, setResults] = useState([]);
  
  // Derived value - computed during render
  const resultCount = results.length;
  
  // Use a ref for values that don't need to trigger re-renders
  const scrollY = useRef(0);
  const handleScroll = (e) => {
    scrollY.current = e.nativeEvent.contentOffset.y;
  };

  // For complex derived data, use useMemo
  const filteredResults = useMemo(() => 
    results.filter(r => r.name.includes(searchText)), 
    [results, searchText]
  );

  return (
    // ...
  );
};
```

**What changed:**
1. `resultCount` is now a simple variable calculated during render.
2. `scrollY` is stored in a `useRef`, allowing you to track position without triggering re-renders.
3. `filteredResults` uses `useMemo` to skip expensive filtering logic unless dependencies actually change.

## Additional Context
Only use `useState` if the value is directly rendered in the UI or needed for a `useEffect` dependency that *must* trigger a re-render. If you just need to "remember" a value across renders, use `useRef`.

## Related Rules
- `state-ground-truth`: Essential for avoiding duplicated state.
- `list-performance-virtualize`: Crucial when state updates affect large lists.
