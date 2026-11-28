# DoltLite Performance Report

> Nightly result: **PASS**
>
> Generated: 2026-11-28 19:28 UTC
>
> Commit: [`37f20e96afeff37eb3afa463f9639df9eaf65a29`](https://github.com/dolthub/doltlite/commit/37f20e96afeff37eb3afa463f9639df9eaf65a29)
>
> Runner: ubuntu24 20261122.23.1
>
> [GitHub Actions run](https://github.com/Hebilicious/doltlite/actions/runs/46172003991)

The SQL workloads compare optimized DoltLite against stock SQLite on the same GitHub-hosted runner, alternating execution order on each repetition. Version-control sections measure DoltLite latency. Reported timings are medians. Paired-ratio noise is the median absolute deviation of the paired DoltLite/SQLite ratios, expressed as a percentage.

## SQL workload summary

The primary view aggregates all key shapes and compares DoltLite with SQLite by storage mode and operation class. Mean multiple is the average over workloads of each workload's paired DoltLite/SQLite ratio, so every workload counts equally whatever its run time.

### In-memory

| Operation | Mean multiple | Paired-ratio noise | Result |
|---|---:|---:|---|
| Reads | 1.11× | 1.2% | **PASS** |
| Writes | 1.64× | 1.1% | **PASS** |

### File-backed

| Operation | Mean multiple | Paired-ratio noise | Result |
|---|---:|---:|---|
| Reads | 0.87× | 1.1% | **PASS** |
| Writes | 1.27× | 1.9% | **PASS** |
| Autocommit writes | 1.96× | 2.6% | **PASS** |

The absolute ceiling is 2.3× per ordinary workload and 1.8× for a section average. Durable autocommit writes use 3.6× and 2.6× ceilings respectively. A breach counts only when the median of the paired ratios exceeds the same ceiling.

<details>
<summary>Key-shape and individual-workload breakdown</summary>

The integer, text, blob, and composite primary-key runs verify that performance holds across key shapes.

| Storage | Operation | Key shape | Workloads | Samples/workload | Mean multiple | Paired-ratio noise | Result |
|---|---|---|---:|---:|---:|---:|---|
| In-memory | Reads | int | 15 | 55 | 1.12× | 1.4% | **PASS** |
| In-memory | Reads | textpk | 15 | 55 | 1.06× | 1.0% | **PASS** |
| In-memory | Reads | blobpk | 15 | 55 | 1.11× | 1.6% | **PASS** |
| In-memory | Reads | compositepk | 15 | 55 | 1.16× | 1.0% | **PASS** |
| In-memory | Writes | int | 8 | 55 | 1.57× | 1.1% | **PASS** |
| In-memory | Writes | textpk | 8 | 55 | 1.66× | 1.0% | **PASS** |
| In-memory | Writes | blobpk | 8 | 55 | 1.67× | 1.2% | **PASS** |
| In-memory | Writes | compositepk | 8 | 55 | 1.67× | 0.9% | **PASS** |
| File-backed | Reads | int | 15 | 55 | 0.92× | 1.1% | **PASS** |
| File-backed | Reads | textpk | 15 | 55 | 0.91× | 1.3% | **PASS** |
| File-backed | Reads | blobpk | 15 | 55 | 0.76× | 0.7% | **PASS** |
| File-backed | Reads | compositepk | 15 | 55 | 0.89× | 1.2% | **PASS** |
| File-backed | Writes | int | 8 | 55 | 1.33× | 1.7% | **PASS** |
| File-backed | Writes | textpk | 8 | 55 | 1.37× | 1.6% | **PASS** |
| File-backed | Writes | blobpk | 8 | 55 | 1.07× | 4.2% | **PASS** |
| File-backed | Writes | compositepk | 8 | 55 | 1.30× | 1.6% | **PASS** |
| File-backed | Autocommit reads | int | 15 | 55 | 1.06× | 1.1% | **PASS** |
| File-backed | Autocommit reads | textpk | 15 | 55 | 1.01× | 1.2% | **PASS** |
| File-backed | Autocommit reads | blobpk | 15 | 55 | 1.01× | 1.1% | **PASS** |
| File-backed | Autocommit reads | compositepk | 15 | 55 | 1.11× | 1.1% | **PASS** |
| File-backed | Autocommit writes | int | 8 | 55 | 2.11× | 1.9% | **PASS** |
| File-backed | Autocommit writes | textpk | 8 | 55 | 2.12× | 2.7% | **PASS** |
| File-backed | Autocommit writes | blobpk | 8 | 55 | 1.65× | 2.2% | **PASS** |
| File-backed | Autocommit writes | compositepk | 8 | 55 | 1.97× | 5.0% | **PASS** |

<details>
<summary>int workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 15.72ms | 17.71ms | 1.1× | 1.2% | PASS |
| mem_reads | `oltp_range_select` | 7.65ms | 8.44ms | 1.1× | 1.8% | PASS |
| mem_reads | `oltp_sum_range` | 7.00ms | 7.98ms | 1.1× | 2.1% | PASS |
| mem_reads | `oltp_order_range` | 1.98ms | 2.09ms | 1.1× | 1.0% | PASS |
| mem_reads | `oltp_distinct_range` | 2.61ms | 2.83ms | 1.1× | 1.5% | PASS |
| mem_reads | `oltp_index_scan` | 2.74ms | 3.49ms | 1.3× | 1.8% | PASS |
| mem_reads | `select_random_points` | 10.33ms | 9.17ms | 0.9× | 1.6% | PASS |
| mem_reads | `select_random_ranges` | 3.10ms | 3.20ms | 1.0× | 1.4% | PASS |
| mem_reads | `covering_index_scan` | 4.78ms | 6.65ms | 1.4× | 0.7% | PASS |
| mem_reads | `groupby_scan` | 21.40ms | 22.59ms | 1.1× | 0.7% | PASS |
| mem_reads | `index_join` | 4.23ms | 6.19ms | 1.5× | 1.4% | PASS |
| mem_reads | `index_join_scan` | 2.32ms | 4.16ms | 1.8× | 2.2% | PASS |
| mem_reads | `types_table_scan` | 757.65ms | 503.14ms | 0.7× | 0.2% | PASS |
| mem_reads | `table_scan` | 853.49ms | 579.84ms | 0.7× | 0.6% | PASS |
| mem_reads | `oltp_read_only` | 70.06ms | 78.65ms | 1.1× | 0.5% | PASS |
| mem_writes | `oltp_bulk_insert` | 108.47ms | 158.89ms | 1.5× | 0.5% | PASS |
| mem_writes | `oltp_insert` | 9.68ms | 15.99ms | 1.7× | 0.7% | PASS |
| mem_writes | `oltp_update_index` | 34.02ms | 60.39ms | 1.8× | 1.1% | PASS |
| mem_writes | `oltp_update_non_index` | 22.26ms | 34.49ms | 1.5× | 1.4% | PASS |
| mem_writes | `oltp_delete_insert` | 30.61ms | 46.61ms | 1.5× | 1.3% | PASS |
| mem_writes | `oltp_write_only` | 14.96ms | 27.60ms | 1.8× | 0.7% | PASS |
| mem_writes | `types_delete_insert` | 16.14ms | 19.77ms | 1.2× | 1.1% | PASS |
| mem_writes | `oltp_read_write` | 44.98ms | 67.30ms | 1.5× | 1.1% | PASS |
| file_reads | `oltp_point_select` | 37.99ms | 23.02ms | 0.6× | 0.7% | PASS |
| file_reads | `oltp_range_select` | 10.39ms | 9.34ms | 0.9× | 1.2% | PASS |
| file_reads | `oltp_sum_range` | 9.58ms | 8.91ms | 0.9× | 0.9% | PASS |
| file_reads | `oltp_order_range` | 2.25ms | 2.19ms | 1.0× | 2.4% | PASS |
| file_reads | `oltp_distinct_range` | 2.90ms | 2.93ms | 1.0× | 1.4% | PASS |
| file_reads | `oltp_index_scan` | 5.04ms | 4.18ms | 0.8× | 1.8% | PASS |
| file_reads | `select_random_points` | 12.49ms | 9.68ms | 0.8× | 2.3% | PASS |
| file_reads | `select_random_ranges` | 5.29ms | 3.71ms | 0.7× | 0.9% | PASS |
| file_reads | `covering_index_scan` | 7.06ms | 7.17ms | 1.0× | 0.5% | PASS |
| file_reads | `groupby_scan` | 22.16ms | 22.55ms | 1.0× | 0.4% | PASS |
| file_reads | `index_join` | 5.47ms | 6.49ms | 1.2× | 1.0% | PASS |
| file_reads | `index_join_scan` | 2.67ms | 4.43ms | 1.7× | 2.7% | PASS |
| file_reads | `types_table_scan` | 759.48ms | 503.14ms | 0.7× | 0.3% | PASS |
| file_reads | `table_scan` | 875.53ms | 581.94ms | 0.7× | 1.1% | PASS |
| file_reads | `oltp_read_only` | 105.77ms | 88.58ms | 0.8× | 1.4% | PASS |
| file_writes | `oltp_bulk_insert` | 118.45ms | 171.60ms | 1.4× | 1.0% | PASS |
| file_writes | `oltp_insert` | 12.98ms | 18.93ms | 1.5× | 1.4% | PASS |
| file_writes | `oltp_update_index` | 48.02ms | 72.04ms | 1.5× | 1.6% | PASS |
| file_writes | `oltp_update_non_index` | 34.30ms | 40.35ms | 1.2× | 1.6% | PASS |
| file_writes | `oltp_delete_insert` | 42.04ms | 54.78ms | 1.3× | 1.7% | PASS |
| file_writes | `oltp_write_only` | 26.04ms | 36.24ms | 1.4× | 2.1% | PASS |
| file_writes | `types_delete_insert` | 24.21ms | 25.47ms | 1.1× | 2.3% | PASS |
| file_writes | `oltp_read_write` | 57.12ms | 75.54ms | 1.3× | 1.8% | PASS |
| ac_reads | `oltp_point_select` | 23.35ms | 23.18ms | 1.0× | 0.7% | PASS |
| ac_reads | `oltp_range_select` | 9.24ms | 9.40ms | 1.0× | 1.0% | PASS |
| ac_reads | `oltp_sum_range` | 8.37ms | 8.97ms | 1.1× | 1.1% | PASS |
| ac_reads | `oltp_order_range` | 2.19ms | 2.24ms | 1.0× | 2.0% | PASS |
| ac_reads | `oltp_distinct_range` | 2.87ms | 2.98ms | 1.0× | 2.2% | PASS |
| ac_reads | `oltp_index_scan` | 3.74ms | 4.38ms | 1.2× | 1.9% | PASS |
| ac_reads | `select_random_points` | 11.78ms | 10.30ms | 0.9× | 1.5% | PASS |
| ac_reads | `select_random_ranges` | 3.98ms | 3.82ms | 1.0× | 1.5% | PASS |
| ac_reads | `covering_index_scan` | 5.59ms | 7.21ms | 1.3× | 0.8% | PASS |
| ac_reads | `groupby_scan` | 21.97ms | 22.61ms | 1.0× | 1.1% | PASS |
| ac_reads | `index_join` | 4.77ms | 6.42ms | 1.3× | 1.7% | PASS |
| ac_reads | `index_join_scan` | 2.53ms | 4.25ms | 1.7× | 2.4% | PASS |
| ac_reads | `types_table_scan` | 755.74ms | 502.41ms | 0.7× | 0.2% | PASS |
| ac_reads | `table_scan` | 855.30ms | 578.13ms | 0.7× | 0.4% | PASS |
| ac_reads | `oltp_read_only` | 80.62ms | 86.08ms | 1.1× | 0.6% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 8.36ms | 13.01ms | 1.6× | 1.9% | PASS |
| ac_writes | `oltp_insert_ac` | 9.44ms | 19.27ms | 2.0× | 1.6% | PASS |
| ac_writes | `oltp_update_index_ac` | 10.30ms | 28.19ms | 2.7× | 1.6% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 8.54ms | 16.12ms | 1.9× | 1.9% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 9.35ms | 21.82ms | 2.3× | 1.8% | PASS |
| ac_writes | `oltp_write_only_ac` | 9.37ms | 21.37ms | 2.3× | 2.3% | PASS |
| ac_writes | `types_delete_insert_ac` | 8.39ms | 16.67ms | 2.0× | 2.8% | PASS |
| ac_writes | `oltp_read_write_ac` | 12.59ms | 26.61ms | 2.1× | 2.6% | PASS |

</details>

<details>
<summary>textpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 25.06ms | 24.52ms | 1.0× | 1.0% | PASS |
| mem_reads | `oltp_range_select` | 12.00ms | 11.23ms | 0.9× | 1.7% | PASS |
| mem_reads | `oltp_sum_range` | 11.62ms | 10.49ms | 0.9× | 2.9% | PASS |
| mem_reads | `oltp_order_range` | 2.40ms | 2.40ms | 1.0× | 1.3% | PASS |
| mem_reads | `oltp_distinct_range` | 3.12ms | 3.15ms | 1.0× | 0.9% | PASS |
| mem_reads | `oltp_index_scan` | 2.85ms | 4.43ms | 1.6× | 2.3% | PASS |
| mem_reads | `select_random_points` | 17.79ms | 16.40ms | 0.9× | 1.6% | PASS |
| mem_reads | `select_random_ranges` | 4.69ms | 4.33ms | 0.9× | 1.3% | PASS |
| mem_reads | `covering_index_scan` | 4.81ms | 6.81ms | 1.4× | 0.8% | PASS |
| mem_reads | `groupby_scan` | 23.36ms | 24.05ms | 1.0× | 0.9% | PASS |
| mem_reads | `index_join` | 9.26ms | 7.37ms | 0.8× | 1.9% | PASS |
| mem_reads | `index_join_scan` | 2.79ms | 5.39ms | 1.9× | 1.0% | PASS |
| mem_reads | `types_table_scan` | 772.65ms | 580.41ms | 0.8× | 0.8% | PASS |
| mem_reads | `table_scan` | 895.85ms | 649.48ms | 0.7× | 0.5% | PASS |
| mem_reads | `oltp_read_only` | 93.39ms | 94.68ms | 1.0× | 0.8% | PASS |
| mem_writes | `oltp_bulk_insert` | 147.40ms | 196.60ms | 1.3× | 0.6% | PASS |
| mem_writes | `oltp_insert` | 11.53ms | 23.17ms | 2.0× | 1.0% | PASS |
| mem_writes | `oltp_update_index` | 46.01ms | 90.53ms | 2.0× | 1.5% | PASS |
| mem_writes | `oltp_update_non_index` | 34.25ms | 56.56ms | 1.7× | 1.3% | PASS |
| mem_writes | `oltp_delete_insert` | 39.88ms | 71.27ms | 1.8× | 1.0% | PASS |
| mem_writes | `oltp_write_only` | 20.62ms | 40.60ms | 2.0× | 1.0% | PASS |
| mem_writes | `types_delete_insert` | 28.79ms | 34.61ms | 1.2× | 1.0% | PASS |
| mem_writes | `oltp_read_write` | 66.46ms | 92.60ms | 1.4× | 1.3% | PASS |
| file_reads | `oltp_point_select` | 47.04ms | 29.96ms | 0.6× | 1.1% | PASS |
| file_reads | `oltp_range_select` | 14.54ms | 12.17ms | 0.8× | 1.2% | PASS |
| file_reads | `oltp_sum_range` | 14.43ms | 11.58ms | 0.8× | 1.3% | PASS |
| file_reads | `oltp_order_range` | 2.75ms | 2.55ms | 0.9× | 2.0% | PASS |
| file_reads | `oltp_distinct_range` | 3.46ms | 3.32ms | 1.0× | 1.4% | PASS |
| file_reads | `oltp_index_scan` | 5.19ms | 5.29ms | 1.0× | 1.2% | PASS |
| file_reads | `select_random_points` | 21.14ms | 17.63ms | 0.8× | 1.3% | PASS |
| file_reads | `select_random_ranges` | 7.09ms | 5.05ms | 0.7× | 1.7% | PASS |
| file_reads | `covering_index_scan` | 7.16ms | 7.73ms | 1.1× | 0.8% | PASS |
| file_reads | `groupby_scan` | 24.02ms | 24.57ms | 1.0× | 0.5% | PASS |
| file_reads | `index_join` | 11.02ms | 8.29ms | 0.8× | 2.3% | PASS |
| file_reads | `index_join_scan` | 3.14ms | 5.76ms | 1.8× | 2.5% | PASS |
| file_reads | `types_table_scan` | 780.28ms | 581.33ms | 0.7× | 0.9% | PASS |
| file_reads | `table_scan` | 896.54ms | 652.52ms | 0.7× | 2.3% | PASS |
| file_reads | `oltp_read_only` | 133.35ms | 106.52ms | 0.8× | 1.5% | PASS |
| file_writes | `oltp_bulk_insert` | 163.80ms | 214.00ms | 1.3× | 0.8% | PASS |
| file_writes | `oltp_insert` | 15.61ms | 28.84ms | 1.8× | 2.0% | PASS |
| file_writes | `oltp_update_index` | 66.48ms | 108.03ms | 1.6× | 2.0% | PASS |
| file_writes | `oltp_update_non_index` | 50.39ms | 66.37ms | 1.3× | 1.2% | PASS |
| file_writes | `oltp_delete_insert` | 59.37ms | 80.36ms | 1.4× | 1.1% | PASS |
| file_writes | `oltp_write_only` | 35.56ms | 48.34ms | 1.4× | 1.7% | PASS |
| file_writes | `types_delete_insert` | 44.64ms | 41.34ms | 0.9× | 2.0% | PASS |
| file_writes | `oltp_read_write` | 84.36ms | 103.17ms | 1.2× | 1.4% | PASS |
| ac_reads | `oltp_point_select` | 33.06ms | 30.18ms | 0.9× | 1.0% | PASS |
| ac_reads | `oltp_range_select` | 13.43ms | 12.16ms | 0.9× | 1.0% | PASS |
| ac_reads | `oltp_sum_range` | 13.52ms | 11.62ms | 0.9× | 1.0% | PASS |
| ac_reads | `oltp_order_range` | 2.69ms | 2.60ms | 1.0× | 1.0% | PASS |
| ac_reads | `oltp_distinct_range` | 3.37ms | 3.33ms | 1.0× | 1.2% | PASS |
| ac_reads | `oltp_index_scan` | 3.77ms | 5.20ms | 1.4× | 1.5% | PASS |
| ac_reads | `select_random_points` | 20.15ms | 17.73ms | 0.9× | 1.2% | PASS |
| ac_reads | `select_random_ranges` | 5.94ms | 5.13ms | 0.9× | 1.4% | PASS |
| ac_reads | `covering_index_scan` | 5.66ms | 7.73ms | 1.4× | 0.8% | PASS |
| ac_reads | `groupby_scan` | 24.02ms | 24.62ms | 1.0× | 0.7% | PASS |
| ac_reads | `index_join` | 10.24ms | 7.92ms | 0.8× | 1.4% | PASS |
| ac_reads | `index_join_scan` | 3.04ms | 5.60ms | 1.8× | 2.9% | PASS |
| ac_reads | `types_table_scan` | 791.32ms | 584.32ms | 0.7× | 1.5% | PASS |
| ac_reads | `table_scan` | 949.88ms | 660.30ms | 0.7× | 7.3% | PASS |
| ac_reads | `oltp_read_only` | 113.95ms | 108.03ms | 0.9× | 1.2% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 9.79ms | 13.56ms | 1.4× | 2.7% | PASS |
| ac_writes | `oltp_insert_ac` | 11.16ms | 20.64ms | 1.8× | 2.9% | PASS |
| ac_writes | `oltp_update_index_ac` | 11.56ms | 34.62ms | 3.0× | 2.5% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 9.48ms | 17.82ms | 1.9× | 2.2% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 10.75ms | 27.70ms | 2.6× | 2.7% | PASS |
| ac_writes | `oltp_write_only_ac` | 10.67ms | 25.80ms | 2.4× | 3.4% | PASS |
| ac_writes | `types_delete_insert_ac` | 10.26ms | 18.86ms | 1.8× | 4.1% | PASS |
| ac_writes | `oltp_read_write_ac` | 14.76ms | 31.15ms | 2.1× | 2.3% | PASS |

</details>

<details>
<summary>blobpk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 19.57ms | 20.62ms | 1.1× | 3.0% | PASS |
| mem_reads | `oltp_range_select` | 8.49ms | 8.75ms | 1.0× | 3.9% | PASS |
| mem_reads | `oltp_sum_range` | 7.90ms | 8.43ms | 1.1× | 2.9% | PASS |
| mem_reads | `oltp_order_range` | 1.86ms | 1.99ms | 1.1× | 1.8% | PASS |
| mem_reads | `oltp_distinct_range` | 2.37ms | 2.45ms | 1.0× | 1.6% | PASS |
| mem_reads | `oltp_index_scan` | 2.30ms | 3.56ms | 1.5× | 1.4% | PASS |
| mem_reads | `select_random_points` | 12.07ms | 12.02ms | 1.0× | 3.2% | PASS |
| mem_reads | `select_random_ranges` | 3.77ms | 3.66ms | 1.0× | 3.6% | PASS |
| mem_reads | `covering_index_scan` | 4.00ms | 5.55ms | 1.4× | 1.2% | PASS |
| mem_reads | `groupby_scan` | 18.59ms | 18.77ms | 1.0× | 1.0% | PASS |
| mem_reads | `index_join` | 6.55ms | 5.70ms | 0.9× | 2.2% | PASS |
| mem_reads | `index_join_scan` | 1.99ms | 4.32ms | 2.2× | 1.5% | PASS |
| mem_reads | `types_table_scan` | 641.32ms | 456.95ms | 0.7× | 0.3% | PASS |
| mem_reads | `table_scan` | 724.45ms | 520.14ms | 0.7× | 0.6% | PASS |
| mem_reads | `oltp_read_only` | 70.16ms | 70.85ms | 1.0× | 0.8% | PASS |
| mem_writes | `oltp_bulk_insert` | 136.78ms | 189.16ms | 1.4× | 0.8% | PASS |
| mem_writes | `oltp_insert` | 10.43ms | 21.37ms | 2.0× | 0.5% | PASS |
| mem_writes | `oltp_update_index` | 37.09ms | 73.70ms | 2.0× | 1.4% | PASS |
| mem_writes | `oltp_update_non_index` | 28.43ms | 45.98ms | 1.6× | 1.2% | PASS |
| mem_writes | `oltp_delete_insert` | 30.55ms | 54.78ms | 1.8× | 1.2% | PASS |
| mem_writes | `oltp_write_only` | 16.57ms | 33.10ms | 2.0× | 2.0% | PASS |
| mem_writes | `types_delete_insert` | 22.46ms | 26.13ms | 1.2× | 2.0% | PASS |
| mem_writes | `oltp_read_write` | 50.32ms | 71.15ms | 1.4× | 1.1% | PASS |
| file_reads | `oltp_point_select` | 71.62ms | 33.86ms | 0.5× | 0.6% | PASS |
| file_reads | `oltp_range_select` | 14.52ms | 10.04ms | 0.7× | 0.6% | PASS |
| file_reads | `oltp_sum_range` | 14.10ms | 9.79ms | 0.7× | 0.8% | PASS |
| file_reads | `oltp_order_range` | 2.59ms | 2.17ms | 0.8× | 0.9% | PASS |
| file_reads | `oltp_distinct_range` | 3.07ms | 2.65ms | 0.9× | 0.6% | PASS |
| file_reads | `oltp_index_scan` | 7.77ms | 5.12ms | 0.7× | 0.7% | PASS |
| file_reads | `select_random_points` | 18.73ms | 13.31ms | 0.7× | 0.7% | PASS |
| file_reads | `select_random_ranges` | 9.33ms | 5.02ms | 0.5× | 0.7% | PASS |
| file_reads | `covering_index_scan` | 9.44ms | 7.07ms | 0.7× | 0.7% | PASS |
| file_reads | `groupby_scan` | 20.19ms | 19.57ms | 1.0× | 1.2% | PASS |
| file_reads | `index_join` | 10.09ms | 7.27ms | 0.7× | 1.2% | PASS |
| file_reads | `index_join_scan` | 3.06ms | 4.47ms | 1.5× | 1.1% | PASS |
| file_reads | `types_table_scan` | 642.65ms | 457.95ms | 0.7× | 0.4% | PASS |
| file_reads | `table_scan` | 723.25ms | 523.70ms | 0.7× | 0.5% | PASS |
| file_reads | `oltp_read_only` | 143.09ms | 89.50ms | 0.6× | 0.6% | PASS |
| file_writes | `oltp_bulk_insert` | 200.06ms | 249.41ms | 1.2× | 2.3% | PASS |
| file_writes | `oltp_insert` | 19.19ms | 43.24ms | 2.3× | 6.9% | PASS |
| file_writes | `oltp_update_index` | 140.93ms | 155.28ms | 1.1× | 2.9% | PASS |
| file_writes | `oltp_update_non_index` | 120.59ms | 105.96ms | 0.9× | 5.4% | PASS |
| file_writes | `oltp_delete_insert` | 141.25ms | 114.47ms | 0.8× | 2.5% | PASS |
| file_writes | `oltp_write_only` | 96.66ms | 80.22ms | 0.8× | 4.9% | PASS |
| file_writes | `types_delete_insert` | 108.68ms | 59.61ms | 0.5× | 7.9% | PASS |
| file_writes | `oltp_read_write` | 134.75ms | 120.51ms | 0.9× | 3.6% | PASS |
| ac_reads | `oltp_point_select` | 37.75ms | 34.34ms | 0.9× | 0.9% | PASS |
| ac_reads | `oltp_range_select` | 10.83ms | 10.13ms | 0.9× | 1.1% | PASS |
| ac_reads | `oltp_sum_range` | 10.24ms | 9.93ms | 1.0× | 0.9% | PASS |
| ac_reads | `oltp_order_range` | 2.23ms | 2.25ms | 1.0× | 1.2% | PASS |
| ac_reads | `oltp_distinct_range` | 2.67ms | 2.71ms | 1.0× | 1.4% | PASS |
| ac_reads | `oltp_index_scan` | 4.15ms | 5.22ms | 1.3× | 1.6% | PASS |
| ac_reads | `select_random_points` | 14.91ms | 13.62ms | 0.9× | 1.3% | PASS |
| ac_reads | `select_random_ranges` | 5.87ms | 5.27ms | 0.9× | 1.1% | PASS |
| ac_reads | `covering_index_scan` | 5.89ms | 7.40ms | 1.3× | 1.6% | PASS |
| ac_reads | `groupby_scan` | 19.95ms | 19.86ms | 1.0× | 1.0% | PASS |
| ac_reads | `index_join` | 8.22ms | 7.51ms | 0.9× | 2.0% | PASS |
| ac_reads | `index_join_scan` | 2.76ms | 4.62ms | 1.7× | 1.6% | PASS |
| ac_reads | `types_table_scan` | 642.72ms | 459.10ms | 0.7× | 0.5% | PASS |
| ac_reads | `table_scan` | 722.74ms | 523.50ms | 0.7× | 0.5% | PASS |
| ac_reads | `oltp_read_only` | 95.68ms | 90.04ms | 0.9× | 0.6% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 22.88ms | 24.13ms | 1.1× | 2.2% | PASS |
| ac_writes | `oltp_insert_ac` | 25.64ms | 38.28ms | 1.5× | 2.4% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.44ms | 57.12ms | 2.2× | 2.2% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 19.84ms | 31.51ms | 1.6× | 1.7% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 25.07ms | 43.06ms | 1.7× | 1.6% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.13ms | 41.90ms | 1.7× | 2.5% | PASS |
| ac_writes | `types_delete_insert_ac` | 19.35ms | 33.63ms | 1.7× | 3.4% | PASS |
| ac_writes | `oltp_read_write_ac` | 27.77ms | 45.07ms | 1.6× | 1.7% | PASS |

</details>

<details>
<summary>compositepk workload details</summary>

| Section | Workload | SQLite median | DoltLite median | Ratio | Paired-ratio noise | Result |
|---|---|---:|---:|---:|---:|---|
| mem_reads | `oltp_point_select` | 32.91ms | 38.37ms | 1.2× | 0.9% | PASS |
| mem_reads | `oltp_range_select` | 19.58ms | 21.65ms | 1.1× | 0.8% | PASS |
| mem_reads | `oltp_sum_range` | 17.53ms | 20.60ms | 1.2× | 1.1% | PASS |
| mem_reads | `oltp_order_range` | 3.48ms | 3.89ms | 1.1× | 0.9% | PASS |
| mem_reads | `oltp_distinct_range` | 4.67ms | 5.08ms | 1.1× | 1.3% | PASS |
| mem_reads | `oltp_index_scan` | 4.44ms | 5.96ms | 1.3× | 1.2% | PASS |
| mem_reads | `select_random_points` | 27.10ms | 29.64ms | 1.1× | 1.1% | PASS |
| mem_reads | `select_random_ranges` | 7.56ms | 8.54ms | 1.1× | 1.2% | PASS |
| mem_reads | `covering_index_scan` | 7.69ms | 10.62ms | 1.4× | 1.0% | PASS |
| mem_reads | `groupby_scan` | 36.11ms | 38.75ms | 1.1× | 0.6% | PASS |
| mem_reads | `index_join` | 7.88ms | 10.67ms | 1.4× | 1.3% | PASS |
| mem_reads | `index_join_scan` | 3.79ms | 6.03ms | 1.6× | 1.3% | PASS |
| mem_reads | `types_table_scan` | 1.03s | 858.64ms | 0.8× | 0.7% | PASS |
| mem_reads | `table_scan` | 1.16s | 956.24ms | 0.8× | 0.6% | PASS |
| mem_reads | `oltp_read_only` | 149.55ms | 169.72ms | 1.1× | 0.9% | PASS |
| mem_writes | `oltp_bulk_insert` | 249.08ms | 326.55ms | 1.3× | 0.9% | PASS |
| mem_writes | `oltp_insert` | 19.01ms | 34.01ms | 1.8× | 0.9% | PASS |
| mem_writes | `oltp_update_index` | 65.59ms | 119.67ms | 1.8× | 1.1% | PASS |
| mem_writes | `oltp_update_non_index` | 50.27ms | 80.18ms | 1.6× | 0.9% | PASS |
| mem_writes | `oltp_delete_insert` | 48.80ms | 91.12ms | 1.9× | 0.7% | PASS |
| mem_writes | `oltp_write_only` | 26.66ms | 53.84ms | 2.0× | 1.0% | PASS |
| mem_writes | `types_delete_insert` | 32.42ms | 48.32ms | 1.5× | 1.5% | PASS |
| mem_writes | `oltp_read_write` | 100.69ms | 149.26ms | 1.5× | 0.9% | PASS |
| file_reads | `oltp_point_select` | 104.06ms | 56.80ms | 0.5× | 1.2% | PASS |
| file_reads | `oltp_range_select` | 27.37ms | 24.06ms | 0.9× | 1.4% | PASS |
| file_reads | `oltp_sum_range` | 25.27ms | 23.05ms | 0.9× | 1.2% | PASS |
| file_reads | `oltp_order_range` | 4.47ms | 4.23ms | 0.9× | 1.8% | PASS |
| file_reads | `oltp_distinct_range` | 5.66ms | 5.44ms | 1.0× | 1.0% | PASS |
| file_reads | `oltp_index_scan` | 11.88ms | 8.31ms | 0.7× | 2.0% | PASS |
| file_reads | `select_random_points` | 36.41ms | 32.99ms | 0.9× | 1.5% | PASS |
| file_reads | `select_random_ranges` | 15.11ms | 10.82ms | 0.7× | 1.6% | PASS |
| file_reads | `covering_index_scan` | 15.26ms | 13.13ms | 0.9× | 1.0% | PASS |
| file_reads | `groupby_scan` | 37.18ms | 39.48ms | 1.1× | 1.1% | PASS |
| file_reads | `index_join` | 12.15ms | 12.55ms | 1.0× | 1.8% | PASS |
| file_reads | `index_join_scan` | 4.88ms | 6.49ms | 1.3× | 1.7% | PASS |
| file_reads | `types_table_scan` | 1.03s | 861.10ms | 0.8× | 0.5% | PASS |
| file_reads | `table_scan` | 1.17s | 959.65ms | 0.8× | 1.1% | PASS |
| file_reads | `oltp_read_only` | 255.92ms | 199.78ms | 0.8× | 0.8% | PASS |
| file_writes | `oltp_bulk_insert` | 263.81ms | 340.59ms | 1.3× | 0.9% | PASS |
| file_writes | `oltp_insert` | 26.09ms | 38.37ms | 1.5× | 1.7% | PASS |
| file_writes | `oltp_update_index` | 94.71ms | 136.93ms | 1.4× | 1.5% | PASS |
| file_writes | `oltp_update_non_index` | 77.81ms | 88.40ms | 1.1× | 1.9% | PASS |
| file_writes | `oltp_delete_insert` | 77.19ms | 101.86ms | 1.3× | 2.2% | PASS |
| file_writes | `oltp_write_only` | 51.38ms | 65.25ms | 1.3× | 2.4% | PASS |
| file_writes | `types_delete_insert` | 49.67ms | 57.57ms | 1.2× | 1.4% | PASS |
| file_writes | `oltp_read_write` | 127.11ms | 160.42ms | 1.3× | 1.5% | PASS |
| ac_reads | `oltp_point_select` | 56.46ms | 56.81ms | 1.0× | 1.1% | PASS |
| ac_reads | `oltp_range_select` | 22.34ms | 24.03ms | 1.1× | 0.9% | PASS |
| ac_reads | `oltp_sum_range` | 20.20ms | 23.01ms | 1.1× | 1.0% | PASS |
| ac_reads | `oltp_order_range` | 3.87ms | 4.23ms | 1.1× | 1.2% | PASS |
| ac_reads | `oltp_distinct_range` | 5.06ms | 5.44ms | 1.1× | 1.2% | PASS |
| ac_reads | `oltp_index_scan` | 7.01ms | 8.31ms | 1.2× | 2.0% | PASS |
| ac_reads | `select_random_points` | 30.77ms | 33.00ms | 1.1× | 1.6% | PASS |
| ac_reads | `select_random_ranges` | 10.22ms | 10.81ms | 1.1× | 1.4% | PASS |
| ac_reads | `covering_index_scan` | 10.34ms | 13.10ms | 1.3× | 1.1% | PASS |
| ac_reads | `groupby_scan` | 36.59ms | 39.50ms | 1.1× | 0.9% | PASS |
| ac_reads | `index_join` | 9.51ms | 12.51ms | 1.3× | 1.4% | PASS |
| ac_reads | `index_join_scan` | 4.29ms | 6.51ms | 1.5× | 1.0% | PASS |
| ac_reads | `types_table_scan` | 1.02s | 861.03ms | 0.8× | 0.6% | PASS |
| ac_reads | `table_scan` | 1.15s | 954.86ms | 0.8× | 0.5% | PASS |
| ac_reads | `oltp_read_only` | 185.06ms | 199.31ms | 1.1× | 1.0% | PASS |
| ac_writes | `oltp_bulk_insert_ac` | 23.53ms | 34.88ms | 1.5× | 6.3% | PASS |
| ac_writes | `oltp_insert_ac` | 23.83ms | 47.87ms | 2.0× | 3.6% | PASS |
| ac_writes | `oltp_update_index_ac` | 25.84ms | 63.11ms | 2.4× | 5.4% | PASS |
| ac_writes | `oltp_update_non_index_ac` | 24.20ms | 42.90ms | 1.8× | 7.1% | PASS |
| ac_writes | `oltp_delete_insert_ac` | 23.53ms | 50.71ms | 2.2× | 3.7% | PASS |
| ac_writes | `oltp_write_only_ac` | 24.00ms | 50.40ms | 2.1× | 4.6% | PASS |
| ac_writes | `types_delete_insert_ac` | 22.26ms | 41.41ms | 1.9× | 6.3% | PASS |
| ac_writes | `oltp_read_write_ac` | 31.17ms | 59.94ms | 1.9× | 4.4% | PASS |

</details>

</details>

## Version-control latency

Wall time: 4m 52s. Samples per benchmark: 101.

| Benchmark | Median | Ceiling | Ceiling used | MAD | Result |
|---|---:|---:|---:|---:|---|
| `status_clean_many_tables` | 23.39ms | 38.00ms | 61.6% | 1.4% | PASS |
| `status_dirty_many_tables` | 25.79ms | 42.00ms | 61.4% | 1.0% | PASS |
| `diff_regular_working_one_table` | 22.10ms | 33.00ms | 67.0% | 1.2% | PASS |
| `diff_regular_working_many_tables` | 28.11ms | 50.00ms | 56.2% | 0.8% | PASS |
| `diff_stat_working_many_tables` | 28.11ms | 48.00ms | 58.6% | 0.9% | PASS |
| `diff_schema_working_many_tables` | 28.84ms | 48.00ms | 60.1% | 1.0% | PASS |
| `branch_list_many_branches` | 16.01ms | 25.00ms | 64.0% | 1.3% | PASS |
| `branch_create_delete` | 16.97ms | 27.00ms | 62.9% | 1.2% | PASS |
| `at_literal_deep_history` | 17.05ms | 28.00ms | 60.9% | 1.0% | PASS |
| `diff_literal_deep_history` | 16.87ms | 28.00ms | 60.2% | 1.5% | PASS |
| `history_literal_deep_history` | 17.97ms | 30.00ms | 59.9% | 0.9% | PASS |
| `checkout_branch_clean` | 22.50ms | 43.00ms | 52.3% | 1.5% | PASS |
| `merge_data_no_conflicts` | 18.42ms | 33.00ms | 55.8% | 1.0% | PASS |
| `merge_data_secondary_index` | 637.05ms | 944.00ms | 67.5% | 0.6% | PASS |
| `merge_schema_no_conflicts` | 15.73ms | 24.00ms | 65.5% | 1.5% | PASS |
| `merge_data_conflicts` | 21.30ms | 33.00ms | 64.5% | 2.0% | PASS |
| `merge_data_conflicts_with_resolve` | 21.83ms | 34.00ms | 64.2% | 1.2% | PASS |

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
| copy_fsync | candidate | 55.622 | 48.222–62.361 | 48.462 | 1797.85 | 107968301 |
| push_initial | candidate | 307.406 | 304.091–309.641 | 306.175 | 325.30 | 107956807 |
| clone_initial | candidate | 343.097 | 339.175–348.143 | 331.168 | 291.46 | 108475105 |
| push_noop | candidate | 0.340 | 0.318–0.388 | 0.194 | — | 0 |
| fetch_noop | candidate | 0.087 | 0.078–0.112 | 0.088 | — | 0 |
| push_incremental | candidate | 42.266 | 41.706–44.512 | 40.966 | 23.66 | 7909775 |
| fetch_incremental | candidate | 48.687 | 48.048–49.764 | 47.756 | 20.54 | 7910084 |
| pull_fast_forward | candidate | 50.441 | 50.126–50.675 | 48.927 | 19.83 | 7910844 |

<details>
<summary>Memory and file I/O</summary>

Successful VFS bytes and VFS requests cover local and remote files during the operation; they include OS-cache hits and are not physical disk traffic. Peak RSS covers the fresh SQL process, including database open. The Python copy control is not instrumented.

| Operation | Binary | Peak RSS MiB | Read MB | Written MB | Read requests | Write requests |
|---|---|---:|---:|---:|---:|---:|
| push_initial | candidate | 90.52 | 175.487 | 108.763 | 18329 | 2066 |
| clone_initial | candidate | 91.57 | 215.906 | 110.833 | 27898 | 2353 |
| push_noop | candidate | 41.79 | 0.075 | 0.000 | 28 | 2 |
| fetch_noop | candidate | 41.79 | 0.068 | 0.000 | 22 | 0 |
| push_incremental | candidate | 41.79 | 10.741 | 7.910 | 1375 | 125 |
| fetch_incremental | candidate | 41.79 | 26.494 | 7.910 | 7016 | 124 |
| pull_fast_forward | candidate | 41.79 | 26.497 | 8.959 | 7032 | 142 |

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
| copy_fsync | candidate | 5814.635 | 5762.876–5835.486 | 333.062 | 171.98 | 1087718066 |
| push_initial | candidate | 9054.174 | 8168.368–9095.171 | 3861.502 | 110.45 | 1079566310 |
| clone_initial | candidate | 9288.847 | 9284.922–9348.229 | 4086.001 | 107.66 | 1102407396 |
| push_noop | candidate | 0.770 | 0.710–0.855 | 0.295 | — | 0 |
| fetch_noop | candidate | 0.129 | 0.123–0.136 | 0.131 | — | 0 |
| push_incremental | candidate | 270.124 | 265.869–292.747 | 221.080 | 3.70 | 15806003 |
| fetch_incremental | candidate | 284.837 | 283.273–300.128 | 218.415 | 3.51 | 15806513 |
| pull_fast_forward | candidate | 299.206 | 296.251–300.326 | 230.154 | 3.34 | 15807273 |

<details>
<summary>Memory and file I/O</summary>

Successful VFS bytes and VFS requests cover local and remote files during the operation; they include OS-cache hits and are not physical disk traffic. Peak RSS covers the fresh SQL process, including database open. The Python copy control is not instrumented.

| Operation | Binary | Peak RSS MiB | Read MB | Written MB | Read requests | Write requests |
|---|---|---:|---:|---:|---:|---:|
| push_initial | candidate | 134.78 | 2091.859 | 1087.622 | 263493 | 20541 |
| clone_initial | candidate | 131.76 | 2178.657 | 1133.868 | 284159 | 32259 |
| push_noop | candidate | 44.46 | 0.078 | 0.000 | 28 | 2 |
| fetch_noop | candidate | 44.46 | 0.069 | 0.000 | 22 | 0 |
| push_incremental | candidate | 56.25 | 47.022 | 15.928 | 10184 | 313 |
| fetch_incremental | candidate | 50.49 | 62.602 | 15.928 | 13983 | 313 |
| pull_fast_forward | candidate | 50.45 | 62.605 | 16.978 | 13999 | 331 |

</details>


</details>

## Reproducing

The workload definitions live in `test/sysbench_compare*.sh`, `test/vc_perf_ceiling.sh`, and `test/remote_perf.py`. The nightly workflow retains the complete raw samples and generated reports as Actions artifacts for 30 days.
