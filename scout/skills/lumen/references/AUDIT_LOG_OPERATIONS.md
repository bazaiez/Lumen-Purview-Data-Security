# Unified Audit Log Operation Names Reference

**Last Updated:** April 2026
**Status:** [VALIDATED]
**Purpose:** Exact operation names for use in Security Copilot prompts to prevent SC mapping failures

---

## Why This Matters

Security Copilot sometimes fails to map natural language descriptions (e.g., "downloads" or "file sharing") to the correct Unified Audit Log (UAL) operation names. When this happens, SC either returns incomplete results or queries the wrong activity type entirely.

**Solution:** Include exact operation names in your prompts as anchors. Instead of asking SC to find "file downloads," ask for `FileDownloaded`, `FileSyncDownloadedFull`, etc. This forces SC to query the correct audit events.

**Usage Pattern:**
```
Analyze audit logs for user <user> looking specifically for these operations:
[EXACT_OPERATION_NAMES]. For each occurrence, provide the timestamp,
target resource, and whether a DLP policy matched.
```

---

## 1. SharePoint and OneDrive Operations

### File Access and Modification

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `FileAccessed` | User accessed a file | Baseline activity analysis |
| `FileAccessedExtended` | Extended file access event (long session) | Detect prolonged data review |
| `FileCheckedIn` | User checked in a file | Version control tracking |
| `FileCheckedOut` | User checked out a file | Pre-edit activity |
| `FileCopied` | User copied a file within SharePoint/OneDrive | Data staging detection |
| `FileDeleted` | User deleted a file | Evidence destruction, data hiding |
| `FileDeletedFirstStageRecycleBin` | File moved to first-stage recycle bin | Soft deletion tracking |
| `FileDeletedSecondStageRecycleBin` | File permanently deleted from recycle bin | Permanent evidence destruction |
| `FileDownloaded` | User downloaded a file | Primary exfiltration indicator |
| `FileModified` | User modified a file | Content alteration tracking |
| `FileModifiedExtended` | Extended file modification event | Large-scale content changes |
| `FileMoved` | User moved a file to different location | Data staging or hiding |
| `FilePreviewed` | User previewed a file | Reconnaissance activity |
| `FileRenamed` | User renamed a file | Obfuscation attempt |
| `FileRestored` | User restored a file from recycle bin | Recovery after attempted deletion |
| `FileSyncDownloadedFull` | User downloaded a file via sync client | Bulk download via OneDrive sync |
| `FileSyncUploadedFull` | User uploaded a file via sync client | Bulk upload tracking |
| `FileUploaded` | User uploaded a file | New content introduction |
| `FileRecycled` | User sent file to recycle bin | Alternative deletion tracking |

### Sharing Operations

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `SharingSet` | User set sharing permissions on a file/folder | External sharing detection |
| `SharingRevoked` | User revoked sharing permissions | Post-incident containment |
| `SharingInvitationCreated` | User created a sharing invitation | External collaboration initiation |
| `SharingInvitationAccepted` | External user accepted sharing invitation | External access confirmed |
| `SharingInvitationBlocked` | Sharing invitation was blocked by policy | Policy enforcement validation |
| `SharingInvitationRevoked` | Sharing invitation was revoked | Access cleanup |
| `AnonymousLinkCreated` | User created an anonymous sharing link | High-risk: anyone-with-link access |
| `AnonymousLinkUsed` | Anonymous link was used to access content | Confirms external data access |
| `AnonymousLinkUpdated` | Anonymous link permissions were changed | Broadening access |
| `AnonymousLinkRemoved` | Anonymous link was removed | Access cleanup |
| `CompanyLinkCreated` | User created an org-wide sharing link | Broad internal sharing |
| `CompanyLinkUsed` | Org-wide link was used | Confirms broad internal access |
| `SecureLinkCreated` | User created a specific-people sharing link | Controlled external sharing |
| `SecureLinkUsed` | Specific-people link was used | Targeted access confirmed |
| `AddedToSecureLink` | User added to specific-people link | Access expansion |
| `RemovedFromSecureLink` | User removed from specific-people link | Access restriction |

