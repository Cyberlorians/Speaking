Got it — you want the **entire 50-minute demo** (all 5 segments, plus intro and wrap-up) formatted as one clean, collapsible Markdown file for GitHub.

Here’s the complete version — everything from start to finish, compact, readable, and GitHub-ready.
Each section uses `<details>` and `<summary>` so you can expand/collapse during the session.

---

````markdown
# 🎥 Microsoft Defender for Endpoint — Show & Tell (50-Minute Live Session)

---

## 🎯 Session Goal
Demonstrate the end-to-end analyst workflow in Microsoft 365 Defender for Endpoint — from data collection → incident correlation → investigation → response → custom detection.

> Slides have already been presented — this session is **fully live** and portal-driven.

---

## 🕒 Timeline Overview
| Segment | Duration | Focus |
|----------|-----------|-------|
| 1 | 5 min | Device Overview (EDR context) |
| 2 | 10 min | Incidents Queue — XDR correlation |
| 3 | 10 min | Multi-Stage Ransomware Investigation |
| 4 | 15 min | Response Actions — containment, Live Response |
| 5 | 10 min | Hands-On Lab — Custom Detection Rule |
| — | 50 min total | |

---

<details>
<summary><h2>🔹 Segment 1 — Device Overview & Onboarding Context (≈5 min)</h2></summary>

**Objective:** Show what EDR “sees” before alerts form.

### 🧩 Steps
1. Open **Microsoft 365 Defender → Devices** → select:
   - One **workgroup (non-Entra joined)** device
   - One **Entra-joined (EIDJ)** device  
2. Open the **Overview** tab for each.

### 💬 Narration
> “Every onboarded endpoint continuously sends telemetry — process starts, network connects, and file writes.  
> Notice how the Entra-joined device has richer identity and compliance context.  
> That identity linkage becomes critical once incidents are correlated.”

### 🔍 Callouts
- Device tags  
- Risk level  
- Onboarding status  
- Defender version / Last seen  

### 🧭 Transition
> “Now that we’ve seen a single device’s telemetry, let’s explore how Defender automatically correlates that data into incidents.”
</details>

---

<details>
<summary><h2>🔹 Segment 2 — Incidents Queue — The Analyst Starting Line (≈10 min)</h2></summary>

**Objective:** Show how multi-signal incidents form and why correlation matters.

### 🧩 Steps
1. Navigate to **Investigation & Response → Incidents & Alerts → Incidents**.  
2. View columns:
   - *Incident Name, Severity, Investigation State, Categories, Impacted Assets, Service Source, Detection Source, Product Names*
3. Apply filter → `Service source = Endpoint`.

### 💬 Narration
> “This is where analysts live day-to-day.  
> Each line represents one or more correlated alerts — across Endpoint, Identity, Office, and Sentinel.  
> This unified view is Microsoft’s XDR story in action.”

### 🧩 Demo Focus
- Highlight **‘Multi-stage incident involving Persistence & Defense evasion including Ransomware’**  
- Open it.

> “This one combines persistence, defense evasion, and ransomware execution — all grouped automatically. Let’s watch how Defender tells that story.”
</details>

---

<details>
<summary><h2>🔹 Segment 3 — Investigate the Multi-Stage Ransomware Incident (≈10 min)</h2></summary>

**Objective:** Walk through a correlated ransomware chain — persistence, defense evasion, and execution.

### 🧭 Setup
> “We’ll dive into a multi-stage ransomware incident.  
> Defender correlated PowerShell abuse, profile tampering, and encryption events into one cohesive case.”

---

### 🪟 Step 1: Open the Incident
1. Filter: `Status = New, In progress` and `Service source = Endpoint`.  
2. Open **Incident ID 1675 — ‘Multi-stage incident involving Persistence & Defense evasion including Ransomware on one endpoint.’**

> “Defender automatically classified this as *Ransomware* and grouped all supporting alerts.”

---

### 📊 Step 2: Review the Attack Story
1. Click the **Attack Story** tab.  
2. Narrate the nodes:
   - *Initial Access → Persistence → Defense Evasion → Execution → Encryption.*

> “Each node maps to a MITRE ATT&CK phase and shows chronological correlation.”

---

### 🧩 Step 3: Explore Related Alerts
Open **Related Alerts** and highlight:
- *Ransomware-linked threat actor detected* (High)  
- *Possible PowerShell profile tampering* (Medium)  
- *Unexpected behavior observed by a process ran with no command line arguments* (Medium)

> “This is layered evidence — high-confidence ransomware behavior with surrounding context that tells the full compromise story.”

---

### 🔍 Step 4: Deep-Dive into One Alert
1. Open **Possible PowerShell profile tampering**.  
2. In the **Alert Story**:
   - Expand **Process Tree**  
   - Review **Command Line**  
   - Note the MITRE mapping (T1546 – Event-Triggered Execution)

> “Defender extracted the full PowerShell command, mapped it to MITRE, and tied it back to the same endpoint.”

---

### 🧱 Step 5: Pivot to the Device
1. Click the device entity (*entraassessment*).  
2. On the Device page:
   - Open **Timeline** (or Alerts tab if Timeline is hidden).  
   - Filter to the same time window.  
   - Show PowerShell start, dropped file, encryption process.

> “This is the forensic backbone — full event chronology before, during, and after encryption.”

---

