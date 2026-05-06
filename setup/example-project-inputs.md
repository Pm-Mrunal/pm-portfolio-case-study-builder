# Example: Completed Project Inputs

> Fictional example. Do not copy. Use as reference for structure and specificity level.
> The more detail you provide in each field, the less Claude will need to ask — and the stronger your case study.

---

## Project Basics

- **Project Title:** Self-Serve Onboarding Redesign
- **Company / Product:** Vantage (Series D, B2B SaaS — spend management)
- **Industry / Domain:** SaaS / Growth
- **Project Stage:** Shipped and fully live since Q3 2024
- **Timeline:** Q1 2024 – Q3 2024 (7 months end-to-end)

## Context

- **Business Context:** Vantage had grown to 14,000 monthly signups but free-to-paid conversion had stalled at 8% for three straight quarters. Sales-assisted conversion was 4x higher but required an SDR call, which didn't scale for SMB.
- **Business Goal:** Increase self-serve conversion to reduce CAC and open the SMB segment without adding headcount.
- **Why Now:** Board flagged CAC efficiency as a Q1 priority. New VP of Growth joined with a self-serve-first mandate.
- **Constraints:** No additional eng headcount. Had to work within the existing design system. Hard Q3 deadline tied to a board update.

## Problem

- **Problem Statement:** New signups couldn't reach their first meaningful action without a sales call. The onboarding flow was built for enterprise buyers who arrived pre-educated, not SMB founders who signed up cold.
- **Who Has the Problem:** SMB buyers — typically a founder or solo ops lead — who signed up from an ad or referral with no prior product knowledge.
- **Prior State:** Time to first meaningful action averaged 4.2 days. 62% of signups never completed the setup wizard. CS was running 300+ onboarding calls per month just to get users started.
- **Why It Mattered:** Every 1% improvement in 7-day activation translated to ~$900K ARR based on our conversion model.
- **Problem Dimensions (3-4 distinct numbered labels — use noun phrases, not questions):**
  1. Setup friction at the critical handoff — 71% of drop-offs concentrated at the API key step; users couldn't proceed without IT access they didn't have
  2. Value gap before commitment — users had to invest time connecting live data before seeing any product value; the aha moment was locked behind setup completion
  3. Audience mismatch in the flow — the wizard was designed for enterprise buyers who arrived educated; SMB founders arrived cold and interpreted it as a sales tour
  4. Operational ceiling from manual CS workload — 300+ onboarding calls per month was a fixed cost that wouldn't scale with growth

## Users

- **Primary Users:** SMB founders and solo operators, 1-50 person companies, first-time users
- **Secondary Users:** Mid-market ops leads inheriting an existing Vantage instance
- **User Needs:** Understand the product's core value in under 10 minutes. Complete initial setup without calling support. See a working dashboard before the end of the first session.

## Role and Scope

- **Title:** Senior Product Manager, Growth
- **Team Composition:** 1 PM (me), 2 FE engineers, 1 BE engineer, 1 designer, 1 data analyst (part-time)
- **What I Owned:** Discovery, problem framing, solution scoping, roadmap, acceptance criteria, experiment design, stakeholder comms to VP Growth and CEO
- **What I Influenced:** Design direction (approved all wireframes), instrumentation priorities, in-product copy (partnered with marketing)
- **What Others Owned:** Engineering architecture, design system compliance, QA, CS handoff playbook update
- **Cross-Functional Partners:** VP Growth, Head of CS, Marketing (copy), Data (instrumentation), Sales (SMB segment intel)

## Discovery

- **Research Methods:** 18 user interviews with churned free users, session recording analysis (200 sessions in Fullstory), funnel drop-off analysis across 6 months of data, 6 discovery calls with CS team
- **Top Insights:**
  - 71% of dropoffs happened at the "connect your data source" step — it required an API key users didn't have ready
  - Users who completed a sample dashboard in their first session converted at 3x the baseline rate
  - SMB users were skipping the guided walkthrough because it looked like a sales tour, not a setup tool
- **Named Insights (labeled findings — each should connect to a decision downstream):**
  - The API key wall: the single highest-friction step was a technical prerequisite that most SMB founders couldn't complete without IT help — this directly drove the sample data mode decision
  - The aha moment dependency: cohort analysis showed users who saw a working dashboard in session one converted at 3x — this made getting users to that moment the entire design goal
  - The mental model mismatch: SMB users interpreted a 9-step wizard as a sales process, not a setup tool — this drove the wizard simplification and reframing
- **Pull Quote (verbatim — only include if you have an actual user quote):**
  - Quote: "I just wanted to see what the product could do before handing over my company's data. I kept getting stuck at the API key step and eventually gave up."
  - Source: SMB founder, churned free user interview, February 2024
