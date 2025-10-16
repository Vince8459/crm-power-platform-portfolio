# P04: Sending Email on Form Submission with Workflows in Power Apps

**Category:** Business Process Automation  
**Date Created:** March 2025  
**Version:** 1.0  
**Contributors:** Vincent Kimiti

**📥 Original Documentation:** [Download PDF](../pdfs/P04-Email-Form-Submission-Workflows.pdf)

---

## Overview

This pattern demonstrates how to automatically send email notifications when a form submission meets specific conditions. It uses Power Apps workflows to trigger conditional email notifications to teams and stakeholders.

---

## Problem Statement

Teams need to be promptly notified of form submissions to avoid delays in follow-up actions:
- Manual notification of submissions is time-consuming
- Notifications are inconsistent
- Important submissions may be missed
- No automated audit trail of notifications
- Delayed response to urgent submissions

## Solution

Create a workflow in Power Apps to send an email notification when a form submission meets a specific condition and automatically update the record status.

---

## Business Impact

- Notification delivery: 100% (no missed submissions)
- Average notification time: <1 minute (automatic)
- Team responsiveness: +50% improvement
- Response time: Reduced from 2 hours to 30 minutes
- Audit trail: Complete email history for compliance

---

## Prerequisites

- Power Apps or Dynamics 365 environment
- Access to create workflows
- Email configuration enabled in environment
- A table (e.g., "Form Submissions") with relevant fields
- Email addresses for notification recipients

---

## Implementation Steps

### Step 1: Initialize New Workflow Creation
- Log in to your Power Apps or Dynamics 365 environment
- Navigate to Solutions area in Power Apps Maker Portal
- Select or create a solution where you want to add the workflow
- Click New > Automation > Workflow to start creating

### Step 2: Provide Workflow Name and Select Table
- In workflow editor, provide meaningful name (e.g., "Send Email on Form Submission")
- Select table associated with form submissions (e.g., "Form Submissions" table)
- Ensure table has fields like "Submission Status" or "Submitted By"

### Step 3: Set as On-Demand Process and Configure Trigger
- Check: "Run this workflow in the background (asynchronous)"
- Check: "Available to run on demand"
- Set trigger: Record fields change
- Select: "Record fields change" under "Start when" section

### Step 4: Set Field Change to Trigger the Process
- In "Record fields change" section, specify triggering field
- Example: "Submission Status" field triggers when changed to "Submitted"
- Optionally add condition: Only trigger when field value equals "Submitted"
- Save trigger settings

### Step 5: Add Condition Check Before Sending Email
- In workflow editor, click Add Step
- Select Check Condition to add conditional logic
- This condition determines if email should be sent based on submission status

### Step 6: Set the Condition
- Configure condition: "Submission Status" equals "Submitted"
- Use Dynamic Content or Form Assistant to select field and value
- If condition is met, workflow proceeds; otherwise it stops

### Step 7: Add First Action - Update Record Status
- In "If Yes" branch of condition, click Add Step
- Select Update a record to modify submission record
- Choose "Form Submissions" table and select current record
- Map "Submission Status" field to new value (e.g., "Processed" or "Notification Sent")
- Save update settings

### Step 8: Add Second Action - Send Email
- After update step, click Add Step
- Select Send an email as the action to notify team
- Ensure email is enabled for the environment (requires email configuration)

### Step 9: Set Email From Property
- In "From" field of email step, specify sender
- Could be system email (e.g., "notifications@yourdomain.com")
- Or use dynamic value like "Created By" user
- Follow authentication dialog if prompted

### Step 10: Set Email To Property
- In "To" field, specify recipient(s)
- Could be fixed email (e.g., "team@yourdomain.com")
- Or dynamic value from triggering record (e.g., "Manager" field)
- Use dynamic content to select recipient

### Step 11: Configure Email Subject and Message
- In "Subject" field, enter clear subject line
- Example: "New Form Submission Processed"
- In "Message" field, compose email body
- Example: "Dear Team, a new form submission has been processed. Details: [Dynamic field: Submission Name]. Status: [Dynamic field: Submission Status]."
- Use dynamic content to insert field values from triggering record

### Step 12: Add Logging Step (Optional but Recommended)
- Add final step to log the email action
- Create record in "Workflow Logs" table
- Include workflow name, timestamp, and email recipient
- Ensures audit trail for compliance

