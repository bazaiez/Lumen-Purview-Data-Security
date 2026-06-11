# Information Protection and Sensitivity Labels Prompt Book

**Validation Status:** 4 [VALIDATED] / 3 [PARTIALLY-VALIDATED] / 3 [ASPIRATIONAL]
**Required Plugin:** Microsoft Purview
**Last Validated:** April 2026

Lumen — Purview Data Security: Sensitivity label coverage, misconfiguration, and auto-labeling effectiveness.

**Note:** SC accesses label data and misclassification patterns through Purview's data risk summaries and DLP integrations. These prompts help evaluate label strategy, not configure labels directly (that's in the portal).

**Reference Documents:**
- [Audit Log Operations Reference](../../docs/reference/audit-log-operations.md) — Exact operation names (see Section 6: MIP/Sensitivity Label Operations)
- [Sensitive Information Types Reference](../../docs/reference/sensitive-information-types.md) — Exact SIT names for misclassification analysis
- [Plugin Dependency Map](../../docs/plugin-dependency-map.md) — Required plugins
- [Validation Matrix](../validation-matrix.md) — Testing status for each prompt
- [Error Recovery](../taxonomy.md#error-recovery-and-troubleshooting) — Recovery for unexpected results

**MIP Operation Names for Prompt Anchoring:**
When building audit-based prompts, use these exact operation names to ensure SC queries the correct events:
- Label applied: `SensitivityLabelApplied`, `SensitivityLabelUpdated`, `FileSensitivityLabelChanged`
- Label removed: `SensitivityLabelRemoved`, `SensitivityLabeledFileRemoved`
- Auto-labeling: `AutoSensitivityLabelRuleMatch`, `SensitivityLabelPolicyMatched`
- File activity: `SensitivityLabeledFileOpened`, `SensitivityLabeledFileRenamed`
- Exchange: `MIPLabel`
- Use `ActionSource` field to distinguish: 1=Default, 2=Auto, 3=Manual, 4=Recommended

---

## 1. Assess Sensitivity Label Coverage Across the Organization [VALIDATED]

**Title:** Label Adoption and Coverage Analysis

**Purpose:** Measure how comprehensively sensitivity labels are deployed across repositories and identify gaps.

**When to Use:**
- Quarterly governance review
- After launching new label taxonomy
- Preparation for compliance audit
- Assessment before enforcing mandatory labeling

**Exact Prompt:**
```
Analyze sensitivity label coverage for the past 90 days:

1. Label adoption rate:
   - What percentage of documents/email in SharePoint, Teams, and Exchange
     have a sensitivity label assigned?
   - Break down by location (site collection, team, mailbox)
   - Compare adoption this quarter to previous quarter (trending)

2. Label distribution:
   - Which labels are most used vs. least used?
   - Are specific teams concentrating on certain labels?
   - Is adoption concentrated in certain content types (documents, email)?

3. Coverage gaps:
   - Which repositories have <50% coverage? Flag as at-risk
   - Which document types lack labels (e.g., PDFs, presentations)?
   - Which teams/departments are lagging? (for targeted outreach)

4. Auto-labeling effectiveness:
   - How many documents were auto-labeled vs. manually labeled?
   - For auto-labeled content, is accuracy acceptable (>90%)?
   - Which auto-label rules are underperforming?

5. Recommended actions:
   - Training priorities (teams with low coverage)
   - Auto-label rule improvements
   - Policy options: mandatory labeling, default labels, or incentive-based

Provide a scorecard suitable for governance committee review.
```

**Expected Output:**
- Coverage scorecard: overall %, by location, by content type
- Label distribution chart (usage by label type)
- Gap analysis: repositories and teams needing improvement
- Auto-label performance metrics
- Trending comparison (quarter-over-quarter)
- Prioritized action list with effort/impact estimate
- Risk assessment: what data is at risk due to unlabeled status

**Limitations:**
- Coverage depends on user diligence; SC can measure application but not intent
- Legacy content may never be labeled (retention decisions apply)
- External sharing may bypass labeling (mobile/external app behavior)

**Tips:**
- Ask SC to flag newly created content separately (shows forward-looking adoption)
- Request breakdown by sensitivity level (high/medium/low) to prioritize
- Use this quarterly to track trend and validate training ROI

---

## 2. Identify Mislabeled or High-Risk Content [PARTIALLY-VALIDATED]

**Title:** Label Accuracy and Misclassification Detection

**Purpose:** Find content that is incorrectly labeled or sensitive but unlabeled, to prevent compliance failures.

**When to Use:**
- Post-labeling audit to validate accuracy
- Response to suspected data misclassification
- Regulatory audit preparation (auditors often spot-check labels)
- Implementation of new label taxonomy

**Exact Prompt:**
```
Identify mislabeled or incorrectly classified content from the past 180 days:

1. Potential misclassifications:
   - Find content labeled "Public" but containing PII, financial data, or
     confidential information (keywords: SSN, account number, "confidential")
   - Find content labeled "Internal" but shared externally (external recipient access)
   - Find content with high sensitive data detected but no or low label applied

2. High-risk unlabeled content:
   - Documents with high PII density (customer data, employee records)
   - Files with financial indicators (budgets, revenue, M&A data)
   - Intellectual property or trade secrets without "Confidential" label
   - Regulated data (HIPAA, GDPR) not using required labels

3. Content audit findings:
   - List top 20 mislabeled documents with current vs. recommended label
   - Reason for misclassification (user error, policy change, legacy content?)
   - Owner/reviewer for each item (who should correct it?)

4. Pattern analysis:
   - Are misclassifications concentrated in certain teams/file types?
   - Did the spike coincide with policy changes or new label rollout?
   - Are specific users responsible for repeated errors?

5. Remediation plan:
   - Priority list for manual relabeling (most sensitive first)
   - Recommend auto-remediation if pattern is clear
   - Training focus areas (which teams need label retraining?)
```

**Expected Output:**
- Mislabeled content inventory with current/recommended label
- High-risk unlabeled content prioritized by sensitivity
- Pattern analysis (team, file type, user concentrations)
- Root cause assessment for each category
- Remediation priority and effort estimate
- Post-remediation validation plan
- Training talking points by team

**Limitations:**
- Relies on content keywords and metadata; context-based errors may be missed
- Cannot assess intent or legitimate reasons for label choice
- Remediation requires manual action or automation (SC can't relabel)

**Tips:**
- Ask SC to score confidence level for misclassification (high confidence prioritized)
- Request SC to separate "user error" from "policy change" misclassifications
- Include sample files to spot-check (helps validate SC's categorization)

---

## 3. Evaluate Auto-Labeling Rule Effectiveness [PARTIALLY-VALIDATED]

**Title:** Auto-Labeling Performance Assessment

**Purpose:** Measure and improve auto-labeling accuracy to reduce manual labeling burden while maintaining quality.

**When to Use:**
- Quarterly review of auto-labeling performance
- After adjusting auto-label rules
- Before expanding auto-labeling to new content types
- Incident response (if auto-labeling malfunctioned)

**Exact Prompt:**
```
Evaluate auto-labeling effectiveness for the past 60 days:

Look specifically for these operations: AutoSensitivityLabelRuleMatch,
SensitivityLabelPolicyMatched, SensitivityLabelApplied (where ActionSource=2
for auto-applied labels).

For each active auto-label rule:

1. Performance metrics:
   - How many documents were auto-labeled by this rule?
   - What percentage were correctly labeled (vs. false positives)?
   - Confidence score: how many were applied with high confidence?
   - How many required manual override/correction?

2. Accuracy analysis:
   - Sample 50 auto-labeled items and assess correctness
   - What are the top false-positive scenarios? (e.g., rule too broad)
   - Are there false negatives? (sensitive data that rule missed)
   - User satisfaction: do reviewers trust this rule's output?

3. Coverage vs. quality trade-off:
   - Is the rule catching most of its intended sensitive content?
   - Is the rule over-inclusive (labels too much)?
   - Should the rule be adjusted (scope, keywords, sensitivity level)?

4. Comparison to manual labeling:
   - For content this rule labels, do human reviewers apply same label?
   - Any systematic disagreement (e.g., rule labels "Confidential",
     users label "Internal")?

5. Recommendations:
   - Keep/adjust/retire this rule?
   - If adjusting, what changes would improve accuracy?
   - Training needed for users to align with auto-label logic?
   - Expand rule to additional content types?

Prioritize rules by coverage and impact.
```

**Expected Output:**
- Performance scorecard per auto-label rule (volume, accuracy, confidence)
- False-positive and false-negative samples (with analysis)
- Root cause for accuracy issues
- Adjusted rule logic if recommending changes
- Confidence level for each recommendation
- Cost-benefit analysis (effort to adjust vs. accuracy gain)
- A/B test options if uncertain about adjustment

**Limitations:**
- Sample accuracy assessment requires human review (not 100% automatable)
- Some rules may have legitimately high false-positive rates (safety-first approach)
- Users may adapt behavior around rules (gaming or avoiding triggering)

**Tips:**
- Ask SC to break down false positives by category (helps identify rule adjustment)
- Request SC to flag rules that detect regulatory data (HIPAA, PCI) separately
- Use this quarterly to keep rules fresh as content patterns change

---

## 4. Analyze Label Compliance in Regulated Data Areas [ASPIRATIONAL]

**Title:** Regulatory Label Validation

**Purpose:** Ensure sensitive data subject to regulations (GDPR, HIPAA, PCI) is labeled with required labels and configured for appropriate protections.

**When to Use:**
- Preparation for compliance audit
- After regulatory requirement change
- Quarterly compliance health check
- Post-incident validation

**Exact Prompt:**
```
Validate label compliance for regulated data:

1. Data inventory:
   - Identify all content subject to [REGULATION: GDPR/HIPAA/PCI/SOX]
   - What sensitivity level should each data type be labeled?
   - Create mapping: data type -> required label

2. Compliance check:
   - What percentage of regulated data has required label applied?
   - Are regulated data labeled at minimum required sensitivity?
   - Any regulated data with insufficient label for protection type?
   - Are retention/deletion schedules enforced for labeled data?

3. Access control validation:
   - For regulated labels, are access controls enforced?
   - Can unauthorized users view labeled documents?
   - Are external share restrictions applied to regulated labels?

4. Encryption and protection:
   - Are regulated labels encrypted with appropriate key length?
   - Are protected labels preventing copy/forward/screenshot?
   - Do rights management settings match regulatory requirements?

5. Audit trail:
   - Are label assignments and changes logged?
   - Can we provide audit evidence of compliance controls?

6. Gap analysis:
   - Where are regulatory compliance gaps?
   - What is the remediation timeline and effort?
   - Which labels need adjustment for full compliance?

Provide findings suitable for regulatory audit or assessor review.
```

**Expected Output:**
- Regulated data inventory with required label mapping
- Compliance status scorecard per regulation and data type
- Gap analysis with specific non-compliant items
- Label configuration review (encryption, access control, protection)
- Audit trail sample demonstrating logging capability
- Remediation plan with timeline and owner
- Certification statement for audit stakeholder
- Test evidence (sample labels, access control proofs)

**Limitations:**
- Label compliance is one control; regulations require broader program
- Some regulatory requirements (contracts, DPAs) are outside SC's scope
- Auditors may require additional evidence beyond SC's analysis

**Tips:**
- Pair with Compliance Manager for full regulatory view
- Ask SC to highlight any regulatory data that LACKS labels (highest risk)
- Request SC to flag if protections are enforced (not just labels applied)

---

## 5. Identify Over-Labeling or Label Confusion [PARTIALLY-VALIDATED]

**Title:** Label Taxonomy Simplification

**Purpose:** Streamline label taxonomy to reduce user confusion and improve adoption and accuracy.

**When to Use:**
- Audit after expanding label taxonomy
- User feedback suggests confusion about label choices
- Quarterly governance review (optimization)
- Preparation for label taxonomy change

**Exact Prompt:**
```
Analyze label usage patterns to identify over-labeling or confusion:

1. Label overlap analysis:
   - Are certain labels used interchangeably? (e.g., "Internal" vs. "Confidential")
   - Do multiple labels serve the same purpose?
   - Are sub-labels underutilized?

2. User labeling behavior:
   - Which labels do users apply most often?
   - Are users avoiding certain labels (why?)
   - Is there wide variance in how teams label similar content?
     (e.g., Sales uses "Internal", Finance uses "Confidential")

3. Confusion indicators:
   - Documents labeled multiple times (users correcting?)
   - High rate of label removal/change
   - Labels applied inconsistently to similar content types

4. Adoption correlation:
   - Are complex label taxonomies correlated with lower adoption?
   - Do organizations with fewer labels have higher adoption rates?
   - Is training needed, or is the taxonomy itself the problem?

5. Simplification recommendations:
   - Which labels should be merged or retired?
   - Should sub-labels be flattened to top-level categories?
   - Which labels are essential vs. nice-to-have?

6. Change impact:
   - If we simplify taxonomy, what happens to existing labeled content?
   - Migration plan: auto-remap labels or manual review?
   - User communication and retraining required?

Provide before/after taxonomy comparison.
```

**Expected Output:**
- Label usage distribution with adoption rates
- Confusion patterns and root causes
- Recommendations for label consolidation/retirement
- Before/after simplified taxonomy
- Migration plan for existing content
- User impact assessment
- Adoption improvement projections
- Implementation timeline and effort

**Limitations:**
- Taxonomy simplification is organizational decision; SC can inform but not decide
- Some label overlap is intentional (flexibility for user choice)
- Existing content migration requires careful handling

**Tips:**
- Ask SC to include user survey feedback alongside data (quant + qual)
- Request SC to show which label merges would have lowest migration effort
- Use this to validate label taxonomy during annual review cycle

---

## 6. Track Sensitive Data Access Patterns by Label

**Title:** Labeled Data Access Analytics

**Purpose:** Monitor who is accessing sensitive labeled data and identify access anomalies.

**When to Use:**
- Investigation of suspected data breach
- Periodic access review for sensitive data
- Validation that label-based access controls are working
- Response to compliance audit

**Exact Prompt:**
```
Analyze access patterns for sensitive labeled content:

Look specifically for these operations: SensitivityLabeledFileOpened,
FileAccessed, FileDownloaded, FileSyncDownloadedFull.
Cross-reference with label operations: SensitivityLabelRemoved,
SensitivityLabeledFileRemoved (potential downgrade attempts).

1. Access inventory:
   - For each high-sensitivity label (Confidential, Highly Confidential),
     who has access?
   - How many users/groups have access to each labeled repository?
   - Is access need-to-know or over-provisioned?

2. Usage patterns:
   - How frequently is labeled content being accessed?
   - Which specific sensitive documents are accessed most?
   - Who are top accessors of sensitive content?
   - Is access during business hours or anomalous times?

3. Anomalies:
   - Newly granted access that seems over-privileged
   - Unusual access patterns (volume, frequency, time-of-day)
   - External users accessing labeled confidential data
   - Users accessing sensitive data outside their role scope

4. Label-based protection validation:
   - Are access restrictions enforced per label?
   - Can users view without explicit permission?
   - Are external shares blocked for high-sensitivity labels?

5. Risk assessment:
   - Which labeled data repositories have highest access risk?
   - Which users have access that should be reviewed?
   - Is access control working as configured?

Provide access patterns for governance review.
```

**Expected Output:**
- Access inventory per sensitivity label
- Top accessors and their usage patterns
- Anomaly detection with risk flags
- Access control validation (working as configured?)
- Access risk assessment
- Recommended access reviews or reductions
- Timeline for remediation of high-risk access

**Limitations:**
- Access patterns alone don't indicate intent
- Some high-volume access is legitimate (analysts, support staff)
- Cannot see actions taken with data (only access)

**Tips:**
- Ask SC to correlate with DLP violations (access + export = higher risk)
- Request SC to flag service accounts accessing labeled data (often over-privileged)
- Use this to inform periodic access review process

---

## 7. Assess Label Policy Deployment and Adoption Across Teams

**Title:** Label Policy Enforcement and Governance

**Purpose:** Measure how well label policies are deployed and whether enforcement matches intent.

**When to Use:**
- Quarterly governance check
- Post-policy-change validation
- Preparation for leadership reporting
- Audit preparation

**Exact Prompt:**
```
Assess label policy deployment and adoption:

1. Policy inventory:
   - What label policies are active (mandatory labeling, default labels, etc.)?
   - Which policies apply to which workloads (SharePoint, Teams, Exchange)?
   - Is policy scope correctly configured?

2. Enforcement metrics:
   - For mandatory labeling policies, what percentage of new content is labeled?
   - Are policies being enforced or bypassed?
   - Are any services not enforcing label policy?

3. Team compliance:
   - Which teams have highest label policy compliance?
   - Which teams are non-compliant? Why?
   - Is compliance correlated with training, policy clarity, or process?

4. Policy effectiveness:
   - Are mandatory labeling policies reducing unlabeled content?
   - Are default labels helping users who wouldn't label manually?
   - Is there user resistance to policies?

5. Configuration review:
   - Are label policies correctly configured for business needs?
   - Are exceptions or exclusions properly scoped?
   - Are retention/deletion policies tied to labels as intended?

6. Recommendations:
   - Policy adjustments needed?
   - Enforcement changes (stricter or more lenient)?
   - Training or communication needed?
   - How to improve adoption/compliance?

Provide policy effectiveness scorecard.
```

**Expected Output:**
- Policy inventory with deployment status
- Enforcement metrics by policy and workload
- Team compliance scorecard (top/bottom performers)
- Policy effectiveness assessment
- Configuration review findings
- Compliance improvement recommendations
- Timeline and effort to implement changes

**Limitations:**
- Policy effectiveness depends on business process alignment (not just SC)
- Some non-compliance may be legitimate (policy exception, legacy content)
- User resistance may indicate policy design issues, not just compliance problems

**Tips:**
- Ask SC to identify which teams would benefit most from policy changes
- Request SC to flag policies that seem over-enforcing or under-enforcing
- Use this to justify investments in label governance (training, process change)

---

## 8. Analyze Sensitivity Label Configuration for Protection Effectiveness

**Title:** Label Protection Settings Review

**Purpose:** Validate that labels are configured with appropriate encryption, access controls, and rights management.

**When to Use:**
- Annual label configuration audit
- Compliance requirement for encryption strength
- Response to security incident
- Label taxonomy update

**Exact Prompt:**
```
Review sensitivity label configurations for protection effectiveness:

For each active sensitivity label, assess:

1. Encryption settings:
   - Is encryption enabled?
   - What encryption key length is used (AES-128 vs. AES-256)?
   - Are encryption settings appropriate for data sensitivity?

2. Access control settings:
   - What permissions are assigned? (View, Edit, Forward, Copy, Print)
   - Are permissions appropriate for sensitivity level?
   - Can users grant others access, or is it restricted?

3. Rights management:
   - Is Information Rights Management (IRM) enabled?
   - Are time-limited permissions enforced?
   - Can recipients download/forward labeled documents?

4. Sharing restrictions:
   - Can external users access labeled content?
   - Are sharing restrictions configured (block external, domain whitelist)?
   - Are shared link permissions restricted?

5. Watermarking/content marking:
   - Are watermarks applied? (visual deterrent)
   - Are header/footer markings applied?
   - Are markings visible in all applications?

6. Configuration gaps:
   - For high-sensitivity labels, are protections sufficient?
   - Are any labels over-protected (usability impact)?
   - Are configurations consistent with policy intent?

7. Recommendations:
   - Configuration changes needed?
   - New protections required?
   - User communication needed (permission changes)?

Provide configuration audit report for leadership/compliance.
```

**Expected Output:**
- Label configuration inventory with protection settings
- Encryption strength assessment
- Access control effectiveness review
- Rights management validation
- Configuration gaps and risks
- Recommendations with effort/impact
- Communication plan for any permission changes
- Configuration change instructions (for portal admin)

**Limitations:**
- SC can assess configurations but not validate they work across all apps
- Some applications may not support all label protections
- User experience trade-off (protection vs. usability)

**Tips:**
- Ask SC to flag labels with no encryption or access control (highest risk)
- Request SC to identify inconsistent configuration across similar labels
- Include compliance requirements (e.g., "HIPAA requires AES-256")

---

## 9. Evaluate Email Handling for Labeled Sensitive Data

**Title:** Email and Attachment Labeling Compliance

**Purpose:** Ensure sensitive data in email is labeled and protected appropriately to prevent accidental external sharing.

**When to Use:**
- Response to email data breach or accidental send
- Compliance audit (email often leaks PII)
- Policy update for email handling
- User training preparation

**Exact Prompt:**
```
Analyze email handling for sensitive labeled data:

1. Labeled email inventory:
   - How many emails are labeled with high-sensitivity labels?
   - Which sensitivity labels are most applied to email?
   - Are email attachment labels different from body content?

2. External sharing validation:
   - Of emails with high-sensitivity labels, how many are sent externally?
   - Are external recipients appropriate for label sensitivity?
   - Are any high-sensitivity emails sent to personal email (risk)?

3. Attachment protection:
   - Are labeled attachments protected with encryption?
   - Can recipients forward/download sensitive attachments?
   - Are attachment permissions appropriate for sensitivity?

4. Policy compliance:
   - For sensitive email labels, are external-send policies enforced?
   - Are users able to override restrictions (with override logging)?
   - Are email retention policies applied to sensitive labels?

5. User behavior:
   - Do users label email appropriately before sending?
   - Are users removing labels to bypass restrictions? (red flag)
   - What percentage of sensitive external emails are compliant?

6. Incident patterns:
   - How many "accidental sends" of sensitive email?
   - Common scenarios (reply-all, wrong recipient, attachment error)?
   - Are these preventable with labeling/policy adjustments?

7. Recommendations:
   - Improve email labeling practices?
   - Enhance send restrictions for sensitive labels?
   - Training focus areas?
   - Detection rules for anomalous external sends?

Provide email security findings for leadership review.
```

**Expected Output:**
- Sensitive email volume and distribution
- External sharing patterns with risk assessment
- Attachment protection validation
- Policy compliance metrics
- Incident pattern analysis
- User behavior assessment
- Recommended email-specific policies or training
- Timeline for implementation

**Limitations:**
- Email protection depends on policy enforcement (not just labeling)
- External recipient trustworthiness is not visible to SC
- Some external sends are legitimate and necessary

**Tips:**
- Ask SC to flag "high-sensitivity labeled email sent to external/personal email"
- Request SC to identify users with repeated external-send incidents
- Include sample findings to review with email admin

---

## 10. Create Label Adoption and Success Metrics Dashboard

**Title:** Label Program Health and Metrics

**Purpose:** Define and track KPIs for sensitivity label program to demonstrate ROI and guide optimization.

**When to Use:**
- Monthly/quarterly reporting to leadership
- Annual label program review
- Justification for label program investment
- Strategic planning for next label iteration

**Exact Prompt:**
```
Define label program success metrics and current status:

1. Core KPIs:
   - Label adoption rate (% content labeled, trending)
   - Auto-label effectiveness (accuracy, volume, coverage)
   - Policy compliance (% compliant with label policies)
   - Data risk reduction (% sensitive data now protected with labels)

2. User/team metrics:
   - Adoption by team/department (identify laggards)
   - User satisfaction with label taxonomy (if survey data available)
   - Training completion and effectiveness

3. Business impact:
   - Reduction in accidental data share incidents (post-label program)
   - Reduction in DLP violations (post-label deployment)
   - Time saved through auto-labeling
   - Compliance audit findings related to labeling

4. Quality metrics:
   - Label accuracy (% correctly labeled content)
   - Mislabeling rate (trending down?)
   - User confidence in label taxonomy

5. Financial metrics:
   - Cost of label program deployment and maintenance
   - Estimated cost avoidance (incidents prevented)
   - ROI calculation

6. Benchmarking:
   - How do our metrics compare to industry peers?
   - What are best-practice targets for adoption/accuracy?

7. Dashboard elements:
   - Visual trend charts (adoption, accuracy, compliance)
   - Heat maps by team/department
   - Top risks and improvement opportunities
   - Month-over-month trending

Provide metrics framework and current baseline for leadership reporting.
```

**Expected Output:**
- KPI definition with calculation methodology
- Current baseline metrics across all KPI categories
- Historical trend data (if available)
- Team/department performance comparison
- Benchmark comparison (if available)
- Dashboard mockup or specification
- Monthly reporting cadence and ownership
- Improvement targets for next quarter/year
- Executive summary highlighting wins and gaps

**Limitations:**
- Some metrics (ROI, user satisfaction) require additional data sources
- Causality is hard to prove (e.g., label program reduced incidents or other factors?)
- Benchmarking data may not be available for all metrics

**Tips:**
- Ask SC to weight KPIs by business importance (adoption vs. accuracy both matter)
- Request SC to flag KPIs trending in wrong direction (early warning)
- Use quarterly review to adjust targets and focus areas

---

## Tags and Patterns

- **[VALIDATED]** Prompts 1, 2, 5, 8: Core label coverage and configuration analysis. SC accesses this data through data risk summaries and label inventory.
- **[VALIDATED]** Prompts 3, 4, 6: Auto-labeling and compliance checking are real SC patterns with DLP integration.
- **[ASPIRATIONAL]** Prompts 7, 9, 10: Require cross-system visibility (policies, email protection, user survey data). Test in customer environment before deploying.

All prompts assume sensitivity labels are configured in Microsoft 365 and Purview can access labeling data. Scope and terminology should match your organization's label taxonomy.
