# P12: Managing Business Units in Power Platform Admin Center

**Category:** Power Platform Administration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Business Unit Organization

**📥 Original Documentation:** [Download PDF](https://github.com/Vince8459/crm-power-platform-portfolio/blob/8bb0cc3e2b4f383a74e8d5d641e659bd4932c5c3/pdf/P12%20Managing%20Business%20Units%20in%20Power%20Platform%20Admin%20Center.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Platform Admin Center

**Application Use Case:**
- Manage business units in a Power Platform environment to organize teams and permissions

**Reference Usage:** Structuring organizational hierarchy in Power Platform environments

---

## Story Behind The Pattern

### The Problem
Without proper business unit management, organizing teams, permissions, and data access in Power Platform can lead to:
- Inefficiencies in data access management
- Security issues from improper data segregation
- Unclear organizational hierarchy
- Difficult permission management
- Data access confusion

### The Solution
Use the Power Platform Admin Center to view, create, and manage business units, ensuring proper organizational structure and access control.

### Dependencies
- Power Platform Admin Center access
- Permissions to manage environments and business units

---

## Understanding Business Units

**Business Units in Power Platform and Dynamics 365** are organizational structures that help manage and segment data, security, and processes within an organization. They represent different parts of an organization, such as:
- Departments
- Divisions
- Teams
- Geographic regions
- Product lines

They are used to control access to data and functionality based on organizational hierarchy.

---

## The Pattern: Step-by-Step Implementation

### Step 1: Access the Power Platform Admin Center

1. Log in to the Power Platform Admin Center (admin.powerplatform.microsoft.com) using your admin credentials
2. Ensure you have the necessary permissions to manage environments and business units

---

### Step 2: Navigate to Environments

1. Use the navigation menu on the left sidebar to go to the **Environments** section
2. Select the desired environment from the list in the main section
3. Example: **"DSFA - DEV - REBUILD A"**

---

### Step 3: View Environment Details

Review the detailed information about the selected environment to confirm you're working in the correct context:

- **Environment URL:** The URL for accessing the environment
- **Region:** The geographic region of the environment
- **Type:** The type of environment (e.g., Sandbox, Production)
- **Organization ID:** The unique ID for the organization
- **Environment ID:** The unique ID for the environment
- **State:** The current state (e.g., Ready, Provisioning)
- **Refresh cadence:** The data refresh schedule (e.g., None, Daily)
- **Security group:** The associated security group (if any)
- **Creator name:** The name of the user who created the environment

Confirm the environment aligns with your business unit management needs.

---

### Step 4: Navigate to Settings

1. Use the navigation menu on the left sidebar to go to the **Settings** section
2. Under the Settings section, select **Business Units** to access business unit management

---

### Step 5: View Active Business Units

1. In the Business Units section, use the dropdown menu labeled **Active Business Units** to view the list of active business units
2. Review the table below the dropdown, which displays columns such as:
   - **Name:** The name of the business unit
   - **Applications:** The applications associated with the business unit
   - **Main Phone:** The primary contact number
   - **Website:** The website URL for the business unit

3. Identify existing business units and their details to understand the current organizational structure

---

### Step 6: Create a New Business Unit

1. Click **Create new business unit** to open the creation form
2. Fill out the required fields in the form:

**Field Details:**

- **Name:** Enter the name of the new business unit
  - Example: "Sales Division A"
  
- **Division:** Specify the division the business unit belongs to
  - Example: "Sales"
  
- **Parent business unit:** This field is pre-filled with the ID of the parent business unit
  - Example: "org7f29b3e1"
  - Adjust if necessary by selecting a different parent unit
  - This creates organizational hierarchy
  
- **Website:** Enter the website URL
  - Example: "https://example.com"
  
- **Main phone:** Provide the main phone number
  - Example: "+1-555-123-4567"
  
- **Other phone:** Provide an additional phone number if applicable
  - Example: "+1-555-987-6543"
  
- **Email:** Enter the email address for the business unit
  - Example: "salesdivision@example.com"

3. Double-check all fields for accuracy

---

### Step 7: Save the New Business Unit

1. Click **Save** to create the new business unit and add it to the environment
2. Verify that the new business unit appears in the Active Business Units list with the correct details
3. If you need to cancel the creation process, click **Cancel** to discard changes

---

### Step 8: Test and Validate the Business Unit

1. Assign users to the new business unit
   - Refer to the "Managing Users in Power Platform Admin Center" solution pattern for user assignment steps
2. Test data access and permissions for users in the new business unit to ensure proper segregation
   - Example: Users in "Sales Division A" can only access relevant records
3. If issues arise, revisit the business unit settings or parent unit assignment to troubleshoot

---

## Business Unit Hierarchy

### Organizational Structure Example

```
Root Business Unit (Organization)
│
├── Sales Division
│   ├── Sales Division A
│   │   ├── Sales Team A1
│   │   └── Sales Team A2
│   └── Sales Division B
│       ├── Sales Team B1
│       └── Sales Team B2
│
├── Support Division
│   ├── Tier 1 Support
│   └── Tier 2 Support
│
└── Administration Division
    ├── HR Department
    └── Finance Department
```

### Access Control by Business Unit

```
User: John Smith (Sales Division A)
Access Level:
- Can view: Sales Division A records
- Can view: Assigned records
- Cannot view: Sales Division B records
- Cannot view: Support Division records
```

---

## Key Concepts

**Parent Business Unit:**
The higher-level business unit in the organizational hierarchy. Child units inherit some properties from parent units.

**Business Unit Hierarchy:**
The tree structure of business units organized by parent-child relationships.

**Data Segregation:**
Limiting user access to data based on business unit assignment. Each user can only access their business unit's data (with some exceptions for managers).

**Security Role:**
Controls what actions users can perform. Combined with business unit assignment for granular access control.

---

## Best Practices

1. **Align with Organization:** Structure business units to match your organization's structure
2. **Clear Hierarchy:** Establish clear parent-child relationships
3. **Meaningful Names:** Use descriptive names for easy identification
4. **Contact Information:** Keep contact details up to date
5. **Regular Review:** Periodically review and update business unit structure
6. **Document Structure:** Document your business unit hierarchy for reference
7. **Consistent Naming:** Use naming conventions for consistency

---

## Business Unit Configuration Examples

### Example 1: Sales Organization
```
Root Business Unit
├── Sales North
│   ├── Boston Office
│   └── New York Office
├── Sales South
│   ├── Atlanta Office
│   └── Miami Office
└── Sales West
    ├── Los Angeles Office
    └── San Francisco Office
```

### Example 2: Service Organization
```
Root Business Unit
├── Premium Services
│   ├── Enterprise Team
│   └── Mid-Market Team
└── Standard Services
    ├── Small Business Team
    └── Self-Service Team
```

---

## Troubleshooting

**Issue: Cannot create business unit**
- Verify you have admin permissions
- Check environment is in proper state (Ready)
- Ensure parent business unit exists
- Review error message for specific issue

**Issue: Business unit not appearing in list**
- Refresh the page
- Verify business unit was saved successfully
- Check dropdown filters
- Try searching for the business unit

**Issue: Users cannot access expected records**
- Verify user is assigned to correct business unit
- Check security roles are appropriate
- Confirm data ownership is correct
- Review access level settings

**Issue: Cannot delete business unit**
- Ensure no users are assigned to it
- Remove from parent unit first if needed
- Verify it's not the root business unit
- Check for dependent configurations

---

## Business Unit Maintenance Checklist

- [ ] Review current business unit structure
- [ ] Document organizational changes
- [ ] Create new business units for new departments
- [ ] Adjust parent-child relationships as needed
- [ ] Update contact information
- [ ] Assign users to appropriate units
- [ ] Test data access permissions
- [ ] Document business unit assignments
- [ ] Archive unused business units
- [ ] Perform quarterly review

---

## Related Patterns

- P10: Managing Users in Power Platform Admin Center
- P14: Managing Security Roles in Power Platform Admin Center
- P15: Managing Connection Roles in Power Platform and Dynamics 365

---

## Implementation Checklist

- [ ] Access Power Platform Admin Center
- [ ] Navigate to Environments
- [ ] Select target environment
- [ ] Go to Settings → Business Units
- [ ] Review current business units
- [ ] Design new business unit structure
- [ ] Identify parent business units
- [ ] Plan hierarchy levels
- [ ] Click "Create new business unit"
- [ ] Fill in business unit name
- [ ] Select parent business unit
- [ ] Enter division
- [ ] Add contact information
- [ ] Complete all fields
- [ ] Save business unit
- [ ] Verify in active list
- [ ] Assign users to unit
- [ ] Test data access
- [ ] Document structure
- [ ] Communicate to organization

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** April 2025  
**Status:** Active