### Site and Permission Operations

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `SiteCollectionAdminAdded` | Site collection admin was added | Privilege escalation |
| `SiteCollectionAdminRemoved` | Site collection admin was removed | Post-incident cleanup |
| `GroupAdded` | SharePoint group was added | Permission structure changes |
| `GroupRemoved` | SharePoint group was removed | Access restriction |
| `GroupUpdated` | SharePoint group membership changed | Permission drift |
| `PermissionLevelAdded` | New permission level was created | Custom access creation |
| `PermissionLevelsModified` | Permission levels were modified | Access broadening |

---

## 2. Exchange Online Operations

### Mail Operations

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `MailItemsAccessed` | Mail items accessed (mailbox audit) | Email reconnaissance |
| `Send` | User sent an email | Primary email exfiltration |
| `SendAs` | User sent email using Send As permission | Delegated sending, impersonation |
| `SendOnBehalf` | User sent email on behalf of another | Delegated access |
| `Create` | New mail item created | Draft creation tracking |
| `Update` | Mail item was updated | Message modification |
| `MoveToDeletedItems` | User moved email to Deleted Items | Evidence hiding |
| `SoftDelete` | User soft-deleted email | Evidence destruction |
| `HardDelete` | User permanently deleted email | Permanent evidence destruction |
| `FolderBind` | User accessed a mailbox folder | Mailbox reconnaissance |
| `MessageBind` | User accessed a specific message | Targeted message access |
| `SearchQueryInitiatedExchange` | User initiated a search in mailbox | Content discovery |

### Mail Flow Rules and Forwarding

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `New-InboxRule` | New inbox rule created | Auto-forwarding setup, data siphoning |
| `Set-InboxRule` | Inbox rule modified | Forwarding rule changes |
| `Remove-InboxRule` | Inbox rule removed | Cleanup after detection |
| `UpdateInboxRules` | Inbox rules were updated via client | Client-side rule manipulation |
| `Set-Mailbox` | Mailbox settings changed | Forwarding address changes |
| `New-TransportRule` | New mail transport rule created | Org-wide mail flow manipulation |
| `Set-TransportRule` | Mail transport rule modified | Policy circumvention |
| `Set-MailboxJunkEmailConfiguration` | Junk email settings changed | Filter manipulation |
| `Enable-MailboxAuditLog` | Mailbox audit logging enabled | Audit configuration |
| `Disable-MailboxAuditLog` | Mailbox audit logging disabled | Audit evasion |

### Delegation and Permissions

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `Add-MailboxPermission` | Mailbox permission granted | Access escalation |
| `Remove-MailboxPermission` | Mailbox permission removed | Post-incident cleanup |
| `Add-RecipientPermission` | Recipient permission granted (Send As) | Impersonation setup |
| `Set-MailboxFolderPermission` | Folder-level permission changed | Targeted access grant |

---

## 3. Microsoft Teams Operations

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `MemberAdded` | Member added to a team | Access expansion |
| `MemberRemoved` | Member removed from a team | Access restriction |
| `MemberRoleChanged` | Member role changed (member to owner) | Privilege escalation |
| `TeamCreated` | New team was created | Shadow team creation |
| `TeamDeleted` | Team was deleted | Evidence destruction |
| `ChannelAdded` | New channel added to team | Communication structure changes |
| `ChannelDeleted` | Channel was deleted | Evidence destruction |
| `ChannelSettingsChanged` | Channel settings modified | Configuration changes |
| `MessageCreatedHasLink` | Message created containing a link | External link sharing |
| `MessageUpdatedHasLink` | Message updated to contain a link | Link injection |
| `ChatCreated` | New chat was created | Communication initiation |
| `MessageRead` | Message was read | Access confirmation |
| `TabAdded` | Tab added to channel | App/connector integration |
| `ConnectorAdded` | Connector added to team | Webhook/external integration |
| `BotAddedToTeam` | Bot added to team | Automation introduction |
| `AppInstalled` | App installed in Teams | Third-party app access |

---

## 4. DLP-Specific Operations

> **Source:** RecordTypes ComplianceDLPSharePoint (11), ComplianceDLPExchange (13), ComplianceDLPSharePointClassification (33), ComplianceDLPExchangeClassification (107), ComplianceDLPApplications (263), OnPremisesFileShareScannerDlp (99), OnPremisesSharePointScannerDlp (100)

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `DLPRuleMatch` | Content matched a DLP policy rule | Primary DLP alert trigger |
| `DLPRuleUndo` | DLP rule action was undone (false positive or override) | User circumvention of DLP |
| `DLPInfo` | DLP policy informational event | Policy processing tracking |
| `DlpClassification` | DLP classification event (SharePoint/Exchange) | Content classification tracking |

