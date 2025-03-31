---
id: query-essentials
title: Query Essentials
---

# Query Essentials

TanStack Query provides a powerful and flexible way to manage data fetching and state in your React applications. This guide covers the essential concepts of queries, including how to define them, handle loading and error states, and configure basic options.

## Defining a Query

To create a query, you use the `useQuery` hook. Here's a basic example:

```typescript
import { useQuery } from '@tanstack/react-query';

function MyComponent() {
  const { data, isLoading, error } = useQuery({
    queryKey: ['myData'],
    queryFn: fetchMyData,
  });

  if (isLoading) return <div>Loading...</div>;
  if (error) return <div>An error occurred: {error.message}</div>;

  return <div>{data}</div>;
}
```

The `useQuery` hook accepts an options object with two main properties:

- `queryKey`: A unique identifier for the query. It can be a string or an array.
- `queryFn`: An async function that fetches the data.

## Handling Query States

TanStack Query provides several properties to help you manage the state of your query:

- `isLoading`: `true` if the query is in a loading state
- `error`: Contains error information if the query encountered an error
- `data`: The successfully fetched data

## Basic Configuration Options

You can customize your query's behavior using various options:

```typescript
const { data } = useQuery({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId),
  enabled: !!userId,
  retry: 3,
  staleTime: 5 * 60 * 1000, // 5 minutes
  cacheTime: 10 * 60 * 1000, // 10 minutes
});
```

- `enabled`: Determines if the query should run. Useful for dependent queries.
- `retry`: Specifies the number of times to retry the query if it fails.
- `staleTime`: The duration until the data is considered stale and needs refetching.
- `cacheTime`: How long the data should remain in the cache when unused.

## TypeScript Support

TanStack Query provides excellent TypeScript support. You can specify the types for your query data and error:

```typescript
interface User {
  id: number;
  name: string;
}

const { data } = useQuery<User, Error>({
  queryKey: ['user', userId],
  queryFn: () => fetchUser(userId),
});
```

## Conclusion

This guide covers the essentials of using queries in TanStack Query. By understanding these concepts, you can effectively manage data fetching in your React applications. For more advanced usage and configurations, refer to the other sections of the documentation.