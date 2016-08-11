[Performance Checklists](http://www.brendangregg.com/blog/2016-05-04/srecon2016-perf-checklists-for-sres.html)

### Linux Perf Analysis in 60s
- `uptime` - load averages
- `dmesg -T | tail` - kernel errors
- `vmstat 1` - overall stats by time
- `mpstat -P ALL 1` - CPU balance
- `pidstat 1` - process usage
- `iostat -xz 1` - disk I/O
- `free -m` - memory usage
- `sar -n DEV 1` - network I/O
- `sar -n TCP,ETCP 1` - TCP stats
- `top` - check overview

These are explained in the post [Linux Performance Analysis in 60 seconds](http -//techblog.netflix.com/2015/11/linux-performance-analysis-in-60s.html) from the Netflix tech blog.

### Linux Disk Checklist
- `iostat –xnz 1` - any disk I/O? if not, stop looking
- `vmstat 1` - is this swapping? or, high sys time?
- `df -h` - are file systems nearly full?
- `ext4slower 10` - (zfs*, xfs*, etc.) slow file system I/O?
- `bioslower 10` - if so, check disks
- `ext4dist 1` - check distribution and rate
- `biolatency 1` - if interesting, check disks
- `cat /sys/devices/…/ioerr_cnt` - (if available) errors
- `smartctl -l error /dev/sda1` - (if available) errors

### Linux Network Checklist
- `sar -n DEV,EDEV 1` - at interface limits? or use nicstat
- `sar -n TCP,ETCP 1` - active/passive load, retransmit rate
- `cat /etc/resolv.conf` - it's always DNS
- `mpstat -P ALL 1` - high kernel time? single hot CPU?
- `tcpretrans` - what are the retransmits? state?
- `tcpconnect` - connecting to anything unexpected?
- `tcpaccept` - unexpected workload?
- `netstat -rnv` - any inefficient routes?
- **check firewall config** - anything blocking/throttling?
- `netstat -s` - play 252 metric pickup

`tcp*`, are from [bcc/BPF tools](https -//github.com/iovisor/bcc).

### Linux CPU Checklist
- `uptime` - load averages
- `vmstat 1` - system-wide utilization, run q length
- `mpstat -P ALL 1` - CPU balance
- `pidstat 1` - per-process CPU
- **CPU flame graph** - CPU profiling
- **CPU subsecond offset heat map** - look for gaps
- `perf stat -a -sleep 10` - IPC, LLC hit ratio

`htop` can do 1-4. I'm tempted to add `execsnoop` for short-lived processes (it's in [perf-tools](https -//github.com/brendangregg/perf-tools) or [bcc/BPF tools](https -//github.com/iovisor/bcc)).