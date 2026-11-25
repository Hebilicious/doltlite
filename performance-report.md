# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-25 21:22 UTC
>
> Commit: [`37f20e96afeff37eb3afa463f9639df9eaf65a29`](https://github.com/dolthub/doltlite/commit/37f20e96afeff37eb3afa463f9639df9eaf65a29)
>
> Runner: ubuntu24 20261116.376.1
>
> [GitHub Actions run](https://github.com/Hebilicious/doltlite/actions/runs/45685888245)

The SQL workloads compare optimized DoltLite against stock SQLite on the same GitHub-hosted runner, alternating execution order on each repetition. Version-control sections measure DoltLite latency. Reported timings are medians. Paired-ratio noise is the median absolute deviation of the paired DoltLite/SQLite ratios, expressed as a percentage.

## SQL workload summary

The primary view aggregates all key shapes and compares DoltLite with SQLite by storage mode and operation class. Mean multiple is the average over workloads of each workload's paired DoltLite/SQLite ratio, so every workload counts equally whatever its run time.

### In-memory

| Operation | Mean multiple | Paired-ratio noise | Result |
|---|---:|---:|---|
| Reads | 1.10× | 1.1% | **PASS** |
| Writes | 1.64× | 1.0% | **PASS** |

### File-backed

| Operation | Mean multiple | Paired-ratio noise | Result |
|---|---:|---:|---|
| Reads | 0.91× | 1.1% | **PASS** |
| Writes | 1.34× | 1.7% | **PASS** |
| Autocommit writes | 2.09× | 2.3% | **PASS** |

The absolute ceiling is 2.3× per ordinary workload and 1.8× for a section average. Durable autocommit writes use 3.6× and 2.6× ceilings respectively. A breach counts only when the median of the paired ratios exceeds the same ceiling.

<details>
<summary>Key-shape and individual-workload breakdown</summary>

The integer, text, blob, and composite primary-key runs verify that performance holds across key shapes.

| Storage | Operation | Key shape | Workloads | Samples/workload | Mean multiple | Paired-ratio noise | Result |
|---|---|---|---:|---:|---:|---:|---|
| In-memory | Reads | int | 15 | 55 | 1.12× | 1.0% | **PASS** |
| In-memory | Reads | textpk | 15 | 55 | 1.07× | 1.1% | **PASS** |
| In-memory | Reads | blobpk | 15 | 55 | 1.09× | 1.3% | **PASS** |
| In-memory | Reads | compositepk | 15 | 55 | 1.14× | 1.0% | **PASS** |
| In-memory | Writes | int | 8 | 55 | 1.56× | 0.9% | **PASS** |
| In-memory | Writes | textpk | 8 | 55 | 1.67× | 1.1% | **PASS** |
| In-memory | Writes | blobpk | 8 | 55 | 1.75× | 1.1% | **PASS** |
| In-memory | Writes | compositepk | 8 | 55 | 1.60× | 0.9% | **PASS** |
| File-backed | Reads | int | 15 | 55 | 0.92× | 1.2% | **PASS** |
| File-backed | Reads | textpk | 15 | 55 | 0.91× | 1.1% | **PASS** |
| File-backed | Reads | blobpk | 15 | 55 | 0.82× | 1.3% | **PASS** |
| File-backed | Reads | compositepk | 15 | 55 | 0.97× | 0.8% | **PASS** |
| File-backed | Writes | int | 8 | 55 | 1.33× | 1.8% | **PASS** |
| File-backed | Writes | textpk | 8 | 55 | 1.38× | 1.8% | **PASS** |
| File-backed | Writes | blobpk | 8 | 55 | 1.30× | 1.5% | **PASS** |
| File-backed | Writes | compositepk | 8 | 55 | 1.35× | 1.6% | **PASS** |
| File-backed | Autocommit reads | int | 15 | 55 | 1.06× | 1.2% | **PASS** |
| File-backed | Autocommit reads | textpk | 15 | 55 | 1.02× | 1.0% | **PASS** |
| File-backed | Autocommit reads | blobpk | 15 | 55 | 1.03× | 1.4% | **PASS** |
| File-backed | Autocommit reads | compositepk | 15 | 55 | 1.09× | 1.1% | **PASS** |
| File-backed | Autocommit writes | int | 8 | 55 | 2.11× | 2.3% | **PASS** |
| File-backed | Autocommit writes | textpk | 8 | 55 | 2.12× | 1.5% | **PASS** |
| File-backed | Autocommit writes | blobpk | 8 | 55 | 2.01× | 5.0% | **PASS** |
| File-backed | Autocommit writes | compositepk | 8 | 55 | 2.11× | 2.0% | **PASS** |

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 15.91ms | 17.90ms | 1.1× | 0.8% | PASS |
| mem_reads | `oltp_range_select` | 7.96ms | 8.60ms | 1.1× | 1.3% | PASS |
| mem_reads | `oltp_sum_range` | 7.41ms | 8.30ms | 1.1× | 1.0% | PASS |
| mem_reads | `oltp_order_range` | 2.05ms | 2.17ms | 1.1× | 1.4% | PASS |
| mem_reads | `oltp_distinct_range` | 2.69ms | 2.89ms | 1.1× | 0.8% | PASS |
| mem_reads | `oltp_index_scan` | 2.87ms | 3.77ms | 1.3× | 1.5% | PASS |
| mem_reads | `select_random_points` | 10.75ms | 9.59ms | 0.9× | 1.3% | PASS |
| mem_reads | `select_random_ranges` | 3.21ms | 3.31ms | 1.0× | 1.4% | PASS |
| mem_reads | `covering_index_scan` | 4.79ms | 6.71ms | 1.4× | 0.9% | PASS |
| mem_reads | `groupby_scan` | 21.47ms | 22.50ms | 1.0× | 0.8% | PASS |
| mem_reads | `index_join` | 4.24ms | 6.22ms | 1.5× | 1.4% | PASS |
| mem_reads | `index_join_scan` | 2.38ms | 4.13ms | 1.7× | 2.6% | PASS |
| mem_reads | `types_table_scan` | 757.11ms | 504.38ms | 0.7× | 0.3% | PASS |
| mem_reads | `table_scan` | 852.72ms | 580.44ms | 0.7× | 0.5% | PASS |
| mem_reads | `oltp_read_only` | 70.16ms | 78.57ms | 1.1× | 0.4% | PASS |
| mem_writes | `oltp_bulk_insert` | 108.19ms | 157.75ms | 1.5× | 0.4% | PASS |
| mem_writes | `oltp_insert` | 9.66ms | 15.80ms | 1.6× | 0.5% | PASS |
| mem_writes | `oltp_update_index` | 33.71ms | 59.13ms | 1.8× | 1.2% | PASS |
| mem_writes | `oltp_update_non_index` | 22.29ms | 34.66ms | 1.6× | 0.9% | PASS |
| mem_writes | `oltp_delete_insert` | 30.83ms | 46.78ms | 1.5× | 0.8% | PASS |
| mem_writes | `oltp_write_only` | 15.04ms | 27.61ms | 1.8× | 1.1% | PASS |
| mem_writes | `types_delete_insert` | 16.07ms | 19.68ms | 1.2× | 0.9% | PASS |
| mem_writes | `oltp_read_write` | 43.24ms | 63.71ms | 1.5× | 1.0% | PASS |
| file_reads | `oltp_point_select` | 36.92ms | 22.30ms | 0.6× | 0.7% | PASS |
| file_reads | `oltp_range_select` | 9.91ms | 9.05ms | 0.9× | 1.7% | PASS |
| file_reads | `oltp_sum_range` | 9.26ms | 8.68ms | 0.9× | 1.2% | PASS |
| file_reads | `oltp_order_range` | 2.23ms | 2.19ms | 1.0× | 2.5% | PASS |
| file_reads | `oltp_distinct_range` | 2.91ms | 2.93ms | 1.0× | 2.6% | PASS |
| file_reads | `oltp_index_scan` | 5.06ms | 4.23ms | 0.8× | 1.2% | PASS |
| file_reads | `select_random_points` | 12.77ms | 10.04ms | 0.8× | 1.3% | PASS |
| file_reads | `select_random_ranges` | 5.33ms | 3.71ms | 0.7× | 0.9% | PASS |
| file_reads | `covering_index_scan` | 7.05ms | 7.19ms | 1.0× | 0.5% | PASS |
| file_reads | `groupby_scan` | 22.10ms | 22.48ms | 1.0× | 0.6% | PASS |
| file_reads | `index_join` | 5.48ms | 6.47ms | 1.2× | 1.3% | PASS |
| file_reads | `index_join_scan` | 2.64ms | 4.34ms | 1.6× | 2.4% | PASS |
| file_reads | `types_table_scan` | 756.48ms | 504.03ms | 0.7× | 0.3% | PASS |
| file_reads | `table_scan` | 860.05ms | 581.13ms | 0.7× | 0.7% | PASS |
| file_reads | `oltp_read_only` | 102.78ms | 87.51ms | 0.9× | 1.1% | PASS |
| file_writes | `oltp_bulk_insert` | 117.94ms | 170.29ms | 1.4× | 1.1% | PASS |
| file_writes | `oltp_insert` | 13.10ms | 18.74ms | 1.4× | 2.1% | PASS |
| file_writes | `oltp_update_index` | 48.99ms | 73.03ms | 1.5× | 1.8% | PASS |
| file_writes | `oltp_update_non_index` | 35.01ms | 41.64ms | 1.2× | 2.1% | PASS |
| file_writes | `oltp_delete_insert` | 43.27ms | 56.94ms | 1.3× | 1.7% | PASS |
| file_writes | `oltp_write_only` | 26.88ms | 37.75ms | 1.4× | 2.1% | PASS |
| file_writes | `types_delete_insert` | 24.37ms | 25.48ms | 1.0× | 1.7% | PASS |
| file_writes | `oltp_read_write` | 57.48ms | 75.97ms | 1.3× | 1.8% | PASS |
| ac_reads | `oltp_point_select` | 23.27ms | 23.14ms | 1.0× | 0.8% | PASS |
| ac_reads | `oltp_range_select` | 8.99ms | 9.24ms | 1.0× | 1.5% | PASS |
| ac_reads | `oltp_sum_range` | 8.07ms | 8.86ms | 1.1× | 1.0% | PASS |
| ac_reads | `oltp_order_range` | 2.12ms | 2.20ms | 1.0× | 2.0% | PASS |
| ac_reads | `oltp_distinct_range` | 2.83ms | 2.92ms | 1.0× | 1.2% | PASS |
| ac_reads | `oltp_index_scan` | 3.64ms | 4.20ms | 1.2× | 1.3% | PASS |
| ac_reads | `select_random_points` | 11.42ms | 10.02ms | 0.9× | 1.5% | PASS |
| ac_reads | `select_random_ranges` | 3.89ms | 3.68ms | 0.9× | 1.5% | PASS |
| ac_reads | `covering_index_scan` | 5.55ms | 7.15ms | 1.3× | 0.5% | PASS |
| ac_reads | `groupby_scan` | 21.80ms | 22.53ms | 1.0× | 0.9% | PASS |
| ac_reads | `index_join` | 4.82ms | 6.55ms | 1.4× | 1.6% | PASS |
| ac_reads | `index_join_scan` | 2.60ms | 4.38ms | 1.7× | 2.1% | PASS |
| ac_reads | `types_table_scan` | 759.01ms | 505.41ms | 0.7× | 0.3% | PASS |
| ac_reads | `table_scan` | 857.56ms | 581.28ms | 0.7× | 0.4% | PASS |
| ac_reads | `oltp_read_only` | 80.39ms | 86.10ms | 1.1× | 0.7% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 8.33ms | 13.09ms | 1.6× | 2.3% | PASS |
| ac_writes | `oltp_insert_ac` | 9.42ms | 19.46ms | 2.1× | 1.9% | PASS |
| ac_writes | `oltp_update_index_ac` | 10.27ms | 28.04ms | 2.7× | 1.9% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 8.45ms | 16.05ms | 1.9× | 2.6% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 9.41ms | 22.23ms | 2.4× | 3.0% | PASS |
| ac_writes | `oltp_write_only_ac` | 9.51ms | 21.94ms | 2.3× | 2.1% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.31ms | 16.41ms | 2.0× | 2.3% | PASS |
| ac_writes | `oltp_read_write_ac` | 11.87ms | 25.43ms | 2.1× | 2.4% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.51ms | 24.84ms | 1.0× | 1.3% | PASS |
| mem_reads | `oltp_range_select` | 12.40ms | 11.52ms | 0.9× | 0.9% | PASS |
| mem_reads | `oltp_sum_range` | 12.47ms | 10.99ms | 0.9× | 1.3% | PASS |
| mem_reads | `oltp_order_range` | 2.53ms | 2.52ms | 1.0× | 1.0% | PASS |
| mem_reads | `oltp_distinct_range` | 3.19ms | 3.26ms | 1.0× | 1.0% | PASS |
| mem_reads | `oltp_index_scan` | 2.96ms | 4.82ms | 1.6× | 2.1% | PASS |
| mem_reads | `select_random_points` | 18.86ms | 17.33ms | 0.9× | 1.2% | PASS |
| mem_reads | `select_random_ranges` | 4.91ms | 4.52ms | 0.9× | 1.1% | PASS |
| mem_reads | `covering_index_scan` | 4.84ms | 7.14ms | 1.5× | 0.6% | PASS |
| mem_reads | `groupby_scan` | 24.04ms | 24.57ms | 1.0× | 0.4% | PASS |
| mem_reads | `index_join` | 9.51ms | 7.46ms | 0.8× | 2.1% | PASS |
| mem_reads | `index_join_scan` | 2.65ms | 5.31ms | 2.0× | 2.9% | PASS |
| mem_reads | `types_table_scan` | 768.83ms | 578.66ms | 0.8× | 0.9% | PASS |
| mem_reads | `table_scan` | 889.98ms | 648.92ms | 0.7× | 1.3% | PASS |
| mem_reads | `oltp_read_only` | 95.17ms | 96.01ms | 1.0× | 0.7% | PASS |
| mem_writes | `oltp_bulk_insert` | 147.68ms | 198.54ms | 1.3× | 0.3% | PASS |
| mem_writes | `oltp_insert` | 11.65ms | 24.14ms | 2.1× | 0.8% | PASS |
| mem_writes | `oltp_update_index` | 47.33ms | 93.22ms | 2.0× | 1.5% | PASS |
| mem_writes | `oltp_update_non_index` | 34.92ms | 56.96ms | 1.6× | 1.4% | PASS |
| mem_writes | `oltp_delete_insert` | 39.27ms | 68.86ms | 1.8× | 1.1% | PASS |
| mem_writes | `oltp_write_only` | 20.55ms | 40.13ms | 2.0× | 1.2% | PASS |
| mem_writes | `types_delete_insert` | 28.04ms | 33.71ms | 1.2× | 1.2% | PASS |
| mem_writes | `oltp_read_write` | 66.18ms | 91.87ms | 1.4× | 1.0% | PASS |
| file_reads | `oltp_point_select` | 47.09ms | 29.93ms | 0.6× | 0.6% | PASS |
| file_reads | `oltp_range_select` | 14.30ms | 12.04ms | 0.8× | 1.0% | PASS |
| file_reads | `oltp_sum_range` | 14.19ms | 11.52ms | 0.8× | 1.2% | PASS |
| file_reads | `oltp_order_range` | 2.71ms | 2.54ms | 0.9× | 1.7% | PASS |
| file_reads | `oltp_distinct_range` | 3.38ms | 3.26ms | 1.0× | 1.3% | PASS |
| file_reads | `oltp_index_scan` | 5.12ms | 5.14ms | 1.0× | 1.2% | PASS |
| file_reads | `select_random_points` | 20.94ms | 17.43ms | 0.8× | 0.9% | PASS |
| file_reads | `select_random_ranges` | 6.99ms | 4.99ms | 0.7× | 1.0% | PASS |
| file_reads | `covering_index_scan` | 7.08ms | 7.62ms | 1.1× | 0.7% | PASS |
| file_reads | `groupby_scan` | 23.86ms | 24.43ms | 1.0× | 0.5% | PASS |
| file_reads | `index_join` | 10.71ms | 7.94ms | 0.7× | 1.8% | PASS |
| file_reads | `index_join_scan` | 3.08ms | 5.63ms | 1.8× | 2.5% | PASS |
| file_reads | `types_table_scan` | 776.37ms | 579.21ms | 0.7× | 0.8% | PASS |
| file_reads | `table_scan` | 900.37ms | 652.39ms | 0.7× | 1.6% | PASS |
| file_reads | `oltp_read_only` | 141.11ms | 110.49ms | 0.8× | 1.1% | PASS |
| file_writes | `oltp_bulk_insert` | 164.21ms | 215.13ms | 1.3× | 1.0% | PASS |
| file_writes | `oltp_insert` | 15.79ms | 29.69ms | 1.9× | 1.6% | PASS |
| file_writes | `oltp_update_index` | 70.39ms | 116.34ms | 1.7× | 1.6% | PASS |
| file_writes | `oltp_update_non_index` | 54.03ms | 69.61ms | 1.3× | 2.2% | PASS |
| file_writes | `oltp_delete_insert` | 61.67ms | 84.10ms | 1.4× | 2.2% | PASS |
| file_writes | `oltp_write_only` | 37.09ms | 51.44ms | 1.4× | 1.7% | PASS |
| file_writes | `types_delete_insert` | 46.27ms | 42.97ms | 0.9× | 3.1% | PASS |
| file_writes | `oltp_read_write` | 84.67ms | 103.42ms | 1.2× | 1.8% | PASS |
| ac_reads | `oltp_point_select` | 32.76ms | 29.91ms | 0.9× | 1.2% | PASS |
| ac_reads | `oltp_range_select` | 13.46ms | 12.14ms | 0.9× | 1.2% | PASS |
| ac_reads | `oltp_sum_range` | 13.39ms | 11.59ms | 0.9× | 0.7% | PASS |
| ac_reads | `oltp_order_range` | 2.65ms | 2.55ms | 1.0× | 0.8% | PASS |
| ac_reads | `oltp_distinct_range` | 3.32ms | 3.28ms | 1.0× | 1.0% | PASS |
| ac_reads | `oltp_index_scan` | 3.83ms | 5.25ms | 1.4× | 1.3% | PASS |
| ac_reads | `select_random_points` | 20.11ms | 17.70ms | 0.9× | 1.1% | PASS |
| ac_reads | `select_random_ranges` | 5.85ms | 5.07ms | 0.9× | 1.0% | PASS |
| ac_reads | `covering_index_scan` | 5.61ms | 7.67ms | 1.4× | 0.5% | PASS |
| ac_reads | `groupby_scan` | 23.82ms | 24.42ms | 1.0× | 0.4% | PASS |
| ac_reads | `index_join` | 10.36ms | 8.07ms | 0.8× | 1.6% | PASS |
| ac_reads | `index_join_scan` | 3.04ms | 5.71ms | 1.9× | 1.5% | PASS |
| ac_reads | `types_table_scan` | 777.06ms | 579.61ms | 0.7× | 0.8% | PASS |
| ac_reads | `table_scan` | 868.35ms | 649.40ms | 0.7× | 0.9% | PASS |
| ac_reads | `oltp_read_only` | 106.76ms | 103.55ms | 1.0× | 0.6% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 9.05ms | 12.69ms | 1.4× | 0.6% | PASS |
| ac_writes | `oltp_insert_ac` | 10.24ms | 18.76ms | 1.8× | 0.8% | PASS |
| ac_writes | `oltp_update_index_ac` | 10.65ms | 32.70ms | 3.1× | 2.4% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 8.85ms | 16.55ms | 1.9× | 1.2% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 10.10ms | 25.45ms | 2.5× | 1.7% | PASS |
| ac_writes | `oltp_write_only_ac` | 9.93ms | 23.50ms | 2.4× | 1.8% | PASS |
| ac_writes | `types_delete_insert_ac` | 9.24ms | 17.11ms | 1.9× | 2.3% | PASS |
| ac_writes | `oltp_read_write_ac` | 13.56ms | 28.47ms | 2.1× | 1.0% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 34.44ms | 36.62ms | 1.1× | 1.7% | PASS |
| mem_reads | `oltp_range_select` | 15.97ms | 14.99ms | 0.9× | 2.2% | PASS |
| mem_reads | `oltp_sum_range` | 14.41ms | 14.48ms | 1.0× | 1.3% | PASS |
| mem_reads | `oltp_order_range` | 3.15ms | 3.26ms | 1.0× | 1.3% | PASS |
| mem_reads | `oltp_distinct_range` | 4.26ms | 4.36ms | 1.0× | 1.2% | PASS |
| mem_reads | `oltp_index_scan` | 3.89ms | 6.12ms | 1.6× | 1.6% | PASS |
| mem_reads | `select_random_points` | 21.19ms | 20.75ms | 1.0× | 2.1% | PASS |
| mem_reads | `select_random_ranges` | 6.33ms | 6.43ms | 1.0× | 1.9% | PASS |
| mem_reads | `covering_index_scan` | 7.68ms | 10.86ms | 1.4× | 0.9% | PASS |
| mem_reads | `groupby_scan` | 33.94ms | 34.05ms | 1.0× | 0.7% | PASS |
| mem_reads | `index_join` | 10.10ms | 9.77ms | 1.0× | 1.5% | PASS |
| mem_reads | `index_join_scan` | 3.76ms | 6.12ms | 1.6× | 1.7% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 873.57ms | 0.8× | 0.5% | PASS |
| mem_reads | `table_scan` | 1.15s | 971.31ms | 0.8× | 0.3% | PASS |
| mem_reads | `oltp_read_only` | 133.98ms | 138.18ms | 1.0× | 1.2% | PASS |
| mem_writes | `oltp_bulk_insert` | 239.96ms | 336.19ms | 1.4× | 1.0% | PASS |
| mem_writes | `oltp_insert` | 18.32ms | 38.02ms | 2.1× | 0.9% | PASS |
| mem_writes | `oltp_update_index` | 63.72ms | 131.32ms | 2.1× | 1.1% | PASS |
| mem_writes | `oltp_update_non_index` | 48.03ms | 81.46ms | 1.7× | 1.5% | PASS |
| mem_writes | `oltp_delete_insert` | 52.22ms | 98.41ms | 1.9× | 1.0% | PASS |
| mem_writes | `oltp_write_only` | 27.48ms | 58.15ms | 2.1× | 1.2% | PASS |
| mem_writes | `types_delete_insert` | 38.08ms | 49.78ms | 1.3× | 1.5% | PASS |
| mem_writes | `oltp_read_write` | 93.91ms | 135.81ms | 1.4× | 1.4% | PASS |
| file_reads | `oltp_point_select` | 105.18ms | 54.93ms | 0.5× | 0.7% | PASS |
| file_reads | `oltp_range_select` | 23.61ms | 17.04ms | 0.7× | 1.8% | PASS |
| file_reads | `oltp_sum_range` | 21.89ms | 16.60ms | 0.8× | 1.5% | PASS |
| file_reads | `oltp_order_range` | 4.18ms | 3.60ms | 0.9× | 1.7% | PASS |
| file_reads | `oltp_distinct_range` | 5.33ms | 4.72ms | 0.9× | 1.8% | PASS |
| file_reads | `oltp_index_scan` | 11.12ms | 8.34ms | 0.7× | 1.1% | PASS |
| file_reads | `select_random_points` | 29.84ms | 23.53ms | 0.8× | 1.9% | PASS |
| file_reads | `select_random_ranges` | 13.88ms | 8.48ms | 0.6× | 1.3% | PASS |
| file_reads | `covering_index_scan` | 15.07ms | 13.20ms | 0.9× | 0.8% | PASS |
| file_reads | `groupby_scan` | 34.98ms | 34.47ms | 1.0× | 0.5% | PASS |
| file_reads | `index_join` | 14.46ms | 11.68ms | 0.8× | 1.3% | PASS |
| file_reads | `index_join_scan` | 4.74ms | 6.54ms | 1.4× | 2.1% | PASS |
| file_reads | `types_table_scan` | 1.03s | 877.15ms | 0.9× | 0.6% | PASS |
| file_reads | `table_scan` | 1.15s | 972.58ms | 0.8× | 0.6% | PASS |
| file_reads | `oltp_read_only` | 243.07ms | 166.54ms | 0.7× | 0.9% | PASS |
| file_writes | `oltp_bulk_insert` | 261.94ms | 353.83ms | 1.4× | 0.8% | PASS |
| file_writes | `oltp_insert` | 25.64ms | 43.56ms | 1.7× | 1.4% | PASS |
| file_writes | `oltp_update_index` | 97.48ms | 151.63ms | 1.6× | 1.6% | PASS |
| file_writes | `oltp_update_non_index` | 89.93ms | 96.54ms | 1.1× | 11.2% | PASS |
| file_writes | `oltp_delete_insert` | 86.61ms | 115.30ms | 1.3× | 1.3% | PASS |
| file_writes | `oltp_write_only` | 57.92ms | 73.04ms | 1.3× | 2.5% | PASS |
| file_writes | `types_delete_insert` | 64.09ms | 60.72ms | 0.9× | 1.5% | PASS |
| file_writes | `oltp_read_write` | 126.88ms | 150.33ms | 1.2× | 0.8% | PASS |
| ac_reads | `oltp_point_select` | 58.21ms | 54.98ms | 0.9× | 1.2% | PASS |
| ac_reads | `oltp_range_select` | 19.13ms | 17.15ms | 0.9× | 1.4% | PASS |
| ac_reads | `oltp_sum_range` | 17.44ms | 16.65ms | 1.0× | 1.4% | PASS |
| ac_reads | `oltp_order_range` | 3.75ms | 3.63ms | 1.0× | 1.8% | PASS |
| ac_reads | `oltp_distinct_range` | 4.79ms | 4.72ms | 1.0× | 1.4% | PASS |
| ac_reads | `oltp_index_scan` | 6.43ms | 8.38ms | 1.3× | 1.9% | PASS |
| ac_reads | `select_random_points` | 24.80ms | 23.54ms | 0.9× | 2.0% | PASS |
| ac_reads | `select_random_ranges` | 9.04ms | 8.54ms | 0.9× | 2.0% | PASS |
| ac_reads | `covering_index_scan` | 10.25ms | 13.21ms | 1.3× | 1.1% | PASS |
| ac_reads | `groupby_scan` | 34.48ms | 34.53ms | 1.0× | 0.8% | PASS |
| ac_reads | `index_join` | 12.03ms | 11.72ms | 1.0× | 1.6% | PASS |
| ac_reads | `index_join_scan` | 4.22ms | 6.62ms | 1.6× | 1.9% | PASS |
| ac_reads | `types_table_scan` | 1.03s | 876.49ms | 0.9× | 0.4% | PASS |
| ac_reads | `table_scan` | 1.15s | 972.31ms | 0.8× | 0.6% | PASS |
| ac_reads | `oltp_read_only` | 172.39ms | 166.67ms | 1.0× | 1.1% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.06ms | 33.66ms | 1.5× | 4.7% | PASS |
| ac_writes | `oltp_insert_ac` | 25.34ms | 52.85ms | 2.1× | 4.8% | PASS |
| ac_writes | `oltp_update_index_ac` | 26.61ms | 67.48ms | 2.5× | 5.8% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 21.60ms | 41.48ms | 1.9× | 5.2% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.66ms | 54.69ms | 2.1× | 7.2% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.66ms | 52.97ms | 2.1× | 5.3% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.89ms | 42.80ms | 1.9× | 4.5% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.68ms | 61.38ms | 1.9× | 4.5% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 21.96ms | 24.61ms | 1.1× | 0.8% | PASS |
| mem_reads | `oltp_range_select` | 14.51ms | 15.49ms | 1.1× | 0.9% | PASS |
| mem_reads | `oltp_sum_range` | 13.74ms | 14.06ms | 1.0× | 0.6% | PASS |
| mem_reads | `oltp_order_range` | 2.66ms | 2.78ms | 1.0× | 1.3% | PASS |
| mem_reads | `oltp_distinct_range` | 3.31ms | 3.48ms | 1.1× | 1.0% | PASS |
| mem_reads | `oltp_index_scan` | 3.15ms | 4.09ms | 1.3× | 1.5% | PASS |
| mem_reads | `select_random_points` | 20.69ms | 21.30ms | 1.0× | 1.0% | PASS |
| mem_reads | `select_random_ranges` | 5.18ms | 5.98ms | 1.2× | 1.2% | PASS |
| mem_reads | `covering_index_scan` | 4.80ms | 7.02ms | 1.5× | 1.1% | PASS |
| mem_reads | `groupby_scan` | 25.23ms | 27.14ms | 1.1× | 0.5% | PASS |
| mem_reads | `index_join` | 5.83ms | 8.47ms | 1.5× | 0.7% | PASS |
| mem_reads | `index_join_scan` | 2.95ms | 4.99ms | 1.7× | 2.8% | PASS |
| mem_reads | `types_table_scan` | 775.76ms | 559.47ms | 0.7× | 0.4% | PASS |
| mem_reads | `table_scan` | 891.47ms | 636.21ms | 0.7× | 1.0% | PASS |
| mem_reads | `oltp_read_only` | 100.44ms | 111.96ms | 1.1× | 0.6% | PASS |
| mem_writes | `oltp_bulk_insert` | 151.37ms | 191.03ms | 1.3× | 0.8% | PASS |
| mem_writes | `oltp_insert` | 12.31ms | 20.18ms | 1.6× | 0.5% | PASS |
| mem_writes | `oltp_update_index` | 44.46ms | 77.48ms | 1.7× | 1.7% | PASS |
| mem_writes | `oltp_update_non_index` | 32.88ms | 50.86ms | 1.5× | 0.9% | PASS |
| mem_writes | `oltp_delete_insert` | 34.38ms | 60.84ms | 1.8× | 1.3% | PASS |
| mem_writes | `oltp_write_only` | 19.14ms | 36.15ms | 1.9× | 1.0% | PASS |
| mem_writes | `types_delete_insert` | 21.86ms | 31.97ms | 1.5× | 0.6% | PASS |
| mem_writes | `oltp_read_write` | 67.94ms | 97.52ms | 1.4× | 0.7% | PASS |
| file_reads | `oltp_point_select` | 44.24ms | 29.94ms | 0.7× | 0.8% | PASS |
| file_reads | `oltp_range_select` | 16.91ms | 16.01ms | 0.9× | 0.8% | PASS |
| file_reads | `oltp_sum_range` | 16.24ms | 14.71ms | 0.9× | 1.1% | PASS |
| file_reads | `oltp_order_range` | 2.93ms | 2.92ms | 1.0× | 1.1% | PASS |
| file_reads | `oltp_distinct_range` | 3.63ms | 3.64ms | 1.0× | 1.3% | PASS |
| file_reads | `oltp_index_scan` | 5.46ms | 4.90ms | 0.9× | 1.7% | PASS |
| file_reads | `select_random_points` | 23.03ms | 22.00ms | 1.0× | 0.4% | PASS |
| file_reads | `select_random_ranges` | 7.43ms | 6.56ms | 0.9× | 0.7% | PASS |
| file_reads | `covering_index_scan` | 7.08ms | 7.53ms | 1.1× | 0.6% | PASS |
| file_reads | `groupby_scan` | 25.41ms | 27.24ms | 1.1× | 0.3% | PASS |
| file_reads | `index_join` | 7.10ms | 8.66ms | 1.2× | 1.1% | PASS |
| file_reads | `index_join_scan` | 3.17ms | 5.01ms | 1.6× | 2.2% | PASS |
| file_reads | `types_table_scan` | 772.98ms | 558.73ms | 0.7× | 0.3% | PASS |
| file_reads | `table_scan` | 869.89ms | 634.49ms | 0.7× | 1.4% | PASS |
| file_reads | `oltp_read_only` | 129.78ms | 118.20ms | 0.9× | 0.5% | PASS |
| file_writes | `oltp_bulk_insert` | 161.20ms | 200.60ms | 1.2× | 0.9% | PASS |
| file_writes | `oltp_insert` | 15.84ms | 24.10ms | 1.5× | 1.5% | PASS |
| file_writes | `oltp_update_index` | 58.75ms | 86.66ms | 1.5× | 1.6% | PASS |
| file_writes | `oltp_update_non_index` | 45.00ms | 56.29ms | 1.3× | 1.7% | PASS |
| file_writes | `oltp_delete_insert` | 46.76ms | 66.19ms | 1.4× | 2.3% | PASS |
| file_writes | `oltp_write_only` | 29.99ms | 42.10ms | 1.4× | 1.7% | PASS |
| file_writes | `types_delete_insert` | 29.69ms | 36.72ms | 1.2× | 1.8% | PASS |
| file_writes | `oltp_read_write` | 77.72ms | 101.92ms | 1.3× | 0.9% | PASS |
| ac_reads | `oltp_point_select` | 28.58ms | 29.32ms | 1.0× | 0.7% | PASS |
| ac_reads | `oltp_range_select` | 14.74ms | 15.78ms | 1.1× | 0.6% | PASS |
| ac_reads | `oltp_sum_range` | 14.01ms | 14.48ms | 1.0× | 0.9% | PASS |
| ac_reads | `oltp_order_range` | 2.78ms | 2.86ms | 1.0× | 1.6% | PASS |
| ac_reads | `oltp_distinct_range` | 3.52ms | 3.64ms | 1.0× | 1.1% | PASS |
| ac_reads | `oltp_index_scan` | 4.16ms | 5.07ms | 1.2× | 1.7% | PASS |
| ac_reads | `select_random_points` | 22.56ms | 23.04ms | 1.0× | 0.8% | PASS |
| ac_reads | `select_random_ranges` | 6.08ms | 6.74ms | 1.1× | 1.1% | PASS |
| ac_reads | `covering_index_scan` | 5.73ms | 7.71ms | 1.3× | 0.9% | PASS |
| ac_reads | `groupby_scan` | 25.44ms | 27.44ms | 1.1× | 0.4% | PASS |
| ac_reads | `index_join` | 6.38ms | 8.50ms | 1.3× | 1.8% | PASS |
| ac_reads | `index_join_scan` | 3.12ms | 5.00ms | 1.6× | 2.3% | PASS |
| ac_reads | `types_table_scan` | 779.38ms | 560.74ms | 0.7× | 0.5% | PASS |
| ac_reads | `table_scan` | 881.79ms | 638.61ms | 0.7× | 1.8% | PASS |
| ac_reads | `oltp_read_only` | 117.07ms | 120.93ms | 1.0× | 2.1% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 8.90ms | 13.07ms | 1.5× | 2.0% | PASS |
| ac_writes | `oltp_insert_ac` | 10.24ms | 22.45ms | 2.2× | 2.7% | PASS |
| ac_writes | `oltp_update_index_ac` | 10.88ms | 30.12ms | 2.8× | 3.7% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 9.15ms | 16.79ms | 1.8× | 1.8% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 9.90ms | 23.95ms | 2.4× | 2.0% | PASS |
| ac_writes | `oltp_write_only_ac` | 10.10ms | 23.16ms | 2.3× | 1.9% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.77ms | 17.55ms | 2.0× | 1.7% | PASS |
| ac_writes | `oltp_read_write_ac` | 13.51ms | 28.20ms | 2.1× | 1.5% | PASS |

</details>

</details>

## Version-control latency

Wall time: 4m 53s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 23.40ms | 38.00ms | 61.6% | 1.5% | PASS |
| `status_dirty_many_tables` | 26.05ms | 42.00ms | 62.0% | 1.5% | PASS |
| `diff_regular_working_one_table` | 22.08ms | 33.00ms | 66.9% | 1.3% | PASS |
| `diff_regular_working_many_tables` | 28.98ms | 50.00ms | 58.0% | 1.0% | PASS |
| `diff_stat_working_many_tables` | 29.25ms | 48.00ms | 60.9% | 1.3% | PASS |
| `diff_schema_working_many_tables` | 30.01ms | 48.00ms | 62.5% | 1.2% | PASS |
| `branch_list_many_branches` | 16.94ms | 25.00ms | 67.8% | 1.4% | PASS |
| `branch_create_delete` | 18.01ms | 27.00ms | 66.7% | 1.2% | PASS |
| `at_literal_deep_history` | 18.00ms | 28.00ms | 64.3% | 0.9% | PASS |
| `diff_literal_deep_history` | 17.72ms | 28.00ms | 63.3% | 1.4% | PASS |
| `history_literal_deep_history` | 17.73ms | 30.00ms | 59.1% | 1.1% | PASS |
| `checkout_branch_clean` | 22.59ms | 43.00ms | 52.5% | 1.1% | PASS |
| `merge_data_no_conflicts` | 18.85ms | 33.00ms | 57.1% | 1.3% | PASS |
| `merge_data_secondary_index` | 637.87ms | 944.00ms | 67.6% | 1.1% | PASS |
| `merge_schema_no_conflicts` | 15.33ms | 24.00ms | 63.9% | 1.8% | PASS |
| `merge_data_conflicts` | 20.88ms | 33.00ms | 63.3% | 1.1% | PASS |
| `merge_data_conflicts_with_resolve` | 21.67ms | 34.00ms | 63.7% | 1.1% | PASS |

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
| copy_fsync | candidate | 54.452 | 48.741–66.845 | 48.802 | 1836.48 | 107968301 |
| push_initial | candidate | 305.842 | 302.599–311.127 | 304.239 | 326.97 | 107956807 |
| clone_initial | candidate | 321.089 | 318.204–322.574 | 315.942 | 311.44 | 108475105 |
| push_noop | candidate | 0.385 | 0.353–0.397 | 0.192 | — | 0 |
| fetch_noop | candidate | 0.083 | 0.078–0.098 | 0.084 | — | 0 |
| push_incremental | candidate | 43.055 | 42.087–45.145 | 42.083 | 23.23 | 7909775 |
| fetch_incremental | candidate | 49.170 | 48.505–49.990 | 48.199 | 20.34 | 7910084 |
| pull_fast_forward | candidate | 50.458 | 49.633–51.039 | 49.083 | 19.82 | 7910844 |

<details>
<summary>Memory and file I/O</summary>

Successful VFS bytes and VFS requests cover local and remote files during the operation; they include OS-cache hits and are not physical disk traffic. Peak RSS covers the fresh SQL process, including database open. The Python copy control is not instrumented.

| Operation | Binary | Peak RSS MiB | Read MB | Written MB | Read requests | Write requests |
|---|---|---:|---:|---:|---:|---:|
| push_initial | candidate | 90.41 | 175.487 | 108.763 | 18329 | 2066 |
| clone_initial | candidate | 91.40 | 215.906 | 110.833 | 27898 | 2353 |
| push_noop | candidate | 41.58 | 0.073 | 0.000 | 27 | 2 |
| fetch_noop | candidate | 41.58 | 0.068 | 0.000 | 22 | 0 |
| push_incremental | candidate | 41.58 | 10.743 | 7.910 | 1375 | 125 |
| fetch_incremental | candidate | 41.58 | 26.494 | 7.910 | 7016 | 124 |
| pull_fast_forward | candidate | 41.58 | 26.497 | 8.959 | 7032 | 142 |

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
| copy_fsync | candidate | 2653.981 | 2641.089–2679.351 | 389.001 | 376.79 | 1087718066 |
| push_initial | candidate | 7272.312 | 7195.153–7298.975 | 5065.511 | 137.51 | 1079566310 |
| clone_initial | candidate | 5470.448 | 5442.190–5575.829 | 5235.365 | 182.80 | 1102407396 |
| push_noop | candidate | 0.802 | 0.764–0.808 | 0.364 | — | 0 |
| fetch_noop | candidate | 0.173 | 0.149–0.198 | 0.174 | — | 0 |
| push_incremental | candidate | 262.543 | 254.194–268.604 | 257.370 | 3.81 | 15806003 |
| fetch_incremental | candidate | 270.829 | 266.637–276.270 | 260.884 | 3.69 | 15806513 |
| pull_fast_forward | candidate | 267.236 | 263.674–278.470 | 261.174 | 3.74 | 15807273 |

<details>
<summary>Memory and file I/O</summary>

Successful VFS bytes and VFS requests cover local and remote files during the operation; they include OS-cache hits and are not physical disk traffic. Peak RSS covers the fresh SQL process, including database open. The Python copy control is not instrumented.

| Operation | Binary | Peak RSS MiB | Read MB | Written MB | Read requests | Write requests |
|---|---|---:|---:|---:|---:|---:|
| push_initial | candidate | 134.56 | 2091.150 | 1087.622 | 263320 | 20541 |
| clone_initial | candidate | 131.51 | 2177.948 | 1133.868 | 283986 | 32259 |
| push_noop | candidate | 44.38 | 0.078 | 0.000 | 28 | 2 |
| fetch_noop | candidate | 44.38 | 0.069 | 0.000 | 22 | 0 |
| push_incremental | candidate | 56.19 | 47.009 | 15.928 | 10181 | 313 |
| fetch_incremental | candidate | 50.36 | 62.589 | 15.928 | 13980 | 313 |
| pull_fast_forward | candidate | 50.37 | 62.592 | 16.978 | 13996 | 331 |

</details>


</details>

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh`, `test/vc_perf_ceiling.sh`, and `test/remote_perf.py`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
