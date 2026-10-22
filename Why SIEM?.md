SIEM is a security solution that collects logs from various types of log sources, standardizes their format into a consistent one, correlates them, and detects malicious activities using detection rules.
**Features of SIEM**
      **1.Centralized Log Collection**
Instead of physically logging into every endpoint, server, or firewall one by one to check its logs, a SIEM **pulls all logs into a single central location**. This is done using lightweight agents installed on devices or via APIs. 
The benefit: you get a single place to analyze everything, rather than "jumping on every machine individually."
     **2. Normalization of Logs**
Different systems produce logs in wildly different formats  a Windows log looks nothing like a Linux log. Since the SIEM is gathering all these logs together, it must make them comparable. This involves two steps:-
- Parsing:- breaking a raw log down into individual fields (e.g., timestamp, user, source IP, action) so it's easy to understand.
- Normalization:-  converting logs from all these different sources into **one consistent format**, so they can be analyzed side by side.
    3, **Correlation of Logs**
A single log entry on its own usually tells you very little. **Correlation** means the SIEM looks across logs from multiple sources and finds relationships between them, which can reveal malicious patterns.
The example given: over a 5-minute window, Haris:
- Logs in via VPN from an IP he's never used before
- Accesses documents on a shared drive
- Executes a script
- The system makes an outbound network connection
Each event alone looks harmless. But **correlated together**, they suggest a possible **data exfiltration** attack — someone using Haris's stolen VPN credentials. This is the power of correlation: seeing the bigger picture.
    **4. Real-time Alerting**
The SIEM contains **detection rules** (many built in by default, plus custom ones analysts write as their needs mature). When the conditions of a rule are met, an **alert is triggered** and analysts are notified — in real time. Analysts can then investigate the alert right inside the SIEM platform.
     **5. Dashboards and Reporting**
Dashboards are described as **the most important component** of a SIEM. After logs are normalized and ingested, the SIEM analyzes them and presents the summary as **actionable insights** through dashboards. There are default dashboards plus the option to build custom ones.
Typical dashboard content includes:
- Alert highlights
- System notifications
- Health alerts
- Failed login attempts
- Event ingestion counts
- Rules triggered
- Top domains visited
![[Pasted image 20261022162003.png]]