**Note:** DLP events always have `UserKey="DlpAgent"`. There are only three core DLP operation types: `DLPRuleMatch`, `DLPRuleUndo`, and `DLPInfo`. `DlpClassification` is logged separately under ComplianceDLPSharePointClassification (33) and ComplianceDLPExchangeClassification (107).

---

## 5. Endpoint DLP Operations

> **Source:** RecordTypes DLPEndpoint (63), ComplianceDLPEndpoint (108), WebpageActivityEndpoint (153)

### DLPEndpoint (63) — Full Endpoint Activity Logging

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `FilePrinted` | File was printed | Physical exfiltration |
| `FileRenamed` | File was renamed on endpoint | Obfuscation attempt |
| `FileCreated` | File was created on endpoint | New file tracking |
| `FileModified` | File was modified on endpoint | Content alteration |
| `FileCopiedToRemovableMedia` | File copied to removable storage (USB) | USB exfiltration |
| `FileCopiedToNetworkShare` | File copied to network share | Lateral data movement |
| `FileCopiedToClipboard` | Sensitive content copied to clipboard | Clipboard exfiltration |
| `FileUploadedToCloud` | File uploaded to cloud storage app | Cloud exfiltration |
| `FileAccessedByUnallowedApp` | Unallowed application accessed sensitive file | Shadow IT detection |
| `FileCopiedToRemoteDesktopSession` | File copied to remote desktop session | RDP exfiltration |
| `FileTransferredByBluetooth` | File transferred via Bluetooth | Bluetooth exfiltration |
| `ArchiveCreated` | Archive file was created containing sensitive data | Archive-based exfiltration |
| `FileArchived` | File was added to an archive | Data staging in archive |
| `WebpageCopiedToClipboard` | Webpage content copied to clipboard | Web content exfiltration |
| `WebpageSavedToLocal` | Webpage saved to local storage | Web content download |
| `FileCreatedOnRemovableMedia` | File created on USB/removable media | USB exfiltration |
| `PastedToBrowser` | Content pasted into a browser | Browser-based exfiltration |
| `BrowseToUrl` | User browsed to a URL with sensitive content | URL-based exfiltration |
| `FileCreatedOnNetworkShare` | File created on network share | Data staging |

### ComplianceDLPEndpoint (108) — DLP Policy Match on Endpoint

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `FilePrinted` | DLP policy matched on printed file | Print policy enforcement |
| `FileCopiedToRemovableMedia` | DLP policy matched on USB copy | USB policy enforcement |
| `FileCopiedToNetworkShare` | DLP policy matched on network share copy | Network share policy enforcement |
| `FileCopiedToClipboard` | DLP policy matched on clipboard copy | Clipboard policy enforcement |
| `FileUploadedToCloud` | DLP policy matched on cloud upload | Cloud upload policy enforcement |
| `FileAccessedByUnallowedApp` | DLP policy matched on unallowed app access | Shadow IT policy enforcement |
| `FileCopiedToRemoteDesktopSession` | DLP policy matched on RDP copy | RDP policy enforcement |
| `FileTransferredByBluetooth` | DLP policy matched on Bluetooth transfer | Bluetooth policy enforcement |
| `PastedToBrowser` | DLP policy matched on browser paste | Browser paste policy enforcement |

### WebpageActivityEndpoint (153)

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `BrowseToUrl` | User browsed to a monitored URL | Web browsing policy enforcement |

---

## 6. MIP / Sensitivity Label Operations

> **Source:** RecordTypes MIPLabel (43), SensitivityLabelAction (83), SensitivityLabeledFileAction (84), SensitivityLabelPolicyMatch (82), LabelContentExplorer (48), MipAutoLabelSharePointItem (71), MipAutoLabelSharePointPolicyLocation (72), MipAutoLabelExchangeItem (75), AipDiscover (93), MipExactDataMatch (109)

