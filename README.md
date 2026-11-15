# Data Access Decision Simulator

|                 |                                                       |
| --------------- | ----------------------------------------------------- |
| **Company**     | EduConnect Ghana (ed-tech platform, 50,000+ students) |
| **Role**        | DevOps Engineer                                       |
| **Date**        | 13 November 2026                                      |
| **Prepared by** | Shina Atete Mpeta                                     |

## Context

It's Monday morning and three data access requests have arrived in the ticketing system. Each one is decided using the company's data classification policy, the data lifecycle, and access control principles.

### Data classification policy

| Tier             | Examples                                    |
| ---------------- | ------------------------------------------- |
| **PUBLIC**       | Marketing materials, public course catalogs |
| **INTERNAL**     | Aggregated analytics, internal reports      |
| **CONFIDENTIAL** | Student PII, grades, payment information    |

### Basis for decisions

- The three-tier data classification policy above
- The principle of least privilege
- Rwanda's Law No. 058/2021 on the protection of personal data and privacy, supervised by the National Cyber Security Authority (NCSA)

### Summary

| #   | Request                                                   | From                                  | Decision                 | Lifecycle stage |
| --- | --------------------------------------------------------- | ------------------------------------- | ------------------------ | --------------- |
| 1   | [Marketing Campaign](#request-1-marketing-campaign)       | Sarah Owusu, Marketing Manager        | **Conditional approval** | Use             |
| 2   | [Analytics Partnership](#request-2-analytics-partnership) | David Mensah, Head of Product         | **Deny** (as submitted)  | Share           |
| 3   | [Archive and Deletion](#request-3-archive-and-deletion)   | Comfort Asante, Customer Support Lead | **Conditional approval** | Destroy         |

---

## Request 1: Marketing Campaign

> **From:** Sarah Owusu, Marketing Manager
>
> "I need the full student database (names, emails, phone numbers, course enrollments) to launch our new referral campaign. This is urgent. Campaign starts Friday!"
>
> **Data classification:** Email addresses (CONFIDENTIAL), Names (INTERNAL), Enrollments (INTERNAL)
> **Current access level:** INTERNAL only

### Decision

**CONDITIONAL APPROVAL.** The full student database export is denied. A limited, consent-based campaign is approved.

### Lifecycle stage

**Use.** Stored student data would be used internally for a new purpose (marketing).

### Justification

- **Classification:** Email addresses are CONFIDENTIAL. Phone numbers are not classified in the brief, but as personal data I treat them as CONFIDENTIAL. Sarah has INTERNAL access only, so she cannot receive them.
- **Least privilege:** The campaign needs to reach students, not to hold their contact details. The full database (including phone numbers) is more than the task requires.
- **Compliance (Law No. 058/2021):** Consent must be for a specified purpose (Art. 6). Students can object to direct marketing at any time (Art. 19). Children under 16 need parental consent (Art. 9). Urgency does not change these rules.

### Action steps

1. Tell Sarah the export is denied and offer the compliant alternative below, which still allows a Friday launch.
2. Consult the Data Owner and the Data Protection Officer to confirm that students have consented to marketing use and to approve the campaign. Inform Sarah's line manager.
3. Build the audience as emails only, limited to students who opted in to marketing. Exclude under-16s without parental consent. Use phone numbers only if students consented to SMS.
4. Send through the approved email platform, run by DevOps or with a restricted tool, so Sarah never receives the raw list. Share only aggregated results with her.
5. **Safeguards:** time-limited access, access logging (Art. 16), an unsubscribe link in every message, immediate handling of opt-outs, and deletion of any extract after the campaign.

---

## Request 2: Analytics Partnership

> **From:** David Mensah, Head of Product
>
> "I've signed a partnership with DataInsights Inc. (US-based) to analyze our student learning patterns. They need access to our AWS database to run their algorithms. Login credentials attached."
>
> **Data classification:** Student activity logs (INTERNAL), Student profiles (CONFIDENTIAL)

### Decision

**DENY as submitted.** The request can be reopened as a conditional approval once the legal and security requirements below are met.

### Lifecycle stage

**Share.** This is a transfer of student data to a third party outside Rwanda.

### Justification

- **Classification:** Student profiles are CONFIDENTIAL. Sharing them externally needs formal approval from the data owner and DPO. A partnership signed by the Head of Product does not replace that. Raw activity logs are INTERNAL but can identify students, so they must be protected as personal data.
- **Least privilege:** Direct access to the AWS database gives DataInsights far more than it needs. Sending credentials in a ticket is itself a security failure.
- **Transfer rules:** Sending personal data outside Rwanda is restricted and needs a permitted ground or NCSA authorisation (Art. 48). A written contract with the recipient is required (Art. 49), and the DPO has issued standard contractual clauses for this. Storage outside Rwanda needs a registration certificate that allows it (Art. 50).
- **Other red flags:** There is no stated consent or purpose for this new use (Art. 6). EduConnect's registration must declare transfer countries and recipients (Art. 30). A processor must work under a written contract (Art. 4).

### Action steps

1. Do not use the credentials. Revoke and rotate them now, remove them from the ticket, and review access logs. If personal data was accessed without authorisation, notify the NCSA within 48 hours (Art. 43).
2. Tell David there is no access until Legal and the DPO have reviewed. Escalate to the DPO, Legal and the Security lead.
3. Before any sharing: sign a data processing and transfer contract using the DPO's standard clauses (Arts. 4, 49), confirm NCSA authorisation or the right certificate (Arts. 48, 50), and update the registration and privacy notice if transfers to the US are new.
4. If approved, share only a pseudonymised or aggregated extract of activity logs, with no CONFIDENTIAL profiles. Use a separate read-only environment with named accounts, MFA, time limits and logging. Prohibit re-identification (Art. 57).
5. **Documentation required:** the signed partnership agreement, the transfer contract, NCSA authorisation or certificate, a list of the fields shared, a risk assessment, access approvals and logs, and an exit clause requiring return or deletion of data.

---

## Request 3: Archive and Deletion

> **From:** Comfort Asante, Customer Support Lead
>
> "A student, James Boateng, has requested full deletion of his account and all associated data per his 'right to be forgotten.' He completed his last course 6 months ago and has no pending payments. How should I proceed?"
>
> **Data classification:** All student data (CONFIDENTIAL)
> **Current status:** Account inactive, no outstanding obligations

### Decision

**CONDITIONAL APPROVAL.** Delete James's data, keeping only records that the law requires.

### Lifecycle stage

**Destroy.** The data reaches the end of its life at the student's request.

### Justification

- **Classification:** All of James's data is CONFIDENTIAL, so deletion must be verified, complete and documented. Support should not delete it informally.
- **Can it be fulfilled?** Yes. Under Art. 23, a student may request erasure when the data is no longer necessary for its original purpose. His account is inactive with no payments or obligations outstanding.
- **Limits:** Erasure does not apply to the extent that processing is needed to meet a legal obligation or to establish or defend legal claims (Art. 23). Records that other laws require, such as payment or tax records (confirm with Finance and Legal), may be kept, restricted, and deleted when the retention period ends (Art. 52). Deleting data that must be kept can itself be an offence (Art. 58).

### Action steps

1. Comfort logs the request in the ticket and verifies James's identity through his registered email. She does not delete anything herself.
2. Route the request to the DPO or Data Owner to decide what must be retained and for how long.
3. DevOps deletes everything else: account, profile, course activity and grades, in production, analytics and caches. Backups are purged on the normal cycle and not restored. Anonymised, aggregated statistics may stay because they are no longer personal data.
4. Tell any third party or processor that received his data that he requested erasure (Art. 23).
5. Record what was erased, when and by whom (Art. 16).
6. Reply to James in writing within 30 days: confirm the deletion, state what was kept and why, and explain that he can appeal to the NCSA within 30 days of the reply (Art. 23).
