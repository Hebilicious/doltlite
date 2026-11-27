# DoltLite Performance Report

> Nightly result: **FAIL**
>
> Generated: 2026-11-27 20:51 UTC
>
> Commit: [`37f20e96afeff37eb3afa463f9639df9eaf65a29`](https://github.com/dolthub/doltlite/commit/37f20e96afeff37eb3afa463f9639df9eaf65a29)
>
> Runner: ubuntu24 20261122.23.1
>
> [GitHub Actions run](https://github.com/Hebilicious/doltlite/actions/runs/46054468719)

The SQL workloads compare optimized DoltLite against stock SQLite on the same GitHub-hosted runner, alternating execution order on each repetition. Version-control sections measure DoltLite latency. Reported timings are medians. Paired-ratio noise is the median absolute deviation of the paired DoltLite/SQLite ratios, expressed as a percentage.

## SQL workload summary

The primary view aggregates all key shapes and compares DoltLite with SQLite by storage mode and operation class. Mean multiple is the average over workloads of each workload's paired DoltLite/SQLite ratio, so every workload counts equally whatever its run time.

### In-memory

| Operation | Mean multiple | Paired-ratio noise | Result |
|---|---:|---:|---|
| Reads | 1.12× | 1.0% | **PASS** |
| Writes | 1.70× | 1.0% | **FAIL** |

### File-backed

| Operation | Mean multiple | Paired-ratio noise | Result |
|---|---:|---:|---|
| Reads | 0.91× | 1.2% | **PASS** |
| Writes | 1.24× | 2.0% | **PASS** |
| Autocommit writes | 1.95× | 4.5% | **PASS** |

The absolute ceiling is 2.3× per ordinary workload and 1.8× for a section average. Durable autocommit writes use 3.6× and 2.6× ceilings respectively. A breach counts only when the median of the paired ratios exceeds the same ceiling.

<details>
<summary>Key-shape and individual-workload breakdown</summary>

The integer, text, blob, and composite primary-key runs verify that performance holds across key shapes.

| Storage | Operation | Key shape | Workloads | Samples/workload | Mean multiple | Paired-ratio noise | Result |
|---|---|---|---:|---:|---:|---:|---|
| In-memory | Reads | int | 15 | 55 | 1.12× | 1.0% | **PASS** |
| In-memory | Reads | textpk | 15 | 55 | 1.13× | 1.0% | **PASS** |
| In-memory | Reads | blobpk | 15 | 55 | 1.09× | 0.7% | **PASS** |
| In-memory | Reads | compositepk | 15 | 55 | 1.15× | 1.4% | **PASS** |
| In-memory | Writes | int | 8 | 55 | 1.58× | 1.0% | **PASS** |
| In-memory | Writes | textpk | 8 | 55 | 1.83× | 0.8% | **FAIL** |
| In-memory | Writes | blobpk | 8 | 55 | 1.72× | 0.8% | **PASS** |
| In-memory | Writes | compositepk | 8 | 55 | 1.66× | 1.6% | **PASS** |
| File-backed | Reads | int | 15 | 55 | 0.92× | 1.1% | **PASS** |
| File-backed | Reads | textpk | 15 | 55 | 0.95× | 1.3% | **PASS** |
| File-backed | Reads | blobpk | 15 | 55 | 0.92× | 0.8% | **PASS** |
| File-backed | Reads | compositepk | 15 | 55 | 0.87× | 1.2% | **PASS** |
| File-backed | Writes | int | 8 | 55 | 1.31× | 1.5% | **PASS** |
| File-backed | Writes | textpk | 8 | 55 | 1.27× | 4.4% | **PASS** |
| File-backed | Writes | blobpk | 8 | 55 | 1.09× | 2.7% | **PASS** |
| File-backed | Writes | compositepk | 8 | 55 | 1.27× | 1.9% | **PASS** |
| File-backed | Autocommit reads | int | 15 | 55 | 1.07× | 1.3% | **PASS** |
| File-backed | Autocommit reads | textpk | 15 | 55 | 1.08× | 1.1% | **PASS** |
| File-backed | Autocommit reads | blobpk | 15 | 55 | 1.05× | 0.8% | **PASS** |
| File-backed | Autocommit reads | compositepk | 15 | 55 | 1.07× | 1.8% | **PASS** |
| File-backed | Autocommit writes | int | 8 | 55 | 2.11× | 3.9% | **PASS** |
| File-backed | Autocommit writes | textpk | 8 | 55 | 2.02× | 4.6% | **PASS** |
| File-backed | Autocommit writes | blobpk | 8 | 55 | 1.72× | 2.7% | **PASS** |
| File-backed | Autocommit writes | compositepk | 8 | 55 | 1.95× | 6.3% | **PASS** |

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 16.03ms | 18.00ms | 1.1× | 0.8% | PASS |
| mem_reads | `oltp_range_select` | 8.02ms | 8.61ms | 1.1× | 0.9% | PASS |
| mem_reads | `oltp_sum_range` | 7.26ms | 8.11ms | 1.1× | 1.3% | PASS |
| mem_reads | `oltp_order_range` | 2.01ms | 2.13ms | 1.1× | 1.8% | PASS |
| mem_reads | `oltp_distinct_range` | 2.68ms | 2.88ms | 1.1× | 1.0% | PASS |
| mem_reads | `oltp_index_scan` | 2.80ms | 3.60ms | 1.3× | 1.8% | PASS |
| mem_reads | `select_random_points` | 10.58ms | 9.43ms | 0.9× | 1.5% | PASS |
| mem_reads | `select_random_ranges` | 3.17ms | 3.25ms | 1.0× | 1.2% | PASS |
| mem_reads | `covering_index_scan` | 4.78ms | 6.69ms | 1.4× | 0.8% | PASS |
| mem_reads | `groupby_scan` | 21.37ms | 22.51ms | 1.1× | 0.9% | PASS |
| mem_reads | `index_join` | 4.23ms | 6.20ms | 1.5× | 1.4% | PASS |
| mem_reads | `index_join_scan` | 2.24ms | 3.95ms | 1.8× | 2.4% | PASS |
| mem_reads | `types_table_scan` | 754.52ms | 500.96ms | 0.7× | 0.2% | PASS |
| mem_reads | `table_scan` | 854.41ms | 579.48ms | 0.7× | 0.5% | PASS |
| mem_reads | `oltp_read_only` | 71.11ms | 79.23ms | 1.1× | 0.6% | PASS |
| mem_writes | `oltp_bulk_insert` | 108.63ms | 159.67ms | 1.5× | 0.5% | PASS |
| mem_writes | `oltp_insert` | 9.73ms | 16.23ms | 1.7× | 1.0% | PASS |
| mem_writes | `oltp_update_index` | 35.55ms | 64.32ms | 1.8× | 0.9% | PASS |
| mem_writes | `oltp_update_non_index` | 23.54ms | 36.98ms | 1.6× | 1.2% | PASS |
| mem_writes | `oltp_delete_insert` | 32.61ms | 50.89ms | 1.6× | 0.9% | PASS |
| mem_writes | `oltp_write_only` | 15.08ms | 28.22ms | 1.9× | 1.4% | PASS |
| mem_writes | `types_delete_insert` | 16.20ms | 20.00ms | 1.2× | 1.3% | PASS |
| mem_writes | `oltp_read_write` | 43.68ms | 64.97ms | 1.5× | 1.1% | PASS |
| file_reads | `oltp_point_select` | 37.56ms | 22.75ms | 0.6× | 0.5% | PASS |
| file_reads | `oltp_range_select` | 10.35ms | 9.31ms | 0.9× | 1.1% | PASS |
| file_reads | `oltp_sum_range` | 9.69ms | 8.97ms | 0.9× | 0.9% | PASS |
| file_reads | `oltp_order_range` | 2.29ms | 2.24ms | 1.0× | 1.7% | PASS |
| file_reads | `oltp_distinct_range` | 2.91ms | 2.94ms | 1.0× | 2.6% | PASS |
| file_reads | `oltp_index_scan` | 5.06ms | 4.28ms | 0.8× | 1.6% | PASS |
| file_reads | `select_random_points` | 12.85ms | 10.10ms | 0.8× | 1.1% | PASS |
| file_reads | `select_random_ranges` | 5.29ms | 3.69ms | 0.7× | 1.0% | PASS |
| file_reads | `covering_index_scan` | 7.02ms | 7.10ms | 1.0× | 0.5% | PASS |
| file_reads | `groupby_scan` | 21.51ms | 22.37ms | 1.0× | 1.4% | PASS |
| file_reads | `index_join` | 5.24ms | 6.00ms | 1.1× | 1.4% | PASS |
| file_reads | `index_join_scan` | 2.48ms | 4.08ms | 1.6× | 2.8% | PASS |
| file_reads | `types_table_scan` | 756.74ms | 502.16ms | 0.7× | 0.2% | PASS |
| file_reads | `table_scan` | 853.13ms | 576.99ms | 0.7× | 0.4% | PASS |
| file_reads | `oltp_read_only` | 100.52ms | 85.42ms | 0.8× | 0.4% | PASS |
| file_writes | `oltp_bulk_insert` | 117.15ms | 166.66ms | 1.4× | 0.9% | PASS |
| file_writes | `oltp_insert` | 13.05ms | 18.15ms | 1.4× | 1.5% | PASS |
| file_writes | `oltp_update_index` | 46.28ms | 67.93ms | 1.5× | 1.8% | PASS |
| file_writes | `oltp_update_non_index` | 33.15ms | 39.22ms | 1.2× | 1.5% | PASS |
| file_writes | `oltp_delete_insert` | 41.05ms | 52.37ms | 1.3× | 2.4% | PASS |
| file_writes | `oltp_write_only` | 25.51ms | 35.64ms | 1.4× | 2.7% | PASS |
| file_writes | `types_delete_insert` | 24.08ms | 25.23ms | 1.0× | 1.3% | PASS |
| file_writes | `oltp_read_write` | 56.62ms | 74.64ms | 1.3× | 1.5% | PASS |
| ac_reads | `oltp_point_select` | 23.10ms | 22.94ms | 1.0× | 0.7% | PASS |
| ac_reads | `oltp_range_select` | 9.07ms | 9.32ms | 1.0× | 1.4% | PASS |
| ac_reads | `oltp_sum_range` | 8.12ms | 8.83ms | 1.1× | 1.3% | PASS |
| ac_reads | `oltp_order_range` | 2.14ms | 2.21ms | 1.0× | 1.2% | PASS |
| ac_reads | `oltp_distinct_range` | 2.87ms | 2.98ms | 1.0× | 1.8% | PASS |
| ac_reads | `oltp_index_scan` | 3.69ms | 4.30ms | 1.2× | 2.1% | PASS |
| ac_reads | `select_random_points` | 11.61ms | 10.11ms | 0.9× | 1.4% | PASS |
| ac_reads | `select_random_ranges` | 3.92ms | 3.76ms | 1.0× | 1.5% | PASS |
| ac_reads | `covering_index_scan` | 5.58ms | 7.16ms | 1.3× | 0.6% | PASS |
| ac_reads | `groupby_scan` | 21.83ms | 22.60ms | 1.0× | 0.8% | PASS |
| ac_reads | `index_join` | 4.81ms | 6.54ms | 1.4× | 2.0% | PASS |
| ac_reads | `index_join_scan` | 2.60ms | 4.44ms | 1.7× | 1.8% | PASS |
| ac_reads | `types_table_scan` | 758.96ms | 503.92ms | 0.7× | 0.3% | PASS |
| ac_reads | `table_scan` | 859.68ms | 578.51ms | 0.7× | 0.7% | PASS |
| ac_reads | `oltp_read_only` | 79.97ms | 85.36ms | 1.1× | 0.5% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 9.19ms | 13.41ms | 1.5× | 7.4% | PASS |
| ac_writes | `oltp_insert_ac` | 10.91ms | 21.08ms | 1.9× | 9.8% | PASS |
| ac_writes | `oltp_update_index_ac` | 10.37ms | 29.15ms | 2.8× | 5.8% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 8.46ms | 16.02ms | 1.9× | 2.0% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 9.22ms | 21.85ms | 2.4× | 1.1% | PASS |
| ac_writes | `oltp_write_only_ac` | 9.52ms | 21.75ms | 2.3× | 2.1% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.35ms | 16.57ms | 2.0× | 3.2% | PASS |
| ac_writes | `oltp_read_write_ac` | 12.87ms | 26.66ms | 2.1× | 4.6% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 31.42ms | 34.68ms | 1.1× | 1.5% | PASS |
| mem_reads | `oltp_range_select` | 14.36ms | 14.19ms | 1.0× | 1.0% | PASS |
| mem_reads | `oltp_sum_range` | 13.33ms | 13.44ms | 1.0× | 0.8% | PASS |
| mem_reads | `oltp_order_range` | 2.94ms | 2.99ms | 1.0× | 0.7% | PASS |
| mem_reads | `oltp_distinct_range` | 3.77ms | 3.88ms | 1.0× | 0.8% | PASS |
| mem_reads | `oltp_index_scan` | 3.61ms | 5.67ms | 1.6× | 1.0% | PASS |
| mem_reads | `select_random_points` | 19.70ms | 20.21ms | 1.0× | 1.6% | PASS |
| mem_reads | `select_random_ranges` | 5.62ms | 6.06ms | 1.1× | 0.9% | PASS |
| mem_reads | `covering_index_scan` | 6.57ms | 9.41ms | 1.4× | 1.0% | PASS |
| mem_reads | `groupby_scan` | 31.28ms | 30.74ms | 1.0× | 0.8% | PASS |
| mem_reads | `index_join` | 9.56ms | 9.26ms | 1.0× | 1.8% | PASS |
| mem_reads | `index_join_scan` | 3.11ms | 5.89ms | 1.9× | 1.1% | PASS |
| mem_reads | `types_table_scan` | 973.05ms | 858.36ms | 0.9× | 1.2% | PASS |
| mem_reads | `table_scan` | 1.16s | 979.13ms | 0.8× | 1.3% | PASS |
| mem_reads | `oltp_read_only` | 125.32ms | 129.24ms | 1.0× | 0.8% | PASS |
| mem_writes | `oltp_bulk_insert` | 207.05ms | 299.17ms | 1.4× | 0.6% | PASS |
| mem_writes | `oltp_insert` | 15.76ms | 34.67ms | 2.2× | 0.7% | PASS |
| mem_writes | `oltp_update_index` | 57.15ms | 123.77ms | 2.2× | 0.9% | PASS |
| mem_writes | `oltp_update_non_index` | 41.37ms | 74.03ms | 1.8× | 1.5% | PASS |
| mem_writes | `oltp_delete_insert` | 46.42ms | 90.24ms | 1.9× | 0.8% | PASS |
| mem_writes | `oltp_write_only` | 23.75ms | 52.05ms | 2.2× | 0.8% | PASS |
| mem_writes | `types_delete_insert` | 32.87ms | 45.41ms | 1.4× | 1.0% | PASS |
| mem_writes | `oltp_read_write` | 84.17ms | 124.71ms | 1.5× | 1.2% | PASS |
| file_reads | `oltp_point_select` | 60.61ms | 41.97ms | 0.7× | 1.1% | PASS |
| file_reads | `oltp_range_select` | 17.48ms | 15.18ms | 0.9× | 1.1% | PASS |
| file_reads | `oltp_sum_range` | 17.13ms | 14.78ms | 0.9× | 1.1% | PASS |
| file_reads | `oltp_order_range` | 3.36ms | 3.17ms | 0.9× | 1.6% | PASS |
| file_reads | `oltp_distinct_range` | 4.22ms | 4.06ms | 1.0× | 1.3% | PASS |
| file_reads | `oltp_index_scan` | 6.71ms | 6.66ms | 1.0× | 1.4% | PASS |
| file_reads | `select_random_points` | 23.58ms | 21.43ms | 0.9× | 2.0% | PASS |
| file_reads | `select_random_ranges` | 9.00ms | 7.08ms | 0.8× | 1.3% | PASS |
| file_reads | `covering_index_scan` | 9.90ms | 10.48ms | 1.1× | 1.9% | PASS |
| file_reads | `groupby_scan` | 31.88ms | 31.04ms | 1.0× | 0.8% | PASS |
| file_reads | `index_join` | 11.77ms | 10.03ms | 0.9× | 2.4% | PASS |
| file_reads | `index_join_scan` | 3.62ms | 6.14ms | 1.7× | 2.0% | PASS |
| file_reads | `types_table_scan` | 978.65ms | 861.59ms | 0.9× | 1.2% | PASS |
| file_reads | `table_scan` | 1.14s | 969.75ms | 0.9× | 1.5% | PASS |
| file_reads | `oltp_read_only` | 170.22ms | 141.29ms | 0.8× | 0.8% | PASS |
| file_writes | `oltp_bulk_insert` | 226.79ms | 313.35ms | 1.4× | 0.7% | PASS |
| file_writes | `oltp_insert` | 20.78ms | 39.03ms | 1.9× | 1.8% | PASS |
| file_writes | `oltp_update_index` | 103.52ms | 139.83ms | 1.4× | 7.0% | PASS |
| file_writes | `oltp_update_non_index` | 89.16ms | 88.27ms | 1.0× | 13.9% | PASS |
| file_writes | `oltp_delete_insert` | 73.89ms | 106.65ms | 1.4× | 1.2% | PASS |
| file_writes | `oltp_write_only` | 64.36ms | 64.39ms | 1.0× | 21.9% | PASS |
| file_writes | `types_delete_insert` | 53.36ms | 55.59ms | 1.0× | 1.6% | PASS |
| file_writes | `oltp_read_write` | 129.44ms | 138.70ms | 1.1× | 12.2% | PASS |
| ac_reads | `oltp_point_select` | 41.09ms | 42.75ms | 1.0× | 2.0% | PASS |
| ac_reads | `oltp_range_select` | 15.75ms | 15.28ms | 1.0× | 1.1% | PASS |
| ac_reads | `oltp_sum_range` | 14.78ms | 14.70ms | 1.0× | 1.1% | PASS |
| ac_reads | `oltp_order_range` | 3.17ms | 3.18ms | 1.0× | 1.4% | PASS |
| ac_reads | `oltp_distinct_range` | 4.03ms | 4.09ms | 1.0× | 1.0% | PASS |
| ac_reads | `oltp_index_scan` | 4.95ms | 6.76ms | 1.4× | 1.1% | PASS |
| ac_reads | `select_random_points` | 22.98ms | 22.38ms | 1.0× | 2.0% | PASS |
| ac_reads | `select_random_ranges` | 7.03ms | 7.13ms | 1.0× | 1.6% | PASS |
| ac_reads | `covering_index_scan` | 7.92ms | 10.66ms | 1.3× | 1.4% | PASS |
| ac_reads | `groupby_scan` | 31.98ms | 31.29ms | 1.0× | 0.7% | PASS |
| ac_reads | `index_join` | 11.23ms | 10.31ms | 0.9× | 2.2% | PASS |
| ac_reads | `index_join_scan` | 3.46ms | 6.36ms | 1.8× | 2.1% | PASS |
| ac_reads | `types_table_scan` | 1.01s | 883.36ms | 0.9× | 0.8% | PASS |
| ac_reads | `table_scan` | 1.20s | 997.59ms | 0.8× | 0.5% | PASS |
| ac_reads | `oltp_read_only` | 144.87ms | 144.52ms | 1.0× | 0.9% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 16.59ms | 23.26ms | 1.4× | 7.9% | PASS |
| ac_writes | `oltp_insert_ac` | 19.12ms | 34.30ms | 1.8× | 4.7% | PASS |
| ac_writes | `oltp_update_index_ac` | 18.96ms | 51.22ms | 2.7× | 3.0% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 15.78ms | 29.50ms | 1.9× | 6.8% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 17.47ms | 40.63ms | 2.3× | 4.0% | PASS |
| ac_writes | `oltp_write_only_ac` | 18.63ms | 40.23ms | 2.2× | 4.5% | PASS |
| ac_writes | `types_delete_insert_ac` | 16.65ms | 30.24ms | 1.8× | 5.0% | PASS |
| ac_writes | `oltp_read_write_ac` | 22.31ms | 46.50ms | 2.1× | 4.1% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.58ms | 30.00ms | 1.0× | 0.7% | PASS |
| mem_reads | `oltp_range_select` | 13.19ms | 14.01ms | 1.1× | 0.5% | PASS |
| mem_reads | `oltp_sum_range` | 12.67ms | 12.68ms | 1.0× | 0.7% | PASS |
| mem_reads | `oltp_order_range` | 2.89ms | 3.03ms | 1.0× | 0.7% | PASS |
| mem_reads | `oltp_distinct_range` | 3.77ms | 4.04ms | 1.1× | 0.5% | PASS |
| mem_reads | `oltp_index_scan` | 3.54ms | 5.31ms | 1.5× | 1.1% | PASS |
| mem_reads | `select_random_points` | 20.08ms | 18.67ms | 0.9× | 1.0% | PASS |
| mem_reads | `select_random_ranges` | 5.46ms | 5.33ms | 1.0× | 0.8% | PASS |
| mem_reads | `covering_index_scan` | 6.36ms | 9.18ms | 1.4× | 0.5% | PASS |
| mem_reads | `groupby_scan` | 31.90ms | 31.77ms | 1.0× | 0.4% | PASS |
| mem_reads | `index_join` | 9.93ms | 8.93ms | 0.9× | 1.9% | PASS |
| mem_reads | `index_join_scan` | 3.05ms | 5.55ms | 1.8× | 1.7% | PASS |
| mem_reads | `types_table_scan` | 962.10ms | 768.06ms | 0.8× | 0.3% | PASS |
| mem_reads | `table_scan` | 1.08s | 848.62ms | 0.8× | 0.3% | PASS |
| mem_reads | `oltp_read_only` | 115.75ms | 121.58ms | 1.1× | 0.7% | PASS |
| mem_writes | `oltp_bulk_insert` | 191.02ms | 261.81ms | 1.4× | 0.6% | PASS |
| mem_writes | `oltp_insert` | 15.20ms | 30.44ms | 2.0× | 1.1% | PASS |
| mem_writes | `oltp_update_index` | 54.20ms | 108.82ms | 2.0× | 1.8% | PASS |
| mem_writes | `oltp_update_non_index` | 40.70ms | 68.39ms | 1.7× | 0.6% | PASS |
| mem_writes | `oltp_delete_insert` | 45.48ms | 85.35ms | 1.9× | 0.6% | PASS |
| mem_writes | `oltp_write_only` | 23.52ms | 49.23ms | 2.1× | 0.9% | PASS |
| mem_writes | `types_delete_insert` | 31.92ms | 40.76ms | 1.3× | 0.8% | PASS |
| mem_writes | `oltp_read_write` | 77.47ms | 111.52ms | 1.4× | 0.9% | PASS |
| file_reads | `oltp_point_select` | 58.75ms | 36.26ms | 0.6× | 0.7% | PASS |
| file_reads | `oltp_range_select` | 16.27ms | 15.00ms | 0.9× | 1.1% | PASS |
| file_reads | `oltp_sum_range` | 15.71ms | 13.58ms | 0.9× | 0.9% | PASS |
| file_reads | `oltp_order_range` | 3.27ms | 3.21ms | 1.0× | 1.3% | PASS |
| file_reads | `oltp_distinct_range` | 4.19ms | 4.23ms | 1.0× | 0.9% | PASS |
| file_reads | `oltp_index_scan` | 6.70ms | 6.26ms | 0.9× | 1.1% | PASS |
| file_reads | `select_random_points` | 23.82ms | 19.68ms | 0.8× | 1.1% | PASS |
| file_reads | `select_random_ranges` | 8.70ms | 6.15ms | 0.7× | 0.8% | PASS |
| file_reads | `covering_index_scan` | 9.51ms | 10.12ms | 1.1× | 0.6% | PASS |
| file_reads | `groupby_scan` | 32.38ms | 32.17ms | 1.0× | 0.5% | PASS |
| file_reads | `index_join` | 12.18ms | 10.03ms | 0.8× | 0.7% | PASS |
| file_reads | `index_join_scan` | 3.55ms | 6.06ms | 1.7× | 1.3% | PASS |
| file_reads | `types_table_scan` | 966.48ms | 773.73ms | 0.8× | 0.5% | PASS |
| file_reads | `table_scan` | 1.12s | 854.07ms | 0.8× | 0.8% | PASS |
| file_reads | `oltp_read_only` | 160.80ms | 132.51ms | 0.8× | 0.6% | PASS |
| file_writes | `oltp_bulk_insert` | 265.40ms | 335.00ms | 1.3× | 2.1% | PASS |
| file_writes | `oltp_insert` | 29.80ms | 53.90ms | 1.8× | 6.1% | PASS |
| file_writes | `oltp_update_index` | 166.19ms | 202.28ms | 1.2× | 2.6% | PASS |
| file_writes | `oltp_update_non_index` | 145.66ms | 126.14ms | 0.9× | 2.4% | PASS |
| file_writes | `oltp_delete_insert` | 160.46ms | 155.62ms | 1.0× | 2.9% | PASS |
| file_writes | `oltp_write_only` | 120.12ms | 109.37ms | 0.9× | 3.6% | PASS |
| file_writes | `types_delete_insert` | 120.40ms | 86.70ms | 0.7× | 5.3% | PASS |
| file_writes | `oltp_read_write` | 170.46ms | 170.05ms | 1.0× | 1.4% | PASS |
| ac_reads | `oltp_point_select` | 39.89ms | 36.33ms | 0.9× | 0.7% | PASS |
| ac_reads | `oltp_range_select` | 14.37ms | 15.03ms | 1.0× | 1.1% | PASS |
| ac_reads | `oltp_sum_range` | 14.01ms | 13.65ms | 1.0× | 0.8% | PASS |
| ac_reads | `oltp_order_range` | 3.09ms | 3.23ms | 1.0× | 1.1% | PASS |
| ac_reads | `oltp_distinct_range` | 3.97ms | 4.18ms | 1.1× | 1.1% | PASS |
| ac_reads | `oltp_index_scan` | 4.80ms | 6.24ms | 1.3× | 1.5% | PASS |
| ac_reads | `select_random_points` | 21.79ms | 19.72ms | 0.9× | 0.7% | PASS |
| ac_reads | `select_random_ranges` | 6.83ms | 6.16ms | 0.9× | 1.2% | PASS |
| ac_reads | `covering_index_scan` | 7.57ms | 10.15ms | 1.3× | 0.8% | PASS |
| ac_reads | `groupby_scan` | 32.37ms | 32.22ms | 1.0× | 0.5% | PASS |
| ac_reads | `index_join` | 11.42ms | 10.10ms | 0.9× | 0.8% | PASS |
| ac_reads | `index_join_scan` | 3.42ms | 6.07ms | 1.8× | 1.4% | PASS |
| ac_reads | `types_table_scan` | 967.88ms | 773.68ms | 0.8× | 0.4% | PASS |
| ac_reads | `table_scan` | 1.09s | 851.39ms | 0.8× | 0.5% | PASS |
| ac_reads | `oltp_read_only` | 130.91ms | 131.53ms | 1.0× | 0.9% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.19ms | 35.70ms | 1.3× | 2.8% | PASS |
| ac_writes | `oltp_insert_ac` | 29.30ms | 50.54ms | 1.7× | 1.7% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.74ms | 66.69ms | 2.2× | 2.3% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 26.95ms | 42.73ms | 1.6× | 3.5% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 28.94ms | 53.44ms | 1.8× | 3.7% | PASS |
| ac_writes | `oltp_write_only_ac` | 28.69ms | 52.17ms | 1.8× | 2.6% | PASS |
| ac_writes | `types_delete_insert_ac` | 28.11ms | 39.80ms | 1.4× | 3.5% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.37ms | 58.76ms | 1.8× | 2.5% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.86ms | 39.93ms | 1.1× | 1.9% | PASS |
| mem_reads | `oltp_range_select` | 21.05ms | 22.05ms | 1.0× | 1.3% | PASS |
| mem_reads | `oltp_sum_range` | 18.44ms | 20.96ms | 1.1× | 1.4% | PASS |
| mem_reads | `oltp_order_range` | 3.60ms | 3.95ms | 1.1× | 1.2% | PASS |
| mem_reads | `oltp_distinct_range` | 4.76ms | 5.14ms | 1.1× | 1.0% | PASS |
| mem_reads | `oltp_index_scan` | 4.69ms | 6.46ms | 1.4× | 2.4% | PASS |
| mem_reads | `select_random_points` | 30.46ms | 31.67ms | 1.0× | 1.7% | PASS |
| mem_reads | `select_random_ranges` | 8.05ms | 8.73ms | 1.1× | 1.4% | PASS |
| mem_reads | `covering_index_scan` | 7.78ms | 11.21ms | 1.4× | 1.4% | PASS |
| mem_reads | `groupby_scan` | 37.06ms | 39.25ms | 1.1× | 0.8% | PASS |
| mem_reads | `index_join` | 8.17ms | 11.88ms | 1.5× | 2.3% | PASS |
| mem_reads | `index_join_scan` | 4.04ms | 6.31ms | 1.6× | 2.7% | PASS |
| mem_reads | `types_table_scan` | 1.10s | 876.30ms | 0.8× | 1.6% | PASS |
| mem_reads | `table_scan` | 1.34s | 978.61ms | 0.7× | 2.5% | PASS |
| mem_reads | `oltp_read_only` | 158.69ms | 174.31ms | 1.1× | 1.2% | PASS |
| mem_writes | `oltp_bulk_insert` | 245.52ms | 323.09ms | 1.3× | 0.9% | PASS |
| mem_writes | `oltp_insert` | 19.17ms | 34.64ms | 1.8× | 1.2% | PASS |
| mem_writes | `oltp_update_index` | 68.63ms | 126.15ms | 1.8× | 1.7% | PASS |
| mem_writes | `oltp_update_non_index` | 53.33ms | 83.02ms | 1.6× | 1.4% | PASS |
| mem_writes | `oltp_delete_insert` | 50.89ms | 92.70ms | 1.8× | 1.7% | PASS |
| mem_writes | `oltp_write_only` | 27.79ms | 55.40ms | 2.0× | 1.7% | PASS |
| mem_writes | `types_delete_insert` | 33.99ms | 49.53ms | 1.5× | 1.7% | PASS |
| mem_writes | `oltp_read_write` | 105.76ms | 152.87ms | 1.4× | 1.5% | PASS |
| file_reads | `oltp_point_select` | 105.59ms | 57.57ms | 0.5× | 1.1% | PASS |
| file_reads | `oltp_range_select` | 28.48ms | 24.18ms | 0.8× | 1.3% | PASS |
| file_reads | `oltp_sum_range` | 26.20ms | 23.21ms | 0.9× | 1.2% | PASS |
| file_reads | `oltp_order_range` | 4.56ms | 4.28ms | 0.9× | 1.8% | PASS |
| file_reads | `oltp_distinct_range` | 5.75ms | 5.47ms | 1.0× | 1.2% | PASS |
| file_reads | `oltp_index_scan` | 12.07ms | 8.37ms | 0.7× | 2.2% | PASS |
| file_reads | `select_random_points` | 38.51ms | 34.02ms | 0.9× | 1.6% | PASS |
| file_reads | `select_random_ranges` | 15.39ms | 10.82ms | 0.7× | 1.2% | PASS |
| file_reads | `covering_index_scan` | 15.47ms | 13.23ms | 0.9× | 1.6% | PASS |
| file_reads | `groupby_scan` | 37.90ms | 39.75ms | 1.0× | 0.8% | PASS |
| file_reads | `index_join` | 12.37ms | 12.86ms | 1.0× | 1.7% | PASS |
| file_reads | `index_join_scan` | 5.09ms | 6.60ms | 1.3× | 2.5% | PASS |
| file_reads | `types_table_scan` | 1.08s | 874.88ms | 0.8× | 0.7% | PASS |
| file_reads | `table_scan` | 1.34s | 981.22ms | 0.7× | 1.2% | PASS |
| file_reads | `oltp_read_only` | 267.12ms | 204.40ms | 0.8× | 1.2% | PASS |
| file_writes | `oltp_bulk_insert` | 259.61ms | 336.26ms | 1.3× | 1.2% | PASS |
| file_writes | `oltp_insert` | 26.65ms | 38.50ms | 1.4× | 2.1% | PASS |
| file_writes | `oltp_update_index` | 99.81ms | 142.48ms | 1.4× | 1.7% | PASS |
| file_writes | `oltp_update_non_index` | 80.65ms | 90.75ms | 1.1× | 1.6% | PASS |
| file_writes | `oltp_delete_insert` | 80.80ms | 105.26ms | 1.3× | 2.2% | PASS |
| file_writes | `oltp_write_only` | 54.92ms | 67.73ms | 1.2× | 2.1% | PASS |
| file_writes | `types_delete_insert` | 52.23ms | 59.44ms | 1.1× | 1.9% | PASS |
| file_writes | `oltp_read_write` | 135.09ms | 165.37ms | 1.2× | 1.9% | PASS |
| ac_reads | `oltp_point_select` | 58.67ms | 57.66ms | 1.0× | 1.2% | PASS |
| ac_reads | `oltp_range_select` | 23.83ms | 24.22ms | 1.0× | 1.6% | PASS |
| ac_reads | `oltp_sum_range` | 21.36ms | 23.19ms | 1.1× | 2.1% | PASS |
| ac_reads | `oltp_order_range` | 4.10ms | 4.29ms | 1.0× | 1.9% | PASS |
| ac_reads | `oltp_distinct_range` | 5.29ms | 5.48ms | 1.0× | 1.8% | PASS |
| ac_reads | `oltp_index_scan` | 7.47ms | 8.44ms | 1.1× | 1.8% | PASS |
| ac_reads | `select_random_points` | 33.09ms | 34.56ms | 1.0× | 2.1% | PASS |
| ac_reads | `select_random_ranges` | 10.64ms | 10.88ms | 1.0× | 1.9% | PASS |
| ac_reads | `covering_index_scan` | 10.69ms | 13.15ms | 1.2× | 1.2% | PASS |
| ac_reads | `groupby_scan` | 37.19ms | 39.80ms | 1.1× | 0.7% | PASS |
| ac_reads | `index_join` | 9.96ms | 13.02ms | 1.3× | 1.8% | PASS |
| ac_reads | `index_join_scan` | 4.60ms | 6.73ms | 1.5× | 2.7% | PASS |
| ac_reads | `types_table_scan` | 1.12s | 887.19ms | 0.8× | 1.5% | PASS |
| ac_reads | `table_scan` | 1.37s | 997.61ms | 0.7× | 0.6% | PASS |
| ac_reads | `oltp_read_only` | 200.53ms | 207.97ms | 1.0× | 0.7% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.75ms | 36.09ms | 1.5× | 6.4% | PASS |
| ac_writes | `oltp_insert_ac` | 25.11ms | 50.71ms | 2.0× | 6.0% | PASS |
| ac_writes | `oltp_update_index_ac` | 28.09ms | 65.33ms | 2.3× | 4.5% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 23.62ms | 43.00ms | 1.8× | 7.6% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.16ms | 54.03ms | 2.1× | 6.2% | PASS |
| ac_writes | `oltp_write_only_ac` | 25.28ms | 52.86ms | 2.1× | 5.1% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.74ms | 41.85ms | 1.8× | 7.8% | PASS |
| ac_writes | `oltp_read_write_ac` | 33.59ms | 63.85ms | 1.9× | 6.4% | PASS |

</details>

</details>

## Version-control latency

Wall time: 4m 31s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 32.90ms | 38.00ms | 86.6% | 1.1% | PASS |
| `status_dirty_many_tables` | 36.84ms | 42.00ms | 87.7% | 1.7% | PASS |
| `diff_regular_working_one_table` | 32.01ms | 33.00ms | 97.0% | 1.5% | PASS |
| `diff_regular_working_many_tables` | 42.40ms | 50.00ms | 84.8% | 1.6% | PASS |
| `diff_stat_working_many_tables` | 41.61ms | 48.00ms | 86.7% | 1.7% | PASS |
| `diff_schema_working_many_tables` | 41.92ms | 48.00ms | 87.3% | 1.2% | PASS |
| `branch_list_many_branches` | 24.18ms | 25.00ms | 96.7% | 1.3% | PASS |
| `branch_create_delete` | 26.69ms | 27.00ms | 98.9% | 1.6% | PASS |
| `at_literal_deep_history` | 26.60ms | 28.00ms | 95.0% | 2.0% | PASS |
| `diff_literal_deep_history` | 26.50ms | 28.00ms | 94.6% | 1.5% | PASS |
| `history_literal_deep_history` | 27.61ms | 30.00ms | 92.0% | 1.8% | PASS |
| `checkout_branch_clean` | 33.67ms | 43.00ms | 78.3% | 1.1% | PASS |
| `merge_data_no_conflicts` | 29.84ms | 33.00ms | 90.4% | 1.2% | PASS |
| `merge_data_secondary_index` | 879.91ms | 944.00ms | 93.2% | 0.8% | PASS |
| `merge_schema_no_conflicts` | 22.50ms | 24.00ms | 93.8% | 1.6% | PASS |
| `merge_data_conflicts` | 31.22ms | 33.00ms | 94.6% | 1.0% | PASS |
| `merge_data_conflicts_with_resolve` | 31.96ms | 34.00ms | 94.0% | 1.0% | PASS |

Version-control ceiling result: **PASS**.

## File remote performance

Status: passed. Transport: local file-backed remote.

100,000,000 logical payload bytes; 100,000 rows; 1,000 changed rows; 5 repetitions per operation and binary.

SQL timings exclude fixture generation, copies, database open and verification. Each SQL sample uses a fresh process with the OS cache primed by setup verification. SQL client cache: 65,536 KiB. copy_fsync times shutil.copyfile plus fsync.

Throughput uses logical payload bytes; destination growth is file-size change, not transferred bytes.

Nightly median ceilings require three-attempt confirmation; copy_fsync is a control.

| Operation | Ceiling ms | Result |
|---|---:|---|
| push_initial | 2000 | PASS |
| clone_initial | 3000 | PASS |
| push_noop | 10 | PASS |
| fetch_noop | 10 | PASS |
| push_incremental | 750 | PASS |
| fetch_incremental | 300 | PASS |
| pull_fast_forward | 300 | PASS |

Timing gate: PASS after 1 attempt(s).

candidate: 37f20e9.

Fixture file bytes: base 107,968,301; delta 115,879,032.

| Operation | Binary | Median ms | Min–max ms | CPU ms | Payload MB/s | Destination growth bytes |
|---|---|---:|---:|---:|---:|---:|
| copy_fsync | candidate | 58.928 | 54.933–60.021 | 38.727 | 1696.99 | 107968301 |
| push_initial | candidate | 503.270 | 495.666–505.949 | 484.376 | 198.70 | 107956807 |
| clone_initial | candidate | 529.967 | 527.483–537.690 | 506.283 | 188.69 | 108475105 |
| push_noop | candidate | 0.682 | 0.646–0.720 | 0.382 | — | 0 |
| fetch_noop | candidate | 0.183 | 0.155–0.191 | 0.188 | — | 0 |
| push_incremental | candidate | 64.535 | 63.923–65.943 | 61.845 | 15.50 | 7909775 |
| fetch_incremental | candidate | 78.671 | 77.896–80.273 | 76.285 | 12.71 | 7910084 |
| pull_fast_forward | candidate | 78.161 | 77.709–79.319 | 75.065 | 12.79 | 7910844 |

<details>
<summary>Memory and file I/O</summary>

Successful VFS bytes and VFS requests cover local and remote files during the operation; they include OS-cache hits and are not physical disk traffic. Peak RSS covers the fresh SQL process, including database open. The Python copy control is not instrumented.

| Operation | Binary | Peak RSS MiB | Read MB | Written MB | Read requests | Write requests |
|---|---|---:|---:|---:|---:|---:|
| push_initial | candidate | 90.36 | 175.487 | 108.763 | 18329 | 2066 |
| clone_initial | candidate | 91.25 | 215.906 | 110.833 | 27898 | 2353 |
| push_noop | candidate | 41.85 | 0.073 | 0.000 | 27 | 2 |
| fetch_noop | candidate | 41.85 | 0.068 | 0.000 | 22 | 0 |
| push_incremental | candidate | 41.85 | 10.738 | 7.910 | 1374 | 125 |
| fetch_incremental | candidate | 41.85 | 26.494 | 7.910 | 7016 | 124 |
| pull_fast_forward | candidate | 41.85 | 26.497 | 8.959 | 7032 | 142 |

</details>


<details>
<summary>1 GB file remotes (observational)</summary>

Both database sizes use a fixed 1 MB scattered update and a warm OS cache. 1 GB timings have no ceiling while a baseline is collected.

### 1 GB file remote performance

Status: passed. Transport: local file-backed remote.

1,000,000,000 logical payload bytes; 1,000,000 rows; 1,000 changed rows; 3 repetitions per operation and binary.

SQL timings exclude fixture generation, copies, database open and verification. Each SQL sample uses a fresh process with the OS cache primed by setup verification. SQL client cache: 65,536 KiB. copy_fsync times shutil.copyfile plus fsync.

Throughput uses logical payload bytes; destination growth is file-size change, not transferred bytes.

No speed thresholds are enforced.

candidate: 37f20e9.

Fixture file bytes: base 1,087,718,066; delta 1,103,525,194.

| Operation | Binary | Median ms | Min–max ms | CPU ms | Payload MB/s | Destination growth bytes |
|---|---|---:|---:|---:|---:|---:|
| copy_fsync | candidate | 4159.644 | 3765.894–4161.202 | 553.819 | 240.41 | 1087718066 |
| push_initial | candidate | 5775.362 | 5760.123–6742.343 | 3102.632 | 173.15 | 1079566310 |
| clone_initial | candidate | 4720.141 | 4701.638–4875.700 | 3404.650 | 211.86 | 1102407396 |
| push_noop | candidate | 0.434 | 0.428–0.463 | 0.202 | — | 0 |
| fetch_noop | candidate | 0.075 | 0.074–0.078 | 0.075 | — | 0 |
| push_incremental | candidate | 184.021 | 179.140–184.647 | 181.323 | 5.43 | 15806003 |
| fetch_incremental | candidate | 183.064 | 181.269–189.833 | 180.095 | 5.46 | 15806513 |
| pull_fast_forward | candidate | 184.969 | 181.225–186.729 | 181.261 | 5.41 | 15807273 |

<details>
<summary>Memory and file I/O</summary>

Successful VFS bytes and VFS requests cover local and remote files during the operation; they include OS-cache hits and are not physical disk traffic. Peak RSS covers the fresh SQL process, including database open. The Python copy control is not instrumented.

| Operation | Binary | Peak RSS MiB | Read MB | Written MB | Read requests | Write requests |
|---|---|---:|---:|---:|---:|---:|
| push_initial | candidate | 134.69 | 2092.233 | 1087.622 | 263585 | 20541 |
| clone_initial | candidate | 131.59 | 2179.032 | 1133.868 | 284251 | 32259 |
| push_noop | candidate | 44.17 | 0.078 | 0.000 | 28 | 2 |
| fetch_noop | candidate | 44.17 | 0.069 | 0.000 | 22 | 0 |
| push_incremental | candidate | 56.34 | 46.846 | 15.928 | 10141 | 313 |
| fetch_incremental | candidate | 50.41 | 62.413 | 15.928 | 13937 | 313 |
| pull_fast_forward | candidate | 50.51 | 62.416 | 16.978 | 13953 | 331 |

</details>


</details>

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh`, `test/vc_perf_ceiling.sh`, and `test/remote_perf.py`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
