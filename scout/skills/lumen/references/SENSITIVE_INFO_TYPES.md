# Sensitive Information Type (SIT) Exact Names Reference

**Last Updated:** April 2026
**Status:** [VALIDATED]
**Purpose:** Exact built-in SIT names for use in Security Copilot prompts to ensure correct detection mapping

---

## Why This Matters

When prompts reference "credit card numbers" or "social security numbers," Security Copilot may not always map to the correct built-in Sensitive Information Type. Using the exact SIT name ensures SC queries the right detection logic and returns accurate results.

**Usage Pattern:**
```
What DLP alerts were triggered by the sensitive information type
"Credit Card Number" or "U.S. Social Security Number (SSN)"
in the past 30 days?
```

---

## 1. Financial Data SITs

| Exact SIT Name | Common Reference | Region |
|---------------|-----------------|--------|
| `Credit Card Number` | Credit card, CC number, PAN | Global |
| `U.S. Bank Account Number` | Bank account, routing number | US |
| `International Banking Account Number (IBAN)` | IBAN | Global |
| `SWIFT Code` | SWIFT, BIC code | Global |
| `U.S. Individual Taxpayer Identification Number (ITIN)` | ITIN, tax ID | US |
| `U.S. / U.K. Passport Number` | Passport number | US/UK |
| `ABA Routing Number` | ABA routing, bank routing | US |
| `EU Debit Card Number` | EU debit card | EU |
| `Japan Bank Account Number` | JP bank account | Japan |
| `Israel Bank Account Number` | IL bank account | Israel |
| `Australia Bank Account Number` | AU bank account | Australia |

## 2. Personal Identification SITs

| Exact SIT Name | Common Reference | Region |
|---------------|-----------------|--------|
| `U.S. Social Security Number (SSN)` | SSN, social security | US |
| `U.K. National Insurance Number (NINO)` | NI number, NINO | UK |
| `EU National Identification Number` | National ID, EU ID | EU |
| `Germany Identity Card Number` | German ID, Personalausweis | Germany |
| `France National ID Card (CNI)` | French CNI, carte nationale | France |
| `Spain DNI` | Spanish DNI | Spain |
| `Italy Fiscal Code` | Codice fiscale | Italy |
| `Canada Social Insurance Number` | SIN, Canadian social insurance | Canada |
| `Australia Tax File Number` | TFN, Aus tax number | Australia |
| `Japan Social Insurance Number (My Number)` | My Number, Japan SIN | Japan |
| `Brazil CPF Number` | CPF | Brazil |
| `India Permanent Account Number (PAN)` | India PAN, tax PAN | India |
| `India Unique Identification (Aadhaar) Number` | Aadhaar | India |

## 3. Healthcare and HIPAA SITs

| Exact SIT Name | Common Reference | Region |
|---------------|-----------------|--------|
| `U.S. Health Insurance Claim Number (HICN)` | HICN, Medicare claim | US |
| `Drug Enforcement Agency (DEA) Number` | DEA number | US |
| `International Classification of Diseases (ICD-9-CM)` | ICD-9 code | Global |
| `International Classification of Diseases (ICD-10-CM)` | ICD-10 code | Global |
| `All Full Names` | Patient names, full names | Global |
| `All Medical Terms and Conditions` | Medical conditions, diagnoses | Global |
| `All Physical Addresses` | Patient address | Global |
| `U.S. State Driver's License Number` | Driver's license | US |

## 4. Privacy and GDPR SITs

| Exact SIT Name | Common Reference | Region |
|---------------|-----------------|--------|
| `EU Social Security Number or Equivalent ID` | EU SSN, social security | EU |
| `EU Driver's License Number` | EU DL | EU |
| `EU Passport Number` | EU passport | EU |
| `EU Tax Identification Number (TIN)` | EU TIN, tax number | EU |
| `Germany Driver's License Number` | German DL | Germany |
| `France Driver's License Number` | French DL | France |
| `IP Address` | IP, network address | Global |
| `Azure AD User Credential` | Azure AD credential | Global |
| `SQL Server Connection String` | SQL connection string | Global |
| `Azure Service Bus Connection String` | Service Bus connection | Global |
| `Azure Storage Account Key` | Storage key | Global |

## 5. Credentials and Secrets SITs

| Exact SIT Name | Common Reference | Region |
|---------------|-----------------|--------|
| `Azure AD Client Secret` | Client secret, app secret | Global |
| `Azure AD Client Access Token` | Access token | Global |
| `Azure Service Bus Shared Access Key` | SAS key | Global |
| `Azure Storage Account Shared Access Signature` | SAS token | Global |
| `Azure IoT Connection String` | IoT connection | Global |
| `Azure Cosmos DB Account Access Key` | Cosmos key | Global |
| `Azure Publish Setting Password` | Publish password | Global |
| `Azure Redis Cache Connection String` | Redis connection | Global |
| `Azure SQL Connection String` | SQL connection string | Global |
| `General Password` | Password, credentials | Global |
| `General Symmetric Key` | Symmetric key, encryption key | Global |
| `SSH Private Key` | SSH key, private key | Global |

## 6. Common Custom SIT Patterns

Organizations frequently create custom SITs. When prompting SC about custom SITs, use the exact custom SIT display name as configured in the Purview compliance portal.

**SC Limitation:** Custom SIT regex internals may produce opaque match explanations. If SC cannot explain why a custom SIT matched, reference the custom SIT name and ask SC to show the content metadata and match count instead.

**Prompt Pattern for Custom SITs:**
```
Summarize the DLP alert with ID <alertId>. The alert triggered on a custom
sensitive information type named "<ExactCustomSITName>". Show the match count,
confidence level, and the policy rule that detected it.
```

---

## Quick Reference: Investigation Goal to SIT Names

| Investigation Goal | Exact SIT Names to Use |
|-------------------|----------------------|
| Financial data exposure | `Credit Card Number`, `U.S. Bank Account Number`, `International Banking Account Number (IBAN)`, `ABA Routing Number` |
| PII exposure (US) | `U.S. Social Security Number (SSN)`, `U.S. State Driver's License Number`, `U.S. / U.K. Passport Number` |
| PII exposure (EU/GDPR) | `EU National Identification Number`, `EU Social Security Number or Equivalent ID`, `EU Passport Number`, `EU Driver's License Number` |
| Healthcare/HIPAA data | `U.S. Health Insurance Claim Number (HICN)`, `Drug Enforcement Agency (DEA) Number`, `International Classification of Diseases (ICD-10-CM)` |
| Credential exposure | `Azure AD Client Secret`, `General Password`, `SSH Private Key`, `SQL Server Connection String`, `Azure Storage Account Key` |
| Multi-region PII | `All Full Names`, `All Physical Addresses`, `IP Address` |

---

## Usage in Prompts

### Correct (use exact SIT name):
```
Show DLP alerts where the sensitive information type "Credit Card Number"
was detected with high confidence (>85%) in the past 7 days.
```

### Incorrect (vague reference):
```
Show DLP alerts about credit card data in the past 7 days.
```

The exact SIT name ensures SC queries the right detection engine and returns only relevant alerts.

---

**See also:**
- [AUDIT_LOG_OPERATIONS.md](AUDIT_LOG_OPERATIONS.md) for exact audit log operation names
- [GROUND_TRUTH.md](GROUND_TRUTH.md) for capability and plugin requirements
