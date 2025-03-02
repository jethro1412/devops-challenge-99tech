Troubleshooting Steps

Verify the Issue:

Log into the VM via SSH: ssh user@vm-ip.

Check memory usage: free -h or top.

Look at total, used, free, and cached memory.

Confirm NGINX is running: ps aux | grep nginx.

Validate only NGINX is active: systemctl list-units --type=service or ps -ef.

Analyze NGINX Process:

Check NGINX memory footprint: top -p $(pidof nginx) or pmap -x $(pidof nginx).

Review NGINX worker processes: ps -C nginx -o pid=,rss=,vsz=.

RSS (Resident Set Size) shows physical memory used; VSZ is virtual memory.

Inspect System Logs:

Check NGINX logs: sudo tail -f /var/log/nginx/error.log and /var/log/nginx/access.log.

Check system logs: sudo journalctl -u nginx and sudo 
dmesg | grep -i "out of memory".

Examine Resource Usage:

Look for memory leaks or over-allocation: htop (interactive view) or vmstat 1 (memory stats over time).

Check for other processes: top or ps aux --sort=-%mem.

Review NGINX Configuration:

Inspect config: sudo cat /etc/nginx/nginx.conf.

Look at worker_processes, worker_connections, and upstream settings.

Monitor Traffic Patterns:

Analyze incoming traffic: sudo tcpdump -i eth0 or check access logs for request volume.

Use nginx -T to dump parsed configuration for anomalies.

Check System-Level Issues:

Verify swap usage: swapon --show or free -h.

Check kernel parameters: sysctl vm.overcommit_memory and vm.swappiness.

Correlate with Monitoring:

Cross-check with your monitoring tool (e.g., CloudWatch, Prometheus) for historical trends or spikes.

Double check with client / company event: Sometimes in if there's a special event or launch by client or companies there will be a high surge of traffic, we need to be informed of this and prepare it in advance by testing the capability of the environment using stress test / performance test