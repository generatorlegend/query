---
id: offline-support
title: Offline Support and Persistence
---

# Offline Support and Persistence in TanStack Query

TanStack Query provides powerful features to implement offline support and persistence in your applications. This guide will walk you through the process of setting up offline capabilities using the query-persist-client-core package.

## Overview

The `query-persist-client-core` package offers essential utilities for implementing offline support and data persistence in your TanStack Query applications. These features allow your app to function smoothly even when the network connection is unreliable or unavailable.

## Key Components

1. `persist`: Core functionality for persisting query data.
2. `retryStrategies`: Strategies for retrying failed queries.
3. `createPersister`: Utility for creating custom persisters.

## Setting Up Offline Support

To implement offline support in your application, follow these steps:

1. Install the necessary package:

```bash
npm install @tanstack/query-persist-client-core
```

2. Import the required functions:

```typescript
import { persist, createPersister, retryStrategies } from '@tanstack/query-persist-client-core';
```

3. Create a persister:

```typescript
const myPersister = createPersister({
  // Configure your persister options here
});
```

4. Set up persistence for your queries:

```typescript
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      persister: myPersister,
    },
  },
});
```

5. Implement retry strategies for offline scenarios:

```typescript
const offlineRetryStrategy = retryStrategies.exponentialDelay({
  maxRetries: 3,
  baseDelay: 1000,
});

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: offlineRetryStrategy,
    },
  },
});
```

## Advanced Usage

### Custom Persisters

You can create custom persisters to suit your specific needs:

```typescript
const myCustomPersister = createPersister({
  persistQuery: async (key, data) => {
    // Implement your custom persistence logic here
  },
  restoreQuery: async (key) => {
    // Implement your custom restoration logic here
  },
});
```

### Selective Persistence

You may want to persist only certain queries. You can achieve this by configuring persistence on a per-query basis:

```typescript
const result = useQuery({
  queryKey: ['myData'],
  queryFn: fetchMyData,
  persist: true, // Enable persistence for this query
});
```

## Best Practices

1. **Optimize Storage**: Be mindful of storage limitations on client devices. Implement mechanisms to clear old or less important persisted data.

2. **Handle Conflicts**: Implement strategies to resolve conflicts between persisted data and fresh server data when the application comes back online.

3. **Security Considerations**: Ensure that sensitive data is not persisted or is properly encrypted when stored offline.

4. **User Experience**: Provide clear feedback to users about the online/offline status of the application and the freshness of the data they are viewing.

By leveraging TanStack Query's offline support and persistence features, you can create robust applications that provide a seamless user experience, even in challenging network conditions.