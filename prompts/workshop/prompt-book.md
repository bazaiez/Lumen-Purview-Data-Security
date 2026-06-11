# Customer Workshop and Demo Prompt Book

**Validation Status:** All [VALIDATED] — Workshop prompts verified for live customer demos
**Required Plugin:** Microsoft Purview
**Last Validated:** April 2026

Lumen — Purview Data Security: CSA-ready demo prompts, workshop scenarios, and talking point generators.

**Note:** These prompts are optimized for live customer presentations and workshops. They're designed to work reliably, generate compelling output, and create "wow" moments that show Purview + SC value. Avoid complex cross-signal correlations in demos; stick to validated, single-plugin capabilities.

**Reference Documents:**
- [Plugin Dependency Map](../../docs/plugin-dependency-map.md) — Required plugins
- [Validation Matrix](../validation-matrix.md) — Testing status for each prompt

---

## 1. Live Demo: "One Click Data Risk Summary"

**Title:** Data Risk Overview Live Demo

**Purpose:** Quickly demonstrate SC's ability to assess organizational data risk in real-time during customer presentation.

**When to Use:**
- Product demo (10-15 min slot)
- Customer workshop opening
- Sales meeting (show value quickly)
- Board presentation (impress executives)

**Exact Prompt:**
```
Quick data risk summary for our organization.

Provide:
1. Overall data risk score
2. Top 3 data risks and why they matter
3. Quick win we could address today

Keep it brief and visual - this is a 2-minute demo!
```

**Expected Output:**
- Risk score (Low/Medium/High/Critical) with visual indicator
- Top 3 bullets on major risks
- 1-2 sentence "quick win" suggestion
- Clean, presentation-ready format

**Limitations:**
- May be limited if org has minimal Purview data
- Customer may ask follow-up questions; have deep-dive prompt ready

**Tips:**
- Run this in customer's production environment (shows real data)
- Pre-check that their data looks reasonable (no surprises)
- Have follow-up prompts ready if they ask "why is that a risk?"

---

## 2. Live Demo: "Show Me Who Has Access to Our Most Sensitive Data"

**Title:** High-Risk User Access Discovery

**Purpose:** Demonstrate SC's ability to quickly identify over-privileged users and access anomalies.

**When to Use:**
- Demo for data governance team
- Compliance officer presentation
- Access control workshop
- Sales meeting (wow factor: "see what your team can access?")

**Exact Prompt:**
```
Identify the top 10 users with access to our most sensitive data
(PII, financial, confidential):

For each user show:
1. Their name/department
2. Types of sensitive data they can access
3. Is this access appropriate for their role? Flag if over-privileged
4. Any unusual access patterns (off-hours, anomalous locations)?

This is for customer demo - make it compelling and actionable!
```

**Expected Output:**
- User list with sensitivity levels (what data they access)
- Risk flag for each user (green = appropriate, yellow = review, red = over-privileged)
- Quick assessment of risk level
- Sample remediation action ("Remove access for users #3, #5, #7")

**Limitations:**
- Accuracy depends on complete access and role catalog
- Some over-access may be legitimate (require manual review)
- May need follow-up investigation if customer wants to act

**Tips:**
- This usually gets excited reaction (people are surprised by access)
- Have next step ready: "Want us to do access review for your team?"
- Don't speculate on intent; just flag risk and let customer investigate

---

## 3. Workshop Scenario: "Simulate a Data Breach Investigation"

**Title:** Incident Response Walkthrough

**Purpose:** Walk customers through incident investigation workflow using SC and Purview.

**When to Use:**
- Half-day workshop on incident response
- Security operations team training
- Playbook development session
- SIEM/SOAR integration discussion

**Exact Prompt:**
```
SCENARIO: We detected unusual activity on the "Customer PII" SharePoint
site on March 15. A user downloaded 500+ files and tried to email them
externally (email was blocked by DLP).

Walk through the investigation:

1. Timeline: What happened between 9 AM and 5 PM on March 15?
   - Who accessed the site?
   - What files were downloaded?
   - Were there any other suspicious activities?

2. User risk assessment:
   - Who is the user? What's their role?
   - Is their behavior baseline-normal or anomalous?
   - Any IRM signals or policy violations?

3. Data risk:
   - What data was accessed? (scope and sensitivity)
   - Was anything exfiltrated?

4. Next steps for the security team:
   - Immediate containment actions
   - Investigation priorities
   - Communication (who should be notified?)

Format as incident response runbook that the team could follow.
```

