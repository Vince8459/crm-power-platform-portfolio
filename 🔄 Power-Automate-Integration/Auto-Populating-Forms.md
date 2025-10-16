# P08: Auto-Populating Forms with Power Automate

**Category:** Power Automate & Integration  
**Version:** 1.0  
**Contributors:** Vincent Kimiti    
**Project:** Form Auto-Population Automation

**📥 Original Documentation:** [Download PDF](../pdfs/P08-Auto-Populating-Forms.pdf)

---

## Context

**Tag:** Vertical Patterns | Power Apps

**Application Use Case:**
- Auto-populate an "Authorised Individuals" form with account data from Dataverse

**Reference Usage:** Automating form population using Dataverse and string manipulation in Power Automate

---

## Story Behind The Pattern

### The Problem
- Manually updating the "Authorised Individuals" form with related account data is inefficient
- Users must look up account information separately
- Error-prone manual data entry
- Time-consuming form completion process
- Forms take 15+ minutes to complete

### The Solution
- Create a Power Automate flow to retrieve row data
- Initialize a variable to store account data
- List related accounts
- Update the form with processed information
- Eliminate manual lookups and data entry

### Dependencies
- Power Automate environment
- Dataverse with "Authorised Individuals" and "Accounts" tables
- Permissions to create flows and access tables

---

## The Pattern: Implementation Method

### Step 1: Create a New Flow in Power Automate

1. Log in to the Power Automate portal (make.powerautomate.com)
2. Click **Create** and select **Automated cloud flow**
3. Name the flow (e.g., **"Authorised Individual Form Auto Populate"**)
4. Click **Create**
5. Choose the trigger: **When a row is added, modified or deleted from the Dataverse connector**

---

### Step 2: Configure the Trigger

In the trigger settings, set the following:

1. **Change type:** Select **"Added, Modified"** (to trigger on new or updated rows; exclude "Deleted" if not needed)
2. **Table name:** Select **"Authorised Individuals"** from the dropdown
3. **Scope:** Choose **"Organization"** to apply the trigger across the organization
4. **Select columns:** Leave blank or enter specific columns (e.g., **"AuthorisedInd_Id"**) to optimize performance
5. **Filter rows:** Optionally, add an OData expression (e.g., **statuscode eq 1**) to limit triggers to active records
6. Click **Save** to proceed

---

### Step 3: Add a Get a Row by ID Action

1. Click **+ New step** below the trigger
2. Search for and select **Get a row by ID** (Dataverse connector)
3. Configure the action:
   - **Table name:** Select **"Authorised Individuals"**
   - **Row ID:** Select the **"Row ID"** dynamic content from the trigger output
   - **Select columns:** Leave blank or specify columns (e.g., **"AuthorisedInd_Id"**) to retrieve specific data

4. This step retrieves the detailed data of the changed row

---

### Step 4: Initialize a Variable

1. Click **+ New step**
2. Search for and select **Initialize variable** (Variables connector)
3. Configure the variable:
   - **Name:** Enter **"acc"** (or a meaningful name like **"accountList"**)
   - **Type:** Select **"String"**
   - **Value:** Leave blank or enter an initial value (e.g., an empty string)

4. This step sets up a string variable to store concatenated account data

---

### Step 5: Add a List Rows Action

1. Click **+ New step**
2. Search for and select **List rows** (Dataverse connector)
3. Configure the action:
   - **Table name:** Select **"Accounts"**
   - **Select columns:** Leave blank or enter specific columns (e.g., **"Account Number,"** **"Firm Name"**)
   - **Filter rows:** Enter an OData filter (e.g., **FirmName eq 'Firm X'**) to limit rows based on the "Authorised Individuals" data
   - **Sort by:** Leave blank or specify a column (e.g., **"Created On"** descending)
   - **Fetch XML Query:** Optionally, use a custom FetchXML query
   - **Row count:** Leave as default (5000) or adjust as needed

4. This step retrieves a list of accounts matching the criteria

---

### Step 6: Apply to Each Loop