### Label Application and Changes

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `SensitivityLabelApplied` | Sensitivity label applied to content (email or file) | Label application tracking |
| `SensitivityLabelUpdated` | Sensitivity label changed on content (forward/reply for email) | Label change tracking |
| `SensitivityLabelChanged` | Sensitivity label changed on SPO/Teams site or M365 Apps content | Site/container label changes |
| `SensitivityLabelRemoved` | Sensitivity label removed from content | Label removal — potential downgrade or policy bypass |
| `SensitivityLabelPolicyMatched` | Recommended sensitivity label policy matched | Auto-label recommendation tracking |
| `FileSensitivityLabelChanged` | File sensitivity label changed via Office on the Web | Web-based label changes |
| `AutoSensitivityLabelRuleMatch` | Auto-labeling rule matched content | Auto-labeling policy enforcement |
| `MIPLabel` | General MIP label event (Exchange workload) | Email label tracking |

### Labeled File Activity

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `SensitivityLabeledFileOpened` | Sensitivity-labeled file was opened | Access to labeled content |
| `SensitivityLabeledFileApplied` | Sensitivity label applied to file | File label application |
| `SensitivityLabeledFileRenamed` | Sensitivity-labeled file was renamed | Labeled file manipulation |
| `SensitivityLabeledFileRemoved` | Sensitivity label removed from file | File label removal — potential data downgrade |

**Note:** `SensitivityLabelApplied` applies only to brand-new email messages. For label changes on forwarded/replied emails, the operation is `SensitivityLabelUpdated`. For files in SharePoint Online via Office on the Web, use `FileSensitivityLabelChanged`. Use `ActionSource` field in the log to distinguish manual (3) from auto (2), default (1), or recommended (4) label application.

---

## 7. Insider Risk Management Operations

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `SecurityComplianceInsiderRiskPolicy` | IRM policy triggered | Policy match tracking |
| `InsiderRiskAlertGenerated` | IRM alert was generated | Alert creation |
| `InsiderRiskAlertActioned` | IRM alert was acted upon | Response tracking |
| `InsiderRiskCaseCreated` | IRM case was created | Case management |
| `InsiderRiskCaseUpdated` | IRM case was updated | Investigation progress |
| `InsiderRiskSequenceDetected` | Sequence of risky activities detected | Pattern detection |

---

## 8. Entra ID / Azure AD Operations (Cross-Plugin: Requires Entra Plugin)

These operations are NOT available through the Purview plugin alone. They require the Microsoft Entra plugin to be enabled in Security Copilot.

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `UserLoggedIn` | User signed in successfully | Access confirmation |
| `UserLoginFailed` | Sign-in attempt failed | Brute force / credential spray |
| `Add member to role` | User added to directory role | Privilege escalation |
| `Add app role assignment to user` | App role assigned to user | Application access changes |
| `Consent to application` | User consented to application permissions | OAuth consent abuse |
| `Update user` | User profile was updated | Account manipulation |
| `Reset user password` | User password was reset | Account takeover indicator |
| `Add owner to application` | Owner added to application registration | Persistent access setup |
| `Add member to group` | User added to security/distribution group | Group membership escalation |

---

## 9. eDiscovery and Compliance Operations

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `SearchCreated` | eDiscovery search was created | Discovery tracking |
| `SearchStarted` | eDiscovery search was executed | Data collection |
| `SearchExported` | eDiscovery search results exported | Data export tracking |
| `CaseCreated` | eDiscovery case was created | Case management |
| `CaseUpdated` | eDiscovery case was updated | Case progress |
| `HoldCreated` | Legal hold was applied | Preservation tracking |
| `HoldRemoved` | Legal hold was removed | Hold lifecycle |
| `ComplianceSearchAction` | Compliance search action was performed | Compliance investigation |

---

## 10. Communication Compliance Operations

| Operation Name | Description | Investigation Use |
|---------------|-------------|-------------------|
| `SupervisoryReviewPolicyMatch` | Communication compliance policy matched | Message flag |
| `SupervisoryReviewTaggedItem` | Item tagged for review | Review queue |
| `SupervisoryReviewResolvedItem` | Item resolved after review | Case resolution |
| `ComplianceSupervisionExchange` | Exchange message flagged | Email compliance |

---

## Quick Reference: Investigation Goal to Operation Names

Use this table to quickly find the right operations for your investigation objective.

