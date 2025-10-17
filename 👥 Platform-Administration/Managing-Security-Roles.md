# P14: Managing Security Roles in Power Platform Admin Center

**Category:** Power Platform Administration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Security Role Management

**📥 Original Documentation:** [Download PDF](../pdfs/P14-Managing-Security-Roles.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Platform Admin Center

**Application Use Case:**
- Manage security roles to control user access and permissions in a Power Platform environment

**Reference Usage:** Administering security roles for data security and compliance in Power Platform

---

## Story Behind The Pattern

### The Problem
Improperly managed security roles can lead to:
- Unauthorized access to sensitive data
- Data breaches from excessive permissions
- Users lacking necessary permissions to perform tasks
- Compliance violations
- Inconsistent access controls
- Security vulnerabilities

### The Solution
Use the Power Platform Admin Center to view, assign, create, and modify security roles, ensuring proper access control and compliance.

### Dependencies
- Power Platform Admin Center access
- Permissions to manage environments and security roles
- Microsoft Entra ID for user authentication

---

## Understanding Security Roles

**Security roles in Power Platform and Dynamics 365** are used to control access to data and resources within an environment. They define:
- What actions users can perform
- What data they can access
- Based on assigned roles

Security roles are essential for:
- Maintaining data security
- Ensuring compliance
- Managing user permissions effectively

---

## The Pattern: Step-by-Step Implementation

### Step 1: Access the Power Platform Admin Center

1. Log in to the Power Platform Admin Center (admin.powerplatform.microsoft.com) using your admin credentials
2. Ensure you have the necessary permissions to manage environments and security roles

---

### Step 2: Navigate to Environments

1. Use the navigation menu on the left sidebar to go to the **Environments** section
2. Select the environment where you want to manage security roles
3. Example: **"DSFA - DEV - REBUILD A"**

---

### Step 3: View Security Roles

1. In the selected environment, go to **Settings** in the sidebar
2. Navigate to **Users + Permissions > Security roles**
3. Review the list of predefined security roles:
   - **System Administrator** - Full access
   - **Environment Maker** - Can create resources
   - **System Customizer** - Can customize system
   - **Other roles** - Custom or pre-defined roles

4. Note the privileges associated with each role to understand their scope

---

### Step 4: Assign Security Roles to Users

1. Navigate to the **Users** section within the environment
2. Under **Users + Permissions**
3. Select a user from the list of users in the environment
4. Click **Manage security roles** to open the pop-up window
5. Assign the appropriate roles to the user by checking
