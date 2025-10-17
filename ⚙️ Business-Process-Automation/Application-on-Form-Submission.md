# P03: Create an Application on Form Submission with Workflows in Power Apps

**Category:** Business Process Automation  
**Date Created:** March 2025  
**Version:** 1.0  
**Contributors:** Vincent Kimiti

**📥 Original Documentation:** [Download PDF](https://github.com/Vince8459/crm-power-platform-portfolio/blob/91f24870678c4af86407370dc990d8b1238af3cf/pdf/P03%20Create%20an%20Application%20on%20form%20submission%20with%20Workflows%20in%20Power%20Apps.pdf)

---

## Overview

This pattern demonstrates how to automate the creation of an application record when a specific field changes in Power Apps. It uses workflows to listen for field changes and automatically create related records in Dataverse.

---

## Problem Statement

Manually creating and updating application records is time-consuming and error-prone:
- Users must manually create records for each application
- Field changes require associated record creation
- High risk of incomplete or missing records
- Duplicate effort across teams
- Difficult to maintain data consistency

## Solution

Use a workflow in Power Apps to automatically create and update application records when a specific field changes. This eliminates manual intervention and ensures consistency.

---

## Business Impact

- Automatic record creation: 100% coverage
- Manual effort: 90% eliminated
- Data consistency: 100% (system-generated)
- Error rate: Reduced from 15% to 0%
- Processing time: From 5 minutes to instantaneous

---

## Prerequisites

- Power Apps or Dynamics 365 environment
- Access to create workflows
- A table (e.g., "Applications") with relevant fields
- Understanding of trigger conditions

---

## Implementation Steps

### Step 1: Initialize New Workflow Creation
- Log in to your Power Apps or Dynamics 365 environment
- Navigate to the Solutions area in the Power Apps Maker Portal
- Select or create a solution where you want to add the workflow
- In the solution, click New > Automation > Workflow to start creating a new workflow

### Step 2: Provide Workflow Name and Select Table
- In the workflow editor, provide a meaningful name (e.g., "Automate Application Creation")
- Select the table that the workflow will be associated with (e.g., "Applications" table)
- Ensure the table has necessary fields (e.g., status, name, or other fields you'll use for triggering or updating)

### Step 3: Set as On-Demand Process and Configure Trigger
- In workflow properties, check: "Run this workflow in the background (asynchronous)"
- Check: "Available to run on demand"
- Set trigger: Record fields change
- Under "Start when" section, select: "Record fields change"

### Step 4: Set Field Change to Trigger the Process
- In "Record fields change" section, specify the field that will trigger the workflow
- Example: Select "Status" field to trigger when status changes
- Save trigger settings to ensure workflow activates on field update

### Step 5: Add Steps to Create Application
- In workflow editor, click Add Step
- Select Create a new record as the action
- Choose the "Applications" table (or relevant table) as the record type to create

### Step 6: Set Application Properties
- In "Create a new record" step, configure properties of the new application record
- Map fields of the new application record:
  - Application Name
  - Created By
  - Status
  - Other relevant fields

### Step 7: Set Dynamic Values for Application Fields
- For fields that need dynamic values, use Dynamic Content or Form Assistant
- Example: Set "Application Name" based on the record that triggered the workflow
- Select corresponding field from triggering record (e.g., "Name" from triggering record)
- Map "Created By" to the user who modified the record

### Step 8: Define Additional Properties
- Add any additional fields that need to be populated in the new application record
- Example: Set "Priority" field to "High" if triggering record meets criteria
- Ensure all required fields in "Applications" table are filled to avoid errors

### Step 9: Add Condition Check Before Proceeding
- Add Check Condition step to ensure workflow only proceeds if criteria are met
- Example: Check if "Status" field = "Approved" before creating record
- If condition not met, workflow can stop or perform alternative action (send notification)

### Step 10: Add Action on Application Creation
- After creating application, add another step to perform an action
- Select Update a record to modify the newly created application record (or triggering record)

### Step 11: Set Update Properties
- In "Update a record" step, select record to update
- Specify fields to update (e.g., set "Status" to "In Progress")
- Map field values using dynamic content

### Step 12: Add Notification Step
- Add step to notify team about new application record
- Select Send an email as the action
- Configure email to include:
  - Application Name
  - Status
  - Link to the record
  - Send to relevant team or user

### Step 13: Activate the Process
- Once all steps configured, click Save
- Click Activate to enable the workflow
- Workflow is now active and ready to run when specified field changes
- Test by updating a record and verifying application creation

---

## Workflow Logic Flow

```
[Trigger: Field Changes]
    ↓
[Check Condition: Is field value = trigger value?]
    ├─ YES → [Create New Application Record]
    │        ↓
    │        [Populate fields with dynamic values]
    │        ↓
    │        [Update record status]
    │        ↓
    │        [Send notification email]
    │        ↓
    │        [Complete - Success]
    │
    └─ NO → [Stop workflow]
```

---

## Key Concepts

**Workflow Trigger:**
The event that initiates the workflow. In this case: "Record fields change"

**Dynamic Values:**
Values that change based on the triggering record's data. Example: Use the name from the triggering record.

**Conditions:**
Logic gates that determine if workflow proceeds. Example: Only create application if status = "Approved"

**Actions:**
Operations performed when conditions are met. Example: Create record, Send email, Update field

---

## Configuration Examples

### Example 1: Basic Application Creation
```
Trigger: Status field changes to "Submitted"
Action 1: Create new Application record
  - Application Name: [From triggering record Name]
  - Status: "New"
  - Owner: [From triggering record Created By]
Action 2: Send email to team
```

### Example 2: Conditional Application Creation
```
Trigger: Status field changes
Condition: Is Status = "Approved" AND Priority = "High"?
Action 1: Create Priority Application record
Action 2: Assign to high-priority queue
Action 3: Send urgent notification
```

---

## Best Practices

1. **Test Thoroughly:** Test workflow with various field values and scenarios
2. **Error Handling:** Add error handling steps for failed record creation
3. **Logging:** Create log records for audit trail
4. **Conditions:** Use conditions to prevent unnecessary record creation
5. **Notifications:** Keep teams informed of automated actions
6. **Documentation:** Document field mappings and triggering conditions

---

## Common Scenarios

**Sales Application:**
- Trigger: When Lead Status = "Qualified"
- Create: Opportunity record
- Benefit: Automatic opportunity creation from qualified leads

**Support Ticket:**
- Trigger: When Ticket Status = "Escalated"
- Create: Case record
- Benefit: Escalated tickets automatically create support cases

**HR Onboarding:**
- Trigger: When Employee Status = "Hired"
- Create: Onboarding Task records
- Benefit: Automatic task creation for new hires

---

## Troubleshooting

**Issue:** Workflow not triggering  
**Solution:** Verify trigger field is correct and workflow is activated. Check that field is actually changing.

**Issue:** Wrong record being created  
**Solution:** Review dynamic value mappings. Ensure correct fields are selected from trigger record.

**Issue:** Required fields not populated  
**Solution:** Verify all required fields have values mapped. Check data types match.

**Issue:** Duplicate records created  
**Solution:** Review conditions. Add logic to prevent creation of duplicates.

---

## Related Patterns

- P04: Sending Email on Form Submission with Workflows
- P01: Creating a Business Process Flow
- P05: Creating Business Rules in Power Apps

---

## Implementation Checklist

- [ ] Design workflow logic on paper
- [ ] Identify trigger field and trigger value
- [ ] Identify target table and fields to populate
- [ ] Create workflow in development environment
- [ ] Configure trigger
- [ ] Add condition checks
- [ ] Add create record step
- [ ] Map all required fields with dynamic values
- [ ] Add update record step
- [ ] Add notification step
- [ ] Add error handling
- [ ] Test with sample data
- [ ] Test edge cases
- [ ] Get stakeholder approval
- [ ] Activate workflow
- [ ] Monitor for first week
- [ ] Document any customizations

---

## Questions or Need Help?

For detailed implementation guidance, refer to the original PDF documentation.

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Last Updated:** March 2025  
**Status:** Active  
**Use Cases:** 5+
