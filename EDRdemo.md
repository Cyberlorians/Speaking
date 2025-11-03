# 🎥 Microsoft Defender for Endpoint — Show & Tell (50-Minute Live Session)

## 🎯 Session Goal

Demonstrate the end-to-end analyst workflow in Microsoft 365 Defender for Endpoint — from data collection → incident correlation → investigation → response → custom detection.

> Slides have already been presented — this session is **fully live** and portal-driven.

## 🕒 Timeline Overview

| Segment | Duration     | Focus                                         |
| ------- | ------------ | --------------------------------------------- |
| 1       | 5 min        | Device Overview (EDR context)                 |
| 2       | 10 min       | Incidents Queue — XDR correlation             |
| 3       | 10 min       | Multi-Stage Ransomware Investigation          |
| 4       | 15 min       | Response Actions — containment, Live Response |
| 5       | 10 min       | Hands-On Lab — Custom Detection Rule          |
| —       | 50 min total |                                               |

---

<details>
<summary><strong>🔹 Segment 1 — Device Overview & Onboarding Context (≈5 min)</strong></summary>

**Objective:** Show what EDR “sees” before alerts form.

**Steps**

1. Open **Microsoft 365 Defender → Devices** → select:

   * One **workgroup (non-Entra joined)** device
   * One **Entra-joined (EIDJ)** device
2. Open the **Overview** tab for each.

**Say**

> “Every onboarded endpoint continuously sends telemetry — process starts, network connects, and file writes.
> Notice how the Entra-joined device has richer identity and compliance context.
> That identity linkage becomes critical once incidents are correlated.”

**Highlight**

* Device tags
* Risk level
* Onboarding status
* Defender version / Last seen

**Transition**

> “Now that we’ve seen a single device’s telemetry, let’s explore how Defender automatically correlates that data into incidents.”

</details>

---

<details>
<summary><strong>🔹 Segment 2 — Incidents Queue — The Analyst Starting Line (≈10 min)</strong></summary>

**Objective:** Show how multi-signal incidents form and why correlation matters.

**Steps**

1. Navigate to **Investigation & response → Incidents & alerts → Incidents**.
2. View columns:
   *Incident Name, Severity, Investigation State, Categories, Impacted Assets, Service Source, Detection Source, Product Names*.
3. Apply filter → `Service source = Endpoint`.

**Say**

> “This is where analysts live day-to-day.
> Each line represents one or more correlated alerts — across Endpoint, Identity, Office, and Sentinel.
> This unified view is Microsoft’s XDR story in action.”

**Demo focus**

* Highlight **“Multi-stage incident involving Persistence & Defense evasion including Ransomware on one endpoint.”**
* Open it.

**Transition**

> “This one combines persistence, defense evasion, and ransomware execution — all grouped automatically. Let’s watch how Defender tells that story.”

</details>

---

<details>
<summary><strong>🔹 Segment 3 — Investigate the Multi-Stage Ransomware Incident (≈10 min)</strong></summary>

**Objective:** Walk through a correlated ransomware chain — persistence, defense evasion, and execution.

**Setup**

> “We’ll dive into a multi-stage ransomware incident. Defender correlated PowerShell abuse, profile tampering, and encryption events into one cohesive case.”

**Step 1 — Open the incident**

1. Filter: `Status = New, In progress` and `Service source = Endpoint`.
2. Open **Incident ID 1675 — “Multi-stage incident involving Persistence & Defense evasion including Ransomware on one endpoint.”**

> “Defender automatically classified this as *Ransomware* and grouped all supporting alerts.”

**Step 2 — Review the Attack Story**

1. Click the **Attack story** tab.
2. Narrate the nodes: *Initial Access → Persistence → Defense Evasion → Execution → Encryption.*

> “Each node maps to a MITRE ATT&CK phase and shows chronological correlation.”

**Step 3 — Explore Related Alerts**
Open **Related alerts** and highlight:

* *Ransomware-linked threat actor detected* (High)
* *Possible PowerShell profile tampering* (Medium)
* *Unexpected behavior observed by a process ran with no command line arguments* (Medium)

> “This is layered evidence — high-confidence ransomware behavior with supporting context that tells the full compromise story.”

**Step 4 — Deep-dive into one alert**

1. Open **Possible PowerShell profile tampering**.
2. In **Alert story**:

   * Expand **Process tree**
   * Review **Command line**
   * Note the MITRE mapping (e.g., T1546 — Event-Triggered Execution)

> “Defender extracted the full PowerShell command, mapped it to MITRE, and tied it back to the same endpoint.”

**Step 5 — Pivot to the device**

1. Click the device entity (*entraassessment*).
2. On the device page:

   * Open **Timeline** (or **Alerts** tab if Timeline isn’t shown)
   * Filter to the same time window
   * Show PowerShell start, dropped file, encryption process

> “This is the forensic backbone — full event chronology before, during, and after encryption.”

**Step 6 — Optional Live Response**

1. Click **Initiate Live Response** on the top bar.
2. Once connected, run:

   ```powershell
   tasklist
   dir C:\Users
   ```

