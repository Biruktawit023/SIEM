##### **Log Sources** 
This section introduces the idea that **every device on a network generates logs** whenever something happens on it  a user visiting a website, connecting to the network, logging into a workstation, etc. These logs are the raw material a SIEM feeds on.
It then zooms in on one of the most common and important log sources in any network: **Windows machines**.
 **Windows Machine Logs**
What Windows does?    windows records _every_ event  logins, process execution, file access, system changes, etc.  in a built-in logging system.
**Where to find them:**  
You open the **Event Viewer** tool (search "Event Viewer" in the Start menu). This is where all the different logs are stored and can be browsed.
**The key concept  Event IDs:**  
Windows assigns a **unique numerical ID** to each type of log activity. This is a big deal for analysts because:
- It makes logs **easy to identify** e.g., Event ID 4688 always means "process execution," 104 means "event log cleared" (as we saw in the previous document).
- It makes logs **easy to track and search**  instead of reading free-text descriptions, you filter by Event ID.
**How logs get to the SIEM:**  
Logs from **all Windows endpoints** across the network are **forwarded to the SIEM solution**. This gives security teams **monitoring and visibility** across the whole environment rather than checking each machine individually (which ties back to the "Centralized Log Collection" feature).
 **Linux, Web Server Logs, and Log Ingestion**
This section continues the "where do logs come from" discussion, covering **Linux machines**, **web servers**, and finally **how logs actually get into the SIEM**.
 **Linux Machine Logs**
Like Windows, Linux stores all its events, errors, and warnings in logs  which are then ingested into the SIEM for continuous monitoring. The key difference: Linux doesn't use an "Event Viewer" or unique numeric Event IDs; instead, logs are stored as **plain text files in standard directory locations**. Knowing these paths is essential for an analyst.
**Common Linux log locations:**

| Path                                      | What it contains                                 |
| ----------------------------------------- | ------------------------------------------------ |
| `/var/log/httpd`                          | HTTP request/response and error logs             |
| `/var/log/cron`                           | Events related to cron jobs (scheduled tasks)    |
| `/var/log/auth.log` and `/var/log/secure` | Authentication-related logs (logins, sudo, etc.) |
| `/var/log/kern`                           | Kernel-related events                            |

**Sample cron log:**

May 28 13:04:20 ebr crond[2843]: /usr/sbin/crond 4.4 dillon's cron daemon, started with loglevel notice
May 28 13:04:20 ebr crond[2843]: no timestamp found (user root job sys-hourly)
...
Jun 13 07:46:22 ebr crond[3592]: unable to exec /usr/sbin/sendmail: cron output for user root job sys-daily to /dev/null
Notice the format: **timestamp → hostname → process (`crond`) → PID → message**. The last line even shows a failure (`unable to exec`), which could be interesting to an analyst. In a SIEM, these lines get **parsed** into fields (timestamp, host, process, message) during normalization.
**Web Server Logs**
Web servers need monitoring because **every request/response could be a web attack attempt** (SQL injection, path traversal, scanning, etc.). On Linux, Apache commonly writes logs to `/var/log/apache` or `/var/log/httpd`.
**Sample Apache logs:**
192.168.21.200 - - [21/March/2022:10:17:10 -0300] "GET /cgi-bin/try/ HTTP/1.0" 200 3395 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64)... Chrome/98..."
127.0.0.1 - - [21/March/2022:10:22:04 -0300] "GET / HTTP/1.0" 200 2216 "-" "curl/7.68.0"
Reading these logs tells a story — this is the classic **Common Log Format**:
- **Source IP** `192.168.21.200` and `127.0.0.1`
- **Timestamp** `[21/March/2022:10:17:10 -0300]`
- **Request** `"GET /cgi-bin/try/ HTTP/1.0"`
- **Status code + size** `200 3395`
- **User agent**  the browser string, or notably `curl/7.68.0`
**Why the second line is interesting:** a request to `/cgi-bin/try/` from an external IP, followed by a request from **localhost using `curl`** (a command-line tool, not a browser), is exactly the kind of pattern a SIEM would **correlate**  it can suggest reconnaissance or scripted probing rather than normal user traffic.
**Log Ingestion  How Logs Reach the SIEM**
Collecting the logs is one thing; **getting them into the SIEM** is another. This section lists the four common ingestion methods:
**a) Agent / Forwarder**  
A lightweight tool installed **directly on the endpoint**. It's configured to capture important logs and ship them to the SIEM server. (This is the "lightweight agents" mentioned in the first document.) Best for endpoints you control and want deep visibility into.
**b) Syslog**  
A **widely used standard protocol** for collecting data from systems like web servers and databases and sending it in **real time** to a centralized destination. Common for network devices and Linux/Unix systems.
**c) Manual Upload**  
Some SIEMs (e.g., Splunk) let you **upload offline data** for quick analysis. Once ingested, it's normalized and available for analysis. Useful for one-off investigations or importing historical data.
**d) Port-Forwarding**  
The SIEM is configured to **listen on a specific port**, and endpoints **forward their data to that port**. A simple, network-based approach that doesn't require installing anything on the source device.
The section notes that **Splunk**, for example, offers several of these ingestion options through its interface.
![[Pasted image 20261026151806.png]]