**Expected Output:**
- Chronological timeline of user activities
- Risk assessment narrative
- Data accessed summary
- Recommended containment steps
- Investigation checklist
- Escalation criteria and contacts
- Post-incident review checklist

**Limitations:**
- Depends on having realistic incident scenario data
- Actual incident may be messier/more complex than demo
- Response procedures require human decision-making (SC guides, doesn't decide)

**Tips:**
- Use a real incident from your org (anonymized) if available
- Pause after each section to let workshop participants discuss
- Highlight the speed (investigation that takes humans 4 hours, SC does in 5 min)

---

## 4. Workshop Scenario: "Label Taxonomy Design Workshop"

**Title:** Sensitivity Label Strategy Co-Design

**Purpose:** Guide customers through designing their sensitivity label taxonomy with SC input.

**When to Use:**
- Label strategy workshop (2-3 hours)
- Pre-implementation planning
- Label taxonomy review (optimization)
- Customer helping themselves design

**Exact Prompt:**
```
We're designing our sensitivity label taxonomy. 

Our business has these data types:
- Customer personal data (PII)
- Financial records and budgets
- Employee records (HR)
- Intellectual property
- Strategic plans
- Public marketing materials

For each data type, recommend:
1. Label name and level (we're thinking Public/Internal/Confidential/Highly Confidential)
2. Who should have access?
3. What protections should we apply? (encryption, external share restrictions, etc.)
4. Any special handling requirements?

Then suggest a finalized label taxonomy that would work for our organization.
```

**Expected Output:**
- Data type to label mapping
- Label descriptions (use cases, examples)
- Access control recommendations per label
- Protection settings (encryption, sharing restrictions, IRM)
- Sample documents/scenarios and how to label them
- Finalized label taxonomy (simplified, aligned to business needs)
- Rollout and user training recommendations

**Limitations:**
- Customer must make final decisions (SC can guide, not decide)
- Taxonomy may need adjustment post-launch
- Implementation requires actual portal configuration

**Tips:**
- This is a workshop exercise; encourage discussion
- Ask "do you agree?" at key decision points
- Mention it's iterative (can refine post-launch)
- Have label governance checklist ready (who approves changes, etc.)

---

## 5. Customer Talking Points Generator

**Title:** Custom Elevator Pitches and Talking Points

**Purpose:** Generate customer-specific value propositions and talking points for different audiences.

**When to Use:**
- Sales enablement (briefing your sales team)
- Customer executive briefing
- Board presentation preparation
- Proposal/RFP response writing

**Exact Prompt:**
```
Generate talking points for pitching Security Copilot + Purview to
[CUSTOMER PROFILE: e.g., "Financial Services Compliance Officer"]:

Customer context:
- Company size: [#]
- Industry: [Financial / Healthcare / Retail / Tech / etc.]
- Current pain point: [e.g., "Manual compliance audits take 3 weeks"]
- Success metric: [e.g., "Reduce audit time to 1 week"]

Generate:

1. Opening hook (1-2 sentences that grab attention):
   - Reference their pain point
   - Promise outcome they care about

2. Value props (3 bullets on what matters to THEM):
   - Not generic; specific to their role/industry
   - Quantified if possible (time saved, risk reduced, $ impact)

3. Customer success story (similar customer in their industry):
   - Company profile
   - Challenge they faced (similar to this customer?)
   - How SC + Purview solved it
   - Results/ROI

4. Common objections and responses:
   - "This will require big implementation" -> Response
   - "We already have [tool X]" -> Response
   - "We don't have the budget" -> Response

5. Call to action:
   - What's the next step? (pilot, trial, deeper conversation?)
   - Timeline and what success looks like

Format for delivery to [CUSTOMER] on [DATE].
```

**Expected Output:**
- 1-paragraph opening/value prop
- 3-5 bulleted talking points
- Customer success story (1/2 page)
- Objection handling (2-3 common objections with responses)
- Proposed next steps and timeline
- Supporting materials checklist (demo script, one-pager, case study)

**Limitations:**
- Talking points need your tone and messaging (customize SC output)
- Success story credibility depends on actual customer reference

**Tips:**
- Customize per customer (their industry, role, pain points)
- Practice the elevator pitch before customer call
- Have supporting materials ready (case study, ROI calculator)

---

## 6. Hands-On Lab: "Run Your Own Data Risk Investigation"

**Title:** Customer-Guided SC Exploration

**Purpose:** Give customer hands-on experience running SC prompts themselves (builds confidence and understanding).

**When to Use:**
- Full-day hands-on workshop
- Proof-of-concept validation
- Customer enablement after sales
- Pilot kickoff session

**Exact Prompt:**
```
HANDS-ON LAB: Guided Security Copilot Investigation

Work in groups of 2-3. Your goal: Investigate a simulated data risk
in your organization using SC.

Scenario: Your legal department flagged that a contract with sensitive
terms may have been accidentally forwarded to external email addresses.

Using SC and Purview, answer:

1. When did this happen and who was involved?
2. What sensitive documents were accessed/shared?
3. Who received the data externally?
4. What's the risk level?
5. What remediation steps would you recommend?

Resources:
- SC prompt template (provided)
- Purview data access guide
- Incident response checklist
- Lab facilitator (available for questions)

Deliverable: 2-page findings report with recommendations.

Time: 1 hour investigation + 30 min presentations

**Tip:** Start broad ("show me sensitive data shared externally"),
then narrow down ("who sent it?" "what was in it?")
```

**Expected Output:**
- Lab instructions and scenario
- SC prompt templates for common investigation tasks
- Purview navigation guide
- Reference materials (how to find data, interpret results)
- Grading rubric (for trainer evaluation)
- Sample findings report (for comparison)
- Debrief talking points (key lessons learned)

**Limitations:**
- Requires customer data access; may need scrubbed/demo data
- Hands-on labs take time; fits in half-day workshop minimum
- Customer success depends on their data quality and completeness

**Tips:**
- Start with guided example (everyone does together)
- Then let groups explore autonomously
- Debrief as group (share findings and approaches)
- Highlight clever investigation paths ("nice use of that feature!")

---

## 7. "Did We Close the Risk?" Post-Incident Validation Demo

**Title:** Remediation Validation Prompt

**Purpose:** Show how SC can validate that security improvements have actually reduced risk.

**When to Use:**
- Post-incident review (show remediation worked)
- Policy change validation
- Customer confidence-building
- Compliance closure

**Exact Prompt:**
```
BEFORE/AFTER DEMO: Show how SC validates security improvements.

Context: Three months ago, we identified that [RISK: e.g., "50+ users
had over-privileged access to PII data"]. We implemented remediation:
[ACTION: e.g., "Automated monthly access reviews, removed over-privileged
accounts"].

Now validate that the risk is actually reduced:

1. Current state:
   - How many users currently have over-privileged access?
   - Trend: is this number decreasing month-over-month?

2. Before vs. after comparison:
   - Original risk score: [X]
   - Current risk score: [Y]
   - Risk reduction: Z%

3. Remaining risks:
   - What over-privileges still exist and why?
   - Are these acceptable (known exception) or still problematic?

4. Continuous monitoring:
   - How will we detect if this risk re-emerges?
   - What alerts or reviews are in place?

Format as "Remediation Success Report" for leadership.
```

**Expected Output:**
- Before/after risk metric comparison
- Percentage reduction in risk
- Remaining exceptions (documented and approved)
- Continuous monitoring strategy
- Success metrics to track going forward
- Sign-off (who approved this remediation?)

**Limitations:**
- Requires having implemented remediation to show results
- May not show full ROI if incident was prevented (hard to quantify)
- Some improvements take time; may not be evident in 30 days

**Tips:**
- This works great for incidents that were actually remediated
- Shows investment in security actually pays off
- Builds confidence for next remediation project
- Great for board reporting on security improvements

---

## 8. "Build Your Security Metrics Dashboard" Workshop

**Title:** KPI Definition and Tracking Workshop

**Purpose:** Help customers define and track security KPIs using SC-generated data.

**When to Use:**
- Metrics/KPI workshop (2-3 hours)
- Executive reporting setup
- Security scorecard design
- Monthly metrics review process

**Exact Prompt:**
```
WORKSHOP: Define security metrics for monthly leadership reporting.

Our organization wants to report monthly on data security posture.
Currently we track: [existing metrics if any]

Help us define:

1. Core KPIs (the ones we MUST track):
   - Sensitivity label coverage (what % of data is labeled?)
   - Compliance status (what % of regulatory requirements met?)
   - Data risk score (overall organizational data risk)
   - Incident metrics (count, MTTR, impact)

2. Health indicators (leading indicators of problems):
   - Policy violations (DLP blocks, email over-shares)
   - Access review completion (on schedule? findings?)
   - User training (% completed, effectiveness)
   - Audit findings (new issues? remediation progress?)

3. For each metric, define:
   - Calculation method
   - Data source (where does SC get this?)
   - Target/goal
   - Red/yellow/green thresholds
   - Who's accountable?

4. Dashboard design:
   - What visualization for each metric? (trending chart, scorecard, heatmap)
   - Frequency (daily, weekly, monthly)?
   - Distribution (who sees this dashboard?)

5. Reporting process:
   - Who generates the report? (manual, automated via SC?)
   - Cadence (weekly, monthly?)
   - Escalation (when/who if metrics go red?)

Provide sample dashboard mockup and monthly reporting checklist.
```

**Expected Output:**
- KPI definitions with calculation methodology
- Target/threshold definitions (red/yellow/green)
- Data source specification (SC queries, manual input, etc.)
- Sample dashboard visualization (mock-up)
- Monthly reporting template
- Roles and responsibilities
- KPI review cadence and escalation procedures
- Historical data (if available) for trending

**Limitations:**
- Some metrics require data from multiple sources (coordination needed)
- Targets should be data-driven (may need baseline period)
- Reporting automation depends on SC/Purview API availability

**Tips:**
- Start with 5-7 core KPIs (not 20; too much noise)
- Make targets realistic (stretch goals that are achievable)
- Show trending (more important than absolute numbers)
- Get leadership buy-in on definitions before implementing

---

## 9. "Spot the Risks" Guided Assessment

**Title:** Quick Risk Identification Workshop

**Purpose:** Train customer security team to use SC to identify common data risks in their environment.

**When to Use:**
- Half-day security team workshop
- Operational readiness training
- Customer enablement post-sale
- Quarterly team training

**Exact Prompt:**
```
GUIDED ASSESSMENT: Common data risks to look for in your organization.

This exercise trains your team to use SC to identify and prioritize risks.

For each risk category, ask SC:

1. Sensitivity label gaps:
   "What percentage of our sensitive data is unlabeled or mislabeled?"
   "Which data repositories have <70% label coverage?"

2. Access anomalies:
   "Identify users with access that seems over-privileged for their role."
   "Are there service accounts with unusual sensitive data access?"

3. Compliance exposure:
   "Are we compliant with [GDPR/HIPAA/CCPA]?"
   "What compliance gaps do we have and how urgent are they?"

4. Data movement risks:
   "Is sensitive data being shared externally inappropriately?"
   "Identify unauthorized external email shares of confidential data."

5. Policy violations:
   "What are our top DLP violations and patterns?"
   "Are violations concentrated in teams/groups (training opportunity)?"

For each risk found, practice:
- Assess severity (Critical/High/Medium/Low)
- Identify root cause (config issue? user error? training gap?)
- Recommend remediation (quick fix? requires process change?)
- Estimate effort/timeline

Document findings in risk register and prioritize for action.

Time: 2-3 hours per category (can run over multiple sessions)
```

**Expected Output:**
- Guided assessment template with risk categories
- Sample SC prompts for each risk type
- Risk assessment framework (severity, root cause, remediation)
- Completed risk register from assessment
- Prioritized action list
- Ownership assignments (who owns each risk?)
- Trainer guide for facilitating assessment

**Limitations:**
- Takes time to work through all risk categories
- Some risks may require additional investigation
- Risk prioritization involves business judgment (not pure SC analysis)

**Tips:**
- Run this annually or quarterly
- Rotate team members so everyone learns
- Use this to populate risk register and roadmap
- Celebrate risks found and fixed (shows security program working)

---

## 10. Sales Enablement: "How to Demo SC to [Customer Type]"

**Title:** Role-Specific Demo Guidance

**Purpose:** Equip sales and CSAs with demo scripts tailored to different customer profiles.

**When to Use:**
- Sales kickoff training
- CSA onboarding
- Demo preparation before customer meeting
- Sales methodology documentation

**Exact Prompt:**
```
Create a demo script for pitching SC + Purview to: [CUSTOMER PROFILE]

Customer profile: [e.g., "Financial Services Compliance Officer"]
Meeting length: [15 min / 30 min / 1 hour]
Customer pain point: [e.g., "Audit takes 3 weeks; need to reduce to 1"]
Success metric: [what would make them say "yes"?]

Deliver:

1. Opening (1 min):
   - Grab attention with their pain point
   - Promise: "In this 15 minutes, I'll show you how to cut audit time in half"

2. Live demo (10 min):
   - What will you show? (pick 1-2 features that address their pain)
   - Talking points for each demo section
   - Expected customer reactions and how to handle them

3. Deep-dive questions (3 min):
   - Anticipate what they'll ask
   - Have answers and follow-up visuals ready

4. Closing (1 min):
   - Recap value
   - Next steps: "Would you be interested in a 2-week pilot?"

5. Supporting materials:
   - One-pager on Purview + SC for this customer type
   - ROI calculator (time/cost savings)
   - Sample success story (similar customer)
   - Competitive comparison (if relevant)

Include timing, talking points, and slide notes.
```

**Expected Output:**
- Timed demo script (with stage directions)
- Talking points for each demo section
- Anticipated Q&A and answers
- Supporting visual materials
- Objection handling responses
- Next-step options (pilot, POC, deployment)
- Sales materials checklist
- Success metrics (how to know if demo worked)

**Limitations:**
- Script needs customization for actual customer
- Live demos can go wrong (have troubleshooting ready)
- Different audiences need different emphasis

**Tips:**
- Rehearse with your team before customer
- Have backup demo if live system is unavailable
- Keep demo simple (one clear value prop, not five)
- Make sure customer leaves with a "next step" commitment

---

## Demo Scenario Library

Pre-built, tested demo scenarios for quick reference:

### Scenario A: "Quick Compliance Check" (5 min demo)
- Prompt: "Are we GDPR compliant?"
- Output: Compliance scorecard with gaps
- Wow factor: Shows compliance gaps instantly
- Next step: "Let's schedule a deeper compliance audit"

### Scenario B: "Who's Accessing Our Crown Jewels?" (10 min demo)
- Prompt: "Top 10 users accessing our most sensitive IP"
- Output: User list with risk scoring
- Wow factor: "You didn't know that person could access this data"
- Next step: Access review project

### Scenario C: "Label Coverage at a Glance" (5 min demo)
- Prompt: "What % of our data is classified?"
- Output: Label coverage scorecard
- Wow factor: Usually <50%, eye-opener
- Next step: Label implementation roadmap

### Scenario D: "Simulate an Incident" (20 min demo)
- Prompt: Run investigation prompts on pre-built incident scenario
- Output: Timeline, risk assessment, recommendations
- Wow factor: Shows speed and comprehensiveness of investigation
- Next step: Incident response playbook co-design

---

## Tags and Patterns

- **[VALIDATED]** Prompts 1, 2, 4, 5, 6, 7: These are proven customer-friendly prompts with reliable output.
- **[VALIDATED]** Prompts 8, 10: Sales enablement and metrics prompts work well in preparation phase.
- **[ASPIRATIONAL]** Prompt 3, 9: Workshop exercises require customer engagement and may need customization per organization.

All demos assume Purview data is present and reasonable. Verify customer tenant has sufficient data before live demo. Have offline slides as backup if live demo fails.

