# useRequest Scenario Examples & Config Details

## 1. Auto Request (Default)

```ts
const { data, error, loading } = useRequest(getUserList)
```

Executes automatically on component mount, manages `loading` / `data` / `error`.

## 2. Manual Trigger

```ts
const { loading, run, runAsync } = useRequest(updateUser, { manual: true })

// run: auto-catches errors, handle via onError
const handleSubmit = () => run({ name: 'blue' })

// runAsync: returns Promise, catch yourself
const handleSubmitAsync = async () => {
  try {
    const result = await runAsync({ name: 'blue' })
    message.success('Updated')
  } catch (e) {
    message.error('Failed')
  }
}
```

## 3. Debounce Search

```ts
const { data, run } = useRequest(searchUser, {
  manual: true,
  debounceWait: 300,
})

const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  run(e.target.value)
}
```

| Config | Type | Default | Description |
|--------|------|---------|-------------|
| `debounceWait` | number | - | Debounce delay (ms), enables debounce mode |
| `debounceLeading` | boolean | false | Execute before delay starts |
| `debounceTrailing` | boolean | true | Execute after delay ends |
| `debounceMaxWait` | number | - | Maximum delay time |

## 4. Throttle Request

```ts
const { data, run } = useRequest(fetchSuggestions, {
  manual: true,
  throttleWait: 300,
})
```

| Config | Type | Default | Description |
|--------|------|---------|-------------|
| `throttleWait` | number | - | Throttle interval (ms) |
| `throttleLeading` | boolean | true | Execute before interval starts |
| `throttleTrailing` | boolean | true | Execute after interval ends |

## 5. Polling

```ts
const { data, run, cancel } = useRequest(getSystemStatus, {
  pollingInterval: 3000,
  pollingWhenHidden: false,
  pollingErrorRetryCount: 3,
})
```

| Config | Type | Default | Description |
|--------|------|---------|-------------|
| `pollingInterval` | number | 0 | Polling interval (ms), >0 enables polling |
| `pollingWhenHidden` | boolean | true | Continue polling when page is hidden |
| `pollingErrorRetryCount` | number | -1 | Retry count on polling error, -1 = unlimited |

Notes:
- Polling waits `pollingInterval` after each request **completes** before starting next
- `cancel()` stops polling, `run()` restarts it
- With `manual=true`, you must call `run` to start polling

## 6. Cache / SWR

```ts
// SWR: return cache first, refresh in background
const { data, loading } = useRequest(getArticle, {
  cacheKey: 'article-detail',
})

// Stale-while-revalidate: don't re-request within 5s
const { data } = useRequest(getUserProfile, {
  cacheKey: 'user-profile',
  staleTime: 5000,
})

// Custom cache (e.g., localStorage)
const { data } = useRequest(getSettings, {
  cacheKey: 'app-settings',
  cacheTime: -1,
  setCache: (data) => localStorage.setItem('settings', JSON.stringify(data)),
  getCache: () => JSON.parse(localStorage.getItem('settings') || '{}'),
})
```

| Config | Type | Default | Description |
|--------|------|---------|-------------|
| `cacheKey` | string | - | Unique cache key, same key shares data globally |
| `cacheTime` | number | 300000 | Cache expiration (ms), -1 = never expires |
| `staleTime` | number | 0 | Data freshness period (ms), no re-request within period, -1 = always fresh |
| `setCache` | function | - | Custom cache write (must pair with getCache) |
| `getCache` | function | - | Custom cache read |

Clear cache:
```ts
import { clearCache } from 'ahooks'
clearCache('article-detail')    // Clear specific
clearCache(['key1', 'key2'])    // Clear multiple
clearCache()                     // Clear all
```

Notes:
- Only successful request data is cached
- Requests with the same `cacheKey` share a Promise to avoid duplicates
- Data syncs across components

## 7. Refresh on Window Focus

```ts
const { data } = useRequest(getNoticeCount, {
  refreshOnWindowFocus: true,
  focusTimespan: 5000,
})
```

| Config | Type | Default | Description |
|--------|------|---------|-------------|
| `refreshOnWindowFocus` | boolean | false | Whether to refresh on window focus |
| `focusTimespan` | number | 5000 | Minimum refresh interval (ms) |

## 8. Dependency Refresh

```ts
const [currentTab, setCurrentTab] = useState('all')

const { data } = useRequest(() => getOrderList(currentTab), {
  refreshDeps: [currentTab],
})

// Equivalent to:
// const { data, refresh } = useRequest(() => getOrderList(currentTab))
// useEffect(() => { refresh() }, [currentTab])
```

| Config | Type | Default | Description |
|--------|------|---------|-------------|
| `refreshDeps` | DependencyList | [] | Auto refresh when dependencies change |
| `refreshDepsAction` | () => void | - | Custom behavior on dependency change |

Note: `refreshDeps` has no effect when `manual=true`.

## 9. Conditional Request (Ready)

```ts
const { data } = useRequest(() => getUserDetail(userId), {
  ready: !!userId,
})
```

| Config | Type | Default | Description |
|--------|------|---------|-------------|
| `ready` | boolean | true | Whether the request is ready |

- Auto mode: triggers request when `ready` changes from false to true
- Manual mode: `run/runAsync` won't execute when `ready=false`

## 10. Delayed Loading

```ts
const { loading, data } = useRequest(getQuickData, {
  loadingDelay: 300,
})
```

If the request completes within 300ms, `loading` won't become `true`, avoiding UI flicker.

## 11. Error Retry

```ts
const { data } = useRequest(getUnstableApi, {
  retryCount: 3,
  retryInterval: 1000,
})
```

| Config | Type | Default | Description |
|--------|------|---------|-------------|
| `retryCount` | number | - | Retry count, -1 = unlimited retries |
| `retryInterval` | number | - | Retry interval (ms), defaults to exponential backoff |

Default exponential backoff: wait `1000 * 2^n` ms for the nth retry, capped at 30s.

## 12. Lifecycle

```ts
const { data } = useRequest(submitForm, {
  manual: true,
  onBefore: (params) => {
    console.log('Request params:', params)
  },
  onSuccess: (data, params) => {
    message.success('Submitted')
  },
  onError: (e, params) => {
    message.error(e.message)
  },
  onFinally: (params, data, e) => {
    setSubmitting(false)
  },
})
```

## Common Composition Patterns

### Search + Debounce + Manual Trigger
```ts
const [keyword, setKeyword] = useState('')

const { data, loading } = useRequest(() => searchAPI(keyword), {
  manual: true,
  debounceWait: 300,
})

const handleSearch = (value: string) => {
  setKeyword(value)
  run()
}
```

### List + Pagination + Cache
```ts
const [page, setPage] = useState(1)

const { data, loading } = useRequest(() => getList(page), {
  refreshDeps: [page],
  cacheKey: `list-page-${page}`,
  staleTime: 60000,
})
```

### Form Submit + Optimistic Update
```ts
const { data, run, mutate } = useRequest(updateProfile, {
  manual: true,
  onBefore: () => {
    // Optimistic update: change UI first
    mutate((old) => ({ ...old, name: newName }))
  },
  onError: () => {
    // Rollback on failure
    refresh()
  },
})
```
