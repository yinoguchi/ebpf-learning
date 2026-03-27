# Experiment 01: Runtime Overhead - bpftrace vs SystemTap

## Objective
To measure and compare the runtime execution overhead of attaching probes to high-frequency system calls (`read`) using `bpftrace` (eBPF) and `SystemTap`.

## Methodology
Generated massive `read` and `write` syscalls by copying 20GB of data using the `dd` command. 
Measured the execution time using the `time` command under three conditions:
1. **Base:** No tracing tools attached.
2. **bpftrace:** Counting `sys_enter_read` events.
3. **SystemTap:** Counting `syscall.read` events.

### Commands Used
* **Workload:** `time dd if=/dev/zero of=/dev/null bs=4k count=5000000`
* **bpftrace:** `bpftrace -e 'tracepoint:syscalls:sys_enter_read /comm == "dd"/ { @ = count(); }'`
* **SystemTap:** `stap -e 'global my_count; probe syscall.read { if (execname() == "dd") { my_count++ } }'`

## Raw Execution Logs

### 1. Base (No Tracing)
```bash
# time dd if=/dev/zero of=/dev/null bs=4k count=5000000
5000000+0 records in
5000000+0 records out
20480000000 bytes (20 GB, 19 GiB) copied, 2.67898 s, 7.6 GB/s

real    0m2.681s
user    0m0.507s
sys     0m2.168s
```

### 2. bpftrace
Terminal 1(Tracing)
```bash
# bpftrace -e 'tracepoint:syscalls:sys_enter_read /comm == "dd"/ { @ = count(); }' -v
Attaching 1 probe...
Attaching tracepoint:syscalls:sys_enter_read
^C

@: 5000003
```
Temirnal 2(Workload):
```bash
# time dd if=/dev/zero of=/dev/null bs=4k count=5000000
5000000+0 records in
5000000+0 records out
20480000000 bytes (20 GB, 19 GiB) copied, 3.45996 s, 5.9 GB/s

real    0m3.462s
user    0m0.564s
sys     0m2.890s
```

### 3. Systemtap
Terminal 1 (Tracing):
```bash
# stap -v -e 'global my_count; probe syscall.read { if (execname() == "dd") { my_count++ } }'
Pass 1: parsed user script and 487 library scripts using 360560virt/121844res/16000shr/140524data kb, in 230usr/170sys/145real ms.
Pass 2: analyzed script: 4 probes, 4 functions, 97 embeds, 5 globals using 452660virt/215444res/17384shr/232624data kb, in 1310usr/90sys/1415real ms.
Pass 3: using cached /root/.systemtap/cache/94/stap_94e18d7dd33156dba434e81b8e0de1fa_66707.c
Pass 4: using cached /root/.systemtap/cache/94/stap_94e18d7dd33156dba434e81b8e0de1fa_66707.ko
Pass 5: starting run.
^Cmy_count=5000003
Pass 5: run completed in 20usr/90sys/20531real ms.
```

Terminal 2 (Workload):
```bash
# time dd if=/dev/zero of=/dev/null bs=4k count=5000000
5000000+0 records in
5000000+0 records out
20480000000 bytes (20 GB, 19 GiB) copied, 3.6489 s, 5.6 GB/s

real    0m3.650s
user    0m0.523s
sys     0m3.116s
```

## Results

| Condition | real (Total Time) | user | sys (Kernel Time) | Overhead (vs Base) |
| :--- | :--- | :--- | :--- | :--- |
| **1. Base** | 2.681s | 0.507s | 2.168s | - |
| **2. bpftrace** | 3.462s | 0.564s | 2.890s | + 0.781s |
| **3. SystemTap**| 3.650s | 0.523s | 3.116s | + 0.969s |

*Note: 5,000,000 system calls were traced in both scenarios.*
