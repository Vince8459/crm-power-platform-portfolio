# P02: Using Form Metadata to Override Default Value in Power Pages

**Category:** Power Pages Development & Configuration  
**Version:** 1.0 - Remove default value on Yes/No fields  
**Contributors:** Vincent Kimiti   
**Project:** DFSA INC02 Fit and Proper Yes/No Questions

**📥 Original Documentation:** [Download PDF](../pdfs/P02-Form-Metadata-Override-Default.pdf)

---

## Context

**Tag:** Vertical Development | Power Apps | Power Pages

**Application Use Case:**
- Questions with type of Yes/No Answer
- Regulatory compliance forms requiring explicit user selection

**Reference Usage:** DFSA INC02 Fit and Proper Yes/No Questions

---

## Story Behind The Pattern

### The Problem

**Scenario 1:**
- Field with Yes/No type would have one of the choices selected by default
- This might lead to user not really selecting the right answer as it's already selected by default
- Users may miss critical compliance questions
- Incomplete or incorrect regulatory submissions

### The Solution
- Utilise metadata to override default value
- Force users to make explicit selection rather than accepting defaults
- Ensure intentional choice for critical questions

### Dependencies
N/A

---

## The Pattern: Step-by-Step Implementation

### Step 1: Go to Power Pages Management

1. Navigate to the **Power Pages Management** app
2. Ensure you are in the correct environment

---

### Step 2: Select the Multistep Form That Include the Questions

1. In Power Pages Management, navigate to **Multistep Forms** or **Basic Forms**
2. Find and select the form that contains your Yes/No questions
3. Open the form record

---

### Step 3: Go to the Step That Has the Question

1. Scroll down to the **Related** section
2. Look for **Form Steps** or **Steps** related records
3. Select the specific step that contains the Yes/No question you want to modify

---

### Step 4: Select Metadata from Related Option

1. In the step record, look for the **Related** section
2. Click on **Metadata**
3. This will show all metadata records associated with this step

---

### Step 5: Select the Type "Attribute"

1. In the metadata list, you may see multiple metadata records
2. Click **New** to create a new metadata record
3. Or edit an existing one if it already exists
4. In the metadata form, select:
   - **Type:** Attribute

---

### Step 6: Select the Question That You Want to Remove Its Default Value

1. In the metadata record, locate the field:
   - **Attribute Logical Name**
2. Click the dropdown and select the Yes/No field (question) you want to modify
3. This is the field where you want to ignore/override the default value

---

### Step 7: Select Override the Default Value from Prepopulate Field

1. In the same metadata record, find:
   - **Prepopulate Field** section
2. Look for the option to **override** or **ignore** the default value
3. Select: **Ignore default value**
4. This tells Power Pages to not use the system default value

---

### Step 8: Save and Close; New Metadata is Added

1. Click **Save** to save the metadata changes
2. Click **Close** to exit the metadata record
3. The new metadata entry will appear in the list

---

### Step 9: Save and Publish from make.powerapps.com

1. Navigate to **make.powerapps.com**
2. Go to **Solutions**
3. Find the solution containing your Power Pages
4. Go to the Portal Management model-driven app if needed
5. Publish all customizations:
   - Click **Publish All Customizations**

---

### Step 10: Confirm the Changes Have Applied

1. Navigate to your **Power Pages portal**
2. Open the form
3. Navigate to the step with the Yes/No question
4. Verify that:
   - No default option is selected
   - User must explicitly choose Yes or No
   - The field appears blank/unselected initially

---

## Metadata Configuration Details

### Metadata Record Structure
```
Type: Attribute
Attribute Logical Name: [Your Yes/No field]
Label: [Display label - optional]
Prepopulate Field: Ignore default value
Visible: Yes
Required: [Set based on business requirement]
```

### Yes/No Field Behavior

**Before Metadata Override:**
```
Yes/No Question: "Are you fit and proper?"
Default: Yes (automatically selected)
User sees: [●] Yes  [  ] No
User can change to: No (but may not notice)
Problem: Default answer suggests expected response
```

**After Metadata Override:**
```
Yes/No Question: "Are you fit and proper?"
Default: None (ignored)
User sees: [  ] Yes  [  ] No
User must: Explicitly select one
Problem solved: User must make intentional choice
```

