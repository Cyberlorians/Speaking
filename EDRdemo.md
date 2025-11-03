# 🎥 Microsoft Defender for Endpoint — Show & Tell (50-Minute Live Session)

---

## 🎯 Session Goal
Demonstrate the end-to-end analyst workflow in Microsoft 365 Defender for Endpoint — from data collection → incident correlation → investigation → response → custom detection.

> Slides already presented — this session is **fully live** (portal-driven storytelling).

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

**Objective:** show what EDR “sees” before alerts form.

**Do live**
1. Open **Microsoft 365 Defender → Devices** → pick two examples:  
   - A **workgroup** (non-Entra joined)  
   - An **Entra-joined (EIDJ)** device  
2. Open each **Overview** tab.

**Say**
> “Every onboarded endpoint feeds continuous telemetry — process starts, network connects, file writes.  
> Notice how the Entra-joined device carries richer identity and compliance context.  
> That identity linkage becomes important once incidents are correlated.”

**Highlight**
- Device tags  
- Risk level  
- Onboarding status  
- Defender version / Last seen  

**Transition**
> “Now let’s move from a single device’s telemetry to how Defender automatically correlates that data into incidents.”

---

## 🔹 Segment 2 — Incidents Queue — The Analyst Starting Line (≈10 min)

**Objective:** show how signals from multiple Defender products combine into one incident.

**Do live**
1. Navigate to **Investigation & Response → Incidents & Alerts → Incidents**.  
2. You’ll see something like your captured screen — columns for  
   *Incident Name, Severity, Investigation State, Categories, Impacted Assets, Service Source, Detection Source, Product Names.*

**Say**
> “This is where analysts live day-to-day.  
> Each line represents correlated alerts from Endpoint, Identity, Office, and even Sentinel — unified XDR in action.”

**Live steps**
- Filter → `Service source = Endpoint`  
- Highlight **‘Multi-stage incident involving Persistence & Defense evasion including Ransomware’**  
- Open it.

**Say**
> “This one combines persistence attempts, defense evasion, and ransomware execution — grouped automatically.  
> Let’s watch how Defender builds that storyline.”

---

## 🔹 Segment 3 — Investigate the Multi-Stage Ransomware Incident (≈10 min)

**Objective:** walk through correlated ransomware chain — persistence, defense evasion, and execution.

### 🧭 Setup
> “We’ll dive into a multi-stage ransomware incident.  
> Defender correlated PowerShell activity, tampering, and encryption into one cohesive case.”

### 🪟 1. Open Incident
1. Filter: `Status = New, In progress` and `Service source = Endpoint`.  
2. Open **Incident ID 1675 — ‘Multi-stage incident involving Persistence & Defense evasion including Ransomware on one endpoint.’**

> “Defender auto-classified this as *Ransomware* and grouped all supporting alerts.”

### 📊 2. Attack Story
1. Click **Attack story**.  
2. Narrate nodes: *Initial Access → Persistence → Defense Evasion → Execution → Encryption.*

> “Each node maps to MITRE ATT&CK phases and shows temporal correlation.”

### 🧩 3. Related Alerts
Open **Related alerts** and highlight:
- *Ransomware-linked threat actor detected* (High)  
- *Possible PowerShell profile tampering* (Medium)  
- *Unexpected behavior observed by a process ran with no command line arguments* (Medium)

> “High-confidence ransomware behavior surrounded by supporting signals — the full context of compromise.”

### 🔍 4. Deep-Dive into Alert
1. Open **Possible PowerShell profile tampering.**  
2. Expand **Process tree**, view **Command line**, note MITRE mapping.

> “Defender mapped this to T1546 (Event-Triggered Execution) and tied it to this endpoint automatically.”

### 🧱 5. Pivot to Device
1. Click device entity (*entraassessment*).  
2. On device page → **Timeline** → filter to incident timeframe.  
3. Show PowerShell start, dropped file, encryption events. If Timeline missing, use Alerts tab.

> “This gives a full chronological view — before, during, and after encryption.”

### ⚙️ 6. Live Response (optional)
1. Click **Initiate Live Response**.  
2. Run:
   ```powershell
   tasklist
   dir C:\Users
