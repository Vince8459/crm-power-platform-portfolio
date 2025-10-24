# P16: SLA Configuration in Dynamics 365

**Category:** Business Process Automation  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Service Level Agreement Management

**📥 Original Documentation:** [Download PDF](https://github.com/Vince8459/crm-power-platform-portfolio/blob/346bfdae6e550ae6d0ecc9383dae47b360e40f7c/pdf/P16%20SLA%20Configuration.pdf)

---

## Context

**Tag:** Vertical Patterns | D365

**Application Use Case:**
- Configures SLAs to ensure timely resolution of customer support tickets, driving service excellence

**Reference Usage:** Used to track and ensure the timely resolution within predefined SLAs

---

## Story Behind The Pattern

### The Problem
Teams need to be promptly notified of form submissions to avoid delays in follow-up actions and ensure efficient response times:
- No automated tracking of resolution times
- Manual monitoring of SLA compliance
- Difficulty identifying at-risk cases
- Inconsistent response times
- No early warning system for SLA breaches
- Poor visibility into performance metrics

### The Solution
Create a workflow to automatically update the record status and trigger the next steps when a form submission meets specific SLA conditions. This ensures timely resolution and service excellence.

### Dependencies
- Power Apps or Dynamics 365 environment
- Predefined SLA conditions and KPIs tied to the case or process being tracked
- Access to Copilot Service Admin Center

---

## The Pattern: Step-by-Step Implementation

### Step 1: Access the Copilot Service Admin Center

1. Begin by navigating to the **Copilot Service Admin Center** in your system dashboard
2. This is where you will configure your service level agreements (SLAs) and key performance indicators (KPIs)
3. Ensure you have appropriate permissions to manage SLAs

---

### Step 2: Select Service Terms

1. In the Copilot Service Admin Center, find and click on **Service Terms**
2. This will take you to the section where you manage various SLAs, including their KPIs
3. Review existing service terms if any

---

### Step 3: Manage SLA KPIs

1. On the page showing the SLA KPIs, click the **Manage** button
2. This will allow you to add or edit the KPIs that are associated with your SLAs
3. Make sure you are working with the correct KPI section for this configuration

---

### Step 4: Create a New KPI

1. Click the **New** button to create a new KPI
2. This is where you will define the performance metric you want to track
3. In the **Name** field, type:
   - **"Case Resolution KPI"** or another descriptive name for your KPI
   - Use clear, meaningful names that reflect the metric

---

### Step 5: Associate the Correct Entity

1. After naming your KPI, click on the **Entity Name** dropdown to select the entity associated with this KPI
2. This is where you link the KPI to the relevant business process
3. For this configuration, click on **Case** to associate the KPI with the case management process
4. This links your SLA to case records

---

### Step 6: Select the KPI Field

1. In the **KPI Field** section, click on the field dropdown
2. Select the appropriate field that will measure this KPI
3. In this case, choose **Resolve By KPI**, which is typically used for case resolution metrics
4. This field will track when cases should be resolved

---

### Step 7: Set the KPI Start Time

1. Click **Applicable From** to select when this SLA KPI should be triggered
2. Choose **Created On** to start the SLA when a case is created
3. This ensures that the SLA begins counting from the moment the case is logged
4. Alternative options:
   - **Modified On** - Start when case is updated
   - **Custom Date Field** - Start from specific milestone

---

### Step 8: Save and Activate the KPI

1. Once you've configured the KPI settings, click **Save** to store your changes
2. After saving, click **Activate** to make the KPI active and start tracking performance
3. Verify the KPI appears in the active KPIs list

---

### Step 9: Configure the SLA

1. Return to **Service Terms**
2. Click on **Service-level Agreements (SLAs)**
3. This is where you will configure the actual SLA that uses the KPI
4. Click **Manage**
5. Then select **New** to create a new SLA

---

### Step 10: Name and Link the SLA to the Entity

1. In the **Name** field, type:
   - **"Case Resolution SLA"** or another relevant name for your SLA
2. For the **Primary Entity**, click the dropdown and select the **Case** entity
3. This links this SLA to case management activities

---

### Step 11: Add SLA Items

1. Click on the **SLA Items** tab
2. Click **New SLA Item** to add an item to the SLA
3. Name this item:
   - **"Case Resolution SLA"**
4. Associate it with the KPI you previously created:
   - Click on the **KPI** field
   - Select **Case Resolution KPI**
5. This links the SLA item to the performance metric

---

### Step 12: Define SLA Success Conditions

1. In the **Success Conditions** section, click **Applicable When**
2. Define the conditions under which the SLA will be considered successful
3. This might include:
   - Timeframes (e.g., "Case Status = Resolved")
   - Case statuses (e.g., "Resolved within 24 hours")
   - Priority levels (e.g., "High priority resolved in 4 hours")

4. Click **Add Row** to enter multiple success conditions if needed

**Example Success Conditions:**
```
Condition 1: Case Status = Active
Condition 2: Priority = High
Condition 3: Resolve By < Now + 4 hours
```

---

### Step 13: Set Duration for Warnings and Failures

1. Click on **Warn and Fail Duration** to define the time duration
2. Set when the SLA should warn or fail based on the success conditions
3. This ensures the SLA system has clear thresholds for warnings and failures

**Example Configuration:**
- **Warning Time:** 80% of SLA duration
  - For 24-hour SLA: Warning at 19 hours
- **Failure Time:** 100% of SLA duration
  - For 24-hour SLA: Failure at 24 hours

---

### Step 14: Save and Close the SLA

1. Once all the settings are in place, click **Save & Close**
2. This stores the SLA configuration and closes the settings window
3. Review the SLA summary to ensure all settings are correct

---

### Step 15: Activate the SLA

1. After saving the SLA configuration, click **Activate** to enable the SLA
2. A confirmation prompt will appear
3. Click **Activate** again to confirm
4. The SLA is now live and will start tracking performance

---

### Step 16: Set the SLA as Default

1. To ensure this SLA is used by default for new cases, click **Set As Default**
2. This will make the SLA the standard for all future case resolution activities
3. After clicking **Set As Default**, a confirmation prompt will appear
4. Click **OK** to confirm the default setting
5. All new cases will now automatically use this SLA

---

## SLA Configuration Architecture

```
[CASE CREATED]
    ↓
[SLA STARTS]
KPI: Case Resolution KPI
Timer: Starts from Created On
    ↓
[MONITORING PHASE]
    ├─ Time elapsed: 0-80% → Green (On Track)
    ├─ Time elapsed: 80-100% → Yellow (Warning)
    └─ Time elapsed: >100% → Red (Failed)
    ↓
[CASE RESOLVED]
    ├─ Within SLA → Success
    └─ After SLA → Failure
    ↓
[PERFORMANCE TRACKED]
Metrics captured for reporting
```

---

## Key Concepts

**SLA (Service Level Agreement):**
A commitment between a service provider and customer that defines expected service levels and response times.

**KPI (Key Performance Indicator):**
A measurable value that indicates how effectively an organization is achieving key business objectives.

**Warning Threshold:**
The point at which the system alerts users that an SLA is at risk of being breached.

**Failure Threshold:**
The point at which the SLA is considered breached and the case is marked as non-compliant.

**Applicable From:**
The field that determines when the SLA timer starts counting.

---

## SLA Configuration Examples

### Example 1: Priority-Based SLA
```
SLA Name: Priority Case Resolution

KPI: Case Resolution by Priority
Entity: Case
Applicable From: Created On

SLA Items:
1. High Priority Cases
   - Success: Resolved within 4 hours
   - Warning: 3 hours
   - Failure: 4 hours

2. Normal Priority Cases
   - Success: Resolved within 24 hours
   - Warning: 20 hours
   - Failure: 24 hours

3. Low Priority Cases
   - Success: Resolved within 72 hours
   - Warning: 60 hours
   - Failure: 72 hours
```

### Example 2: Customer Tier SLA
```
SLA Name: Customer Tier Response SLA

KPI: First Response Time
Entity: Case
Applicable From: Created On

SLA Items:
1. Premium Customers
   - Success: First response within 1 hour
   - Warning: 45 minutes
   - Failure: 1 hour

2. Standard Customers
   - Success: First response within 8 hours
   - Warning: 6 hours
   - Failure: 8 hours
```

---

## Best Practices

1. **Realistic Timeframes:** Set achievable SLA targets based on historical data
2. **Clear Success Criteria:** Define specific, measurable success conditions
3. **Early Warnings:** Set warning thresholds to allow proactive management
4. **Regular Review:** Periodically review and adjust SLAs based on performance
5. **Communication:** Ensure team understands SLA requirements
6. **Automation:** Link SLAs to automated notifications and escalations
7. **Reporting:** Create dashboards to monitor SLA compliance

---

## SLA Monitoring Dashboard

Create reports to track:
- **SLA Compliance Rate:** % of cases resolved within SLA
- **Average Resolution Time:** Mean time to resolution
- **At-Risk Cases:** Cases approaching SLA breach
- **Failed SLAs:** Cases that exceeded SLA targets
- **By Priority:** SLA performance by case priority
- **By Team:** SLA performance by support team
- **Trends:** SLA performance over time

---

## Troubleshooting

**Issue: SLA not triggering**
- Verify SLA is activated
- Check case meets applicable conditions
- Confirm entity is correctly configured
- Review KPI field mapping

**Issue: Incorrect timing**
- Verify "Applicable From" field is correct
- Check business hours configuration
- Confirm timezone settings
- Review calendar exceptions (holidays)

**Issue: Warnings not appearing**
- Verify warning threshold is set
- Check notification settings
- Confirm user permissions for alerts
- Review workflow configurations

**Issue: Multiple SLAs applying**
- Review SLA applicability conditions
- Check default SLA settings
- Verify priority order of SLAs
- Adjust conditions to avoid overlap

---

## Related Patterns

- P03: Configuring and Deploying Business Process Flow for Case Management
- P04: Sending Email on Form Submission with Workflows
- P06: Creating a Notification Flow in Power Automate

---

## Implementation Checklist

- [ ] Access Copilot Service Admin Center
- [ ] Navigate to Service Terms
- [ ] Click Manage for SLA KPIs
- [ ] Create new KPI
- [ ] Name the KPI appropriately
- [ ] Select entity (Case)
- [ ] Choose KPI field (Resolve By KPI)
- [ ] Set Applicable From (Created On)
- [ ] Save and activate KPI
- [ ] Navigate to SLAs
- [ ] Create new SLA
- [ ] Name the SLA
- [ ] Link to primary entity
- [ ] Add SLA items
- [ ] Associate with KPI
- [ ] Define success conditions
- [ ] Set warn and fail durations
- [ ] Save and close SLA
- [ ] Activate SLA
- [ ] Set as default
- [ ] Test with sample case
- [ ] Monitor SLA performance
- [ ] Create reporting dashboards
- [ ] Train support team
- [ ] Document SLA policies

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** August 2025  
**Status:** Active
