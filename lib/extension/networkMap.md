# Raw network-map evidence

Raw maps retain the existing `nodes`, `links`, and per-link `routes`. They also
include optional evidence fields for consumers that need to assess incomplete
scans or inspect entries that cannot be drawn as links:

- `scan.startedAt` and `scan.completedAt` bound the sequential network scan.
- A node's `neighborTable` is the complete LQI-table result returned by herdsman,
  including inactive relationships and original IEEE addresses. Graph links still
  filter inactive relationships and apply the existing Xiaomi IEEE correction.
- A node's `routingTable` is the complete routing-table result. Entries remain
  available even when their next hop has no graph link or the LQI read failed.
- `lqiRead` and `routingTableRead` record `startedAt`, `completedAt`, `attempts`,
  and `status` (`success` or `failed`). Attempts count calls to herdsman's table
  reader; each call can itself request several pages. Read windows include the
  existing retry delay, but exclude the delay before scanning the device.

All timestamps are Unix milliseconds from the host clock, not radio measurement
times or the time a neighbor last transmitted.
