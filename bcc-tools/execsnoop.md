# bcc-tools: execsnoop

## What is execsnoop?
`execsnoop` is a eBPF-based tool included in the `bcc-tools` package. It traces new process execution system-wide by hooking into the `execve()` system call.

## Why is it crucial?
Traditional monitoring commands like `top` or `ps` sample the system state periodically (e.g., every 1 second). They completely miss **short-lived processes** that start and exit within milliseconds. `execsnoop` guarantees 100% visibility of all executed commands, making it the ultimate tool for investigating periodic CPU spikes, hidden cron jobs, or malicious script executions.

## Useful Option
- -T : Include a timestamp column (HH:MM:SS). Crucial for correlating process executions with other system logs or CPU utilization graphs.

## Execution Log
Terminal 1 (User Action):
```bash
# ls -l /tmp
# date
```

Terminal 2 (Monitoring):
```bash
# /usr/share/bcc/tools/execsnoop -T
TIME     COMM             PID     PPID    RET ARGS
17:15:33 ls               705858  704750    0 /usr/bin/ls --color=auto -l /tmp
17:15:34 date             705859  704750    0 /usr/bin/date
```

