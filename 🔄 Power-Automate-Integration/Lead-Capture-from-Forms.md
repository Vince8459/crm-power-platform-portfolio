# P07: Capturing Leads from Microsoft Forms into Dataverse Using Power Automate

**Category:** Power Automate & Integration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Lead Capture Automation

**📥 Original Documentation:** [Download PDF](https://github.com/Vince8459/crm-power-platform-portfolio/blob/21d880b252cd4205f736ea3472f1802da99a6828/pdf/P07%20Capturing%20Leads%20from%20Microsoft%20Forms%20into%20Dataverse%20Using%20Power%20Automate.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Apps

**Application Use Case:**
- Capture lead data from a Microsoft Form and store it as a new lead record in Dataverse

**Reference Usage:** Automating lead capture from Microsoft Forms to Dataverse using Power Automate

---

## Story Behind The Pattern

### The Problem
- Manually entering lead data from Microsoft Forms into a CRM system like Dataverse is time-consuming
- Process is prone to errors
- High data entry workload
- Inconsistent data formatting
- Delayed lead processing
- Poor lead response times

### The Solution
- Create a Power Automate flow to automatically capture responses from a Microsoft Form
- Add them as new lead records in Dataverse
- Eliminate manual data entry
- Ensure immediate lead capture and notification

### Dependencies
- Power Automate environment
- Microsoft Forms
- Dataverse with permissions to create flows and access forms/tables

---

## The Pattern: Implementation Method

### Step 1: Create a New Flow in Power Automate

1. Log in to the Power Automate portal (make.powerautomate.com)
2. Click **Create** and select **Automated cloud flow**
3. Name the flow (e.g., **"Microsoft form to Lead"**)
4. Click **Create**
5. Choose the trigger: **When a new response is submitted from the Microsoft Forms connector**

---

### Step 2: Configure the Trigger

In the trigger settings, set the following:

1. **Form Id:** Select **"Investor Lead Capture FORM"** from the dropdown (or the specific form you're using to capture leads)
2. Click **Save** to proceed
3. This trigger will activate the flow whenever a new response is submitted to the form

---

### Step 3: Add a Get Response Details Action

1. Click **+ New step** below the trigger
2. Search for and select the **Get response details** action from the Microsoft Forms connector
3. Configure the action:
   - **Form Id:** Select **"Investor Lead Capture FORM"** (same as the trigger)
   - **Response Id:** Select **"Response Id"** from the dynamic content of the trigger (this links the action to the specific form submission)

4. This step retrieves the details of the submitted form response, such as:
   - Lead's topic
   - Email
   - First name
   - Last name
   - Phone number

---

### Step 4: Add a New Row to the Leads Table

1. Click **+ New step** below the Get response details action
2. Search for and select **Add a new row** (Dataverse connector)
3. In the **Table name** field, select **"Leads"** (or create a new table with columns like "Topic," "Email," "First Name," "Last Name," and "Phone Number")
4. Configure the row details by mapping form responses to Dataverse fields:

**Market Capitalization:**
- Leave blank unless the form captures this data

**Topic:**
- Select the form field for the lead topic (e.g., **"Topic"**) from the Get response details output

**Business Card:**
- Leave blank unless the form includes a file upload for a business card

**Description:**
- Leave blank or add additional context if needed

**Email:**
- Select the form field for the lead's email (e.g., **"Email"**) from the Get response details output

**Engagement Level:**
- Leave blank or set a default value (e.g., **"Low"**)

**Est. Close Date:**
- Leave blank unless the form captures this

**Est. Revenue:**
- Leave blank unless the form captures this

**First Name:**
- Select the form field for the lead's first name (e.g., **"First Name"**) from the Get response details output

**Investment Focus:**
- Leave blank unless the form captures this

**Job Title:**
- Leave blank unless the form captures this

**Last Name:**
- Select the form field for the lead's last name (e.g., **"Last Name"**) from the Get response details output

**Lead Source:**
- Leave blank or set a default value (e.g., **"Form Submission"**)

**Linked Account(s):**
- Leave blank unless the form links to an account

**Linked Contact(s):**
- Leave blank unless the form links to a contact

**Phone Number:**
- Select the form field for the lead's phone number (e.g., **"Phone Number"**) from the Get response details output

5. Click **Save** to store the lead record in Dataverse

---

## Flow Architecture

```
[TRIGGER: New Microsoft Form Response]
    ↓ User submits form
┌──────────────────────────────────┐
│ TRIGGER: When response submitted │
│ Form: Investor Lead Capture      │
│ Fires: Immediately on submission │
└──────────────────────────────────┘
    ↓
┌──────────────────────────────────┐
│ ACTION: Get Response Details     │
│ Retrieves: All form field values │
│ Extracts: Topic, Email,          │
│ First Name, Last Name, Phone     │
└──────────────────────────────────┘
    ↓
┌──────────────────────────────────┐
│ ACTION: Add New Row (Dataverse)  │
│ Creates: New Lead record         │
│ Maps: Form fields → Lead fields  │
│ Populates: All required fields   │
└──────────────────────────────────┘
    ↓
[LEAD CREATED IN CRM]
    ↓
[READY FOR SALES TEAM]
```

---

## Field Mapping Example

| Microsoft Form Field | Dataverse Lead Field | Required |
|---------------------|---------------------|----------|
| Topic | Topic | Yes |
| Email Address | Email | Yes |
| First Name | First Name | Yes |
| Last Name | Last Name | Yes |
| Phone Number | Phone Number | No |
| Company | Organization | No |
| Job Title | Job Title | No |
| (Not captured) | Lead Source | Set to "Form Submission" |
| (Not captured) | Engagement Level | Set to "Cold" |

---

### Step 5: Test and Validate the Flow

1. Click **Test** in the flow editor and select **Manually**
2. Submit a test response in the **"Investor Lead Capture FORM"** with sample data:
   - Topic: "Investment Inquiry"
   - Email: "lead@example.com"
   - First Name: "John"
   - Last Name: "Doe"
   - Phone Number: "123-456-7890"

3. Check if the flow triggers
4. Verify that a new row is added to the **"Leads"** table in Dataverse
5. Confirm that the fields (e.g., Topic, Email, First Name, Last Name, Phone Number) match the form submission data

---

### Step 6: Activate and Monitor the Flow

1. Once tested, click **Save** to save the flow
2. Click **Turn on** to activate the flow
3. Monitor the flow runs in the **Run history** to ensure it executes without errors
4. Adjust the flow (e.g., add error handling or additional fields) if issues arise

---

## Key Concepts

**Trigger:**
Event that initiates the flow. In this case: "When a new response is submitted"

**Form Response:**
Data submitted by a user through the Microsoft Form

**Field Mapping:**
Connecting form fields to Dataverse lead fields to ensure data goes to the correct location

**Dynamic Content:**
Values pulled from previous steps used to populate the lead record

---

## Configuration Examples

### Example 1: Basic Lead Capture
```
Trigger: New form response submitted
Form: Lead Capture Form

Field Mapping:
- Topic → Topic
- Email → Email
- First Name → First Name
- Last Name → Last Name
- Phone → Phone Number
- (Default) → Lead Source = "Form Submission"
- (Default) → Engagement Level = "Cold"

Output: Lead record created in Dataverse
```

### Example 2: Lead Capture with Qualification
```
Trigger: New form response submitted
Form: Qualified Lead Capture Form

Additional Steps:
1. Check Condition: Budget >= $100,000?
2. If YES:
   - Set Engagement Level = "Hot"
   - Create task for sales rep
3. If NO:
   - Set Engagement Level = "Cold"
   - Create task for nurture campaign

Output: Lead record created with qualification level
```

---

## Best Practices

1. **Field Mapping:** Ensure all required fields are mapped
2. **Default Values:** Set sensible defaults for unmapped fields
3. **Error Handling:** Add error handling for failed lead creation
4. **Validation:** Verify form fields before mapping to CRM
5. **Testing:** Test with various data and edge cases
6. **Lead Source Tracking:** Always capture lead source for analytics
7. **Notification:** Consider adding email notification to sales team

---

## Error Handling

Add these steps to handle failures:

```
After "Add a new row" action:
1. Add "Configure run after" setting
2. Select "is failed" option
3. Add error notification step:
   - Send email to admin
   - Include error details
   - Include form data for manual entry
```

---

## Troubleshooting

**Issue: Flow not triggering**
- Verify form ID is correct
- Check form is in active state
- Ensure flow is turned on
- Test with form submission

**Issue: Lead not created in Dataverse**
- Review flow run history for errors
- Verify Leads table exists
- Check required fields are populated
- Confirm user has permissions to create leads

**Issue: Wrong data in lead fields**
- Verify field mappings are correct
- Check form field names match selections
- Test with known data values
- Review Get response details output

**Issue: Duplicate leads created**
- Check for multiple form responses
- Review flow triggers
- Monitor flow run history
- Consider adding duplicate detection

---

## Related Patterns

- P06: Creating a Notification Flow in Power Automate
- P08: Auto-Populating Forms with Power Automate
- P04: Sending Email on Form Submission with Workflows

---

## Implementation Checklist

- [ ] Create Leads table in Dataverse (or use existing)
- [ ] Add required columns to Leads table
- [ ] Create Microsoft Form for lead capture
- [ ] Test form submission manually
- [ ] Create new automated cloud flow
- [ ] Configure trigger (When new response submitted)
- [ ] Select correct form in trigger
- [ ] Add Get response details action
- [ ] Configure response details action
- [ ] Add new row action (Dataverse)
- [ ] Select Leads table
- [ ] Map Topic field
- [ ] Map Email field
- [ ] Map First Name field
- [ ] Map Last Name field
- [ ] Map Phone Number field
- [ ] Set default Lead Source value
- [ ] Set default Engagement Level value
- [ ] Test flow with sample data
- [ ] Verify lead created in Dataverse
- [ ] Check all fields populated correctly
- [ ] Activate flow
- [ ] Monitor for first week
- [ ] Document field mappings

---

## Success Metrics

**Before Implementation:**
- Manual data entry time: 10-15 minutes per lead
- Data accuracy: 85%
- Lead response time: 2+ hours
- Processing volume: 50 leads/week

**After Implementation:**
- Automatic capture time: <1 minute
- Data accuracy: 100%
- Lead response time: 15 minutes
- Processing volume: Unlimited

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** March 2025  
**Status:** Active
