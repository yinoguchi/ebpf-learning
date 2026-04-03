# ebpf-learning

## What's this?
My eBPF / bpftrace learning logs for Linux troubleshooting.

## How To Run
eBPF requires root privileges. Run the scripts using `sudo` or as the root user:

```bash
# Example
sudo bpftrace kstack.bt
```
## Tips

How to find trace points:
```bash
# bpftrace -l <tracepoint>
```
For example:
```bash
# bpftrace -l 'kprobe:*netif_receive_skb*'
kprobe:__netif_receive_skb
kprobe:__netif_receive_skb_core.constprop.0
kprobe:__netif_receive_skb_list_core
kprobe:__netif_receive_skb_one_core
kprobe:__traceiter_netif_receive_skb
kprobe:__traceiter_netif_receive_skb_entry
kprobe:__traceiter_netif_receive_skb_exit
kprobe:__traceiter_netif_receive_skb_list_entry
kprobe:__traceiter_netif_receive_skb_list_exit
kprobe:netif_receive_skb
kprobe:netif_receive_skb_core
kprobe:netif_receive_skb_list
kprobe:netif_receive_skb_list_internal
```
