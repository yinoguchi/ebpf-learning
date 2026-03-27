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
