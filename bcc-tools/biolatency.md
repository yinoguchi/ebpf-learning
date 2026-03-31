# bcc-tools: biolatency

## What is biolatency?
`biolatency` traces block device I/O (disk reads/writes) and summarizes the latency (time between I/O request and completion) as a histogram.


## Why it's crucial for SMEs (Support Engineers)
Averages (as seen in `iostat`) can be misleading. A system can have a healthy average latency but still suffer from "outliers" unoccasional I/O requests that 
take seconds to complete, causing application timeouts. `biolatency` visualizes the distribution, making it easy to spot these performance spikes.

## Execution Log Analysis

**Command:**
```bash
# Trace disk I/O latency by device (-D)
```bash
# /usr/share/bcc/tools/biolatency -D
Tracing block device I/O... Hit Ctrl-C to end.
disk = sda
     usecs               : count     distribution

         0 -> 1          : 0        |                                        |
         2 -> 3          : 0        |                                        |
         4 -> 7          : 0        |                                        |
         8 -> 15         : 0        |                                        |
        16 -> 31         : 0        |                                        |
        32 -> 63         : 0        |                                        |
        64 -> 127        : 0        |                                        |
       128 -> 255        : 439      |****************************************|
       256 -> 511        : 77       |*******                                 |
```