### ⚙️ Step 6: Optional Live Response
1. Click **Initiate Live Response** on the top bar.  
2. Once connected, run:
   ```powershell
   tasklist
   dir C:\Users
````

> “Live Response gives analysts a secure, audited shell — perfect for collecting memory, files, or killing processes remotely.”

---

### 🔚 Step 7: Wrap the Investigation

Back to **Incident Summary** and call out:

* **Severity:** High
* **Service Source:** Endpoint
* **Detection Source:** EDR
* **Investigation State:** In Progress / Completed

> “From initial execution through encryption, Defender stitched everything together into one story — not scattered alerts.”

### 🧭 Transition

> “Next, let’s see how we’d contain or remediate this device using response actions.”

</details>

---

<details>
<summary><h2>🔹 Segment 4 — Response Actions (≈15 min)</h2></summary>

**Objective:** Demonstrate containment, isolation, and forensic collection.

### 🧩 Steps

1. From Device page, show the top action bar:

   * 🧩 **Run antivirus scan**
   * 📦 **Collect investigation package**
   * 🔒 **Isolate device** *(hover only)*
   * 💻 **Initiate Live Response**
2. Start a Live Response session and run:

   ```powershell
   dir C:\Users
   tasklist
   ```

> “This is a secure PowerShell-like console, with every command logged for audit.”

3. Back to **Incident → File Entity**

   * Highlight **Stop & Quarantine**, **Add Indicator**, **Download File**

> “All analyst actions flow through Action Center for tracking and rollback.”

### 🧭 Transition

> “Now let’s build our own detection to catch this behavior automatically.”

</details>

---

<details>
<summary><h2>🔹 Segment 5 — Hands-On Lab: Custom Detection Rule (≈10 min)</h2></summary>

**Objective:** Create a custom alert for remote PowerShell abuse and validate it live.

---

### 🧩 Step 1 — Explain the Goal

> “We’ll detect PowerShell remote code execution patterns and promote this hunt into a scheduled detection rule.”

---

### 🧩 Step 2 — Build the Hunting Query

1. Open **Hunting → Advanced Hunting**.
2. Paste and run:

   ```kql
   DeviceProcessEvents
   | where ProcessCommandLine has_any ("Invoke-Expression", "IEX", "WebClient", "DownloadString")
   | extend Account = tostring(InitiatingProcessAccountName)
   | project Timestamp, DeviceName, FileName, ProcessCommandLine, Account
   ```
3. Show any returned results (or empty).

> “This identifies PowerShell commands often used in remote code execution.”

---

### 🧩 Step 3 — Create the Detection Rule

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

---

### 🧩 Step 4 — Test Trigger (Lab Only)

> ⚠️ **Run only in a safe lab VM — not production.**

Copy and execute:

```cmd
cmd.exe /c powershell.exe -ExecutionPolicy Bypass -NoProfile -Command "Invoke-WebRequest -Uri 'https://sacyberrange00.blob.core.windows.net/vm-applications/7z2408-x64.exe' -OutFile C:\ProgramData\7z2408-x64.exe; Start-Process 'C:\ProgramData\7z2408-x64.exe' -ArgumentList '/S' -Wait"
```

* This simulates downloader + execution behavior.
* Wait 1–3 minutes → refresh **Incidents → filter Service Source = Endpoint.**

> “You should now see a *Custom Detection Rule* incident appear.”

---

### 🧩 Step 5 — Validate the Detection

1. Open the new incident.
2. Show:

   * **Detection Source:** Custom Detection Rule
   * **Product Name:** Microsoft 365 Defender
3. Pivot from alert → file → device.

> “We turned a hunt into continuous detection and verified the end-to-end automation.”

---

### 🧩 Step 6 — Clean Up

Run:

```powershell
Remove-Item C:\ProgramData\7z2408-x64.exe
```

Disable or delete the custom rule if not needed.

</details>

---

<details>
<summary><h2>🔹 Wrap-Up & Key Takeaways (≈2 min)</h2></summary>

> “This is the full lifecycle of EDR — continuous telemetry, correlation, manual and automated response, and custom detection.”

### Optional Demo Query

```kql
DeviceFileEvents
| where Timestamp >= ago(7d)
| take 5
```

> “Six months of raw telemetry means you can always go back and hunt deeper.”

</details>

---

<details>
<summary><h2>✅ Pre-Session Checklist</h2></summary>

| Item             | Description                                                            |
| ---------------- | ---------------------------------------------------------------------- |
| 🎓 Lab Tenant    | Ensure 1–2 existing incidents (e.g., 1675 Ransomware chain).           |
| 🧩 Permissions   | Account must have Security Operations role.                            |
| 💻 Device Access | One onboarded VM for Live Response demo.                               |
| 🚫 Safety        | Don’t isolate or quarantine production assets.                         |
| 🧠 Backup        | Keep screenshots for Attack Story and Process Tree in case of latency. |

</details>

---

<details>
<summary><h2>🧭 Closing Line</h2></summary>

> “Everything you’ve seen today — from telemetry to response — exists in your tenant right now.
> The more you hunt, tune, and tag, the smarter your EDR becomes.”

</details>
```

---

✅ **Usage tip for GitHub:**
When you paste this into your `Cyberlorians/Speaking/EDR-ShowAndTell.md`, the collapsible sections will render cleanly and make navigation easy during the live demo.
Each `<details>` can expand or collapse so you can focus only on the segment you’re currently delivering.