| Investigation Goal | Exact Operation Names to Use in Prompt |
|-------------------|----------------------------------------|
| Track file downloads | `FileDownloaded`, `FileSyncDownloadedFull` |
| Detect USB exfiltration | `FileCreatedOnRemovableMedia`, `FileCopiedToRemovableMedia` |
| Find external sharing | `SharingSet`, `SharingInvitationCreated`, `AnonymousLinkCreated`, `SecureLinkCreated` |
| Detect email forwarding rules | `New-InboxRule`, `Set-InboxRule`, `UpdateInboxRules`, `Set-Mailbox` |
| Track email exfiltration | `Send`, `SendAs`, `SendOnBehalf` |
| File deletion / evidence hiding | `FileDeleted`, `FileDeletedFirstStageRecycleBin`, `FileDeletedSecondStageRecycleBin`, `SoftDelete`, `HardDelete`, `MoveToDeletedItems` |
| Cloud upload exfiltration | `FileUploadedToCloud`, `FileUploaded`, `FileSyncUploadedFull` |
| Print exfiltration | `FilePrinted` |
| Clipboard exfiltration | `FileCopiedToClipboard`, `WebpageCopiedToClipboard`, `PastedToBrowser` |
| Permission escalation | `SiteCollectionAdminAdded`, `Add-MailboxPermission`, `MemberRoleChanged`, `Add member to role` |
| DLP policy matches | `DLPRuleMatch`, `DLPRuleUndo`, `DLPInfo` |
| Insider risk signals | `SecurityComplianceInsiderRiskPolicy`, `InsiderRiskSequenceDetected`, `InsiderRiskAlertGenerated` |
| Sign-in anomalies | `UserLoggedIn`, `UserLoginFailed` (requires Entra plugin) |
| Teams data movement | `MessageCreatedHasLink`, `ChannelAdded`, `MemberAdded`, `AppInstalled` |
| Shadow IT / unallowed apps | `FileAccessedByUnallowedApp`, `BrowseToUrl` |
| Communication compliance | `SupervisoryReviewPolicyMatch`, `SupervisoryReviewTaggedItem` |
| eDiscovery activity | `SearchCreated`, `SearchExported`, `CaseCreated`, `HoldCreated` |
| Sensitivity label applied | `SensitivityLabelApplied`, `SensitivityLabelUpdated`, `FileSensitivityLabelChanged`, `MIPLabel` |
| Sensitivity label removed/downgraded | `SensitivityLabelRemoved`, `SensitivityLabeledFileRemoved` |
| Auto-labeling activity | `AutoSensitivityLabelRuleMatch`, `SensitivityLabelPolicyMatched` |
| Labeled file access | `SensitivityLabeledFileOpened`, `SensitivityLabeledFileRenamed` |

---

## Usage Examples in Security Copilot Prompts

### Example 1: DLP + File Exfiltration Investigation
```
Analyze audit logs for user <user> in the past 30 days. Look specifically
for these operations: FileDownloaded, FileSyncDownloadedFull,
FileCreatedOnRemovableMedia, FileCopiedToRemovableMedia,
FileUploadedToCloud, SharingSet, AnonymousLinkCreated, Send.

For each occurrence, provide:
- Timestamp and workload
- File or resource name
- Destination (external domain, USB device, cloud app)
- Whether a DLP policy matched (DLPRuleMatch)
```

### Example 2: Email Forwarding Rule Investigation
```
Check for suspicious mail flow changes for user <user> in the past 14 days.
Look specifically for these operations: New-InboxRule, Set-InboxRule,
UpdateInboxRules, Set-Mailbox.

For each rule found, show:
- Rule name and conditions
- Forwarding destination (especially external domains)
- When the rule was created or modified
- Whether the rule is still active
```

### Example 3: Evidence Destruction Detection
```
Analyze audit logs for user <user> in the past 7 days for evidence of
file or email deletion. Look for these operations: FileDeleted,
FileDeletedFirstStageRecycleBin, FileDeletedSecondStageRecycleBin,
SoftDelete, HardDelete, MoveToDeletedItems, FileRecycled.

For each deletion:
- What was deleted and when
- Was the content sensitive (any DLP match history)?
- Was deletion followed by recycle bin purge?
- Timeline correlation with other suspicious activities
```

---

## Appendix A: Content Blob to Operation Routing