---

## Implementation Flow

```
Open Power Pages Management
    ↓
Select Multistep Form
    ↓
Select Step with Yes/No Question
    ↓
Create/Edit Metadata Record
    ↓
Set Type: Attribute
    ↓
Select Question Field (Attribute Logical Name)
    ↓
Set: Ignore Default Value
    ↓
Save Metadata
    ↓
Save and Publish Form
    ↓
Sync Portal Configuration
    ↓
Test Form in Portal
    ↓
Verify No Default Selected
```

---

## Key Concepts

**Form Metadata:**
Configuration that augments or overrides default Power Pages behavior for form fields. Metadata allows fine-tuning without code changes.

**Prepopulate Field:**
Setting that controls whether fields are pre-filled with values. "Ignore default value" prevents the system default from being used.

**Yes/No Field:**
Boolean field with two options: Yes (true) or No (false). Default value may not be appropriate for all regulatory contexts.

**Attribute Logical Name:**
Internal name of the field in Dataverse. Must match exactly to apply metadata correctly.

---

## Best Practices

1. **Regulatory Compliance:** Always override defaults for legal/compliance questions
2. **Explicit User Choices:** Force intentional selection for critical questions
3. **Test Thoroughly:** Verify no defaults appear in various browsers
4. **Document Changes:** Note which fields have default values ignored
5. **User Communication:** Inform users they must answer all mandatory questions
6. **Metadata Organization:** Use clear labeling in metadata records
7. **Publish After Changes:** Always publish customizations to apply changes

---

## Use Cases

**When to Use This Pattern:**

1. **Regulatory Forms**
   - Fit and Proper questionnaires
   - Compliance certifications
   - Legal affirmations

2. **Critical Decision Questions**
   - Consent questions
   - Acknowledgment fields
   - Risk acceptance statements

3. **High-Impact Selections**
   - Policy enrollment
   - Termination confirmations
   - Authorization approvals

**When NOT to Use This Pattern:**

1. **Optional Fields** - Where defaults are acceptable
2. **User Preference Fields** - Where defaults improve UX
3. **Low-Risk Questions** - Where default is safe

---

## Troubleshooting

**Issue: Default value still showing**
- Verify metadata "Ignore default value" is set
- Confirm publish all customizations was performed
- Clear browser cache and retry
- Check metadata is associated with correct step

**Issue: Metadata not appearing on form**
- Verify metadata type is set to "Attribute"
- Confirm attribute logical name is correct
- Check field belongs to correct table
- Sync portal configuration

**Issue: Changes not applying to portal**
- Publish all customizations from Power Apps
- Sync portal configuration
- Wait 5-10 minutes for changes to propagate
- Clear browser cache
- Test in incognito/private browsing mode

**Issue: Field shows blank instead of question text**
- Verify field label is set correctly
- Check form design still includes the field
- Confirm permissions allow field visibility
- Review field configuration in form designer

---

## Related Patterns

- P01: Enable File Upload for Subgrid Components
- P06: Configuring a 3D Matrix Setup with Parent-Child Relationship
- P11: Enable Field Validation Using Regular Expressions

---

## Implementation Checklist

- [ ] Identify Yes/No questions needing default override
- [ ] Navigate to Power Pages Management
- [ ] Locate the multistep form
- [ ] Find the step containing the question
- [ ] Create new metadata record
- [ ] Set Type: Attribute
- [ ] Select question field (Attribute Logical Name)
- [ ] Set Prepopulate Field: Ignore default value
- [ ] Save metadata record
- [ ] Publish all customizations
- [ ] Navigate to portal
- [ ] Open form and navigate to step
- [ ] Verify no default is selected
- [ ] Test Yes selection works
- [ ] Test No selection works
- [ ] Clear cache and retest
- [ ] Test in different browser
- [ ] Document which fields have override applied

---

## Compliance Considerations

**Regulatory Impact:**
- Ensures explicit user consent
- Creates audit trail of user choices
- Prevents accidental affirmation of false answers
- Maintains compliance with governance requirements

**Documentation:**
- Metadata changes should be documented
- Keep record of which questions have overrides
- Document business justification
- Version control metadata configurations

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** January 2025  
**Status:** Active
