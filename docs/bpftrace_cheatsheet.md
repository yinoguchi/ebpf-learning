# bpftrace Cheat Sheet

## How to List Available Tracepoints and Probes
When you need to find the exact name of a tracepoint or kprobe, use the following commands.

### 1. Using bpftrace (Recommended)
You can search for tracepoints, kprobes, and software events using the `-l` (list) option. Wildcards (`*`) are supported.

```bash
# List all tracepoints related to system calls
sudo bpftrace -l 'tracepoint:syscalls:*'

# Search for openat related tracepoints
sudo bpftrace -l '*openat*'
```
### 2. Using Linux DebugFS (Direct Kernel Check)
If you want to see the tracepoints directly provided by the kernel without using bpftrace, you can list the directories in the debugfs.

```Bash
# List all event categories
sudo ls /sys/kernel/debug/tracing/events/

# List specific syscall events (e.g., read)
sudo ls /sys/kernel/debug/tracing/events/syscalls/ | grep read
```
### Tracepoint vs kprobe (Quick Memo)
- tracepoint: Stable, officially maintained hooks by kernel developers. (e.g., sys_enter_openat). Always prefer this if available.
- kprobe: Dynamic hooks to internal kernel functions. Names and arguments might change between kernel versions. (e.g., vfs_read).
