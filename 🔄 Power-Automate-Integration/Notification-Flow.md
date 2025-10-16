# P06: Creating a Notification Flow in Power Automate

**Category:** Power Automate & Integration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Case Assignment Notifications

**📥 Original Documentation:** [Download PDF](../pdfs/P06-Notification-Flow.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Apps

**Application Use Case:**
- Send a notification when a case/lead/opportunity is assigned

**Reference Usage:** Automating case assignment notifications using Power Automate

---

## Story Behind The Pattern

### The Problem
- Users are not immediately notified of new case/lead/opportunity assignments
- Delays in action and response times
- Users may miss assignments
- Manual notification process is unreliable
- No automated tracking of notifications

### The Solution
- Create a Power Automate flow to trigger a notification
- Log notification when a case/opportunity/lead is added, modified, or deleted
- Automatic notification to assigned user
- Improved response time and user awareness

### Dependencies
- Power Automate environment
- Permissions to create flows and access tables

---

## The Pattern: Implementation Method

### Step 1: Create a New Flow in Power Automate

1. Log in to the Power Automate portal (make.powerautomate.com)
2. Click **Create** and select **Automated cloud flow**
3. Name the flow (e.g., **"Case_New Assignment Toast Notification"**)
4. Click **Create**
5. Choose the trigger: **When a row is added, modified or deleted from the Dataverse connector**

---

### Step 2: Configure the Trigger

In the trigger settings, set the following:

1. **Change type:** Select **"Added, Modified"** (to trigger on new or updated cases; exclude "Deleted" if not needed)
2. **Table name:** Select **"Cases"** from the dropdown
3. **Scope:** Choose **"Organization"** to apply the trigger across the organization
4. **Select columns:** Leave blank or enter specific columns (e.g., **"Case Title,"** **"Owner"**) to optimize performance
5. **Filter rows:** Optionally, add an OData expression (e.g., **statuscode eq 1**) to limit triggers to active cases
6. Click **Save** to proceed

---

### Step 3: Add a Parse JSON Action

1. Click **+ New step** below the trigger
2. Search for and select the **Parse JSON** action
3. In the **Content** field, select the **"Body"** output from the trigger (this contains the row data in JSON format)
4. In the **Schema** field, click **Generate from sample** and paste a sample JSON payload:

```json
{
  "case_title": "Sample Case",
  "ownerid": "user-id-123",
  "owner_name": "John Doe",
  "createdon": "2025-01-15T10:30:00Z",
  "case_id": "case-123"
}
```

5. Click **Done** to generate the schema
6. This step parses the JSON to extract actionable data (e.g., URLs or case details)

---

### Step 4: Add a New Row to the Notifications Table

1. Click **+ New step** below the Parse JSON action
2. Search for and select **Add a new row** (Dataverse connector)
3. In the **Table name** field, select **"Notifications"** (or create a new table with columns like "Title," "Body," "Owner")
4. Configure the row details:

**Title Field:**
- Enter a dynamic value, e.g., **"New case is assigned to you"**
- Or use an expression: `concat('New case ', triggerOutputs()?['body/case_title'], ' is assigned to you')`

**Body Field:**
- Enter a dynamic value, e.g., **"New case [Case Title] is assigned to you"** with an icon
- Or: `concat('New case ', triggerOutputs()?['body/case_title'], ' is assigned to you')`

**Data Field:**
- Leave blank or add custom data (e.g., a URL from the parsed JSON)

**Expiry (seconds):**
- Set to a value (e.g., 86400 for 24 hours) to auto-delete the notification

**IconType:**
- Select **"Info"** from the dropdown

**Model-driven apps:**
- Leave blank (not used unless specified)

**Notification:**
- Set to a unique identifier (e.g., a GUID or dynamic value)

**Owner:**
- Select the owner dynamically (e.g., `triggerOutputs()?['body/ownerid']`)
- Or manually enter

**Priority:**
- Set to **"Normal"** or another value from the dropdown

5. Click **Save** to store the notification

---

### Step 5: Test and Validate the Flow

1. Click **Test** in the flow editor
2. Select **Manually** to trigger the flow
3. Create, modify, or delete a case in the **"Cases"** table
4. Check if the flow triggers
5. Verify that a new row is added to the **"Notifications"** table with the correct title, body, and owner
6. Ensure the toast notification appears in the Model-Driven App (if integrated)

---

### Step 6: Activate and Monitor the Flow

1. Once tested, click **Save** to save the flow
2. Click **Turn on** to activate the flow
3. Monitor the flow runs in the **Run history** to ensure it executes without errors
4. Adjust the flow (e.g., refine the JSON schema or notification fields) if issues arise

---

## Flow Architecture

```
[TRIGGER: Row Added/Modified]
    ↓ Case record created or modified
┌──────────────────────────────────┐
│ TRIGGER: When case is assigned   │
│ Table: Cases                     │
│ Change type: Added/Modified      │
│ Scope: Organization              │
└──────────────────────────────────┘
    ↓
┌──────────────────────────────────┐
│ ACTION: Parse JSON               │
│ Extract case details             │
│ - Title                          │
│ - Owner                          │
│ - Dates                          │
└──────────────────────────────────┘
    ↓
┌──────────────────────────────────┐
│ ACTION: Add New Row              │
│ Table: Notifications             │
│ Populate:                        │
│ - Title: Case assignment info    │
│ - Body: Case details             │
│ - Owner: Assigned user           │
│ - Expiry: 24 hours               │
│ - IconType: Info                 │
└──────────────────────────────────┘
    ↓
[NOTIFICATION CREATED]
    ↓
[USER RECEIVES NOTIFICATION]
```

---

## Key Concepts

**Trigger:**
Event that initiates the flow. In this case: "When a row is added, modified, or deleted"

**OData Filter:**
Query language to filter which records trigger the flow. Example: `statuscode eq 1` (only active cases)

**JSON Schema:**
Structure that defines the shape of data flowing through the flow. Used to validate and map data.

**Dynamic Content:**
Values pulled from previous steps. Example: `triggerOutputs()?['body/case_title']`

**Notifications Table:**
Table that stores notification records. Each record represents one notification to display to a user.

---

## Configuration Examples

### Example 1: Simple Case Assignment Notification
```
Trigger: Case added or modified
Filter: statuscode = Active

Notification Fields:
- Title: "New case assigned: [Case Title]"
- Body: "Case [Case ID] has been assigned to you"
- Owner: [Owner field from case]
- Priority: Normal
- Expiry: 86400 seconds (24 hours)
```

### Example 2: High Priority Case Alert
```
Trigger: Case modified
Filter: priority = High AND assigned = True

Notification Fields:
- Title: "URGENT: High Priority Case Assignment"
- Body: "Case [Case Title] requires immediate attention"
- Owner: [Owner field]
- Priority: High
- IconType: Alert
- Expiry: 172800 seconds (48 hours)
```

---

## Best Practices

1. **Filter Appropriately:** Use filters to only trigger on relevant changes
2. **Clear Notifications:** Include essential information (case ID, title, owner)
3. **Expiry Times:** Set appropriate expiry times based on urgency
4. **Error Handling:** Add retry logic for failed notifications
5. **Testing:** Test with various case types and assignments
6. **Monitoring:** Monitor flow runs for errors or patterns

---

## Troubleshooting

**Issue: Flow not triggering**
- Verify trigger is properly configured
- Check scope is set to Organization
- Ensure table name is correct
- Test by manually creating a case

**Issue: Notification not appearing**
- Verify Notifications table exists
- Check all required fields are populated
- Confirm owner field has valid user ID
- Review flow run history for errors

**Issue: Wrong information in notification**
- Verify JSON schema is correct
- Check dynamic value mappings
- Ensure filter conditions are met
- Test with known case values

**Issue: Too many notifications**
- Review filter conditions
- Check for duplicate triggers
- Adjust scope if needed
- Monitor flow runs

---

## Related Patterns

- P07: Capturing Leads from Microsoft Forms into Dataverse
- P08: Auto-Populating Forms with Power Automate
- P04: Sending Email on Form Submission with Workflows

---

## Implementation Checklist

- [ ] Create Notifications table in Dataverse
- [ ] Add required columns (Title, Body, Owner, etc.)
- [ ] Create new automated cloud flow
- [ ] Configure trigger (When row added/modified)
- [ ] Set change type to Added/Modified
- [ ] Select correct table (Cases)
- [ ] Set scope to Organization
- [ ] Add filter conditions if needed
- [ ] Add Parse JSON action
- [ ] Generate JSON schema from sample
- [ ] Add new row action
- [ ] Map all required fields
- [ ] Configure dynamic content for Title
- [ ] Configure dynamic content for Body
- [ ] Set Owner field dynamically
- [ ] Set Expiry time
- [ ] Test flow with sample data
- [ ] Verify notification appears
- [ ] Activate flow
- [ ] Monitor for errors
- [ ] Document configuration

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** March 2025  
**Status:** Active
