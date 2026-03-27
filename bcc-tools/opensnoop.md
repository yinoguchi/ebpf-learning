# bcc-tools: opensnoop

## What is opensnoop?
`opensnoop` traces the `open()` and `openat()` system calls system-wide. It shows which processes are attempting to access which files, including the result (success/failure) and the file descriptor (FD).

## Why it's crucial for SMEs (Support Engineers)
When an application fails with a vague "File not found" or "Permission denied" error, `opensnoop` reveals exactly which path the process was trying to access. It captures "hidden" file attempts that are not documented in application logs, such as library loading or localization file searches.

## Execution Log Analysis

**Command:**
```bash
# Monitoring only 'cat' processes
/usr/share/bcc/tools/opensnoop -n cat
```
** Raw Output **
```bash
PID    COMM               FD ERR PATH
705949 cat                 3   0 /etc/ld.so.cache
705949 cat                 3   0 /lib64/libc.so.6
705949 cat                -1   2 /usr/lib/locale/locale-archive
705949 cat                 3   0 /usr/share/locale/locale.alias
...
705950 cat                 3   0 /etc/hosts
705950 cat                -1   2 /tmp/this_file_does_not_exist
```

## Useful Options for Troubleshooting
- -x : Failed attempts only. Extremely useful for finding missing config files or permission issues without the noise of successful reads.
- -T : Include timestamps. Essential for correlating file access with application error logs.
- -n [name] : Filter by process name.
