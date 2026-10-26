 **Behind the Triggered Alerts Explained**
This document explains **how SIEM detections actually work**  the "magic" behind alerts  and what analysts do once an alert fires.
 1. Detection Rules: The Core Mechanism
A SIEM doesn't magically "know" what's bad. It relies on **detection rules**, which are essentially **logical expressions** (if-this-then-alert conditions). When the conditions are met, the rule triggers.
Examples given:
- **5 failed logins in 10 seconds** → alert: _Multiple Failed Login Attempts_
- **Successful login after multiple failures** → alert: _Successful Login After Multiple Login Attempts_ (a classic sign of brute-forcing)
- **Any USB plugged in** → alert (useful when USB is restricted by policy)
- **Outbound traffic > 25 MB** → alert: _Potential Data Exfiltration_ (thresold usually set by company policy)

The key insight: rules watch **field values** in logs  which is exactly why **normalized logs** (from the previous document) matter. A rule can only reliably check a field like `EventID` if every log source presents that field consistently.
2. How a Detection Rule Is Created — Two Use Cases
**Use-Case 1: Detecting Event Log Clearing**  
Adversaries often wipe logs during post-exploitation to erase their tracks. Windows logs **Event ID 104** whenever event logs are cleared. So the rule becomes:- If Log source = WinEventLog **AND** EventID = 104 → alert: _Event Log Cleared_
**Use-Case 2: Detecting the `whoami` Command**  
Attackers commonly run `whoami` after exploitation/privilege escalation to see who they're running as. To build this rule, you identify three fields:
- **Log source**  where the event is captured (WinEventLog)
- **Event ID**  the ID for process execution (4688)
- **NewProcessName**  the process to watch for (`whoami`)
 If Log source = WinEventLog **AND** EventCode = 4688 **AND** NewProcessName contains `whoami` → alert: _WHOAMI Command Execution Detected_
 
Notice the pattern: rules are built by combining **field-value pairs** with AND logic. This is why normalized logs are essential  the rule is only as good as the consistency of the fields it inspects.
**Alert Investigation: What Analysts Do**
Analysts spend most of their time on **dashboards**, which summarize network activity. When an alert fires, the analyst:
 - Examines the **events/flows associated with the alert**
 - Checks **which rule conditions were actually met**
 - Decides whether it's a **True Positive** or **False Positive**
Then, based on that determination:

| Outcome                           | Action                                                  |
| --------------------------------- | ------------------------------------------------------- |
| **False Positive**                | Tune the rule to prevent similar false alarms in future |
| **True Positive**                 | Investigate further                                     |
| —                                 | Contact the asset owner to ask about the activity       |
| **Suspicious activity confirmed** | Isolate the infected host; block the suspicious IP      |

**Key Takeaways**

- **Detection rules = logical if-then expressions** watching specific field values in logs.
- Rules depend on **normalized, parsed logs** — consistent fields make reliable rules possible.
- **Event IDs and process names** are common building blocks (e.g., 104 for log clearing, 4688 + `whoami` for recon).
- Alert response is a **triage cycle**: investigate → classify (true/false positive) → tune the rule or escalate/contain.
In short: the SIEM's "magic" is just well-designed rules watching the right fields — and skilled analysts deciding what each alert really means.
![[Pasted image 20261026152349.png]]