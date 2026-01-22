# 🏏 CricMix: Box Cricket Team Manager - PRD

## 1. Project Overview
**CricMix** is an intelligent league-management application designed for Box Cricket organizers. It automates the transition from raw WhatsApp availability lists to perfectly balanced, competitive teams. It integrates Google Sheets as a performance database and Splitwise for automated financial settlement, ensuring fair play and transparent expense sharing.

---

## 2. Core Functional Requirements

### A. Team Balancing & Rotation Logic
- **Skill-Based Distribution:** Reference Google Sheets for ratings (1-5) in Batting, Bowling, Fielding, and Wicketkeeping.
- **Wicketkeeper Rule:** Each team must have exactly one designated Wicketkeeper (stat >= 4).
- **Captaincy Logic:** Choose captains based on high aggregate skill + "Recency Rule" (cannot be captain if they led in the last 3 sessions).
- **Common Player Protocol:** If total players are odd (e.g., 13), designate one "Common Player" who bats/bowls for both sides.
  - **Rotation:** Track "Common History" to ensure this role rotates fairly among all members.
- **Social Mixing:** Prioritize putting players together who have NOT been on the same team in the last 2 weeks (referenced from Match History).
- **Regenerate Button:** Allow the organizer to re-shuffle teams before finalizing.

### B. WhatsApp Parser Spec
- **Input:** Raw text from WhatsApp (e.g., `11. Ritesh`, `12. KC + 1 (Krishna)`).
- **Cleaning:** Strip numbering, invisible characters, and symbols to match names against the Master Sheet.
- **Guest Detection:** Identify "+1" or "Guest" tags and link them to the preceding member (Host) for financial proxying.
- **Visuals:** Mark Guests with a specific icon (⚠️ or 👤+) in the UI.

### C. Splitwise Financial Integration
- **Default Currency:** QAR.
- **Expense Logic:** - Base Court Booking (e.g., QAR 400).
  - Dynamic Add-ons (Water, Balls, etc.).
- **Proxy Payment:** If a member brings a guest, the guest's share is automatically added to the member's Splitwise total.
- **Execution:** Post a single group expense to the Splitwise API only for players marked as "Present."

---

## 3. Data Architecture (Google Sheets)

| Sheet Name | Purpose | Key Columns |
| :--- | :--- | :--- |
| **Master_Players** | Player Stats | Name, Nickname, Batting, Bowling, Fielding, WK, Splitwise_ID |
| **Match_History** | Track Records | Date, Team_A, Team_B, Captains, Common_Player |
| **Rotation_Stats** | Fairness Tracking | Player_ID, Last_Captain_Date, Last_Common_Date |

---

## 4. UI/UX Design (CricMix Theme)
- **Vibe:** Modern "Night Stadium" dark mode.
- **Colors:** Deep Black/Grey, Neon Green (Grass), Cricket-Ball Red.
- **Components:**
  - **Input Area:** Text box for WhatsApp list.
  - **Draft View:** Side-by-side team cards with skill metrics and balance percentages.
  - **Settlement Popup:** Real-time cost-per-head calculator with "Add Expense" rows.

---

## 5. Technology Stack
- **Frontend:** Next.js (React) + Tailwind CSS.
- **Backend:** FastAPI (Python)
- **APIs:** Google Sheets API (v4), Splitwise API.
- **Deployment:** Dockerized for Linux VPS/HPC environment.

---

## 6. Commands for AI Agent
1. **"Parse this list":** Clean text and fetch stats.
2. **"Generate Teams":** Run the balancing algorithm with history check.
3. **"Finalize Match":** Save results to History and Rotation sheets.
4. **"Settle Up":** Create the Splitwise expense.