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

## 🔹 Segment 1 — Device Overview & Onboarding Context (≈5 min)

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

---

## 🔹 Segment 2 — Incidents Queue — The Analyst Starting Line (≈10 min)

**Objective:** Show how multi-signal incidents form and why correlation matters.

### 🧩 Steps
1. Navigate to **Investigation & Response → Incidents & Alerts → Incidents**.  
2. View the columns:
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

---

## 🔹 Segment 3 — Investigate the Multi-Stage Ransomware Incident (≈10 min)

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