> **Source:** [Microsoft 365 Compliance audit log activities via O365 Management API - Part 2](https://techcommunity.microsoft.com/blog/microsoft-security-blog/microsoft-365-compliance-audit-log-activities-via-o365-management-api---part-2/2957297)

When querying the O365 Management Activity API, operations are distributed across content blobs. Use this table to know WHERE to find each operation type.

| Content Blob | Operations | Notes |
|---|---|---|
| `Audit.AzureActiveDirectory` | `UserLoggedIn` | General investigation and sign-in context |
| `Audit.Exchange` | `MIPLabel`, `DLPRuleMatch`, `AutoSensitivityLabelRuleMatch` | Exchange-specific MIP and DLP events |
| `Audit.SharePoint` | `DLPRuleMatch`, `DLPRuleUndo`, `DlpInfo`, `FileSensitivityLabelChanged` | `FileSensitivityLabelChanged` is for Office on the Web |
| `Audit.General` | `SensitivityLabelApplied`, `SensitivityLabelUpdated`, `SensitivityLabelChanged`, `SensitivityLabelRemoved`, `SensitivityLabeledFileOpened`, `SensitivityLabeledFileApplied`, `SensitivityLabeledFileRenamed`, `SensitivityLabeledFileRemoved`, `FileSensitivityLabelChanged`, `AutoSensitivityLabelRuleMatch`, `DLPRuleMatch`, `DLPRuleUndo`, `DlpInfo` | Main blob for MIP and DLP cross-workload events |
| `DLP.All` | `DLPRuleMatch`, `DLPRuleUndo`, `DlpInfo`, `MIPLabel` | DLP events may include sensitive data (if configured). Requires "Read DLP sensitive data" permission |

---

## Appendix B: MIP/DLP Schema Key Fields

> **Source:** [Microsoft 365 Compliance audit log activities via O365 Management API - Part 1](https://techcommunity.microsoft.com/blog/microsoft-security-blog/microsoft-365-compliance-audit-log-activities-via-o365-management-api---part-1/2957171)

### ActionSource (How the label was applied)

| Value | Meaning | Investigation Use |
|-------|---------|-------------------|
| 0 | None | Covers operations like CopiedToUSB |
| 1 | Default | Default label was applied automatically |
| 2 | Auto | Auto-labeling policy applied the label |
| 3 | Manual | User manually applied the label |
| 4 | Recommended | User accepted a recommended label |

### ActionSourceDetail

| Value | Meaning |
|-------|---------|
| 0 | None |
| 1 | AutoByPolicyMatch |
| 2 | AutoByReplyOrForward |
| 3 | AutoByInheritance |
| 4 | AutoByDeploymentPipeline |
| 5 | PublicAPI |
| 6 | AutoByLibraryDefault |

### LabelEventType

| Value | Meaning | Investigation Use |
|-------|---------|-------------------|
| 0 | None | — |
| 1 | LabelUpgraded | Label was upgraded (sensitivity increased) |
| 2 | LabelDowngraded | Label was downgraded — investigate justification |
| 3 | LabelRemoved | Label was removed entirely |
| 4 | LabelChangedSameOrder | Label changed but same sensitivity tier |

### DLP Extended Schema Fields

Use these field names when asking Security Copilot to provide detailed DLP context:

| Field | Description | Prompt Use |
|-------|-------------|------------|
| `PolicyId` / `PolicyName` | The DLP policy that matched | Correlate matches to specific policies |
| `RuleId` / `RuleName` | The specific DLP rule that matched | Identify which rule conditions were met |
| `IncidentId` | Unique DLP incident ID | Correlate related events across time |
| `Actions` | Actions taken (e.g., BlockAccess, NotifyUser, Encrypt) | Understand enforcement response |
| `Severity` | Low / Medium / High | Prioritize investigation effort |
| `DetectedValues` | Sensitive data detected (Name + Value) | Requires "Read DLP sensitive data" permission |
| `Reason` (ExceptionInfo) | Override / DocumentChange / PolicyChange | Understand why DLP action was undone |
| `Justification` | User-provided justification for override | Assess override legitimacy |

---

**See also:**
- [GROUND_TRUTH.md](GROUND_TRUTH.md) for SC capabilities, plugin mechanics, and data-access limitations
- [SENSITIVE_INFO_TYPES.md](SENSITIVE_INFO_TYPES.md) for exact sensitive-information-type names
