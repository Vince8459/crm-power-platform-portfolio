# P10: Configuring Scale with Options Form Metadata for Power Pages

**Category:** Power Pages Development & Configuration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Form Metadata Enhancement

**📥 Original Documentation:** [Download PDF](../pdfs/P10-Form-Metadata-Scale-Options.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Apps

**Application Use Case:**
- Enhance form functionality in Power Pages by configuring metadata for multistep or basic forms

**Reference Usage:** Customizing form fields with metadata in Power Pages portals

---

## Story Behind The Pattern

### The Problem
- Native form editing in Power Pages lacks the flexibility to apply advanced behavior modifications
- Cannot apply custom styles (e.g., custom labels, grouped controls)
- Cannot customize form fields without custom code
- Limited options for visual presentation of option sets
- Difficult to group related controls

### The Solution
- Use form metadata to augment or override default field behavior
- Enable custom labels, control styles, and grouped options
- Apply HTML formatting for enhanced presentations
- Organize controls logically without code

### Dependencies
- Power Pages environment
- Portal Management app
- Access to multistep/basic forms
- Permissions to edit metadata

---

## The Pattern: Understanding Metadata

### Why Use Metadata?

Metadata in Power Pages allows additional behavior modification logic to augment or override the native capabilities of form fields. This is particularly useful for:
- Applying custom styles (e.g., vertical or horizontal radio button lists)
- Grouping controls together
- Adding descriptions with HTML
- Customizing labels and placeholders
- Creating visual hierarchies without custom development

---

## Step-by-Step Implementation

### Step 1: Open the Portal Management App

1. Log in to your Power Pages site
2. Navigate to the **Portal Management app** from the Power Pages admin center or by accessing the app directly via the Dynamics 365 environment

---

### Step 2: Locate Your Multistep Form or Basic Form

1. In the Portal Management app, go to the **Content** section
2. Select **Multistep Forms** or **Basic Form** (depending on your form type) from the sitemap
3. Find and select the specific multistep form or basic form you are working with
   - Example: **"Authorised Individual Form"**

---

### Step 3: Access Metadata for Each Step

1. For the selected form, navigate to the **Related** tab
2. Click **Metadata** to view a list of metadata records associated with each step of the multistep form or the entire basic form
3. Each metadata record corresponds to a field or control within the form step

---

### Step 4: Setup Metadata Properties

1. Select a metadata record to view and edit its properties:
   - **Attribute Logical Name:** The internal name of the field being modified (e.g., "new_status")
   - **Label:** A custom label to display for the field (e.g., "Current Status")
   - **Control Style:** The visual style applied to the field (e.g., "Option Set as Vertical Radio Button List" or "Horizontal Radio Button List")
   - **Group Name:** A name to group related controls together (e.g., "StatusGroup" for related option sets)

2. Ensure the associated table column in Dataverse contains the choice values in its option set
   - Verify **"new_status"** has predefined options like **"Active,"** **"Inactive"**

---

### Step 5: Navigate to the Form Metadata Option

1. Within the metadata record, locate the option to set up or edit metadata
2. Click **"Edit"** or **"New Metadata Record"** to open the configuration interface
3. This opens the interface for defining metadata properties

---

### Step 6: Setup the Metadata for the First Option

Configure the first metadata option as follows:

1. **Attribute Logical Name:** Set to the target field (e.g., **"new_status"**)
2. **Label:** Enter a custom label (e.g., **"Select Status"**)
3. **Control Style:** Choose a style (e.g., **"Option Set as Vertical Radio Button List"**)
4. **Group Name:** Assign a group name (e.g., **"StatusGroup"**) to associate related options
5. **Add Description Option:** Enable the description field and add HTML to enhance the display
   - This is useful for scale options or descriptions

---

### Step 7: Add HTML to the Description for Scale with Options

In the description field, insert HTML code to enable a scale or styled options. Example HTML code:

```html
<div class="scale-options">
  <div class="scale-item">
    <span class="scale-label">Not Important</span>
    <input type="radio" name="importance" value="1">
  </div>
  <div class="scale-item">
    <span class="scale-label">Somewhat Important</span>
    <input type="radio" name="importance" value="2">
  </div>
  <div class="scale-item">
    <span class="scale-label">Very Important</span>
    <input type="radio" name="importance" value="3">
  </div>
  <div class="scale-item">
    <span class="scale-label">Critical</span>
    <input type="radio" name="importance" value="4">
  </div>
</div>
```

This HTML creates a vertical radio button list with a description, aligning with the control style.

---

### Step 8: Setup the Second Metadata for the Next Option

1. Add a new metadata record for the next option (e.g., a follow-up field like **"Priority"**)
2. Configure only the **Group Name** (e.g., **"StatusGroup"**) to link it with the first option's configuration
3. Leave other properties (e.g., Label, Control Style) as inherited from the first option unless specific changes are needed

---

### Step 9: Save and Test the Configuration

1. Click **Save** or **Save & Close** in the Portal Management app to apply the metadata changes
2. Navigate to the **Power Pages site**, open the form, and verify that the metadata modifications (e.g., styled radio buttons, grouped controls) are displayed as expected
3. Test each step to ensure the behavior aligns with the intended outcome

---

## Control Style Options

### Radio Button Presentations

**Vertical Radio Button List:**
```
( ) Option 1
( ) Option 2
( ) Option 3
( ) Option 4
```

**Horizontal Radio Button List:**
```
( ) Option 1  ( ) Option 2  ( ) Option 3  ( ) Option 4
```

**Dropdown List:**
```
[Select an option ▼]
- Option 1
- Option 2
- Option 3
```

**Checkboxes (Multiple Selection):**
```
☑ Option 1
☐ Option 2
☐ Option 3
```

---

## Metadata Configuration Examples

### Example 1: Importance Scale
```
Metadata Record 1:
- Type: Attribute
- Attribute Logical Name: new_importance
- Label: How important is this?
- Control Style: Option Set as Vertical Radio Button List
- Group Name: ImportanceGroup

Options:
- Not Important (Value: 1)
- Somewhat Important (Value: 2)
- Very Important (Value: 3)
- Critical (Value: 4)
```

### Example 2: Status with Grouping
```
Metadata Record 1:
- Type: Attribute
- Attribute Logical Name: new_status
- Label: Application Status
- Control Style: Option Set as Vertical Radio Button List
- Group Name: StatusGroup

Metadata Record 2:
- Type: Attribute
- Attribute Logical Name: new_approval_status
- Control Style: [