- **Assumptions That Changed:** We assumed users dropped because the product was complex. They actually dropped because of setup friction — the product tested well in moderated sessions once users got past setup.

## Strategy and Decisions

- **Selected Direction:** Full onboarding redesign anchored on getting users to a working dashboard before any data connection was required.

- **Named Decisions (provide at least 2 — for each, name the decision and include alternatives explicitly rejected with rationale):**

  **Decision 1: Sample data mode vs. requiring live data connection**
  - Choice: Build a sample data mode — pre-populated dashboard visible from first visit, no API key needed
  - Alternatives considered: Keep live data connection at step 1 (status quo); replace setup with a guided video demo
  - Why this: Cohort analysis showed users who saw a working dashboard in session one converted at 3x. Sample data mode was the fastest path to that moment within the engineering timeline.
  - Why not the alternatives: Live data first was the direct cause of 71% of drop-offs — keeping it would change nothing. Video demo tested poorly in moderated sessions; users skipped it and still churned at the data connection step.
  - Tradeoff accepted: Risk that sample-data users would never connect real data. Accepted because historical data showed sample-data cohort activation correlated with eventual paid conversion.

  **Decision 2: Full redesign vs. incremental fixes**
  - Choice: Full redesign of the onboarding flow from step 1
  - Alternatives considered: Add an in-app chat / concierge hybrid to guide users through existing flow; build a video walkthrough library for the setup steps
  - Why this: Both alternatives addressed guidance — the symptom. The root cause was setup friction at the data connection step, which guidance couldn't remove. A redesign was the only option that eliminated the friction rather than adding support around it.
  - Why not the alternatives: In-app chat would have added CS load instead of reducing it. Video library had been tried informally and users ignored it.

## Solution

- **Solution Summary:** Rebuilt onboarding around progressive disclosure. Deferred data connection to step 3. Added sample data mode. Redesigned the setup wizard to feel like product setup, not a sales tour.
- **Solution Breakdown Type:** By feature — each element of the solution maps to a specific problem dimension. Use this when the solution is a set of distinct product changes rather than a single unified system.
- **Key Changes:**
  - Sample data mode — pre-populated dashboard visible on first visit, no API key needed
  - Deferred data connection — moved from step 1 to step 3, after users had seen product value
  - Simplified setup wizard — reduced from 9 steps to 4, removed all "tell us about your company" friction fields
  - Progress indicator — showed users exactly how close they were to a working dashboard
- **Scope Boundaries:** Did not touch the post-onboarding product. Did not redesign the pricing page. Did not change the email nurture sequence (owned by Marketing).
- **Rollout:** 10% → 30% → 100% staged rollout over 3 weeks. Monitored activation and conversion rate at each stage.

## Execution

- **Major Milestones:** Discovery (6 weeks), Solution design + eng scoping (4 weeks), Build (8 weeks), Staged rollout (3 weeks)
- **Iterations:** 3 rounds of usability testing during build. Removed a personalization step in round 2 after it added confusion with no activation benefit.
- **Major Challenges:** Engineering pushback on sample data mode — BE lead estimated 5 weeks, not 3. Timeline would slip past the board update.
- **How Challenges Were Handled:** Scoped down to static sample data for V1 (vs. dynamic generation). Communicated the tradeoff to VP Growth with clear rationale. Shipped on time.

## Outcomes

- **Launch Status:** Fully live — 100% of new signups since July 2024
- **Quantitative Results:**
  - 7-day activation rate: 18% → 31% (72% relative lift)
  - CS onboarding calls: 300/month → 140/month (53% reduction)
  - Free-to-paid conversion: 8.1% → 10.4%
  - Estimated ARR impact: $2.1M in first two quarters (based on conversion model)
- **Qualitative Results:** CS reported significantly fewer "I don't know how to start" tickets. Sales reported faster SMB demo-to-close because prospects arrived more product-literate.
- **Validation Signals:** Activation lift held consistently across all SMB cohorts in the staged rollout. No regression in mid-market activation.

## Reflection

- **Key Learnings:** Setup friction and product complexity are different problems. Don't conflate them. The fix for setup friction is removing steps, not adding guidance.
- **What I Would Do Differently:** Would have run a parallel pricing page test — we left conversion on the table by not addressing pricing tier confusion visible in session recordings.
- **Open Questions:** Does sample data mode reduce long-term retention? Not enough cohort data yet to know.
- **What This Case Demonstrates:** Discovery-led problem reframing, scope negotiation under engineering constraints, comfort with growth metrics and funnel analysis, ability to ship in a resource-constrained environment.

## Source Documents Available

- PRD (internal)
- Fullstory session analysis summary
- Pre/post funnel dashboard screenshots
- User interview synthesis doc
