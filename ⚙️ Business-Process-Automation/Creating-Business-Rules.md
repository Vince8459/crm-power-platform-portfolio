# P05: Creating Business Rules in Power Apps

**Category:** Business Process Automation  
**Version:** 1.0  
**Contributors:** Vincent Kimiti  
**Project:** Business Logic Automation

**📥 Original Documentation:** [Download PDF](../pdfs/P05-Creating-Business-Rules.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Apps

**Application Use Case:**
- Enforce business logic by setting default values and displaying error messages based on conditions

**Reference Usage:** Automating form behavior with business rules in Power Apps

---

## Story Behind The Pattern

### The Problem
Manual enforcement of business logic (e.g., setting default values or validating inputs) on forms is error-prone and inconsistent:
- Users inconsistently apply business rules
- Manual validation leads to data errors
- No standardized enforcement of business logic
- Difficult to maintain consistency across forms
- High support overhead for user errors

### The Solution
Use business rules in Power Apps to automate form behavior, such as setting default values and displaying error messages when conditions are met or not met. This ensures consistent enforcement of business logic.

### Dependencies
- Power Apps environment
- Access to a table (entity) with a form
- Permissions to create and edit business rules

---

## The Pattern: Implementation Method

This step-by-step guide demonstrates creating a business rule in Power Apps to enforce logic on a form, such as setting a default value and displaying an error message based on a condition.

---

### Step 1: Setup Business Rule Name (Naming)

1. Log in to your Power Apps environment
2. Navigate to the **Solutions** area in the Power Apps Maker Portal
3. Select or create a solution where you want to add the business rule
4. In the solution, select the table (entity) you want to create the business rule for (e.g., **"Accounts"** or **"Contacts"**)
5. Under the table, go to the **Business Rules** tab and click **New Business Rule**
6. In the business rule editor, provide a meaningful name, such as **"Set Default Priority and Validate Status"**

---

### Step 2: Setting Up Condition (Set Condition and Name of Condition)

1. In the business rule editor, click **+ Add Condition** to define a condition that will trigger the rule
2. Name the condition for clarity, e.g., **"Check Status Field"**
3. Set the condition logic, such as:
   - **Field:** "Status" (or another field on the form)
   - **Operator:** Equals (or another operator like Contains, Greater Than, etc.)
   - **Value:** "Active" (or another value relevant to your use case)

4. This condition will determine when the business rule actions are applied (e.g., if the **"Status"** field is **"Active"**)

---

### Step 3: Setup Action on Condition Fulfillment (Set Default Value Action)

1. In the **"If Yes"** branch of the condition, click **+ Add Action** to define what happens when the condition is met
2. Select **Set Field Value** as the action type
3. Configure the action to set a default value:
   - **Field:** "Priority" (or another field you want to set a default value for)
   - **Value:** "High" (or another default value, such as a number, text, or choice)

4. This action ensures that when the condition is fulfilled (e.g., Status = Active), the **"Priority"** field is automatically set to **"High"**

---

### Step 4: Set Error Message Action (Set Error Message)

1. In the **"If No"** branch of the condition (or add another condition if needed), click **+ Add Action** to define what happens when the condition is not met
2. Select **Show Error Message** as the action type
3. Configure the error message:
   - **Field:** Select the field to associate the error message with (e.g., **"Status"**)
   - **Message:** Enter a clear error message, such as **"Status must be set to Active to proceed"**

4. This action will display the error message on the form if the condition fails (e.g., if the **"Status"** field is not **"Active"**)

---

### Step 5: Save Business Rule

1. Once the condition and actions are configured, click **Validate** in the business rule editor to check for errors
2. If no errors are found, click **Save** to save the business rule
3. Click **Activate** to enable the business rule, making it active on the form
4. Test the business rule by opening a record in the associated table's form, changing the **"Status"** field, and verifying that:
   - The **"Priority"** field updates when Status = Active
   - The error message appears as expected

---

## Business Rule Components

### Conditions

**Purpose:** Define the criteria that trigger business rule actions

**Types of Operators:**
- **Equals:** Exact match
- **Does not equal:** Not equal to
- **Contains:** Text field contains value
- **Does not contain:** Text field does not contain value
- **Is greater than:** Numeric comparison
- **Is less than:** Numeric comparison
- **Begins with:** Text field starts with value
- **Ends with:** Text field ends with value

**Example Conditions:**
```
Condition 1: Status = "Active"
Condition 2: Priority = "High" AND Status = "Pending"
Condition 3: Account.Industry Contains "Technology"
```

### Actions

**Available Actions:**

1. **Set Field Value**
   - Sets a field to a specific value
   - Can be static or based on other field values
   - Example: Set Priority to "High"

2. **Clear Field Value**
   - Removes the value from a field
   - Useful for conditional field clearing
   - Example: Clear Description field if Status = Closed

3. **Set Field Visibility**
   - Show or hide fields based on conditions
   - Example: Show "Resolution Notes" only if Status = Resolved

4. **Set Field Required/Optional**
   - Make a field required or optional based on conditions
   - Example: Make "Resolution Date" required if Status = Closed

5. **Show Error Message**
   - Display error message on form
   - Prevents form submission if validation fails
   - Example: "Status must be set to Active to proceed"

6. **Show Recommendation**
   - Display informational recommendation to user
   - Does not prevent form submission
   - Example: "Consider setting priority based on customer tier"

---

## Implementation Examples

### Example 1: Basic Default Value Setting
```
Business Rule: "Set Default Priority"

Condition:
IF Status = "Active"

Actions (If Yes):
- Set Field Value: Priority = "High"

Actions (If No):
- Set Field Value: Priority = "Normal"
```

### Example 2: Validation with Error Message
```
Business Rule: "Validate Account Status"

Condition:
IF Account.Status ≠ "Active"

Actions (If Yes):
- Show Error Message: "Only active accounts can be assigned opportunities"

Actions (If No):
- No action needed
```

### Example 3: Complex Multi-Condition Rule
```
Business Rule: "Assignment Rules"

Condition 1:
IF Status = "Active" AND Priority = "High"

Actions (If Yes):
- Set Field Value: Assigned To = [Manager]
- Set Required: Resolution Date
- Show: Escalation Path field

Actions (If No):
- Clear Field Value: Escalation Path
- Set Optional: Resolution Date
```

### Example 4: Conditional Field Visibility
```
Business Rule: "Show Resolution Fields"

Condition:
IF Status = "Resolved" OR Status = "Closed"

Actions (If Yes):
- Set Visible: Resolution Notes
- Set Visible: Resolution Date
- Set Required: Resolution Type

Actions (If No):
- Set Hidden: Resolution Notes
- Set Hidden: Resolution Date
- Set Optional: Resolution Type
```

---

## Key Concepts

**Business Rule:**
A set of conditions and actions that automatically enforce business logic on forms without requiring code.

**Condition:**
A logical statement that determines whether actions will be executed. Business rules evaluate conditions when a form loads and when a field changes.

**Action:**
An automated operation that occurs when a condition is met. Actions can set values, show/hide fields, make fields required, or display messages.

**Scope:**
Defines where the business rule applies:
- **Entity:** Rule applies to all forms for the table
- **Form:** Rule applies to specific form only
- **Form tab:** Rule applies to specific tab on form

---

## Best Practices

1. **Logical Naming:** Use clear, descriptive names for business rules and conditions
2. **Test All Paths:** Verify both "If Yes" and "If No" branches work correctly
3. **Avoid Complexity:** Keep conditions simple and understandable
4. **Performance:** Minimize number of conditions evaluated on form load
5. **User Feedback:** Use clear, helpful error messages
6. **Documentation:** Document business logic in rule descriptions
7. **Combine with Workflows:** Use workflows for complex multi-step logic

---

## Business Rule Scope

### Entity Scope
- Rule applies to all forms and interfaces
- Useful for universal business logic
- Recommended for core data validation

### Form Scope
- Rule applies only to specific form
- Useful for form-specific logic
- Recommended for presentation logic

### Form Tab Scope
- Rule applies only to specific tab within form
- Useful for tab-specific validation
- Recommended for complex multi-tab forms

---

## Troubleshooting

**Issue: Business rule not executing**
- Verify rule is activated
- Check condition logic
- Review field names for spelling errors
- Test on form to ensure rule scope is correct

**Issue: Unexpected field values**
- Review "Set Field Value" actions
- Check condition logic evaluates correctly
- Verify field types match expected values

**Issue: Error message not displaying**
- Confirm condition is being triggered
- Verify error message is in "If No" or appropriate branch
- Check field selected for error association

**Issue: Required field validation not working**
- Verify "Set Required" action is configured
- Check that field becomes required when condition is met
- Test form submission behavior

**Issue: Field visibility not changing**
- Confirm visibility actions are in correct branch
- Verify condition triggers field show/hide
- Test on published form (not editor)

---

## Related Patterns

- P01: Creating a Business Process Flow
- P03: Create an Application on Form Submission with Workflows
- P04: Sending Email on Form Submission with Workflows

---

## Implementation Checklist

- [ ] Identify the table and form for the business rule
- [ ] Document the business logic to be enforced
- [ ] Design conditions and actions on paper
- [ ] Create business rule in Power Apps
- [ ] Set business rule name
- [ ] Add condition(s)
- [ ] Configure "If Yes" actions
- [ ] Configure "If No" actions
- [ ] Validate business rule for errors
- [ ] Test on development form
- [ ] Verify all condition paths work
- [ ] Test with various field values
- [ ] Verify error messages display correctly
- [ ] Test field visibility changes
- [ ] Test required field behavior
- [ ] Get stakeholder approval
- [ ] Activate business rule
- [ ] Test on production form
- [ ] Monitor for first week
- [ ] Document business logic for team

---

## Success Metrics

**Before Business Rules Implementation:**
- Manual validation errors: 10-15%
- User confusion: High (inconsistent rule application)
- Data quality issues: Frequent
- Support overhead: High

**After Business Rules Implementation:**
- Manual validation errors: <1% (automated enforcement)
- User confusion: Low (guided by rules)
- Data quality issues: Minimal
- Support overhead: Reduced 60%+

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** March 2025  
**Status:** Active
