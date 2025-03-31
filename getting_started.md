---
id: getting-started
title: Getting Started with TanStack Query
---

# Getting Started with TanStack Query

TanStack Query (formerly known as React Query) is a powerful library for managing server state in your React applications. This guide will walk you through the process of setting up TanStack Query and using its core features.

## Installation

To get started with TanStack Query, you need to install it in your project. You can do this using npm, yarn, pnpm, or bun:

```bash
npm install @tanstack/react-query
# or
yarn add @tanstack/react-query
# or
pnpm add @tanstack/react-query
# or
bun add @tanstack/react-query
```

TanStack Query is compatible with React v18+ and works with both ReactDOM and React Native.

## Basic Setup

To use TanStack Query in your React application, you need to set up the `QueryClient` and wrap your app with the `QueryClientProvider`. Here's a basic setup:

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

// Create a client
const queryClient = new QueryClient()

function App() {
  return (
    // Provide the client to your App
    <QueryClientProvider client={queryClient}>
      {/* Your app components */}
    </QueryClientProvider>
  )
}
```

## Using Queries

Queries are at the core of TanStack Query. They allow you to fetch and cache data from your server. Here's a simple example of how to use a query:

```jsx
import { useQuery } from '@tanstack/react-query'

function TodoList() {
  const { isPending, error, data } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
  })

  if (isPending) return 'Loading...'
  if (error) return 'An error has occurred: ' + error.message

  return (
    <ul>
      {data.map(todo => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}
```

In this example, `fetchTodos` is a function that returns a promise resolving to your todo data. The `queryKey` is used for caching and invalidation.

## Using Mutations

Mutations are used to create, update, or delete data on the server. Here's how you can use a mutation:

```jsx
import { useMutation, useQueryClient } from '@tanstack/react-query'

function AddTodo() {
  const queryClient = useQueryClient()

  const mutation = useMutation({
    mutationFn: newTodo => {
      return fetch('/api/todos', {
        method: 'POST',
        body: JSON.stringify(newTodo),
      })
    },
    onSuccess: () => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['todos'] })
    },
  })

  return (
    <form onSubmit={(e) => {
      e.preventDefault()
      mutation.mutate({ title: 'New Todo' })
    }}>
      <button type="submit">Add Todo</button>
    </form>
  )
}
```

This example shows how to create a new todo item and then invalidate the 'todos' query to refetch the updated list.

## Query Invalidation

Query invalidation is a powerful feature that allows you to mark certain queries as stale and potentially refetch them. Here's a simple example:

```jsx
const queryClient = useQueryClient()

// Invalidate every query in the cache
queryClient.invalidateQueries()

// Invalidate every query with a key that starts with `todos`
queryClient.invalidateQueries({ queryKey: ['todos'] })
```

## Next Steps

This guide covers the basics of TanStack Query. To dive deeper, explore the following topics:

1. [Query Keys and Query Functions](./guides/queries.md)
2. [Query Options](./reference/useQuery.md)
3. [Mutation Options](./reference/useMutation.md)
4. [Query Invalidation Strategies](./guides/query-invalidation.md)
5. [Caching and Stale-While-Revalidate](./guides/caching.md)

For more advanced usage and best practices, check out the [official TanStack Query documentation](https://tanstack.com/query/latest).

Remember to install the recommended ESLint plugin for TanStack Query to catch potential bugs and inconsistencies:

```bash
npm install -D @tanstack/eslint-plugin-query
```

Happy querying!