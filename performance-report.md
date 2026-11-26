# DoltLite Performance Report

> Nightly result: **FAIL**
>
> Generated: 2026-11-26 21:07 UTC
>
> Commit: [`37f20e96afeff37eb3afa463f9639df9eaf65a29`](https://github.com/dolthub/doltlite/commit/37f20e96afeff37eb3afa463f9639df9eaf65a29)
>
> Runner: ubuntu24 20261122.23.1
>
> [GitHub Actions run](https://github.com/Hebilicious/doltlite/actions/runs/45882009050)

The SQL workloads compare optimized DoltLite against stock SQLite on the same GitHub-hosted runner, alternating execution order on each repetition. Version-control sections measure DoltLite latency. Reported timings are medians. Paired-ratio noise is the median absolute deviation of the paired DoltLite/SQLite ratios, expressed as a percentage.

## SQL workload summary

The primary view aggregates all key shapes and compares DoltLite with SQLite by storage mode and operation class. Mean multiple is the average over workloads of each workload's paired DoltLite/SQLite ratio, so every workload counts equally whatever its run time.

### In-memory

| Operation | Mean multiple | Paired-ratio noise | Result |
|---|---:|---:|---|
| Reads | 1.05× | 1.8% | **PASS** |
| Writes | 1.60× | 1.7% | **PASS** |

### File-backed

| Operation | Mean multiple | Paired-ratio noise | Result |
|---|---:|---:|---|
| Reads | 0.80× | 1.6% | **PASS** |
| Writes | 1.10× | 4.2% | **FAIL** |
| Autocommit writes | 1.85× | 3.9% | **PASS** |

The absolute ceiling is 2.3× per ordinary workload and 1.8× for a section average. Durable autocommit writes use 3.6× and 2.6× ceilings respectively. A breach counts only when the median of the paired ratios exceeds the same ceiling.

<details>
<summary>Key-shape and individual-workload breakdown</summary>

The integer, text, blob, and composite primary-key runs verify that performance holds across key shapes.

| Storage | Operation | Key shape | Workloads | Samples/workload | Mean multiple | Paired-ratio noise | Result |
|---|---|---|---:|---:|---:|---:|---|
| In-memory | Reads | int | 15 | 55 | 1.12× | 1.3% | **PASS** |
| In-memory | Reads | textpk | 15 | 55 | 0.98× | 1.7% | **PASS** |
| In-memory | Reads | blobpk | 15 | 55 | 1.01× | 2.5% | **PASS** |
| In-memory | Reads | compositepk | 15 | 55 | 1.10× | 2.5% | **PASS** |
| In-memory | Writes | int | 8 | 55 | 1.58× | 0.9% | **PASS** |
| In-memory | Writes | textpk | 8 | 55 | 1.62× | 1.9% | **PASS** |
| In-memory | Writes | blobpk | 8 | 55 | 1.61× | 2.1% | **PASS** |
| In-memory | Writes | compositepk | 8 | 55 | 1.59× | 1.7% | **PASS** |
| File-backed | Reads | int | 15 | 55 | 0.92× | 0.9% | **PASS** |
| File-backed | Reads | textpk | 15 | 55 | 0.74× | 1.4% | **PASS** |
| File-backed | Reads | blobpk | 15 | 55 | 0.75× | 1.7% | **PASS** |
| File-backed | Reads | compositepk | 15 | 55 | 0.81× | 2.4% | **PASS** |
| File-backed | Writes | int | 8 | 55 | 1.31× | 2.2% | **PASS** |
| File-backed | Writes | textpk | 8 | 55 | 0.99× | 15.6% | **PASS** |
| File-backed | Writes | blobpk | 8 | 55 | 1.10× | 4.1% | **FAIL** |
| File-backed | Writes | compositepk | 8 | 55 | 0.98× | 4.6% | **PASS** |
| File-backed | Autocommit reads | int | 15 | 55 | 1.07× | 1.5% | **PASS** |
| File-backed | Autocommit reads | textpk | 15 | 55 | 0.92× | 1.1% | **PASS** |
| File-backed | Autocommit reads | blobpk | 15 | 55 | 0.95× | 1.9% | **PASS** |
| File-backed | Autocommit reads | compositepk | 15 | 55 | 1.03× | 2.4% | **PASS** |
| File-backed | Autocommit writes | int | 8 | 55 | 2.05× | 4.6% | **PASS** |
| File-backed | Autocommit writes | textpk | 8 | 55 | 1.91× | 35.3% | **PASS** |
| File-backed | Autocommit writes | blobpk | 8 | 55 | 1.60× | 3.9% | **PASS** |
| File-backed | Autocommit writes | compositepk | 8 | 55 | 1.85× | 2.6% | **PASS** |

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 16.09ms | 18.04ms | 1.1× | 1.1% | PASS |
| mem_reads | `oltp_range_select` | 8.08ms | 8.66ms | 1.1× | 1.1% | PASS |
| mem_reads | `oltp_sum_range` | 7.48ms | 8.32ms | 1.1× | 1.6% | PASS |
| mem_reads | `oltp_order_range` | 2.05ms | 2.16ms | 1.1× | 1.9% | PASS |
| mem_reads | `oltp_distinct_range` | 2.72ms | 2.91ms | 1.1× | 1.3% | PASS |
| mem_reads | `oltp_index_scan` | 2.88ms | 3.75ms | 1.3× | 1.8% | PASS |
| mem_reads | `select_random_points` | 10.87ms | 9.65ms | 0.9× | 1.7% | PASS |
| mem_reads | `select_random_ranges` | 3.23ms | 3.32ms | 1.0× | 1.4% | PASS |
| mem_reads | `covering_index_scan` | 4.82ms | 6.79ms | 1.4× | 0.8% | PASS |
| mem_reads | `groupby_scan` | 21.41ms | 22.72ms | 1.1× | 0.6% | PASS |
| mem_reads | `index_join` | 4.25ms | 6.31ms | 1.5× | 1.6% | PASS |
| mem_reads | `index_join_scan` | 2.39ms | 4.28ms | 1.8× | 4.8% | PASS |
| mem_reads | `types_table_scan` | 758.50ms | 504.42ms | 0.7× | 0.2% | PASS |
| mem_reads | `table_scan` | 858.08ms | 583.32ms | 0.7× | 0.6% | PASS |
| mem_reads | `oltp_read_only` | 71.42ms | 79.56ms | 1.1× | 0.7% | PASS |
| mem_writes | `oltp_bulk_insert` | 109.37ms | 160.95ms | 1.5× | 0.6% | PASS |
| mem_writes | `oltp_insert` | 9.79ms | 16.55ms | 1.7× | 0.6% | PASS |
| mem_writes | `oltp_update_index` | 35.43ms | 63.69ms | 1.8× | 1.1% | PASS |
| mem_writes | `oltp_update_non_index` | 23.14ms | 36.05ms | 1.6× | 1.0% | PASS |
| mem_writes | `oltp_delete_insert` | 31.28ms | 47.68ms | 1.5× | 0.6% | PASS |
| mem_writes | `oltp_write_only` | 15.18ms | 28.00ms | 1.8× | 1.3% | PASS |
| mem_writes | `types_delete_insert` | 16.16ms | 19.85ms | 1.2× | 1.3% | PASS |
| mem_writes | `oltp_read_write` | 43.72ms | 64.95ms | 1.5× | 0.7% | PASS |
| file_reads | `oltp_point_select` | 37.25ms | 22.61ms | 0.6× | 0.7% | PASS |
| file_reads | `oltp_range_select` | 10.41ms | 9.26ms | 0.9× | 1.0% | PASS |
| file_reads | `oltp_sum_range` | 9.49ms | 8.83ms | 0.9× | 0.9% | PASS |
| file_reads | `oltp_order_range` | 2.26ms | 2.19ms | 1.0× | 3.0% | PASS |
| file_reads | `oltp_distinct_range` | 2.91ms | 2.91ms | 1.0× | 1.5% | PASS |
| file_reads | `oltp_index_scan` | 4.98ms | 4.06ms | 0.8× | 1.3% | PASS |
| file_reads | `select_random_points` | 12.48ms | 9.71ms | 0.8× | 1.6% | PASS |
| file_reads | `select_random_ranges` | 5.25ms | 3.65ms | 0.7× | 0.9% | PASS |
| file_reads | `covering_index_scan` | 7.03ms | 7.09ms | 1.0× | 0.5% | PASS |
| file_reads | `groupby_scan` | 22.05ms | 22.40ms | 1.0× | 0.9% | PASS |
| file_reads | `index_join` | 5.35ms | 6.16ms | 1.1× | 1.5% | PASS |
| file_reads | `index_join_scan` | 2.41ms | 4.04ms | 1.7× | 1.9% | PASS |
| file_reads | `types_table_scan` | 757.48ms | 503.66ms | 0.7× | 0.3% | PASS |
| file_reads | `table_scan` | 858.92ms | 581.86ms | 0.7× | 0.3% | PASS |
| file_reads | `oltp_read_only` | 100.83ms | 85.78ms | 0.9× | 0.6% | PASS |
| file_writes | `oltp_bulk_insert` | 117.39ms | 169.01ms | 1.4× | 1.3% | PASS |
| file_writes | `oltp_insert` | 13.02ms | 17.88ms | 1.4× | 1.8% | PASS |
| file_writes | `oltp_update_index` | 46.05ms | 67.33ms | 1.5× | 2.5% | PASS |
| file_writes | `oltp_update_non_index` | 31.96ms | 38.15ms | 1.2× | 1.9% | PASS |
| file_writes | `oltp_delete_insert` | 40.03ms | 51.28ms | 1.3× | 2.5% | PASS |
| file_writes | `oltp_write_only` | 25.14ms | 34.16ms | 1.4× | 2.7% | PASS |
| file_writes | `types_delete_insert` | 23.80ms | 24.58ms | 1.0× | 2.4% | PASS |
| file_writes | `oltp_read_write` | 54.77ms | 71.17ms | 1.3× | 2.0% | PASS |
| ac_reads | `oltp_point_select` | 22.59ms | 22.59ms | 1.0× | 0.8% | PASS |
| ac_reads | `oltp_range_select` | 8.56ms | 9.10ms | 1.1× | 1.7% | PASS |
| ac_reads | `oltp_sum_range` | 7.71ms | 8.62ms | 1.1× | 1.7% | PASS |
| ac_reads | `oltp_order_range` | 2.13ms | 2.19ms | 1.0× | 1.5% | PASS |
| ac_reads | `oltp_distinct_range` | 2.85ms | 2.95ms | 1.0× | 1.7% | PASS |
| ac_reads | `oltp_index_scan` | 3.64ms | 4.20ms | 1.2× | 1.5% | PASS |
| ac_reads | `select_random_points` | 11.37ms | 9.89ms | 0.9× | 1.3% | PASS |
| ac_reads | `select_random_ranges` | 3.92ms | 3.73ms | 1.0× | 1.8% | PASS |
| ac_reads | `covering_index_scan` | 5.57ms | 7.18ms | 1.3× | 0.7% | PASS |
| ac_reads | `groupby_scan` | 21.90ms | 22.55ms | 1.0× | 0.8% | PASS |
| ac_reads | `index_join` | 4.72ms | 6.30ms | 1.3× | 1.6% | PASS |
| ac_reads | `index_join_scan` | 2.51ms | 4.22ms | 1.7× | 3.9% | PASS |
| ac_reads | `types_table_scan` | 760.32ms | 506.45ms | 0.7× | 0.3% | PASS |
| ac_reads | `table_scan` | 861.31ms | 582.88ms | 0.7× | 0.4% | PASS |
| ac_reads | `oltp_read_only` | 80.52ms | 85.92ms | 1.1× | 0.5% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 8.38ms | 12.98ms | 1.5× | 0.9% | PASS |
| ac_writes | `oltp_insert_ac` | 10.97ms | 20.98ms | 1.9× | 6.1% | PASS |
| ac_writes | `oltp_update_index_ac` | 12.21ms | 30.89ms | 2.5× | 7.4% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 9.21ms | 17.69ms | 1.9× | 6.6% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 9.45ms | 21.85ms | 2.3× | 2.2% | PASS |
| ac_writes | `oltp_write_only_ac` | 9.37ms | 21.04ms | 2.2× | 1.8% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.35ms | 16.32ms | 2.0× | 3.1% | PASS |
| ac_writes | `oltp_read_write_ac` | 15.48ms | 29.80ms | 1.9× | 11.8% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 30.54ms | 29.57ms | 1.0× | 1.7% | PASS |
| mem_reads | `oltp_range_select` | 16.14ms | 12.52ms | 0.8× | 3.5% | PASS |
| mem_reads | `oltp_sum_range` | 13.89ms | 11.98ms | 0.9× | 3.0% | PASS |
| mem_reads | `oltp_order_range` | 3.22ms | 2.81ms | 0.9× | 1.2% | PASS |
| mem_reads | `oltp_distinct_range` | 4.02ms | 3.63ms | 0.9× | 1.1% | PASS |
| mem_reads | `oltp_index_scan` | 3.35ms | 5.09ms | 1.5× | 1.4% | PASS |
| mem_reads | `select_random_points` | 19.54ms | 16.86ms | 0.9× | 2.8% | PASS |
| mem_reads | `select_random_ranges` | 5.84ms | 4.98ms | 0.9× | 1.4% | PASS |
| mem_reads | `covering_index_scan` | 6.01ms | 8.34ms | 1.4× | 0.9% | PASS |
| mem_reads | `groupby_scan` | 31.36ms | 29.63ms | 0.9× | 0.9% | PASS |
| mem_reads | `index_join` | 9.69ms | 8.29ms | 0.9× | 3.4% | PASS |
| mem_reads | `index_join_scan` | 3.34ms | 5.56ms | 1.7× | 3.0% | PASS |
| mem_reads | `types_table_scan` | 1.07s | 789.01ms | 0.7× | 1.9% | PASS |
| mem_reads | `table_scan` | 1.38s | 867.92ms | 0.6× | 1.2% | PASS |
| mem_reads | `oltp_read_only` | 130.55ms | 112.90ms | 0.9× | 2.7% | PASS |
| mem_writes | `oltp_bulk_insert` | 192.06ms | 259.30ms | 1.4× | 0.7% | PASS |
| mem_writes | `oltp_insert` | 14.52ms | 30.47ms | 2.1× | 1.2% | PASS |
| mem_writes | `oltp_update_index` | 57.02ms | 110.60ms | 1.9× | 1.8% | PASS |
| mem_writes | `oltp_update_non_index` | 43.58ms | 67.48ms | 1.5× | 2.0% | PASS |
| mem_writes | `oltp_delete_insert` | 46.99ms | 80.20ms | 1.7× | 1.9% | PASS |
| mem_writes | `oltp_write_only` | 24.62ms | 47.91ms | 1.9× | 2.0% | PASS |
| mem_writes | `types_delete_insert` | 33.74ms | 39.11ms | 1.2× | 2.5% | PASS |
| mem_writes | `oltp_read_write` | 86.69ms | 107.99ms | 1.2× | 2.7% | PASS |
| file_reads | `oltp_point_select` | 101.05ms | 45.99ms | 0.5× | 1.1% | PASS |
| file_reads | `oltp_range_select` | 23.32ms | 14.19ms | 0.6× | 1.8% | PASS |
| file_reads | `oltp_sum_range` | 20.45ms | 13.54ms | 0.7× | 1.4% | PASS |
| file_reads | `oltp_order_range` | 3.88ms | 3.00ms | 0.8× | 2.0% | PASS |
| file_reads | `oltp_distinct_range` | 4.76ms | 3.81ms | 0.8× | 1.2% | PASS |
| file_reads | `oltp_index_scan` | 10.41ms | 6.76ms | 0.6× | 0.9% | PASS |
| file_reads | `select_random_points` | 25.94ms | 18.21ms | 0.7× | 1.3% | PASS |
| file_reads | `select_random_ranges` | 13.03ms | 6.74ms | 0.5× | 1.1% | PASS |
| file_reads | `covering_index_scan` | 13.46ms | 10.15ms | 0.8× | 0.9% | PASS |
| file_reads | `groupby_scan` | 32.21ms | 29.78ms | 0.9× | 1.2% | PASS |
| file_reads | `index_join` | 13.65ms | 9.24ms | 0.7× | 2.6% | PASS |
| file_reads | `index_join_scan` | 4.17ms | 5.63ms | 1.3× | 2.7% | PASS |
| file_reads | `types_table_scan` | 893.38ms | 736.39ms | 0.8× | 2.1% | PASS |
| file_reads | `table_scan` | 1.11s | 815.96ms | 0.7× | 1.9% | PASS |
| file_reads | `oltp_read_only` | 218.42ms | 131.76ms | 0.6× | 1.5% | PASS |
| file_writes | `oltp_bulk_insert` | 280.97ms | 345.99ms | 1.2× | 8.6% | PASS |
| file_writes | `oltp_insert` | 29.02ms | 54.81ms | 1.9× | 31.3% | PASS |
| file_writes | `oltp_update_index` | 186.88ms | 190.80ms | 1.0× | 8.7% | PASS |
| file_writes | `oltp_update_non_index` | 152.17ms | 125.22ms | 0.8× | 11.0% | PASS |
| file_writes | `oltp_delete_insert` | 215.31ms | 171.77ms | 0.8× | 21.5% | PASS |
| file_writes | `oltp_write_only` | 128.44ms | 105.58ms | 0.8× | 20.2% | PASS |
| file_writes | `types_delete_insert` | 161.66ms | 90.38ms | 0.6× | 22.3% | PASS |
| file_writes | `oltp_read_write` | 200.85ms | 173.62ms | 0.9× | 10.3% | PASS |
| ac_reads | `oltp_point_select` | 53.34ms | 45.82ms | 0.9× | 1.6% | PASS |
| ac_reads | `oltp_range_select` | 18.75ms | 14.14ms | 0.8× | 2.1% | PASS |
| ac_reads | `oltp_sum_range` | 16.62ms | 13.95ms | 0.8× | 1.9% | PASS |
| ac_reads | `oltp_order_range` | 3.56ms | 3.02ms | 0.8× | 1.0% | PASS |
| ac_reads | `oltp_distinct_range` | 4.36ms | 3.84ms | 0.9× | 1.0% | PASS |
| ac_reads | `oltp_index_scan` | 6.05ms | 6.88ms | 1.1× | 0.9% | PASS |
| ac_reads | `select_random_points` | 22.20ms | 18.54ms | 0.8× | 2.4% | PASS |
| ac_reads | `select_random_ranges` | 8.42ms | 6.75ms | 0.8× | 0.9% | PASS |
| ac_reads | `covering_index_scan` | 8.82ms | 10.09ms | 1.1× | 0.9% | PASS |
| ac_reads | `groupby_scan` | 30.88ms | 29.59ms | 1.0× | 1.1% | PASS |
| ac_reads | `index_join` | 11.15ms | 9.13ms | 0.8× | 1.0% | PASS |
| ac_reads | `index_join_scan` | 3.77ms | 5.60ms | 1.5× | 1.1% | PASS |
| ac_reads | `types_table_scan` | 914.20ms | 740.49ms | 0.8× | 2.2% | PASS |
| ac_reads | `table_scan` | 1.15s | 817.30ms | 0.7× | 4.7% | PASS |
| ac_reads | `oltp_read_only` | 149.92ms | 131.14ms | 0.9× | 2.5% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 27.22ms | 39.82ms | 1.5× | 36.1% | PASS |
| ac_writes | `oltp_insert_ac` | 26.55ms | 43.53ms | 1.6× | 34.5% | PASS |
| ac_writes | `oltp_update_index_ac` | 39.50ms | 91.71ms | 2.3× | 48.4% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 22.36ms | 40.49ms | 1.8× | 28.2% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 26.32ms | 57.10ms | 2.2× | 24.1% | PASS |
| ac_writes | `oltp_write_only_ac` | 22.68ms | 49.54ms | 2.2× | 18.6% | PASS |
| ac_writes | `types_delete_insert_ac` | 23.34ms | 38.49ms | 1.6× | 38.4% | PASS |
| ac_writes | `oltp_read_write_ac` | 35.60ms | 63.92ms | 1.8× | 39.8% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.43ms | 23.70ms | 0.9× | 3.0% | PASS |
| mem_reads | `oltp_range_select` | 12.78ms | 10.70ms | 0.8× | 2.2% | PASS |
| mem_reads | `oltp_sum_range` | 11.08ms | 10.24ms | 0.9× | 2.2% | PASS |
| mem_reads | `oltp_order_range` | 2.57ms | 2.42ms | 0.9× | 1.3% | PASS |
| mem_reads | `oltp_distinct_range` | 3.09ms | 2.91ms | 0.9× | 1.7% | PASS |
| mem_reads | `oltp_index_scan` | 2.88ms | 4.39ms | 1.5× | 3.5% | PASS |
| mem_reads | `select_random_points` | 16.90ms | 13.89ms | 0.8× | 4.3% | PASS |
| mem_reads | `select_random_ranges` | 4.82ms | 4.20ms | 0.9× | 1.8% | PASS |
| mem_reads | `covering_index_scan` | 4.40ms | 6.52ms | 1.5× | 4.4% | PASS |
| mem_reads | `groupby_scan` | 24.28ms | 22.18ms | 0.9× | 1.3% | PASS |
| mem_reads | `index_join` | 9.15ms | 7.81ms | 0.9× | 4.4% | PASS |
| mem_reads | `index_join_scan` | 3.22ms | 5.68ms | 1.8× | 5.3% | PASS |
| mem_reads | `types_table_scan` | 805.68ms | 593.74ms | 0.7× | 2.5% | PASS |
| mem_reads | `table_scan` | 933.10ms | 654.98ms | 0.7× | 1.6% | PASS |
| mem_reads | `oltp_read_only` | 101.78ms | 88.64ms | 0.9× | 3.3% | PASS |
| mem_writes | `oltp_bulk_insert` | 155.25ms | 209.27ms | 1.3× | 0.8% | PASS |
| mem_writes | `oltp_insert` | 12.05ms | 25.32ms | 2.1× | 1.7% | PASS |
| mem_writes | `oltp_update_index` | 49.73ms | 95.06ms | 1.9× | 2.5% | PASS |
| mem_writes | `oltp_update_non_index` | 38.53ms | 55.44ms | 1.4× | 2.0% | PASS |
| mem_writes | `oltp_delete_insert` | 40.00ms | 69.73ms | 1.7× | 2.3% | PASS |
| mem_writes | `oltp_write_only` | 21.63ms | 41.96ms | 1.9× | 1.7% | PASS |
| mem_writes | `types_delete_insert` | 29.87ms | 33.88ms | 1.1× | 2.2% | PASS |
| mem_writes | `oltp_read_write` | 77.86ms | 95.63ms | 1.2× | 2.7% | PASS |
| file_reads | `oltp_point_select` | 87.42ms | 40.61ms | 0.5× | 1.7% | PASS |
| file_reads | `oltp_range_select` | 19.05ms | 12.38ms | 0.7× | 2.1% | PASS |
| file_reads | `oltp_sum_range` | 17.18ms | 12.01ms | 0.7× | 2.5% | PASS |
| file_reads | `oltp_order_range` | 3.12ms | 2.51ms | 0.8× | 1.5% | PASS |
| file_reads | `oltp_distinct_range` | 3.60ms | 3.00ms | 0.8× | 1.1% | PASS |
| file_reads | `oltp_index_scan` | 8.69ms | 5.96ms | 0.7× | 0.8% | PASS |
| file_reads | `select_random_points` | 22.11ms | 15.32ms | 0.7× | 3.1% | PASS |
| file_reads | `select_random_ranges` | 10.60ms | 5.70ms | 0.5× | 1.7% | PASS |
| file_reads | `covering_index_scan` | 10.46ms | 8.29ms | 0.8× | 1.0% | PASS |
| file_reads | `groupby_scan` | 24.18ms | 21.58ms | 0.9× | 1.4% | PASS |
| file_reads | `index_join` | 11.70ms | 8.08ms | 0.7× | 2.5% | PASS |
| file_reads | `index_join_scan` | 3.40ms | 4.95ms | 1.5× | 1.8% | PASS |
| file_reads | `types_table_scan` | 698.14ms | 501.17ms | 0.7× | 0.9% | PASS |
| file_reads | `table_scan` | 879.00ms | 621.36ms | 0.7× | 2.3% | PASS |
| file_reads | `oltp_read_only` | 176.77ms | 105.12ms | 0.6× | 1.6% | PASS |
| file_writes | `oltp_bulk_insert` | 209.49ms | 259.49ms | 1.2× | 3.8% | PASS |
| file_writes | `oltp_insert` | 18.98ms | 46.63ms | 2.5× | 2.6% | FAIL |
| file_writes | `oltp_update_index` | 149.02ms | 169.98ms | 1.1× | 4.3% | PASS |
| file_writes | `oltp_update_non_index` | 130.01ms | 109.44ms | 0.8× | 5.5% | PASS |
| file_writes | `oltp_delete_insert` | 150.48ms | 128.87ms | 0.9× | 3.5% | PASS |
| file_writes | `oltp_write_only` | 110.09ms | 86.76ms | 0.8× | 8.0% | PASS |
| file_writes | `types_delete_insert` | 110.90ms | 66.09ms | 0.6× | 5.6% | PASS |
| file_writes | `oltp_read_write` | 154.58ms | 130.04ms | 0.8× | 3.3% | PASS |
| ac_reads | `oltp_point_select` | 43.49ms | 37.37ms | 0.9× | 2.3% | PASS |
| ac_reads | `oltp_range_select` | 13.91ms | 11.23ms | 0.8× | 2.2% | PASS |
| ac_reads | `oltp_sum_range` | 13.40ms | 11.83ms | 0.9× | 2.9% | PASS |
| ac_reads | `oltp_order_range` | 2.86ms | 2.59ms | 0.9× | 1.5% | PASS |
| ac_reads | `oltp_distinct_range` | 3.39ms | 3.14ms | 0.9× | 1.5% | PASS |
| ac_reads | `oltp_index_scan` | 5.34ms | 6.38ms | 1.2× | 1.8% | PASS |
| ac_reads | `select_random_points` | 21.00ms | 17.16ms | 0.8× | 4.7% | PASS |
| ac_reads | `select_random_ranges` | 7.21ms | 5.90ms | 0.8× | 1.8% | PASS |
| ac_reads | `covering_index_scan` | 7.14ms | 9.07ms | 1.3× | 1.9% | PASS |
| ac_reads | `groupby_scan` | 24.35ms | 22.52ms | 0.9× | 1.6% | PASS |
| ac_reads | `index_join` | 10.96ms | 9.01ms | 0.8× | 5.3% | PASS |
| ac_reads | `index_join_scan` | 3.37ms | 5.66ms | 1.7× | 6.7% | PASS |
| ac_reads | `types_table_scan` | 808.19ms | 618.70ms | 0.8× | 1.8% | PASS |
| ac_reads | `table_scan` | 960.50ms | 706.30ms | 0.7× | 1.4% | PASS |
| ac_reads | `oltp_read_only` | 138.43ms | 117.86ms | 0.9× | 2.1% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 26.15ms | 30.77ms | 1.2× | 3.9% | PASS |
| ac_writes | `oltp_insert_ac` | 28.78ms | 44.13ms | 1.5× | 3.9% | PASS |
| ac_writes | `oltp_update_index_ac` | 29.29ms | 61.79ms | 2.1× | 5.8% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.54ms | 35.83ms | 1.5× | 3.0% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 28.39ms | 45.03ms | 1.6× | 3.9% | PASS |
| ac_writes | `oltp_write_only_ac` | 27.69ms | 45.77ms | 1.7× | 3.0% | PASS |
| ac_writes | `types_delete_insert_ac` | 21.24ms | 36.68ms | 1.7× | 2.3% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.60ms | 49.34ms | 1.6× | 4.1% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.49ms | 23.25ms | 1.1× | 3.0% | PASS |
| mem_reads | `oltp_range_select` | 11.78ms | 12.10ms | 1.0× | 2.5% | PASS |
| mem_reads | `oltp_sum_range` | 10.55ms | 11.66ms | 1.1× | 1.7% | PASS |
| mem_reads | `oltp_order_range` | 2.21ms | 2.25ms | 1.0× | 0.8% | PASS |
| mem_reads | `oltp_distinct_range` | 2.68ms | 2.77ms | 1.0× | 1.1% | PASS |
| mem_reads | `oltp_index_scan` | 2.93ms | 3.88ms | 1.3× | 1.4% | PASS |
| mem_reads | `select_random_points` | 18.60ms | 17.97ms | 1.0× | 3.0% | PASS |
| mem_reads | `select_random_ranges` | 4.52ms | 4.86ms | 1.1× | 1.5% | PASS |
| mem_reads | `covering_index_scan` | 3.84ms | 5.58ms | 1.5× | 2.5% | PASS |
| mem_reads | `groupby_scan` | 20.16ms | 20.89ms | 1.0× | 0.9% | PASS |
| mem_reads | `index_join` | 4.67ms | 6.44ms | 1.4× | 2.3% | PASS |
| mem_reads | `index_join_scan` | 2.73ms | 4.48ms | 1.6× | 2.5% | PASS |
| mem_reads | `types_table_scan` | 684.38ms | 464.84ms | 0.7× | 2.6% | PASS |
| mem_reads | `table_scan` | 818.01ms | 542.28ms | 0.7× | 2.6% | PASS |
| mem_reads | `oltp_read_only` | 88.44ms | 91.12ms | 1.0× | 2.7% | PASS |
| mem_writes | `oltp_bulk_insert` | 141.01ms | 184.76ms | 1.3× | 0.8% | PASS |
| mem_writes | `oltp_insert` | 10.94ms | 19.91ms | 1.8× | 1.5% | PASS |
| mem_writes | `oltp_update_index` | 41.15ms | 70.30ms | 1.7× | 1.5% | PASS |
| mem_writes | `oltp_update_non_index` | 30.66ms | 45.91ms | 1.5× | 3.2% | PASS |
| mem_writes | `oltp_delete_insert` | 28.57ms | 49.86ms | 1.7× | 1.7% | PASS |
| mem_writes | `oltp_write_only` | 16.02ms | 30.16ms | 1.9× | 1.6% | PASS |
| mem_writes | `types_delete_insert` | 19.96ms | 25.87ms | 1.3× | 2.5% | PASS |
| mem_writes | `oltp_read_write` | 60.16ms | 82.61ms | 1.4× | 2.7% | PASS |
| file_reads | `oltp_point_select` | 76.76ms | 38.42ms | 0.5× | 2.2% | PASS |
| file_reads | `oltp_range_select` | 17.23ms | 13.74ms | 0.8× | 2.0% | PASS |
| file_reads | `oltp_sum_range` | 16.14ms | 13.15ms | 0.8× | 1.8% | PASS |
| file_reads | `oltp_order_range` | 2.89ms | 2.50ms | 0.9× | 1.5% | PASS |
| file_reads | `oltp_distinct_range` | 3.38ms | 3.00ms | 0.9× | 1.7% | PASS |
| file_reads | `oltp_index_scan` | 8.77ms | 5.76ms | 0.7× | 2.7% | PASS |
| file_reads | `select_random_points` | 23.93ms | 19.79ms | 0.8× | 2.5% | PASS |
| file_reads | `select_random_ranges` | 10.22ms | 6.56ms | 0.6× | 1.8% | PASS |
| file_reads | `covering_index_scan` | 9.88ms | 7.70ms | 0.8× | 3.2% | PASS |
| file_reads | `groupby_scan` | 21.42ms | 21.91ms | 1.0× | 1.6% | PASS |
| file_reads | `index_join` | 8.27ms | 8.52ms | 1.0× | 4.6% | PASS |
| file_reads | `index_join_scan` | 3.32ms | 4.70ms | 1.4× | 4.4% | PASS |
| file_reads | `types_table_scan` | 727.87ms | 476.53ms | 0.7× | 5.5% | PASS |
| file_reads | `table_scan` | 911.25ms | 554.21ms | 0.6× | 4.5% | PASS |
| file_reads | `oltp_read_only` | 167.62ms | 111.18ms | 0.7× | 2.4% | PASS |
| file_writes | `oltp_bulk_insert` | 190.06ms | 228.99ms | 1.2× | 3.7% | PASS |
| file_writes | `oltp_insert` | 19.51ms | 27.39ms | 1.4× | 2.7% | PASS |
| file_writes | `oltp_update_index` | 140.93ms | 128.78ms | 0.9× | 5.0% | PASS |
| file_writes | `oltp_update_non_index` | 120.67ms | 88.76ms | 0.7× | 2.0% | PASS |
| file_writes | `oltp_delete_insert` | 111.14ms | 98.80ms | 0.9× | 7.1% | PASS |
| file_writes | `oltp_write_only` | 80.84ms | 76.59ms | 0.9× | 5.4% | PASS |
| file_writes | `types_delete_insert` | 74.39ms | 58.59ms | 0.8× | 5.1% | PASS |
| file_writes | `oltp_read_write` | 130.65ms | 128.58ms | 1.0× | 4.1% | PASS |
| ac_reads | `oltp_point_select` | 38.55ms | 37.09ms | 1.0× | 1.6% | PASS |
| ac_reads | `oltp_range_select` | 13.51ms | 13.41ms | 1.0× | 2.4% | PASS |
| ac_reads | `oltp_sum_range` | 12.38ms | 13.08ms | 1.1× | 2.1% | PASS |
| ac_reads | `oltp_order_range` | 2.51ms | 2.48ms | 1.0× | 1.4% | PASS |
| ac_reads | `oltp_distinct_range` | 2.99ms | 2.96ms | 1.0× | 0.8% | PASS |
| ac_reads | `oltp_index_scan` | 4.84ms | 5.34ms | 1.1× | 3.3% | PASS |
| ac_reads | `select_random_points` | 19.17ms | 19.50ms | 1.0× | 2.7% | PASS |
| ac_reads | `select_random_ranges` | 6.49ms | 6.42ms | 1.0× | 1.8% | PASS |
| ac_reads | `covering_index_scan` | 6.32ms | 7.83ms | 1.2× | 2.5% | PASS |
| ac_reads | `groupby_scan` | 20.78ms | 21.31ms | 1.0× | 1.1% | PASS |
| ac_reads | `index_join` | 6.29ms | 7.97ms | 1.3× | 3.4% | PASS |
| ac_reads | `index_join_scan` | 3.01ms | 4.75ms | 1.6× | 2.3% | PASS |
| ac_reads | `types_table_scan` | 731.54ms | 481.85ms | 0.7× | 4.2% | PASS |
| ac_reads | `table_scan` | 890.84ms | 571.52ms | 0.6× | 5.1% | PASS |
| ac_reads | `oltp_read_only` | 107.63ms | 106.22ms | 1.0× | 2.7% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 18.73ms | 29.89ms | 1.6× | 2.4% | PASS |
| ac_writes | `oltp_insert_ac` | 22.77ms | 39.45ms | 1.7× | 3.0% | PASS |
| ac_writes | `oltp_update_index_ac` | 24.74ms | 54.03ms | 2.2× | 3.7% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 18.92ms | 34.63ms | 1.8× | 2.1% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 21.90ms | 43.42ms | 2.0× | 2.5% | PASS |
| ac_writes | `oltp_write_only_ac` | 21.92ms | 42.29ms | 1.9× | 2.6% | PASS |
| ac_writes | `types_delete_insert_ac` | 20.98ms | 34.72ms | 1.7× | 5.6% | PASS |
| ac_writes | `oltp_read_write_ac` | 24.17ms | 45.95ms | 1.9× | 2.0% | PASS |

</details>

</details>

## Version-control latency

Wall time: 6m 42s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 26.16ms | 38.00ms | 68.9% | 1.5% | PASS |
| `status_dirty_many_tables` | 28.66ms | 42.00ms | 68.2% | 1.1% | PASS |
| `diff_regular_working_one_table` | 25.00ms | 33.00ms | 75.8% | 1.2% | PASS |
| `diff_regular_working_many_tables` | 32.71ms | 50.00ms | 65.4% | 1.8% | PASS |
| `diff_stat_working_many_tables` | 32.45ms | 48.00ms | 67.6% | 1.0% | PASS |
| `diff_schema_working_many_tables` | 32.70ms | 48.00ms | 68.1% | 0.7% | PASS |
| `branch_list_many_branches` | 19.43ms | 25.00ms | 77.7% | 1.1% | PASS |
| `branch_create_delete` | 21.76ms | 27.00ms | 80.6% | 1.9% | PASS |
| `at_literal_deep_history` | 20.80ms | 28.00ms | 74.3% | 0.8% | PASS |
| `diff_literal_deep_history` | 20.77ms | 28.00ms | 74.2% | 0.9% | PASS |
| `history_literal_deep_history` | 21.97ms | 30.00ms | 73.2% | 1.0% | PASS |
| `checkout_branch_clean` | 26.42ms | 43.00ms | 61.4% | 0.9% | PASS |
| `merge_data_no_conflicts` | 23.36ms | 33.00ms | 70.8% | 1.3% | PASS |
| `merge_data_secondary_index` | 761.62ms | 944.00ms | 80.7% | 2.2% | PASS |
| `merge_schema_no_conflicts` | 19.36ms | 24.00ms | 80.7% | 2.1% | PASS |
| `merge_data_conflicts` | 25.95ms | 33.00ms | 78.6% | 0.6% | PASS |
| `merge_data_conflicts_with_resolve` | 26.64ms | 34.00ms | 78.3% | 0.8% | PASS |

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
| copy_fsync | candidate | 469.285 | 399.992–526.597 | 32.003 | 213.09 | 107968301 |
| push_initial | candidate | 807.906 | 726.663–851.422 | 359.874 | 123.78 | 107956807 |
| clone_initial | candidate | 897.839 | 890.159–904.203 | 380.634 | 111.38 | 108475105 |
| push_noop | candidate | 0.601 | 0.568–0.623 | 0.320 | — | 0 |
| fetch_noop | candidate | 0.143 | 0.133–0.169 | 0.145 | — | 0 |
| push_incremental | candidate | 72.181 | 68.398–79.338 | 50.160 | 13.85 | 7909775 |
| fetch_incremental | candidate | 80.184 | 73.862–104.875 | 59.112 | 12.47 | 7910084 |
| pull_fast_forward | candidate | 78.606 | 72.588–165.259 | 59.107 | 12.72 | 7910844 |

<details>
<summary>Memory and file I/O</summary>

Successful VFS bytes and VFS requests cover local and remote files during the operation; they include OS-cache hits and are not physical disk traffic. Peak RSS covers the fresh SQL process, including database open. The Python copy control is not instrumented.

| Operation | Binary | Peak RSS MiB | Read MB | Written MB | Read requests | Write requests |
|---|---|---:|---:|---:|---:|---:|
| push_initial | candidate | 90.60 | 175.487 | 108.763 | 18329 | 2066 |
| clone_initial | candidate | 91.52 | 215.906 | 110.833 | 27898 | 2353 |
| push_noop | candidate | 41.60 | 0.073 | 0.000 | 27 | 2 |
| fetch_noop | candidate | 41.60 | 0.068 | 0.000 | 22 | 0 |
| push_incremental | candidate | 41.60 | 10.738 | 7.910 | 1374 | 125 |
| fetch_incremental | candidate | 41.60 | 26.494 | 7.910 | 7016 | 124 |
| pull_fast_forward | candidate | 41.60 | 26.497 | 8.959 | 7032 | 142 |

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
| copy_fsync | candidate | 2681.017 | 2634.617–2682.071 | 382.508 | 372.99 | 1087718066 |
| push_initial | candidate | 7216.181 | 7139.631–7246.396 | 4979.060 | 138.58 | 1079566310 |
| clone_initial | candidate | 5450.819 | 5393.447–5465.409 | 5181.637 | 183.46 | 1102407396 |
| push_noop | candidate | 0.774 | 0.719–0.783 | 0.322 | — | 0 |
| fetch_noop | candidate | 0.149 | 0.149–0.159 | 0.152 | — | 0 |
| push_incremental | candidate | 243.995 | 241.713–245.848 | 239.377 | 4.10 | 15806003 |
| fetch_incremental | candidate | 255.465 | 254.202–256.602 | 249.957 | 3.91 | 15806513 |
| pull_fast_forward | candidate | 258.117 | 257.092–258.614 | 251.352 | 3.87 | 15807273 |

<details>
<summary>Memory and file I/O</summary>

Successful VFS bytes and VFS requests cover local and remote files during the operation; they include OS-cache hits and are not physical disk traffic. Peak RSS covers the fresh SQL process, including database open. The Python copy control is not instrumented.

| Operation | Binary | Peak RSS MiB | Read MB | Written MB | Read requests | Write requests |
|---|---|---:|---:|---:|---:|---:|
| push_initial | candidate | 135.99 | 2091.224 | 1087.622 | 263338 | 20541 |
| clone_initial | candidate | 131.55 | 2178.022 | 1133.868 | 284004 | 32259 |
| push_noop | candidate | 44.45 | 0.078 | 0.000 | 28 | 2 |
| fetch_noop | candidate | 44.45 | 0.069 | 0.000 | 22 | 0 |
| push_incremental | candidate | 56.23 | 46.657 | 15.928 | 10095 | 313 |
| fetch_incremental | candidate | 50.48 | 62.237 | 15.928 | 13894 | 313 |
| pull_fast_forward | candidate | 50.48 | 62.240 | 16.978 | 13910 | 331 |

</details>


</details>

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh`, `test/vc_perf_ceiling.sh`, and `test/remote_perf.py`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
