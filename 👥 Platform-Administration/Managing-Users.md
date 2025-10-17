# P10: Managing Users in Power Platform Admin Center

**Category:** Power Platform Administration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** User Access Management

**📥 Original Documentation:** [Download PDF](../pdfs/P10-Managing-Users-Admin.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Platform Admin Center

**Application Use Case:**
- Manage user access, permissions, and security roles in a Power Platform environment

**Reference Usage:** Administering user access in Power Platform environments

---

## Story Behind The Pattern

### The Problem
Managing user access in Power Platform environments can be complex, leading to:
- Unauthorized access to sensitive data
- Insufficient permissions for users
- Inconsistent access controls
- Difficulty tracking user entitlements
- Manual and error-prone permission management

### The Solution
Use the Power Platform Admin Center to add, remove, and manage users, validate their permissions, and assign appropriate security roles.

### Dependencies
- Power Platform Admin Center access for user management
- Permissions to manage environments and security roles

---

## The Pattern: Step-by-Step Implementation

### Step 1: Access the Power Platform Admin Center

1. Log in to the **Power Platform Admin Center** (admin.powerplatform.microsoft.com) using your admin credentials
2. Navigate to the environment where you want to manage users
3. Select the environment from the environment list in the top-right corner
4. Example: **"DQ FACTORY - DFSA - Primary DEV"**

---

### Step 2: View the User List

1. In the selected environment, go to the **Users** section
2. Typically found under the main section or in the sidebar under **"Settings"**
3. Review the list of users who can access data within the environment
4. Check the columns for:
   - **Name:** User's full name
   - **Username:** Login username
   - **Email:** Email address

---

### Step 3: Add a New User

1. Click the **Add user** button to open the pop-up window
2. Enter the user's name or email address in the search field
3. Find them in **Microsoft Entra ID**
4. Verify that the user meets the access requirements:
   - The user must be **enabled in Microsoft Entra ID**
   - The user must be a **member of the environment's security group** (if applicable)

5. Select the user from the search results
6. Click **Add** to include them in the environment
7. Confirm the user appears in the user list with the correct details

---

### Step 4: Remove a User

1. Navigate to the **Groups** list within the environment
2. This may be under **"Settings"** or **"Security"**
3. Select the user you want to remove from the list
4. Follow the prompts to remove the user from the environment
5. Click **"Remove"** and confirm the action
6. Verify the user no longer appears in the user list

---

### Step 5: Validate User Permissions

1. Go to the **App Users** section within the environment
2. This is often found under **"Apps"** or **"Security"**
3. Select a user to check their permissions for specific applications
4. Review the assigned permissions and make adjustments if necessary:
   - Grant access to additional apps
   - Restrict access as needed
5. Save any changes to ensure the user has the correct access

---

### Step 6: Navigate Using the Sidebar

Use the sidebar on the left to access different sections:

- **Home:** Overview of the admin center
- **Environments:** Manage different environments
- **Data:** Manage data integrations
- **Solutions:** View and manage solutions
- **Apps:** Manage applications
- **Analytics:** Access usage and performance reports
- **Settings:** Configure environment settings, including user management

Use these sections to explore additional user management options if needed.

---

### Step 7: Manage Security Roles

1. From the user list, select the user for whom you want to manage security roles
2. Click **Manage security roles** to open the pop-up window
3. Assign the appropriate roles from the list:
   - **"Activity Feeds"** - View activity information
   - **"App End User"** - Use apps
   - **"Analytics Report Author"** - Create reports
   - Other roles specific to your organization

4. Remove any roles that are not required by unchecking them
5. Click **Save** to apply the changes
6. Verify the updated roles by checking the user's permissions in the environment

---

### Step 8: Test and Validate Changes

1. Log in as the user (or use a test account) to confirm they have the expected access and permissions
2. Test specific actions (e.g., accessing an app, viewing data) to ensure the security roles are correctly applied
3. If issues arise, revisit the user's permissions or security roles and adjust as needed

---

## User Management Workflow

```
[NEW USER REQUEST]
    ↓
[ACCESS POWER PLATFORM ADMIN CENTER]
    ↓
[NAVIGATE TO ENVIRONMENT → USERS]
    ↓
[CLICK "ADD USER"]
    ↓
[SEARCH IN MICROSOFT ENTRA ID]
    ├─ User found and enabled?
    ├─ YES → Add to environment
    └─ NO → Request enable in Entra ID
    ↓
[USER ADDED TO ENVIRONMENT]
    ↓
[MANAGE SECURITY ROLES]
    ├─ Select appropriate roles
    └─ Save changes
    ↓
[USER ACCESS PROVISIONED]
    ↓
[TEST AND VALIDATE]
    ├─ Confirm user can access apps
    ├─ Verify permissions correct
    └─ Document access level
    ↓
[USER MANAGEMENT COMPLETE]
```

---

## User Access Levels

### System Administrator
- Full access to all features
- Can manage users and security roles
- Can modify system settings
- Can access all apps and data

### Environment Admin
- Can manage environment
- Can modify settings
- Can manage users within environment
- Limited to specific environment

### App User
- Can access assigned apps
- Can view and edit records based on security roles
- Cannot modify system settings
- Limited to assigned app and data access

### Read-Only User
- Can view data
- Cannot create or edit records
- Used for reporting and analysis
- Cannot modify security settings

---

## Best Practices

1. **Principle of Least Privilege:** Give users only the permissions they need
2. **Regular Audits:** Periodically review user access and permissions
3. **Offboarding Process:** Remove access promptly when users leave
4. **Documentation:** Document user access levels and justification
5. **Security Groups:** Use Microsoft Entra ID groups for batch user management
6. **Role-Based Access:** Assign roles based on job function
7. **Monitor Access:** Track who has access and when

---

## Troubleshooting

**Issue: User not appearing in search**
- Verify user is enabled in Microsoft Entra ID
- Check user is part of the organization
- Confirm user email is correct
- Try searching with different identifier

**Issue: Cannot add user to environment**
- Verify user is enabled in Entra ID
- Check you have admin permissions
- Confirm environment still has user licenses available
- Review security group membership if required

**Issue: User cannot access app after being added**
- Verify user is added to environment
- Check security roles are assigned
- Confirm app is assigned to user
- Test user account directly

**Issue: User has excessive permissions**
- Review assigned security roles
- Remove unnecessary roles
- Assign more restrictive role
- Document reason for access level

---

## User Onboarding Checklist

- [ ] Verify user enabled in Microsoft Entra ID
- [ ] Check user meets access requirements
- [ ] Add user to Power Platform environment
- [ ] Add user to security groups if needed
- [ ] Assign appropriate security roles
- [ ] Assign to specific apps
- [ ] Document access level
- [ ] Notify user of access
- [ ] Test user access
- [ ] Verify all permissions working
- [ ] Provide training if needed

---

## User Offboarding Checklist

- [ ] Document current access level
- [ ] Identify owned records
- [ ] Reassign records as needed
- [ ] Remove from security groups
- [ ] Remove security roles
- [ ] Remove from apps
- [ ] Remove from environment
- [ ] Verify access removed
- [ ] Archive user profile
- [ ] Document offboarding date

---

## Related Patterns

- P14: Managing Security Roles in Power Platform Admin Center
- P12: Managing Business Units in Power Platform Admin Center
- P11: Configuring Dynamics 365 Apps in Power Platform Admin Center

---

## Implementation Checklist

- [ ] Access Power Platform Admin Center
- [ ] Navigate to Users section
- [ ] Create list of users to add
- [ ] Search for each user in Entra ID
- [ ] Verify user is enabled
- [ ] Add user to environment
- [ ] Assign security roles
- [ ] Assign to apps
- [ ] Test access
- [ ] Document access
- [ ] Create regular audit schedule
- [ ] Remove inactive users

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** April 2025  
**Status:** Active