The **"List rows"** output will automatically create an **"Apply to each"** loop:

1. In the **"Select an output from previous steps"** field, choose the **"value"** output from the **"List rows"** action
2. This step iterates over each account row retrieved

---

### Step 7: Append to String Variable

1. Inside the **"Apply to each"** loop, click **Add an action**
2. Search for and select **Append to string variable** (Variables connector)
3. Configure the action:
   - **Name:** Select **"acc"** (the variable initialized earlier)
   - **Value:** Select **"Account Number"** (or another field) from the **"List rows"** output
   - Build the string: `concat(variables('acc'), item()?['accountnumber'], ', ')`

4. This step builds a string of account numbers from the retrieved rows

---

### Step 8: Update a Row

1. Inside the **"Apply to each"** loop, click **Add an action**
2. Search for and select **Update a row** (Dataverse connector)
3. Configure the action:
   - **Table name:** Select **"Authorised Individuals"**
   - **Row ID:** Select the **"AuthorisedInd_Id"** from the **"Get a row by ID"** output

4. Map the following fields (if applicable to your use case):
   - **Address party to any civil proceeding?:** Leave blank or map a field
   - **Address been a director...?:** Leave blank or map a field
   - **Firm Name:** Set to the concatenated **"acc"** variable (e.g., `variables('acc')`)
   - Other fields as needed

5. Click **Save** to update the row with the processed account data

---

## Flow Architecture

```
[TRIGGER: Row Added/Modified]
    ↓ Authorised Individual record created/modified
┌──────────────────────────────────┐
│ TRIGGER: Authorised Individuals  │
│ When: Added or Modified          │
├──────────────────────────────────┤
│ Get Row by ID                    │
│ Retrieve full record details     │
└──────────────────────────────────┘
    ↓
┌──────────────────────────────────┐
│ Initialize Variable              │
│ Name: acc (empty string)         │
│ Purpose: Store account data      │
└──────────────────────────────────┘
    ↓
┌──────────────────────────────────┐
│ List Rows (Accounts Table)       │
│ Filter by: Firm Name             │
│ Returns: Related account records │
└──────────────────────────────────┘
    ↓
┌──────────────────────────────────┐
│ FOR EACH Account Record          │
│                                  │
│ ┌─────────────────────────────┐  │
│ │ Append to String Variable   │  │
│ │ acc += AccountNumber + ", " │  │
│ └─────────────────────────────┘  │
└──────────────────────────────────┘
    ↓
┌──────────────────────────────────┐
│ Update Row                       │
│ Authorised Individual record     │
│ Set: Firm Name = acc (all nums)  │
└──────────────────────────────────┘
    ↓
[FORM AUTO-POPULATED]
```

---

## Key Concepts

**Trigger:**
Event that initiates the flow. In this case: "When a row is added or modified"

**Loop (Apply to Each):**
Iterates through collection of records (accounts) and performs actions on each

**String Variable:**
Stores and builds a concatenated string of data across loop iterations

**Dynamic Content:**
Values pulled from previous steps used throughout the flow

**OData Filter:**
Query to retrieve only relevant rows from table

---

## Configuration Examples

### Example 1: Basic Account Data Population
```
Trigger: Authorised Individual added/modified

Steps:
1. Get row by ID (Authorised Individual)
2. Initialize variable "acc"
3. List rows (Accounts - filtered by Firm Name)
4. For each account:
   - Append account number to acc variable
5. Update Authorised Individual:
   - Firm Name = acc variable (concatenated)

Result: Firm Name field populated with all related account numbers
```

### Example 2: Complex Account Data Mapping
```
Trigger: Authorised Individual added/modified
Filter: Status = Active

Steps:
1. Get row by ID
2. Initialize multiple variables:
   - acc_names
   - acc_contacts
   - acc_values
3. List rows with multiple filters
4. For each account:
   - Append to multiple variables
5. Update Authorised Individual with all concatenated data

Result: Multiple fields populated with comprehensive account information
```

---

## Best Practices

1. **Proper Filtering:** Filter related records accurately
2. **
