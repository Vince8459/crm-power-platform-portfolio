# P01: Enable File Upload for Subgrid Components in Power Pages

**Category:** Power Pages Development & Configuration  
**Version:** 1.0 - Enable file upload via notes option  
**Contributors:** Vincent Kimiti   
**Project:** DFSA INC02 3D Matrix Questions

**📥 Original Documentation:** [Download PDF](https://github.com/Vince8459/crm-power-platform-portfolio/blob/2655f29f9b999e98f03fa72386910c0ff66dd346/pdf/P01%20Enable%20file%20upload%20for%20subgrid%20components.pdf)

---

## Context

**Tag:** Vertical Development | Power Apps | DevOps

**Application Use Case:**
- 3D matrix questions that require file upload option
- Checking in solution to repository for version control

**Reference Usage:** DFSA INC02 3D Matrix Questions

---

## Story Behind The Pattern

### The Problem

**Scenario 1:**
- Field with file attachment types isn't visible unless a record is created
- This is an issue when the user is creating a record in 3D matrix in the create basic form
- The user needs to create the records first and then edit it to be able to upload file
- Or the upload file is excluded from the 3D matrix

### The Solution
- Utilise the note options from the basic form meta data
- Use the note meta data to upload files

### Dependencies
N/A

---

## The Pattern: Implementation Method

### IMPORTANT NOTE:
**Sequence is important!**

---

## Step 1: Enable Attachments for the Table

1. Go to **make.powerapps.com**
2. Select the **3D matrix (subgrid table)**
3. Go to **table properties**
4. Select **Enable Attachment** from the option below
5. **Save and sync**

---

## Step 2: Add Timeline Component to the Basic Form

1. Navigate to the basic form linked to the subgrid table
2. In the form designer, add a new component
3. Select **Timeline component**
4. Configure timeline to display:
   - Notes (Annotations)
   - Activities
   - Attachments
5. Save and publish the form

---

## Step 3: Configure the Note Meta Data

1. Go to the **subgrid basic form**
2. Go to **basic form meta data**
3. Add new meta data:
   - Click **"New basic form metadata"**
4. Update the form with the following values:
   - **Type:** Select **Attribute**
   - **Attribute Logical Name:** Select the Signature column (or file upload field)
   - **Control Style:** Select **Code Component**

5. Ensure the notes options are filled as following, especially the highlighted ones:
   - **Type:** Attribute
   - **Attribute Logical Name:** [Select your file field]
   - **Control Name:** [Leave default or specify]
   - **Control Style:** Code Component

6. **Save and close**

---

## Step 4: On the Basic Form Additional Settings Enable the Following Settings

1. Open the basic form in Power Pages Management
2. Go to **Additional Settings**
3. Enable the following checkboxes:
   - **Enable Attachment Sync**
   - **Allow file uploads in notes**
   - **Show attachments section**
4. Configure attachment settings:
   - Maximum file size
   - Allowed file types
   - Storage location

---

## Step 5: Configure Table Permissions - Subgrid Table Permissions

1. Go to **Power Pages Management**
2. Go to **table permissions** under security
3. Create the **subgrid table permission** from the new option:
   - Click **"New Table Permission"**

4. Fill the following info:
   - **Name:** Mainform_Subgridform
   - **Table Name:** select the subgrid table from the dropdown list
   - **Website:** select the active website from the lookup search
   - **Access Type:** Parent

5. Fill the **Parent Access Type** details:
   - **Parent Table Permission:** the main table that include the main form that has the 3D matrix question
   - **Parent Relationship:** select the column from dropdown list

6. Select the **privileges options** as following:
   - Select all privileges:
     - Read
     - Write
     - Create
     - Delete
     - Append
     - Append To

7. **Save and sync**

---

## Step 6: Configure the Note (Annotation) Permissions

1. Add **new table permission**
2. Fill the following info:
   - **Name:** MainFrom_SubgridFormNotes
   - **Table name:** select **Notes (annotation)** from the dropdown list
   - **Website:** select the target website
   - **Access Type:** Parent

3. Fill the **Parent Access Type Details:**
   - **Parent table permission:** subgrid table
   - **Parent relationship:** select the item from dropdown list

4. Select the **privileges as following:**
   - Select all:
     - Read
     - Write
     - Create
     - Delete
     - Append
     - Append To

5. **Save and sync**

---

## File Upload Implementation Flow

```
User opens 3D Matrix Form
    ↓
Clicks to Create New Child Record
    ↓
Basic CREATE Form Opens
    ↓
[Timeline Component with Notes Section]
    ├─ Shows note input area
    ├─ Provides file attachment button
    └─ Allows inline file uploads
    ↓
User Uploads File via Notes
    ↓
File Stored in Annotations Table
    ↓
Timeline Shows File Attachment
    ↓
Record Created Successfully with File
    ↓
Subgrid Displays Record with Attachment Icon
```

---

## Key Concepts

**Annotations (Notes):**
System table that stores notes and attachments associated with records. Using annotations allows file uploads without needing a separate file attachment field.

**Timeline Component:**
Displays notes, activities, and attachments in chronological order. Provides user-friendly interface for viewing and adding attachments.

**Table Permissions:**
Control who can access what data. Parent-type permissions inherit permissions from parent record.

**Metadata Configuration:**
Defines how form fields are rendered and behave in Power Pages. Control Style "Code Component" enables special functionality.

---

## Configuration Details

### Attachment Metadata Configuration
```
Type: Attribute
Attribute Logical Name: [Your file field - or use notes]
Control Style: Code Component
Required: No
Visible: Yes
```

### Parent Table Permission Structure
```
Main Table (Parent)
    ↓ 1:N Relationship
Subgrid Table (Child)
    ↓ Links to
Notes/Annotations Table (File storage)
```

### Permission Hierarchy
```
MainForm_Subgridform (Parent permission for child table)
    ├─ Privileges: Read, Write, Create, Delete, Append
    └─ Links to
        MainFrom_SubgridFormNotes (Annotation permission)
            └─ Privileges: Read, Write, Create, Delete, Append
```

---

## Best Practices

1. **Enable Table Attachments First:** Must enable attachments on table before configuring in portal
2. **Use Notes for Files:** Notes/Annotations are more reliable than direct file attachment fields
3. **Configure Permissions Correctly:** Ensure both child table and annotation table permissions are set
4. **Test File Types:** Verify allowed file types are appropriate for use case
5. **Set File Size Limits:** Protect storage by setting reasonable file size restrictions
6. **Sequence Matters:** Follow the implementation steps in exact order
7. **Sync After Changes:** Always sync portal configuration after making changes

---

## Troubleshooting

**Issue: File upload button not appearing**
- Verify table attachment is enabled
- Check timeline component is added to form
- Confirm metadata is configured with Code Component style
- Ensure annotation permissions are set correctly

**Issue: Users cannot upload files**
- Review table permissions on subgrid table
- Verify annotation (notes) permissions are configured
- Check "Allow file uploads in notes" is enabled
- Confirm user has append permission on notes table

**Issue: Files not appearing after upload**
- Verify sync was performed after configuration changes
- Check that timeline component is visible on form
- Confirm annotation permissions include read privilege
- Test with different browser to rule out caching

**Issue: "Unable to upload" error**
- Check file size is within limits
- Verify file type is allowed
- Confirm storage quota is not exceeded
- Review Power Pages logs for detailed error

---

## Related Patterns

- P06: Configuring a 3D Matrix Setup with Parent-Child Relationship
- P02: Using Form Metadata to Override Default Value
- P05: Configuring Signature Input in Power Pages

---

## Implementation Checklist

- [ ] Enable attachments on subgrid table
- [ ] Save and sync table changes
- [ ] Add timeline component to basic form
- [ ] Configure timeline to show notes/attachments
- [ ] Create metadata entry for file field
- [ ] Set metadata control style to Code Component
- [ ] Create subgrid table permission
- [ ] Set parent relationship correctly
- [ ] Select all privileges for subgrid permission
- [ ] Save and sync subgrid permission
- [ ] Create annotation table permission
- [ ] Set annotation parent relationship
- [ ] Select all privileges for annotation permission
- [ ] Save and sync annotation permission
- [ ] Enable file upload in basic form settings
- [ ] Test file upload functionality
- [ ] Test with multiple file types
- [ ] Verify files persist after record creation
- [ ] Test with non-admin user account

---

## Useful Resources

- [Microsoft Learn: Configure Notes](https://learn.microsoft.com/en-us/power-pages/configure/configure-notes)

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** January 2025  
**Status:** Active
