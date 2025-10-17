# P06: Configuring a 3D Matrix Setup with Parent-Child Relationship in Dataverse

**Category:** Power Pages Development & Configuration  
**Version:** 1.0 - 3D Matrix Setup  
**Contributors:** Vincent Kimiti  
**Project:** Parent-Child Relationship in Dataverse for Subgrid Setup

**📥 Original Documentation:** [Download PDF](https://github.com/Vince8459/crm-power-platform-portfolio/blob/02f461918075eff780e691107e35e592971ca175/pdf/P06%20Configuring%20a%203D%20Matrix%20Setup%20with%20Parent-Child%20Relationship%20in%20Dataverse.pdf)

---

## Context

**Tag:** Vertical Development

**Application Use Case:**
- Structuring and managing related data across multiple tables in Dataverse

**Reference Usage:** Parent-Child Relationship in Dataverse for Subgrid Setup

---

## Story Behind The Pattern

### The Problem
- Efficiently managing data within a parent-child relationship
- Enabling easy data entry and updates via subgrids
- Complex hierarchical data structures difficult to manage
- User friction with multi-step data entry processes

### The Solution
- Set up a 3D matrix using a child table
- Establish the relationship with the parent
- Create necessary forms
- Configure metadata for efficient record management

### Dependencies
N/A

---

## The Pattern: Step-by-Step Implementation

### Step 1: Create Extra (3D) Table (Child Table)

1. Go to the **Dataverse (Powerpages) portal**
2. Create a **new table** (child table) that will be used to store the related records
3. This table should include the necessary columns to hold the data, including fields for the relationship to the parent table
4. Example child table structure:

```
Table Name: Authorized Individuals (Child Table)
Columns:
- Authorized Individual ID (Primary Key - auto-generated)
- First Name (Single Line Text)
- Last Name (Single Line Text)
- Position (Choice)
- Email (Email)
- Phone (Phone)
- Authorization Date (Date)
- Parent Application (Lookup to parent table)
- Status (Choice: Active/Inactive)
```

---

### Step 2: Create Two Forms in the Child Table (CREATE & UPDATE)

**CREATE Form:**
1. Create a form specifically for adding new child records
2. This form should allow users to create new records in the child table
3. Add necessary fields to capture the details of the child record
4. Keep the form focused on initial data entry
5. Include only essential fields to reduce friction

**UPDATE Form:**
1. Create a form specifically for editing existing child records
2. Include all fields that may need modification
3. Add read-only fields showing audit info (created by, modified date)
4. Provide comprehensive editing capability

---

### Step 3: Create Columns for the Subgrid in the Child Table

1. Add columns in the child table that will be included in the subgrid in the parent table form
2. Ensure that these columns are relevant and contain the correct responding columns/questions related to the parent-child relationship

---

### Step 4: Create Relationship from the Parent Table

1. Set up a **one-to-many relationship (1:N)** from the parent table to the child table
2. Configuration details:
   - **Parent Table:** Applications (or your main entity)
   - **Child Table:** Authorized Individuals
   - **Relationship Name:** application_authorizedindividuals
   - **Relationship Behavior:** Parental (cascade delete recommended)
   - **Lookup Field:** Parent Application (on child table)
   - **Related Entity Navigation:** Yes

3. This relationship allows the child table's records to be linked and displayed in the subgrid on the parent table form

---

### Step 5: Add Sub-grid Component in the Parent Table Form

1. Open the form of the parent table and add the sub-grid component
2. Link the subgrid to the child table using the one-to-many relationship created earlier
3. Choose the columns to display in the subgrid:
   - Ensure they match the ones created in the child table
   - Include fields that respond to the parent's data

---

### Step 6: Create a View for the Subgrid

1. Create a view for the child table records that will be displayed in the subgrid
2. Import this view into the parent table's subgrid component
3. Ensure the relevant columns are included and displayed in the correct order
4. Example view configuration:

```
View Name: Active Authorized Individuals
View Type: Public
Columns:
- First Name
- Last Name
- Position
- Email
- Phone
- Authorization Date

Sort By: Last Name (A-Z)
Filter: Status = Active
```

---

### Step 7: Create Two Basic Forms (PPM) for Subgrid Data Management

**Basic CREATE Form:**
1. Create a form specifically for adding new child records from within the subgrid
2. Name: "AuthorizedIndividual_Create"
3. Table: Authorized Individuals
4. Form: CREATE form (from Step 2)
5. Mode: Insert
6. On Success: Redirect to parent form

**Basic UPDATE Form:**
1. Create a form for updating child records directly within the subgrid
2. Name: "AuthorizedIndividual_Update"
3. Table: Authorized Individuals
4. Form: UPDATE form (from Step 2)
5. Mode: Edit
6. On Success: Redirect to parent form

7. These forms should support actions such as adding new records and updating existing ones

---

### Step 8: Configure Subgrid Metadata with 3 Actions (Create, Edit, Delete)

1. Create metadata for the subgrid that includes the following three actions:

**Action 1 - CREATE:**
- Type: Create
- Allows users to add new child records from within the subgrid
- Basic Form: AuthorizedIndividual_Create
- Button Label: "+ Add Individual"
- Button Style: Primary

**Action 2 - EDIT:**
- Type: Edit
- Enables users to update existing child records within the subgrid
- Basic Form: AuthorizedIndividual_Update
- Button Label: "Edit"
- Button Style: Default

**Action 3 - DELETE:**
- Type: Delete
- Permits users to delete child records from the subgrid
- Confirmation Message: "Are you sure you want to remove this individual?"
- Button Label: "Delete"
- Button Style: Danger

2. Ensure that these actions are properly linked to the corresponding forms

---

### Step 9: Add Web Roles (Admin, Authenticated Users, etc.)

1. Define web roles for users who will interact with the subgrid
2. Examples:
   - Admin
   - Authenticated Users
   - Portal Users
   - Applicants
3. Assign appropriate permissions (e.g., Admin, Authenticated Users) to enable different levels of access to the subgrid data

---

### Step 10: Table Permissions (Read, Write, Append, Delete)

1. Configure table permissions to control user access to the child table records
2. Ensure that the appropriate roles (Admin, Users, etc.) have the following permissions on both the child and parent tables:
   - **Read:** Can view records
   - **Write:** Can edit records
   - **Create:** Can create new records
   - **Delete:** Can delete records
   - **Append:** Can link records
   - **Append To:** Can be linked from other records

3. This allows users to interact with the data as needed

---

## 3D Matrix Architecture

```
PARENT FORM (Application)
┌─────────────────────────────────────────────────────────┐
│ Application Details                                     │
│ - Company Name: [ABC Corporation]                      │
│ - Registration Number: [12345]                         │
│ - Status: [Pending Review]                             │
├─────────────────────────────────────────────────────────┤
│ ▼ Authorized Individuals (Subgrid - 3D Matrix)         │
│ ┌───────────────────────────────────────────────────┐  │
│ │ [+ Add Individual] Button                         │  │
│ ├───────┬────────┬──────────┬──────────┬──────────┤  │
│ │ First │ Last   │ Position │  Email   │  Phone   │  │
│ ├───────┼────────┼──────────┼──────────┼──────────┤  │
│ │ John  │ Doe    │ Director │ j@x.com  │555-0100  │  │
│ │ [Edit] [Delete]                                  │  │
│ ├───────┼────────┼──────────┼──────────┼──────────┤  │
│ │ Jane  │ Smith  │ CFO      │ js@x.com │555-0200  │  │
│ │ [Edit] [Delete]                                  │  │
│ └───────────────────────────────────────────────────┘  │
│ Inline Create/Edit/Delete - No navigation needed      │
└─────────────────────────────────────────────────────────┘

RELATIONSHIP STRUCTURE:
┌──────────────────┐
│  Application     │ (Parent - 1)
│  (Parent Table)  │
└────────┬─────────┘
         │ 1:N Relationship
         │ (One-to-Many)
         ↓
┌──────────────────────┐
│ Authorized           │ (Child - Many)
│ Individuals          │
│ (Child Table)        │
│ - Record 1           │
│ - Record 2           │
│ - Record 3           │
│ - Record N...        │
└──────────────────────┘
```

---

## Key Concepts

**Parent-Child Relationship:**
One-to-many relationship where one parent record can have multiple child records. Child records are linked to and dependent on the parent record.

**Subgrid (3D Matrix):**
Form component that displays child records in a table-like grid within the parent form. Allows inline viewing, creating, editing, and deleting of child records.

**Metadata Configuration:**
Defines how form components behave and appear in Power Pages. Metadata can configure subgrid actions and button behavior.

**Web Roles:**
Define user groups with specific permissions in Power Pages. Control what data users can access and modify.

**Table Permissions:**
Dataverse security feature that controls record-level access. Defines which roles can read, write, create, delete records.

---

## Implementation Flow

```
1. Create Child Table
   ↓
2. Create Forms (Create & Update)
   ↓
3. Create Relationship (1:N)
   ↓
4. Add Subgrid to Parent Form
   ↓
5. Create/Assign View
   ↓
6. Create Basic Forms (Portal versions)
   ↓
7. Configure Subgrid Metadata
   ├─ Create action
   ├─ Edit action
   └─ Delete action
   ↓
8. Define Web Roles
   ↓
9. Configure Table Permissions
   ├─ Parent table permissions
   └─ Child table permissions (Parent type)
   ↓
10. Test All Operations
    ├─ Create child record
    ├─ Edit child record
    ├─ Delete child record
    └─ Verify data persistence
```

---

## Best Practices

1. **Proper Relationship Configuration:** Use Parental relationship behavior for cascade operations
2. **Security First:** Configure table permissions BEFORE testing portal
3. **Form Optimization:** Keep CREATE form minimal, UPDATE form comprehensive
4. **Metadata Accuracy:** Subgrid metadata configuration is critical
5. **Testing Sequence:** Test in this order: Model-app → Portal (anonymous) → Portal (authenticated)
6. **Permission Levels:** Ensure both parent and child table permissions are configured
7. **User Experience:** Minimize navigation; enable inline operations

---

## Common Issues and Solutions

**Issue: Subgrid not showing child records**
- Verify parent-child relationship is correctly configured
- Check view is assigned to subgrid
- Confirm permissions allow viewing child records
- Ensure child records exist and are linked to parent

**Issue: Create/Edit buttons not working**
- Verify basic forms are correctly configured
- Check web roles have appropriate permissions
- Confirm table permissions include create/write privilege
- Verify form metadata is properly configured

**Issue: Delete not working**
- Check "Delete" privilege is enabled in permissions
- Verify relationship allows cascading deletes
- Confirm user role has delete permission
- Test with admin account to rule out permission issue

**Issue: Changes not visible after update**
- Refresh the form/portal page
- Clear browser cache
- Verify record was actually updated in database
- Check user has appropriate read permissions

---

## Related Patterns

- P01: Enable File Upload for Subgrid Components
- P02: Using Form Metadata to Override Default Value
- P11: Enable Field Validation Using Regular Expressions

---

## Implementation Checklist

- [ ] Design child table structure
- [ ] Create child table in Dataverse
- [ ] Add all necessary columns to child table
- [ ] Create CREATE form for child records
- [ ] Create UPDATE form for child records
- [ ] Create 1:N relationship to parent table
- [ ] Add subgrid component to parent form
- [ ] Create view for subgrid display
- [ ] Create basic CREATE form for portal
- [ ] Create basic UPDATE form for portal
- [ ] Configure subgrid metadata
- [ ] Add create action to metadata
- [ ] Add edit action to metadata
- [ ] Add delete action to metadata
- [ ] Define web roles
- [ ] Configure parent table permissions
- [ ] Configure child table permissions (Parent type)
- [ ] Set all required privileges
- [ ] Test create functionality
- [ ] Test edit functionality
- [ ] Test delete functionality
- [ ] Test with non-admin user
- [ ] Verify cascade delete behavior
- [ ] Document configuration

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** February 2025  
**Status:** Active
