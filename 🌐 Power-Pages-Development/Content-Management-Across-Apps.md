# P09: Managing Page Content Across Power Pages with Model-Driven Apps

**Category:** Power Pages Development & Configuration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Cross-Platform Content Management

**📥 Original Documentation:** [Download PDF](../pdfs/P09-Content-Management-Across-Apps.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Apps

**Application Use Case:**
- Manage and publish page content from a Model-Driven App to Power Pages using Dataverse and Liquid Code

**Reference Usage:** Integrating Model-Driven Apps, Power Pages, and Dataverse for content management

---

## Story Behind The Pattern

### The Problem
- Manually updating and synchronizing page content across Model-Driven Apps and Power Pages is inefficient and prone to errors
- Content changes require manual updates in multiple places
- Difficult to maintain consistency
- No centralized content management
- Risk of outdated information on portal

### The Solution
- Use a Model-Driven App to update page content stored in Dataverse
- Publish changes to Power Pages
- Leverage Liquid Code to dynamically read and update the Power Pages site
- Centralize content management for consistency

### Dependencies
- Power Apps environment
- Dataverse
- Power Pages site
- Liquid Code configuration
- Permissions to create and manage apps and pages

---

## The Pattern: Step-by-Step Implementation

### Step 1: Create or Configure the Model-Driven App

1. Log in to your Power Apps environment (make.powerapps.com)
2. Navigate to the **Solutions** area in the Power Apps Maker Portal
3. Create a new Model-Driven App or open an existing one (e.g., **"Content Management App"**)
4. Add a form or page where users can update page content:
   - Add a **"Page Content"** field or a rich text editor
   - Include fields for page title, content, last updated, author

---

### Step 2: Define the Dataverse Table for Page Content

1. In the same solution, create or select a Dataverse table to store page content
   - Name: **"Page Content"** table
2. Add necessary columns:
   - **"Page Title"** (Text) - Name of the page
   - **"Page Content"** (Multiple Lines of Text) - The actual page content
   - **"Last Updated"** (DateTime) - When content was last modified
   - **"Author"** (Text) - Who created/modified the content
   - **"Status"** (Choice) - Active/Inactive/Draft
   - **"Page Name"** (Text) - Internal page identifier
3. Save and publish the table to make it available for the Model-Driven App

---

### Step 3: Link the Model-Driven App to the Dataverse Table

1. In the Model-Driven App designer, add the **"Page Content"** table to the app
2. Configure the form to display and allow edits to:
   - **"Page Title"** field
   - **"Page Content"** field
   - **"Status"** field
   - Other relevant fields

3. Save and publish the app to enable users to update page content, which will be stored in Dataverse

---

### Step 4: Set Up the Power Pages Site

1. Navigate to the **Power Pages** section in the Power Apps Maker Portal
2. Create a new Power Pages site or open an existing one (e.g., **"Company Website"**)
3. Add a page (e.g., **"Dynamic Content Page"**) where the content will be displayed

---

### Step 5: Configure Liquid Code to Read from Dataverse

1. In the Power Pages site editor, access the page's web template or code editor
2. Add **Liquid Code** to dynamically fetch content from Dataverse
3. Example Liquid Code:

```liquid
{% fetchxml query %}
  <fetch version="1.0" output-format="xml-platform" mapping="logical" distinct="false">
    <entity name="page_content">
      <attribute name="page_title"/>
      <attribute name="page_content"/>
      <attribute name="last_updated"/>
      <filter type="and">
        <condition attribute="page_name" operator="eq" value="homepage"/>
        <condition attribute="statecode" operator="eq" value="0"/>
      </filter>
      <order attribute="created_on" descending="true"/>
    </entity>
  </fetch>
{% endfetchxml %}

{% if query.results.entities.size > 0 %}
  {% assign page = query.results.entities[0] %}
  <h1>{{ page.page_title }}</h1>
  <div class="content">
    {{ page.page_content }}
  </div>
  <p class="meta">Last updated: {{ page.last_updated | date: "%B %d, %Y" }}</p>
{% else %}
  <p>No content available</p>
{% endif %}
```

This code:
- Retrieves the latest page content from Dataverse
- Displays page title and content
- Shows last updated timestamp
- Handles case when no content exists

---

### Step 6: Publish Changes from the Model-Driven App to Dataverse

1. As a user, open the **Model-Driven App**
2. Navigate to the form for the **"Page Content"** table
3. Update the **"Page Title"** and **"Page Content"** fields with new information:
   - Example: **"Welcome Page"** and a paragraph of text
4. Set **"Status"** to **"Active"** to publish the content
5. Click **Save** to save the record
6. The changes are automatically stored in **Dataverse**

---

### Step 7: Publish Changes from Power Pages to the Power Pages Site

1. In the **Power Pages editor**, click **Sync Configuration** to ensure the site reflects the Dataverse data
2. Click **Publish** to make the updated page content live on the Power Pages site
3. Verify that the changes (e.g., the new **"Welcome Page"** content) appear on the published site

---

### Step 8: Test the End-to-End Workflow

1. As a user, update the page content again in the **Model-Driven App**
2. Check **Dataverse** to confirm the update is stored
3. Refresh the **Power Pages** site and verify that **Liquid Code** has updated the content dynamically
4. Ensure no errors occur during the process (e.g., connectivity issues or permission errors)

---

## Content Management Architecture

```
MODEL-DRIVEN APP (Content Editor)
┌──────────────────────────────┐
│ Content Management Interface │
│ - Page Title input           │
│ - Rich Text Editor           │
│ - Status selector            │
│ - Save Button                │
└──────────────────────────────┘
         │ User updates content
         ↓
┌──────────────────────────────┐
│ DATAVERSE                    │
│ Page Content Table           │
│ - Page Title                 │
│ - Page Content               │
│ - Last Updated               │
│ - Author                     │
│ - Status (Active/Inactive)   │
└──────────────────────────────┘
         │ Data stored
         ↓
┌──────────────────────────────┐
│ POWER PAGES                  │
│ Dynamic Content Page         │
│ [Liquid Code reads from DB]  │
│                              │
│ ┌─────────────────────────┐  │
│ │ Welcome Page            │  │
│ │ (Content from Dataverse)│  │
│ │                         │  │
│ │ [Page content displays] │  │
│ │ Last updated: [Date]    │  │
│ └─────────────────────────┘  │
└──────────────────────────────┘
         │ Visitors view
         ↓
   END USERS
```

---

## Key Concepts

**FetchXML:**
Dataverse query language used in Liquid Code to retrieve data. Defines what records and fields to fetch.

**Liquid Code:**
Template language used in Power Pages to dynamically render content. Can query Dataverse and include conditional logic.

**Model-Driven App:**
Desktop application interface for managing data with forms and views. Used by content editors to manage page content.

**Power Pages:**
Low-code platform for creating dynamic portals. Displays content stored in Dataverse using Liquid templates.

**Synchronization:**
Process of updating Power Pages configuration to reflect changes in Dataverse or code.

---

## Advanced Liquid Code Examples

### Example 1: Multiple Page Sections
```liquid
{% fetchxml query %}
  <fetch version="1.0" output-format="xml-platform" mapping="logical">
    <entity name="page_content">
      <attribute name="page_title"/>
      <attribute name="page_content"/>
      <attribute name="section_order"/>
      <filter type="and">
        <condition attribute="status" operator="eq" value="Active"/>
      </filter>
      <order attribute="section_order" descending="false"/>
    </entity>
  </fetch>
{% endfetchxml %}

{% for page in query.results.entities %}
  <section class="content-section">
    <h2>{{ page.page_title }}</h2>
    <div class="content">{{ page.page_content }}</div>
  </section>
{% endfor %}
```

### Example 2: Conditional Content Display
```liquid
{% if user.loggedin %}
  <!-- Show authenticated user content -->
  {% fetchxml query %}
    <fetch version="1.0">
      <entity name="page_content">
        <attribute name="page_content"/>
        <filter type="and">
          <condition attribute="visibility" operator="eq" value="authenticated"/>
        </filter>
      </entity>
    </fetch>
  {% endfetchxml %}
  {{ query.results.entities[0].page_content }}
{% else %}
  <!-- Show public content -->
  <p>Please log in to view this content</p>
{% endif %}
```

---

## Best Practices

1. **Version Control:** Track content changes and maintain history
2. **Approval Workflow:** Require approval before publishing to production
3. **Caching Strategy:** Consider caching content for performance
4. **Error Handling:** Include fallback content if query fails
5. **Permissions:** Restrict content editing to authorized users
6. **Audit Trail:** Log who made changes and when
7. **Testing:** Test content changes in staging before production

---

## Troubleshooting

**Issue: Content not displaying on portal**
- Verify FetchXML query is correct
- Check Dataverse records exist and are active
- Ensure Liquid Code is properly formatted
- Confirm user has read permissions on content table
- Review portal logs for query errors

**Issue: Old content still showing**
- Click "Sync Configuration" in Power Pages
- Clear browser cache
- Verify new content status is "Active"
- Check FetchXML filter conditions
- Wait 5-10 minutes for changes to propagate

**Issue: Permission errors**
- Verify user/role has read access to page content table
- Check table permissions in security settings
- Confirm user is assigned appropriate web role
- Test with admin account to verify issue is permission-related

**Issue: Liquid Code syntax errors**
- Validate FetchXML structure
- Check variable names are correct
- Verify filter conditions use proper operators
- Test query in Dataverse explorer first
- Review Power Pages error logs

---

## Related Patterns

- P09: Managing Page Content Across Model-Driven Apps and Power Pages
- P06: Configuring a 3D Matrix Setup with Parent-Child Relationship
- P11: Enable Field Validation Using Regular Expressions

---

## Implementation Checklist

- [ ] Create Dataverse table for page content
- [ ] Add required columns (Title, Content, Last Updated, etc.)
- [ ] Create or configure Model-Driven App
- [ ] Add Page Content table to app
- [ ] Create form for content editing
- [ ] Publish app
- [ ] Create or open Power Pages site
- [ ] Create page for dynamic content
- [ ] Write Liquid Code query (FetchXML)
- [ ] Test Liquid Code syntax
- [ ] Implement error handling in Liquid
- [ ] Add code to page template
- [ ] Test content retrieval
- [ ] Create sample content records in Dataverse
- [ ] Verify content displays on portal
- [ ] Test content updates workflow
- [ ] Verify updates appear on portal
- [ ] Test with different content status values
- [ ] Document Liquid Code for team
- [ ] Set up content approval process (optional)

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** March 2025  
**Status:** Active
