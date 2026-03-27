# bpftrace Cheat Sheet: Maps (`@`)

## 1. What is `@`?
In bpftrace, variables starting with `@` are called **Maps** (Associative Arrays / Hash tables in eBPF). 
They are used to store data, count events, and collect statistics globally across different probes.
**Auto-Print Feature:** When the bpftrace script exits (e.g., via Ctrl+C), the contents of all Maps are automatically printed to the standard output.

## 2. Named vs. Unnamed Maps
You can use a single unnamed map, or give them names to track multiple metrics.
* **Unnamed Map:** `@ = count();` (Good for quick, single-purpose one-liners)
* **Named Map:** `@reads = count();`, `@writes = count();` (Recommended for scripts)

## 3. Using Keys (Grouping Data)
You can group or categorize data by passing keys inside brackets `[]`.
* `@calls[comm] = count();` : Count events per command name (e.g., grouping by 'cat', 'dd', 'sshd').
* `@files[tid] = args->filename;` : Store a filename temporarily, using the Thread ID (tid) as the key to avoid mixing data between concurrent processes.
* `@io[pid, comm] = count();` : Use multiple keys separated by commas.

## 4. Powerful Aggregation Functions
Maps show their true power when combined with built-in aggregation functions:
* `count()`: Counts the number of times an event occurs.
* `hist(value)`: Creates a log2 histogram (Excellent for latency/performance analysis).
* `sum(value)`: Calculates the total sum of the values.
* `avg(value)`: Calculates the average.
* `min(value)` / `max(value)`: Records the minimum or maximum value observed.

## 5. Memory Management
In production environments, Maps consume kernel memory. The need for cleanup depends on the map type:
* **Maps WITH Keys (e.g., `@map[tid]`):** Grow dynamically. **MUST be cleaned up!**
* **Maps WITHOUT Keys (e.g., `@ = count();`):** Fixed size. **Safe to leave.**

**Cleanup Commands:**
* `delete(@map[key]);` : Deletes a specific key-value pair. Typically used in `sys_exit` probes after data is processed to prevent memory leaks.
* `clear(@map);` : Empties the entire map. Often used with `interval` probes to reset counters periodically (e.g., every 1 second).