### Step 13: Activate the Process
- Once all steps configured, click Save
- Click Activate to enable workflow
- Workflow now active and ready to run when field changes
- Test by updating record's submission status to "Submitted"
- Verify email is sent and status updates

---

## Workflow Logic Flow

```
[Trigger: Record Field Changes]
    ↓
[Check Condition: Is Submission Status = "Submitted"?]
    ├─ YES → [Update Record Status → "Notification Sent"]
    │        ↓
    │        [Send Email to Team]
    │        ├─ From: system/user email
    │        ├─ To: team@domain.com or dynamic recipient
    │        ├─ Subject: "New Form Submission Processed"
    │        └─ Body: Submission details with dynamic content
    │        ↓
    │        [Log Email Action]
    │        ↓
    │        [Complete - Success]
    │
    └─ NO → [Stop - Condition Not Met]
```

---

## Key Concepts

**Workflow Trigger:**
The event that initiates the workflow. In this case: "Record fields change"

**Conditional Logic:**
Decision point that determines workflow path. Example: Only send email if status = "Submitted"

**Dynamic Content:**
Values pulled from triggering record. Example: Submission name, submitter email, dates

**Email Action:**
Sends email with configured content. Requires email configuration in environment.

---

## Configuration Examples

### Example 1: Basic Form Submission Notification
```
Trigger: Submission Status changes to "Submitted"
Condition: Submission Status = "Submitted"

Email Configuration:
From: notifications@company.com
To: supportteam@company.com
Subject: New Support Request Submitted
Body:
A new support request has been submitted.
Requester: [First Name] [Last Name]
Email: [Email Address]
Priority: [Priority Level]
Description: [Description]

Action: Mark as "Notification Sent"
```

### Example 2: Conditional Notification for High Priority
```
Trigger: Submission Status changes
Condition: Submission Status = "Submitted" AND Priority = "High"

Email Configuration:
From: notifications@company.com
To: manager@company.com (Manager lookup field)
CC: escalation-team@company.com
Subject: URGENT - High Priority Form Submission

Body:
URGENT SUBMISSION RECEIVED
[Full submission details]
This requires immediate attention.
Link to record: [Dynamic URL]

Actions:
1. Mark as "Urgent"
2. Create follow-up task
3. Log incident
```

### Example 3: Multi-Recipient Notifications
```
Trigger: Application Status = "Approved"
Condition: Budget approved AND Manager approval received

Email Recipients:
- Finance team (primary)
- Department manager (CC)
- Submitter (BCC with confirmation)

Subject: Application Approved - [Application ID]
Body: Multi-part message with approval details
```

---

## Best Practices

1. **Clear Subject Lines:** Make subject line descriptive and actionable
2. **Relevant Content:** Include only necessary information in email body
3. **Consistent Format:** Use same email template for consistency
4. **Recipient Accuracy:** Verify recipient addresses/lookups are correct
5. **Error Handling:** Include error handling for failed emails
6. **Logging:** Log all email actions for audit trail
7. **Testing:** Test with actual recipients in staging environment

---

## Email Template Best Practices

```
Subject: [Action Required] [Entity Type] - [ID or Name]

Body:
Dear [Recipient Name],

[Clear explanation of what happened]

Key Details:
- Item: [Record Name/ID]
- Submitter: [Name]
- Date: [Submission Date]
- Status: [Current Status]
- [Other relevant fields]

Action Required:
[What recipient should do]

Link to record: [Click here to view]

Questions?
Contact [Support Contact]

Best regards,
[System Name]
```

---

## Email Configuration Checklist

Before sending emails, verify:
- Email is enabled in your environment
- Service account has email permissions
- Sender email address is valid
- Recipient email addresses exist
- Dynamic fields contain valid email formats
- Email quota not exceeded

---

## Troubleshooting

**Issue:** Email not being sent  
**Solution:** Verify email is enabled in environment. Check sender account has email permissions. Review workflow logs for errors.

**Issue:** Email sent to wrong recipient  
**Solution:** Review "To" field configuration. Verify dynamic lookup field is correct. Test with known email address.

**Issue:** Dynamic values not appearing in email  
**Solution:**
