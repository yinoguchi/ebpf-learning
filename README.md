# ebpf-learning

## What's this?
My eBPF / bpftrace learning logs for Linux troubleshooting.

## How To Run
eBPF requires root privileges. Run the scripts using `sudo` or as the root user:

```bash
# Example
sudo bpftrace kstack_cat.bt
```
## Contents
- kstack_cat.bt : Print kernel stack trace when cat is executed.
- filename_cat_all.bt : Show all files opened by the cat command.
