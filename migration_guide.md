---
id: migration-guide
title: Migration Guide
---

# Migration Guide: React Query to TanStack Query

This guide will help you migrate from previous versions of React Query to the latest version of TanStack Query. We'll cover key changes, new features, and provide examples to ensure a smooth transition.

## Table of Contents

1. [Renaming and Scope Changes](#renaming-and-scope-changes)
2. [Installation Changes](#installation-changes)
3. [QueryClient Configuration](#queryclient-configuration)
4. [Hook Usage Updates](#hook-usage-updates)
5. [New Features](#new-features)
6. [Breaking Changes](#breaking-changes)

## Renaming and Scope Changes

React Query has been renamed to TanStack Query to reflect its expanded support for multiple frameworks, including React, Solid, Svelte, and Vue. This change allows for a more unified API across different frameworks.

## Installation Changes

To install the latest version of TanStack Query for React, use the following command:

```bash
npm install @tanstack/react-query
# or
yarn add @tanstack/react-query
```

Note that the package name has changed from `react-query` to `@tanstack/react-query`.

## QueryClient Configuration

The `QueryClient` configuration has been simplified. Here's an example of how to set up a `QueryClient` in the latest version:

```tsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'

const queryClient = new QueryClient()

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      {/* Your app components */}
    </QueryClientProvider>
  )
}
```

## Hook Usage Updates

The core hooks have been updated to use an options object instead of multiple parameters. Here's an example of the new `useQuery` usage:

```tsx
// Old usage
const { data, isLoading } = useQuery('repoData', fetchRepoData)

// New usage
const { data, isPending } = useQuery({
  queryKey: ['repoData'],
  queryFn: fetchRepoData,
})
```

Note that `isLoading` has been renamed to `isPending` for more clarity.

## New Features

TanStack Query introduces several new features and improvements:

1. **Improved TypeScript support**: Better type inference and stricter types for a more robust development experience.

2. **Suspense mode**: Native support for React Suspense, allowing for more declarative data fetching.

3. **Query cancellation**: Built-in support for cancelling ongoing queries when components unmount or dependencies change.

4. **Persistence plugins**: Easier integration with various storage solutions for persisting query data.

## Breaking Changes

Be aware of these breaking changes when migrating:

1. The `useQuery` hook now returns `isPending` instead of `isLoading`.

2. Query keys are now always treated as arrays. If you were using string keys, wrap them in an array:

   ```tsx
   // Old
   useQuery('users', fetchUsers)

   // New
   useQuery({ queryKey: ['users'], queryFn: fetchUsers })
   ```

3. The `refetchInterval` option is now in milliseconds instead of seconds.

4. Some less commonly used options and methods have been renamed or removed. Consult the official documentation for a complete list of changes.

## Conclusion

This migration guide covers the most significant changes when upgrading to TanStack Query. For a more comprehensive list of changes and new features, please refer to the [official documentation](https://tanstack.com/query/latest/docs/react/overview).

Remember to test your application thoroughly after migration to ensure everything works as expected. If you encounter any issues, the [TanStack Query GitHub repository](https://github.com/TanStack/query) and [Discord community](https://discord.com/invite/WrRKjPJ) are great resources for getting help.