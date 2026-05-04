---
name: ahooks-userequest
description: Best practices and standard usage guide for ahooks useRequest. Covers auto/manual requests, polling, debounce/throttle, cache/SWR, error retry, dependency refresh, conditional requests, and loading management in React applications.
---

# ahooks useRequest Best Practices

**Core Principle**: Use ahooks `useRequest` for all data requests. Never hand-roll `useState + useEffect` request patterns. Never wrap `useRequest` with a secondary abstraction.

## Quick Reference

| Scenario | Config | Example |
|----------|--------|---------|
| Auto request | default | `useRequest(fn)` |
| Manual trigger | `manual: true` | `const { run } = useRequest(fn, { manual: true })` |
| Debounce search | `debounceWait` | `useRequest(fn, { manual: true, debounceWait: 300 })` |
| Throttle request | `throttleWait` | `useRequest(fn, { manual: true, throttleWait: 300 })` |
| Polling | `pollingInterval` | `const { run, cancel } = useRequest(fn, { pollingInterval: 3000 })` |
| Cache / SWR | `cacheKey` | `useRequest(fn, { cacheKey: 'user-list' })` |
| Stale-while-revalidate | `staleTime` | `useRequest(fn, { cacheKey: 'user', staleTime: 5000 })` |
| Refresh on focus | `refreshOnWindowFocus` | `useRequest(fn, { refreshOnWindowFocus: true })` |
| Dependency refresh | `refreshDeps` | `useRequest(fn, { refreshDeps: [userId] })` |
| Conditional request | `ready` | `useRequest(fn, { ready: !!userId })` |
| Delayed loading | `loadingDelay` | `useRequest(fn, { loadingDelay: 300 })` |
| Error retry | `retryCount` | `useRequest(fn, { retryCount: 3 })` |

## Return Values

- `data` / `loading` / `error` — request state trio
- `run(params)` / `runAsync(params)` — manual trigger (`run` auto-catches, `runAsync` returns a Promise you catch yourself)
- `refresh()` / `refreshAsync()` — re-request with last params
- `cancel()` — cancel request / stop polling
- `mutate(newData)` — directly modify data, supports functional `mutate(old => new)`
- `params` — current request params array

## Lifecycle

```ts
useRequest(fn, {
  onBefore: (params) => { /* before request */ },
  onSuccess: (data, params) => { /* success */ },
  onError: (e, params) => { /* failure */ },
  onFinally: (params, data, e) => { /* completed (success or failure) */ },
})
```

## Detailed Reference

- [Scenario examples and config details](references/userequest-scenarios.md)
