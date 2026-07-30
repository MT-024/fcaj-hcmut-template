---
title: "FCAJ x AABW Hackathon"
date: 2026-07-25
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
includeInReport: false
---

# Summary Report: FCAJ x AABW Hackathon (Agentic AI Build Week)

### Event Information

| | |
| --- | --- |
| **Event name** | FCAJ x AABW Hackathon — Agentic AI Build Week |
| **Date** | ~25/07/2026 (confirm with organizers if exact day differs) |
| **Location** | <!-- TODO: actual venue (e.g., HCMC or Hanoi) --> |
| **Role** | Attendee |
| **Format** | 4 teams pitch AI products built on AWS services, each presenting end-to-end |

### Event Objectives

- Combine FCAJ (AWS community in Vietnam) and AABW (Asia AWS Builders Workshop) for a focused build-week of **agentic AI** products on AWS.
- Compress the **build → demo → feedback** loop: each team pitches to a panel and the community.
- Encourage integrating many AWS services into a single product (not just one isolated model).

### Teams & products

#### 1. Team 3KA — *Hackathon Journey* (Huỳnh An Khương, Nguyễn Quốc Huy, Ngô Quang Khôi, Hoàng Lê Thành Đức, Đặng Nguyễn Phước Lộc, Đặng Trường Hưng)

A narrative-driven talk about the team's own hackathon journey.

**Key takeaways:**
- The slide deck was light on detailed text and carried mostly personal narrative.
- Reminder that **storytelling** matters as much as technical depth — even when the deck is light.

**Resources:**
- Slide: `AWS/FCAJ x AABW/Hackathon_Journey_3KA.pptx`


#### 2. One Team — *AI-Powered Conversation Ordering* (Anh Duy, Tran Dong, Doan Trung, Minh Viet, Anshul Roy)

An AI product that **reorders conversations** by priority / topic.

**Key takeaways:**
- A **crisp pitch flow**: Trigger → Problem → Product. Memorable, reusable for the final demo.
- Real problem: long multi-thread conversations where users don't want to re-read everything from the top.
- AI used as a **filter + reorder**, not a wholesale replacement of the conversation.

**Resources:**
- Slide: `AWS/FCAJ x AABW/OneTeam_CommunityDay.pptx`


#### 3. Plan V — *Solution Architect Professional Native App* (Pham Tien Thuan Phat, Huynh Hoang Long, Le Minh Nghia, Tran Dai Vi, Nguyen An)

An **AI assistant** for Solution Architects: from natural-language requirements → architecture options + diagram + cost estimate.

**Key takeaways:**
- **Real problem**: SA receives "build an AI system for SOP docs, need it by Thursday" — has to manually read the BRD and draw the architecture from scratch in hours.
- **Solution**: AI assistant helps:
  - Parse natural-language requirements.
  - Draft architecture options (hybrid-cloud, company-standard).
  - Generate **Drawio diagrams** + **AWS Architecture Icons**.
  - Estimate **cost for `ap-southeast-1`**.
  - **Refine** via chat sidebar with custom instructions per project.
- Live workflow & architecture demo.

**Resources:**
- Slide: `AWS/FCAJ x AABW/SA_Professional_Native_App.pptx`


#### 4. Signal Scout — *Corporate strategy early detection* (Le Tan Luc, Do Hoang Hieu, Trieu Quoc Hao, Nguyen Van Duy Khiem, Nguyen Cong Minh, Nguyen Tran Minh Quan)

A product that **detects corporate strategic shifts early** (restructuring signals) for Enterprise Risk Management, Competitive Intelligence, and B2B account management.

**Key takeaways:**
- **Key partners**: AWS, LangFuse, TinyFish, Apify.
- **Value propositions**:
  - Detect restructuring signals.
  - Analyze metrics & build scenarios.
  - Decision dashboard.
  - **Evidence-backed** — every inference carries its citation trail.
- **Customer segments**: Enterprise risk management, Competitive intelligence, B2B account management.
- Two proposed **architecture variants**: standard and cost-efficient — the exact "design for cost first, scale up as needed" mindset.

**Resources:**
- Slide: `AWS/FCAJ x AABW/SignalScout.pptx`

### What I learned from this event

- **Plan V** addresses a real SA pain point — worth studying how they combined LLMs with Drawio + AWS Architecture Icons + cost estimation in a single assistant workflow. Directly relevant to my capstone because **architecture diagrams** and **cost guardrails** are core deliverables.
- **Signal Scout** demonstrates the value of **evidence-backed decisions** and **partner-aware architecture** (LangFuse + TinyFish + Apify). Their two variants (standard vs cost-efficient) echo my own 200 USD budget constraint — useful pattern: **design the cost-efficient path up front**, then offer the standard path as the upgrade option.
- **One Team's** pitch flow (Trigger → Problem → Product) is concise and memorable. I will **reuse this structure** when I demo the heart-attack-risk API in the final presentation.
- **Team 3KA** reminded me that **storytelling** matters as much as technical depth — even when the slide deck is light on content.
- **Cross-cutting takeaway**: every team leaned heavily on generative AI for the build, but the differentiator was the **workflow integration** (parse → draft → diagram → estimate → refine). Generic chatbots were the baseline; **verticalized assistants** won.

### Applying to my capstone

- When I demo the capstone, I'll use One Team's pitch flow: **Trigger (real-time endpoint latency eats budget) → Problem (200 USD cap) → Product (Serverless Inference + Drift Detection + Cost guardrail)**.
- Borrowing Signal Scout's approach: design the **cost-efficient path** first (single region, no GPU, serverless) → only then consider a standard variant (multi-AZ, GPU spot) if the budget upgrades. This is how I'll present trade-offs cleanly when the mentor reviews.
- Plan V's pattern (LLM parses requirement → produces artifact) suggests I **break the inference script into reusable steps**: parse request → preprocess → predict → assemble response — independently testable, swappable.

### Some event photos

*Drop photos into the `images/` folder and embed them here. Suggested names:*
- `cover.jpg` — banner photo of the hackathon
- `teams.jpg` — all 4 teams
- `pitch.jpg` — a team pitching
- `judging.jpg` — panel / Q&A moment

> Overall, the hackathon showed that **a one-week build can produce a shipping product** — and that's exactly how I want to run the final stretch of my capstone.

### References

* Slide decks in `AWS/FCAJ x AABW/`:
  * `Hackathon_Journey_3KA.pptx`
  * `OneTeam_CommunityDay.pptx`
  * `SA_Professional_Native_App.pptx`
  * `SignalScout.pptx`