# Query Invalidation

Query invalidation is a powerful feature in TanStack Query that allows you to mark queries as stale and potentially trigger refetches. This process is crucial for keeping your application's data fresh and consistent with the server state.

## Understanding Query Invalidation

When you invalidate a query, you're essentially telling TanStack Query that the data for that query is no longer up-to-date. This doesn't immediately trigger a refetch, but rather marks the query as stale. The next time the query is used, TanStack Query will know to refetch the data.

## How to Invalidate Queries

TanStack Query provides the `invalidateQueries` method on the `QueryClient` instance for invalidating queries. Here's a basic example:

```typescript
const queryClient = new QueryClient();

// Invalidate every query in the cache
queryClient.invalidateQueries();

// Invalidate every query with a key that starts with `todos`
queryClient.invalidateQueries({ queryKey: ['todos'] });
```

## When to Invalidate Queries

You should invalidate queries whenever you know that the server-side data may have changed. Common scenarios include:

1. After performing a mutation (e.g., creating, updating, or deleting data)
2. When receiving real-time updates from a WebSocket
3. When the user performs an action that might affect the data (e.g., clicking a refresh button)

## Best Practices for Query Invalidation

1. **Be specific**: Instead of invalidating all queries, try to invalidate only the queries that are affected by the change.

2. **Use query keys effectively**: Structure your query keys in a way that allows for easy and precise invalidation.

3. **Combine with optimistic updates**: For a better user experience, combine query invalidation with optimistic updates.

4. **Consider refetch behavior**: Remember that invalidation doesn't immediately trigger a refetch. Configure your queries' refetch behavior appropriately.

5. **Use in mutation callbacks**: Invalidate related queries in the `onSuccess` callback of your mutations.

Here's an example demonstrating some of these best practices:

```typescript
const queryClient = new QueryClient();

// Mutation with query invalidation
const mutation = useMutation({
  mutationFn: updateTodo,
  onSuccess: (data, variables) => {
    // Invalidate and refetch
    queryClient.invalidateQueries({ queryKey: ['todos'] });
    
    // Or be more specific
    queryClient.invalidateQueries({ queryKey: ['todos', variables.id] });
  },
});
```

## Advanced Query Invalidation

TanStack Query also provides more advanced invalidation options:

1. **Selective refetching**: You can specify which queries to refetch after invalidation.

```typescript
queryClient.invalidateQueries(
  { queryKey: ['todos'] },
  { refetchType: 'active' }
);
```

2. **Cancelling in-flight queries**: You can cancel ongoing queries before invalidation.

```typescript
await queryClient.cancelQueries({ queryKey: ['todos'] });
queryClient.invalidateQueries({ queryKey: ['todos'] });
```

3. **Using predicates**: You can use a predicate function for more complex invalidation logic.

```typescript
queryClient.invalidateQueries({
  predicate: (query) => query.queryKey[0] === 'todos' && query.queryKey[1] >= 10,
});
```

By mastering query invalidation, you can ensure that your application always displays the most up-to-date data while minimizing unnecessary network requests.