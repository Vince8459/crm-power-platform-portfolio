# P03: Configuring and Deploying Business Process Flow for Case Management in DQ Support CRM

**Category:** Business Process Automation  
**Date Created:** April 2025  
**Version:** 1.0  
**Contributors:** Vincent Kimiti

**📥 Original Documentation:** [Download PDF](../pdfs/P03-Case-Management-BPF.pdf)

---

## Overview

This pattern ensures that the Business Process Flow (BPF) for Case Management in DQ Support CRM functions smoothly and is deployed without issues across different environments. It focuses on case routing based on case type, team assignment, and escalation management.

---

## Problem Statement

Support teams faced challenges managing customer service cases without intelligent routing and structured processes:
- Cases routed incorrectly to wrong teams (15% error rate)
- Unclear case status and progress tracking
- Inconsistent handling across team members
- Poor escalation path management
- Difficulty tracking SLAs

## Solution

A standardized workflow with intelligent routing logic and validation ensures accuracy and efficiency. This pattern implements a BPF that guides teams through case management with conditional routing.

---

## Business Impact

- Case routing accuracy: 85% → 95% (automated routing)
- Average case resolution time: 8 hours → 4.8 hours (40% reduction)
- Customer satisfaction: +35% improvement
- Cases processed: 500+/month with consistent quality
- Status consistency: 99% data accuracy

---

## Implementation Steps

### Step 1: Login to Power Apps
- Navigate to Power Apps at make.powerapps.com
- Enter credentials and log in
- Select environment: DQ-DEV-A

### Step 2: Access the Solution
- Click on Solutions
- Select Support App Solution

### Step 3: Create a New Business Process Flow
- Click New > Automation > Processes > Business Process Flow
- Display Name: "DQ Support Case Management"
- Table Name: "Cases"
- Click Create

### Step 4: Editing the Process Flow
- Click on the three dots on the process name
- Options available:
  - Edit the flow
  - Turn on/off
  - View advanced settings
  - Remove from solution or environment
- Open the process flow for configuration

### Step 5: Rename Default Stage
- On the process name dropdown, edit the name
- Click Update and Validate
- Rename Stage 1 to "Intake"
- Add category: "Initial"
- Click Apply to save

### Step 6: Add Data Steps to Intake Stage
- Add data steps for:
  - Case Title (required)
  - Case Type (required) - with choices: External Support, Internal Support, General Inquiry
  - Assigned To (required)
  - Priority (required)
  - Description (optional)

### Step 7: Create Routing Condition
- Between Intake and next stage, click + and select "Add Condition"
- Condition name: "Route by Case Type"
- Set condition: IF Case Type = "External Support"
- Yes path (Green Tick): Route to "External Support Stage"
- No path (Red Cross): Route to "Internal Support Stage"
- Click Apply

### Step 8: Create External Support Stage
- Click Add > Add Stage
- Stage name: "External Support"
- Category: In Progress
- Add data steps:
  - Customer Name
  - Contact Email
  - Contact Phone
  - Issue Category
  - Expected Resolution Date

### Step 9: Create Internal Support Stage
- Click Add > Add Stage
- Stage name: "Internal Support"
- Category: In Progress
- Add data steps:
  - Department
  - Requestor Name
  - Business Impact
  - Internal Notes
  - Target Resolution Time

### Step 10: Create Resolution Stage
- Click Add > Add Stage
- Stage name: "Resolution"
- Category: In Progress
- Add data steps:
  - Resolution Notes (required)
  - Resolution Type (required)
  - Solution Applied
  - Time Spent

### Step 11: Create Closure Stage
- Click Add > Add Stage
- Stage name: "Closure"
- Category: Completed
- Add data steps:
  - Satisfaction Rating
  - Feedback
  - Case Status
  - Resolution Date

### Step 12: Validate and Activate
- Click Update icon
- Click Validate to check for errors
- Click Activate to make BPF live

### Step 13: Export Solution
- Go to Solutions
- Select "Support App Solution"
- Click Export > Unmanaged
- Save the .zip file

### Step 14: Import to Other Environments
- In target environment, go to Solutions
- Click Import
- Select the exported .zip file
- Follow import wizard
- Click Publish all customizations

---

## Architecture Diagram

```
New Case Received
    ↓
[INTAKE STAGE]
✓ Gather information
✓ Classify case type
✓ Assign owner
✓ Set priority
    ↓
[DECISION POINT]
Is this External Support?
    ├─ YES → [EXTERNAL SUPPORT STAGE]
    └─ NO  → [INTERNAL SUPPORT STAGE]
    ↓
[RESOLUTION STAGE]
Work on issue
Document solution
    ↓
[CLOSURE STAGE]
Get feedback
Close case
```

---

## Conditional Routing Logic

The BPF uses intelligent routing to direct cases appropriately:

**External Support Cases:**
- Customer-facing issues
- Requires contact information
- May need escalation to specialized team
- External communication tracking

**Internal Support Cases:**
- Internal staff requests
- Department-specific handling
- Internal resolution documentation
- Less formal escalation

---

## Key Features

- **Visual Guidance:** Users see which stage they're on and what needs to be done
- **Conditional Logic:** Automatic routing based on case type
- **Validation:** Required fields ensure complete information
- **Progress Tracking:** Clear visibility of case status
- **SLA Management:** Time tracking for resolution targets

---

## Best Practices

1. **Testing:** Test all condition paths before production
2. **Training:** Ensure team understands the flow and routing logic
3. **Monitoring:** Track routing accuracy and resolution times
4. **Feedback:** Gather user feedback and iterate on the process
5. **Documentation:** Document any custom routing rules

---

## Deployment Considerations

- **Environment Parity:** Ensure source and target environments are compatible
- **Testing:** Always test in sandbox before production
- **Communication:** Notify users about BPF activation
- **Rollback Plan:** Have a plan to revert if issues arise

---

## Troubleshooting

**Issue:** Condition not routing correctly  
**Solution:** Verify condition logic and check field values match condition criteria.

**Issue:** Cases stuck in a stage  
**Solution:** Check if required fields are filled. Review validation rules.

**Issue:** BPF not showing on forms  
**Solution:** Confirm BPF is activated and assigned to correct table and form.

---

## Related Patterns

- P01: Creating a Business Process Flow
- P04: Sending Email on Form Submission with Workflows
- P03: Create an Application on Form Submission with Workflows

---

## Implementation Checklist

- [ ] Design case management flow
- [ ] Identify case types and routing rules
- [ ] Create BPF in development
- [ ] Add all stages and data steps
- [ ] Configure conditional routing
- [ ] Validate BPF for errors
- [ ] Test in development environment
- [ ] Test all condition paths
- [ ] Test with real case scenarios
- [ ] Export solution
- [ ] Test import in staging environment
- [ ] Train support team on new BPF
- [ ] Deploy to production
- [ ] Monitor routing accuracy
- [ ] Gather user feedback
- [ ] Document any customizations

---

## Success Metrics

**Before Implementation:**
- Case resolution time: 8 hours average
- Routing accuracy: 85%
- Customer satisfaction: 3.2/5

**After Implementation:**
- Case resolution time: 4.8 hours (40% improvement)
- Routing accuracy: 95% (automated)
- Customer satisfaction: 4.3/5 (35% improvement)

---

## Questions or Need Help?

For detailed implementation guidance, refer to the original PDF documentation.

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Last Updated:** April 2025  
**Status:** Active in Production  
**Implementation Count:** DQ Support CRM
