In **TanStack Query** (formerly React Query), **`staleTime`** defines how long fetched data is considered "fresh" in milliseconds. During this period, the library acts like a "do not disturb" sign for your network—serving data entirely from the cache without triggering background refetches.
The Core Mechanics

- **`staleTime: 0` (Default)**: Data is instantly marked as stale. While TanStack Query still shows cached data immediately for a responsive UI, it will _always_ execute a background refetch when triggers occur.
- **During `staleTime` (Fresh State)**: Data is read directly from the JavaScript memory cache. No network requests will fire, even if the user navigates away and re-mounts the component.
- **After `staleTime` (Stale State)**: The data remains visible, but automatic background refetches are triggered whenever a component mounts, the window is refocused, or the network reconnects.

---
# How to Configure `staleTime`

You can implement `staleTime` at an individual query level or globally across your application.

1. Individual Query Level

Configure it directly inside the `useQuery` hook for specific endpoints like static data:

```js
import { useQuery } from '@tanstack/react-query';

const { data } = useQuery({
  queryKey: ['product-details'],
  queryFn: fetchProductDetails,
  // Data stays fresh for 5 minutes (5 * 60 * 1000 ms)
  staleTime: 5 * 60 * 1000, 
});
```

2. Global Level

Define it globally inside your global configuration file via the `QueryClient`: 

```js
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      // All queries default to 30 seconds of freshness
      staleTime: 30 * 1000, 
    },
  },
});
```

---

`staleTime` vs `gcTime` (formerly `cacheTime`)

These two properties manage entirely different lifecycles of cached data. 

| Feature      | `staleTime`                                      | `gcTime` / `cacheTime`                                            |
| ------------ | ------------------------------------------------ | ----------------------------------------------------------------- |
| **Purpose**  | Determines when data is old and needs a refresh. | Determines when unused data is deleted from memory.               |
| **Default**  | `0` (instant staleness).                         | `5 * 60 * 1000` (5 minutes).                                      |
| **Triggers** | Active observers, remounts, focus updates.       | Commences when a query has **zero active observers** (unmounted). |

---

Common Use Cases & Best Practices

- **`staleTime: Infinity`**: Perfect for completely static configurations, like app localization strings, country code lists, or user permissions that do not change during a user session.
- **Short Window (`10_000` to `30_000` ms)**: Highly effective for standard list dashboards. It prevents multiple back-to-back API calls if a user accidentally double-clicks a tab or toggles a view quickly.
- **Pairing with Mutations**: If you increase `staleTime`, remember to manually invoke `queryClient.invalidateQueries` inside your mutations. This forces data updates immediately after a user adds, edits, or deletes an item.