> “Live Response gives analysts a secure, audited shell — perfect for collecting memory, files, or killing processes remotely.”

**Step 7 — Wrap the investigation**
Back to **Incident summary** and call out:

* **Severity:** High
* **Service source:** Endpoint
* **Detection source:** EDR
* **Investigation state:** In progress / Completed

> “From initial execution through encryption, Defender stitched everything together into one story — not scattered alerts.”

**Transition**

> “Next, let’s see how we’d contain or remediate this device using response actions.”

</details>

---

<details>
<summary><strong>🔹 Segment 4 — Response Actions (≈15 min)</strong></summary>

**Objective:** Demonstrate containment, isolation, and forensic collection.

**Steps**

1. From the device page, show the top action bar:

   * 🧩 **Run antivirus scan**
   * 📦 **Collect investigation package**
   * 🔒 **Isolate device** *(hover only)*
   * 💻 **Initiate Live Response**
2. Start a **Live Response** session and run:

   ```powershell
   dir C:\Users
   tasklist
   ```

> “This is a secure PowerShell-like console, with every command logged for audit.”

3. Back to **Incident → File entity**

   * Highlight **Stop & Quarantine**, **Add Indicator**, **Download File**

> “All analyst actions flow through **Action center** for tracking and rollback.”

**Transition**

> “Now let’s build our own detection to catch this behavior automatically.”

</details>

---

<details>
<summary><strong>🔹 Segment 5 — Hands-On Lab: Custom Detection Rule (≈10 min)</strong></summary>

**Objective:** Create a custom alert for remote PowerShell abuse and validate it live.

**Step 1 — Explain the goal**

> “We’ll detect PowerShell remote code execution patterns and promote this hunt into a scheduled detection rule.”

**Step 2 — Build the hunting query**

1. Open **Hunting → Advanced hunting**.
2. Paste and run:

   ```kql
   DeviceProcessEvents
   | where ProcessCommandLine has_any ("Invoke-WebRequest")
   | extend Account = tostring(InitiatingProcessAccountName)
   | project Timestamp, DeviceName, FileName, ProcessCommandLine, Account
   ```
3. Show any results (or empty).

> “This identifies PowerShell commands often used in remote code execution.”

**Step 3 — Create the detection rule**

1. Click **New detection rule → Create from query**
2. Configure:

   * **Name:** `Remote Code Execution – PowerShell Abuse`
   * **Run frequency:** Every 1 hour
   * **Lookback period:** 1 hour
   * **Severity:** High
   * **Category:** Execution → Command and Control
   * **Assign to:** SOC group or yourself
   * **Save**

> “Now Defender continuously runs this logic and auto-generates alerts when matched.”

**Step 4 — Test trigger (lab only)**

> ⚠️ Run only in a safe lab VM — not production.

Execute:

```cmd
cmd.exe /c powershell.exe -ExecutionPolicy Bypass -NoProfile -Command "Invoke-WebRequest -Uri 'https://sacyberrange00.blob.core.windows.net/vm-applications/7z2408-x64.exe' -OutFile C:\ProgramData\7z2408-x64.exe; Start-Process 'C:\ProgramData\7z2408-x64.exe' -ArgumentList '/S' -Wait"
```

* Simulates downloader + execution behavior.
* Wait 1–3 minutes → refresh **Incidents → filter Service source = Endpoint**.

> “You should now see a *Custom detection rule* incident appear.”

**Step 5 — Validate the detection**

1. Open the new incident.
2. Show:

   * **Detection source:** Custom detection rule
   * **Product name:** Microsoft 365 Defender
3. Pivot alert → file → device.

> “We turned a hunt into continuous detection and verified the end-to-end automation.”

**Step 6 — Clean up**

```powershell
Remove-Item C:\ProgramData\7z2408-x64.exe
```

Disable or delete the custom rule if not needed.

</details>

---

<details>
<summary><strong>🔹 Wrap-Up & Key Takeaways (≈2 min)</strong></summary>

> “This is the full lifecycle of EDR — continuous telemetry, correlation, manual and automated response, and custom detection.”

**Optional demo query**

```kql
DeviceFileEvents
| where Timestamp >= ago(7d)
| take 5
```

> “Six months of raw telemetry means you can always go back and hunt deeper.”

</details>

---

<details>
<summary><strong>✅ Pre-Session Checklist</strong></summary>

| Item             | Description                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| 🎓 Lab tenant    | Ensure 1–2 existing incidents (e.g., 1675 ransomware chain).           |
| 🧩 Permissions   | Account has Security Operations role.                                  |
| 💻 Device access | One onboarded VM for Live Response demo.                               |
| 🚫 Safety        | Don’t isolate or quarantine production assets.                         |
| 🧠 Backup        | Keep screenshots for Attack story and Process tree in case of latency. |

</details>

---

<details>
<summary><strong>🧭 Closing line</strong></summary>

> “Everything you’ve seen today — from telemetry to response — exists in your tenant right now.
> The more you hunt, tune, and tag, the smarter your EDR becomes.”

</details>
