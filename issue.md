---
name: Feature request
about: Suggest an idea for this project
title: 'LRU Cache and Eviction Policies for Backend Clientsets'
labels: kind/feature
assignees: ''
---

## Is your feature request related to a problem? Please describe the impact that the lack of the feature requested is creating.
Headlamp caches cluster clientsets in the Go backend to speed up queries. In environments with a large number of active clusters, stateless configurations, or high user concurrency, keeping all client connections in memory can lead to resource exhaustion.

## Describe the solution you'd like
Design and integrate an Least-Recently-Used (LRU) eviction mechanism into the backend k8cache manager:
1. Limit the maximum number of active clientsets in memory.
2. Safely close connection pools and stop goroutines associated with evicted clientsets.
3. Implement telemetry logging to track cache hits, evictions, and cache-janitor activity.

## What users will benefit from this feature?
Stateless configurations, environments with a large number of active clusters, or high user concurrency.

## Are you able to implement this feature?
Yes (I will propose a PR).

## Additional context
Requires deep knowledge of Go concurrency, mutexes, Go context cancellation propagation, memory profiles, and backend testing with mock Kubernetes servers.
