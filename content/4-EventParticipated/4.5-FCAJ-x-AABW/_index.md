---
title: "FCAJ x AABW Hackathon"
date: 2026-07-25
weight: 5
chapter: false
pre: " <b> 4.5. </b> "
includeInReport: false
---
{{% notice info %}}
**Event:** FCAJ x AABW Hackathon (Agentic AI Build Week)
**Date:** ~25/07/2026 (file modification date — confirm with organizers)
**Format:** 4 teams pitch AI products built on AWS services
{{% /notice %}}


### Teams & products

#### 1. Team 3KA — *Hackathon Journey* (HUỲNH AN KHƯƠNG, NGUYỄN QUỐC HUY, NGÔ QUANG KHÔI, HOÀNG LÊ THÀNH ĐỨC, ĐẶNG NGUYỄN PHƯỚC LỘC, ĐẶNG TRƯỜNG HƯNG)

A hackathon journey story. Slide deck had no detailed text — attendance noted.

**Resources:**
- Slide: `AWS/FCAJ x AABW/Hackathon_Journey_3KA.pptx`


#### 2. One Team — *AI-Powered Conversation Ordering* (Anh Duy, Tran Dong, Doan Trung, Minh Viet, Anshul Roy)

**Key takeaways:**
- Product helps order conversations using AI.
- Pitch flow: The Trigger → The Problem → The Product.

**Resources:**
- Slide: `AWS/FCAJ x AABW/OneTeam_CommunityDay.pptx`


#### 3. Plan V — *Solution Architect Professional Native App* (Pham Tien Thuan Phat, Huynh Hoang Long, Le Minh Nghia, Tran Dai Vi, Nguyen An)

**Key takeaways:**
- **Problem:** SA receives request "build AI system for SOP docs, need it by Thursday" — has to manually read BRD, draw architecture from scratch.
- **Solution:** AI assistant helps:
  - Parse natural-language requirements.
  - Draft architecture options (hybrid-cloud, company-standard).
  - Generate Drawio diagrams + AWS Architecture Icons.
  - Estimate cost for `ap-southeast-1`.
  - Refine via chat sidebar with custom instructions per project.
- Workflow & architecture demo.

**Resources:**
- Slide: `AWS/FCAJ x AABW/SA_Professional_Native_App.pptx`


#### 4. Signal Scout — *Corporate strategy early detection* (Le Tan Luc, Do Hoang Hieu, Trieu Quoc Hao, Nguyen Van Duy Khiem, Nguyen Cong Minh, Nguyen Tran Minh Quan)

**Key takeaways:**
- Detect corporate strategic changes early.
- **Key partners:** AWS, LangFuse, TinyFish, Apify.
- **Value propositions:** detect restructuring signals, analyze metrics, build scenarios, present via dashboard, evidence-backed decisions.
- **Customer segments:** Enterprise risk management, Competitive intelligence, B2B account management.
- Two architecture variants: standard and cost-efficient.

**Resources:**
- Slide: `AWS/FCAJ x AABW/SignalScout.pptx`


### What I learned from this event

- **Plan V** addresses a real SA pain point — worth studying how they combined LLMs with Drawio + AWS Architecture Icons + cost estimation in a single assistant workflow. Directly relevant to my capstone because architecture diagrams and cost guardrails are core deliverables.
- **Signal Scout** demonstrates the value of evidence-backed decisions and partner-aware architecture (LangFuse + TinyFish + Apify). Their two variants (standard vs cost-efficient) echo my own 200 USD budget constraint — useful pattern: design the cost-efficient path up front, then offer the standard path as the upgrade option.
- **One Team**'s pitch flow (Trigger → Problem → Product) is concise and memorable. I will reuse this structure when I demo the heart-attack-risk API in the final presentation.
- **Team 3KA** reminded me that story-telling (the hackathon journey) matters as much as technical depth — even when the slide deck is light on content.
- Cross-cutting takeaway: every team leaned heavily on generative AI for the build, but the differentiator was the **workflow integration** (parse → draft → diagram → estimate → refine). Generic chatbots were the baseline; verticalized assistants won.

**Resources:**
- Slides folder: `AWS/FCAJ x AABW/`
  - `Hackathon_Journey_3KA.pptx`
  - `OneTeam_CommunityDay.pptx`
  - `SA_Professional_Native_App.pptx`
  - `SignalScout.pptx`