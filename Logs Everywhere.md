 1) **Host-Centric Log Sources**
These log sources capture events that occurred within or related to the host. Devices that generate host centric logs include Windows, Linux, servers, etc. Some examples of host-centric logs are:
- A user accessing a file
- A user attempting to authenticate.
- A process execution activity
- A process adding/editing/deleting a registry key or value.
- PowerShell execution
 1) **Network-Centric Log Sources**
Network-related logs are generated when the hosts communicate with each other or access the internet to visit a website. Devices that generate network-centric logs are firewalls,IDS/IPS, routers, etc. Some examples of network-centric logs are:
- SSH connection
- A file being accessed via FTP
- Web traffic
- A user accessing the company's resources through VPN
- Network file sharing Activity
Together, these host-centric and network-centric log sources constantly create numerous logs in a network.
## Answers Nowhere
It seems pretty straightforward that these log sources generate logs, we analyze them, and identify malicious activities. However, it's not that simple. It has some challenges those are 
1. Numerous Log Sources
**What it says:** A network has many devices, each generating hundreds of events per second. Logs are scattered everywhere.
**What it means in practice:**-Imagine a small company with:
- 50 employee laptops
- 10 servers
- 5 firewalls
- 3 routers
- A cloud environment (AWS/Azure)
- Email gateway, antivirus, VPN...
Every single one of these generates logs. A firewall alone might produce **thousands of events per second**. Multiply that across all devices, and you're drowning in data.
**Why it's a problem?** During an incident you can't manually check every device. It's like searching for a needle in a haystack  except there are hundreds of haystacks.
2. No Centralization
**What it says:** Logs live on the machine that generated them. To check them, you'd have to SSH/RDP into each device individually.
**What it means in practice:**  
Say you suspect a breach. You'd have to:-
          SSH into Server A → check logs
          SSH into Server B → check logs
          Log into Firewall → check logs
         Log into VPN → check logs
         ...repeat 50 times
**Why it's a problem?** because It's slow and inefficient. By the time you finish checking device #20, the attacker has moved on. This is exactly what a centralized log management system (like a SIEM) solves all logs in one place.
3. Limited Context (This is the most important one!)
**What it says:** A single log tells you nothing suspicious. But **correlating** multiple logs reveals the attack.
**Example  the "harmless" file access:**
- Log on Server B: `User John accessed file payroll.xlsx` → _Looks totally normal._
- Log on Server A: `John logged in from IP 10.0.0.5 at 2 AM` → _Odd, but maybe okay._
- Log on Firewall: `Outbound connection to known-malicious IP from Server A` → _Uh oh._
- Log on VPN: `Login from John's account from a country he's never been to` → _Now the picture is clear._
**Conclusion:** John's account was compromised → attacker moved **laterally** from Server A → Server B → stole payroll data.
**Key concept:** Each log alone = innocent. Combined = a full attack story. This is called **correlation** and it's the heart of SIEM.
 4. Limited Analysis
**What it says:** Humans can't manually analyze millions of logs per second.
**Why it's a problem:**
- A human analyst might realistically review a few thousand logs per day.
- A medium network generates **millions per day**.
- Even a dedicated team **will miss things** not because they're bad, but because of sheer volume.
**The solution direction:** Automated detection rules, anomaly detection, and machine learning (all part of SIEM/SOAR).
5. Format Issues
**What it says:** Every log source speaks its own "language."
**Examples of formats:**
- Firewall → syslog (RFC 5424)
- Windows → EVTX (XML-based)
- Web server → Apache/Nginx access logs (custom text)
- Cloud → JSON
- Router → Cisco proprietary format
- Antivirus → vendor-specific CSV/XML
**Why it's a problem:** An analyst would need to learn every format. A SIEM solves this by **normalizing** converting all formats into one common schema (e.g., ECS - Elastic Common Schema).
![[Pasted image 20261021151510.png]]