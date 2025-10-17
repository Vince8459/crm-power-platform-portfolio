# P13: Integrating SharePoint with Dataverse

**Category:** Power Platform Administration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Enterprise Document Management

**📥 Original Documentation:** [Download PDF](https://github.com/Vince8459/crm-power-platform-portfolio/blob/438522706a223af8f363626cf34adda0072b9046/pdf/P13%20Integrating%20SharePoint%20with%20Dataverse.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Platform Admin Center

**Application Use Case:**
- Enable document management by integrating SharePoint with Dataverse

**Reference Usage:** Managing documents in Dataverse using SharePoint integration

---

## Story Behind The Pattern

### The Problem
Storing and managing documents directly in Dataverse can be inefficient, and users need a seamless way to:
- Manage documents using SharePoint while working in Dataverse
- Store files efficiently
- Access documents from multiple locations
- Maintain version control
- Share documents with teams

### The Solution
Integrate SharePoint with Dataverse to enable document management functionality, allowing users to store, access, and manage documents in SharePoint directly from Dataverse.

### Dependencies
- Power Platform Admin Center access
- SharePoint site
- Power Pages site (optional)
- Dataverse environment
- Permissions to manage integrations and table permissions

---

## The Pattern: Step-by-Step Implementation

### Step 1: Access the Power Platform Admin Center

1. Log in to the Power Platform Admin Center (admin.powerplatform.microsoft.com) using your admin credentials
2. Ensure you have the necessary permissions to manage environments, integrations, and table permissions

---

### Step 2: Navigate to Environments

1. Use the navigation menu on the left sidebar to go to the **Environments** section
2. Select the environment where you want to enable SharePoint integration
3. Example: **"DSFA - DEV - REBUILD A"**

---

### Step 3: Enable Document Management Functionality

1. In the selected environment, go to **Settings** in the sidebar
2. Navigate to **Integration > Document Management Settings**
3. Enable the document management functionality for your environment:
   - Toggle the relevant option to **"On"**
   - Or check the box to enable it
4. Save the changes to activate document management capabilities

---

### Step 4: Set Up SharePoint Integration

1. In the Power Platform Admin Center, under **Resources**, choose **Power Pages sites**
2. Select the Power Pages site where you want to enable SharePoint integration
3. Click **Manage** from the main menu to open the site settings
4. On the site information page, locate the **Services** section
5. Toggle the **SharePoint Integration** control to the **Yes** position
6. Confirm the action by clicking **Enable** in the confirmation window that appears
7. Verify that SharePoint integration is now enabled for the site

---

### Step 5: Configure Document Management for Tables

1. Return to the Power Platform Admin Center
2. Go to **Settings > Integration > Document Management Settings**
3. Select the entities (tables) you want to enable for SharePoint document management:
   - **Accounts**
   - **Contacts**
   - **Opportunities**
   - Other relevant tables

4. Enable document management for these entities by checking the boxes next to each table
5. Specify the SharePoint site URL where documents will be stored:
   - Example: **"https://yourcompany.sharepoint.com/sites/DataverseDocs"**

6. Configure the folder structure if prompted:
   - Create folders based on table names
   - Create folders based on record IDs
   - Custom folder structure

7. Save the settings to apply document management to the selected tables

---

### Step 6: Set Up Table Permissions

1. Open **Power Apps** (make.powerapps.com) and navigate to the environment you're working in
2. Go to the **Solutions** area and open the solution containing your tables (or use the default solution)
3. Under **Security Roles**, configure the appropriate permissions for the tables enabled for document management:

**Permission Configuration:**

- Grant **Read, Write, and Append** permissions for the selected tables:
  - **Accounts**
  - **Contacts**
  - etc.

- Ensure **SharePoint Integration** permissions are enabled for relevant roles:
  - **"SharePoint Document Management"** privilege
  - Enable document management actions

4. Assign these security roles to the appropriate **Web Roles** in Power Pages (if using a Power Pages site):
   - Ensure users have access to manage documents
   - Apply to **Authenticated Users** or specific roles

5. Save and publish the changes

---

### Step 7: Test the Integration

1. Open a record in Dataverse (e.g., an Account record) that has document management enabled
2. Navigate to the **Documents** or **Files** tab within the record
3. Upload a test document (e.g., a PDF or Word file)
4. Verify that it is stored in the associated SharePoint site
5. Check the SharePoint site to confirm the document is saved in the correct folder structure
6. Perform actions like downloading, editing, or deleting the document from Dataverse to ensure the integration works seamlessly

---

### Step 8: Monitor and Maintain the Integration

1. Regularly monitor the integration by checking for errors in document uploads or access issues
   - Check Power Platform Admin Center analytics
   - Monitor health dashboards
   - Review error logs

2. Review user feedback to identify any issues:
   - Permission errors
   - Missing documents
   - Access problems

3. Make adjustments as needed:
   - Update SharePoint site URLs
   - Modify table permissions
   - Enable document management for additional tables

4. Ensure SharePoint storage limits and retention policies align with your organization's requirements:
   - Monitor storage usage
   - Archive old documents
   - Set retention policies

---

## Document Management Architecture

```
DATAVERSE RECORD
(Account, Contact, etc.)
        ↓
[Documents Tab]
        ↓
┌────────────────────────┐
│ Document Management UI │
│ - Upload files         │
│ - View files           │
│ - Edit metadata        │
│ - Delete files         │
└────────────────────────┘
        ↓
┌────────────────────────┐
│ SharePoint Integration │
│ - Copy/Move files      │
│ - Version control      │
│ - Permissions sync     │
└────────────────────────┘
        ↓
┌────────────────────────┐
│ SharePoint Site        │
│ /Sites/DataverseDocs/  │
│ ├─ /Accounts/          │
│ ├─ /Contacts/          │
│ ├─ /Opportunities/     │
│ └─ [Document files]    │
└────────────────────────┘
```

---

## Folder Structure Best Practices

### Option A: By Entity Type
```
SharePoint/
├── Accounts/
│   ├── Account-001/
│   ├── Account-002/
│   └── Account-003/
├── Contacts/
│   ├── Contact-001/
│   └── Contact-002/
└── Opportunities/
    └── Opp-001/
```

### Option B: Hierarchical
```
SharePoint/
├── Account-001/
│   ├── Documents/
│   └── Contacts/
│       └── Contact-001/
│           └── Documents/
├── Account-002/
│   └── Documents/
```

### Option C: Custom
```
SharePoint/
├── 2025/
│   ├── Q1/
│   │   └── Documents/
│   └── Q2/
│       └── Documents/
```

---

## Key Concepts

**Document Management:**
System for organizing, storing, and retrieving documents associated with records.

**SharePoint Integration:**
Connection between Dataverse and SharePoint that enables seamless document storage and retrieval.

**Folder Structure:**
Organization of files in SharePoint that aligns with Dataverse record hierarchy.

**File Versioning:**
Automatic tracking of file versions as documents are updated.

**Permissions Sync:**
Automatic synchronization of Dataverse permissions to SharePoint.

---

## Best Practices

1. **Plan Folder Structure:** Design before implementation
2. **Security First:** Configure permissions properly
3. **User Training:** Teach users about new document access
4. **Sync Regularly:** Ensure settings synced across platforms
5. **Monitor:** Track document usage and access patterns
6. **Backup:** Regular backup and archival strategy
7. **Retention:** Set document retention policies

---

## Troubleshooting

**Issue: Documents not uploading**
- Verify SharePoint site is accessible
- Check storage quota not exceeded
- Confirm permissions are correct
- Review error messages in logs

**Issue: Files not appearing in SharePoint**
- Verify integration is enabled
- Check folder structure exists
- Confirm permissions are synchronized
- Wait for sync to complete (can take minutes)

**Issue: Permission errors**
- Verify user has document management permissions
- Check table permissions are configured
- Confirm SharePoint site permissions allow access
- Review security role settings

**Issue: Performance issues**
- Check SharePoint site performance
- Monitor Dataverse limits
- Optimize folder structure
- Consider archiving old documents

---

## Document Management Limits

**SharePoint Limits:**
- File size: 5GB per file (online)
- Storage: Based on tenant subscription
- Retention: Configure based on needs

**Dataverse Limits:**
- Document records: No hard limit
- File integration: Performance dependent on volume
- Storage: Included in tenant allocation

---

## Related Patterns

- P01: Enable File Upload for Subgrid Components
- P06: Configuring a 3D Matrix Setup with Parent-Child Relationship
- P10: Managing Users in Power Platform Admin Center

---

## Implementation Checklist

- [ ] Plan document management strategy
- [ ] Design SharePoint folder structure
- [ ] Create SharePoint site and folders
- [ ] Access Power Platform Admin Center
- [ ] Enable document management
- [ ] Navigate to document management settings
- [ ] Configure SharePoint integration
- [ ] Select tables for document management
- [ ] Enter SharePoint site URL
- [ ] Configure folder mapping
- [ ] Set up security roles
- [ ] Configure table permissions
- [ ] Assign web roles
- [ ] Test document upload
- [ ] Test download and retrieval
- [ ] Test with non-admin user
- [ ] Verify SharePoint folder structure
- [ ] Test version control
- [ ] Set retention policies
- [ ] Document configuration
- [ ] Train users on document access
- [ ] Monitor usage patterns
- [ ] Set up regular backups

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** April 2025  
**Status:** Active
