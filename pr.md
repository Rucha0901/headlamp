## Summary

This PR adds a Least-Recently-Used (LRU) eviction policy and active capacity limits to the backend clientset cache manager to avoid memory/socket leaks and resource exhaustion.

## Related Issue

Fixes #5

## Changes

- Updated `clientsetCache` to use `*list.Element` from Go's standard `"container/list"` package.
- Added `clientsetLRUList` to track accessed clientsets in LRU order.
- Added `lruEntry` struct to store cache keys alongside cached clientsets.
- Implemented `getMaxClientsetsLimit()` allowing configuration of clientset limits via the `HEADLAMP_MAX_CLIENTSETS` environment variable (default: 100).
- Implemented a reflection-based `closeClientsetIdleConnections()` utility that recursively unwraps the clientset transport wrappers to invoke `CloseIdleConnections()` on the underlying `*http.Transport`, safely closing TCP connection pools and stopping read/write loop goroutines.
- Added structured telemetry logging (`telemetry: ...`) for cache hits, misses, evictions (expired/janitor, context cleanup, LRU capacity limits), and janitor sweeps.
- Added comprehensive unit tests validating capacity eviction limits, LRU re-ordering on access, and transport connection closure.

## Steps to Test

1. Run the test suite:
   ```bash
   cd backend
   go test -v ./pkg/k8cache/...
   ```
2. Check that all new tests (`TestLRUEviction_CapacityLimit`, `TestLRUEviction_MoveToFront`, and `TestLRUEviction_ClosesConnections`) pass successfully.
3. Verify telemetry logs printed to the stdout show cache hit, miss, and eviction logs correctly.

## Screenshots (if applicable)

N/A

## Notes for the Reviewer

None.
