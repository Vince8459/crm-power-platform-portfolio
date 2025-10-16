# P05: Configuring Signature Input in Power Pages Using Dataverse

**Category:** Power Pages Development & Configuration  
**Version:** 1.0 - Signature Configuration Steps  
**Contributors:** Vincent Kimiti  
**Project:** Signature Capture Implementation

**📥 Original Documentation:** [Download PDF](../pdfs/P05-Signature-Input-Configuration.pdf)

---

## Context

**Tag:** Vertical Development

**Application Use Case:**
- Capturing user signatures in forms

**Reference Usage:** Signature Column Configuration in Dataverse

---

## Story Behind The Pattern

### The Problem
- Collecting signatures from users within Power Pages forms
- Storing signatures efficiently in Dataverse
- Lack of native signature capture capability in standard forms

### The Solution
- Configure a signature input column in Dataverse
- Integrate it into Power Pages forms
- Sync the configuration to enable signature capture

### Dependencies
N/A

---

## The Pattern: Step-by-Step Implementation

### Step 1: Access Power Apps

1. Go to the **Power Apps portal** (make.powerapps.com)
2. Navigate to **Dataverse**
3. Select your environment

---

### Step 2: Create a Text Area Column in Dataverse

1. Go to the table where you want to store the signature
2. Add a **new column** of type: **"Multiple lines of text"**
3. Set the **maximum character count** to **15,000** or **20,000** to accommodate the signature
4. Column configuration:
   - **Name:** Signature (or similar descriptive name)
   - **Type:** Multiple lines of text
   - **Format:** Text Area
   - **Max length:** 15,000 - 20,000 characters

---

### Step 3: Update the Dataverse Form

1. In the form section, locate the form linked to the Power Pages portal
2. **Drag and drop** the new text area column into the form
3. Select the new text area field
4. Go to the **"Components"** section
5. Click on **"+ component"**
6. Choose the **"Pen input"** control
7. If the **"Pen input"** control is not available:
   - Use the **"Get more components"** option to find it
   - Install the component if needed

---

### Step 4: Add the Form to Power Pages

1. Go to the **Power Pages portal**
2. Navigate to the page where you want to add the signature control
3. **Add the form** linked to the updated table
4. Initially, the signature field will render as a regular text field

---

### Step 5: Update Form Metadata

1. Open the **Power Pages Management app**
2. Go to **"Basic Forms"** or **"Multistep Forms"** (if applicable)
3. Locate the form and **open the record**
4. Under **"Basic form metadata,"** click **"New basic form metadata"**
5. Update the form with the following values:
   - **Type:** Select **Attribute**
   - **Attribute Logical Name:** Select the **Signature column**
   - **Control Style:** Select **Code Component**

6. Fill in all required metadata fields:
   - **Label:** Signature (or your preferred label)
   - **Placeholder:** Click here to sign
   - **Required:** Yes (if mandatory) or No (if optional)
   - **Visible:** Yes

7. **Save and close** the changes

---

### Step 6: Enable Additional Settings on the Basic Form

1. Open the basic form in **Power Pages Management**
2. Go to **Additional Settings**
3. Enable the following settings:
   - **Enable pen input:** Yes
   - **Show pen input control:** Yes
   - **Allow signature capture:** Yes (if option exists)
   - **Clear button for signature:** Yes (optional - allows user to redo)

---

### Step 7: Sync Website Configuration

1. Go back to the **portal**
2. Click on the **"Sync"** button to synchronize the website configuration
3. After synchronization, the pen control will be available for use in the form

---

## Signature Capture Flow

```
User Opens Power Pages Form
    ↓
Locates Signature Field
    ↓
[Signature Field Renders with Pen Input Control]
    ├─ Drawing canvas appears
    ├─ "Clear" button available (optional)
    └─ "Sign" button to confirm
    ↓
User Signs on Screen/Tablet
    ↓
[Signature Captured as Image Data]
    ↓
Converted to Text Format (Base64 encoding)
    ↓
Stored in Dataverse Signature Column
    ↓
Form Submitted
    ↓
Signature Persisted with Record
```

---

## Configuration Details

### Column Configuration
```
Table: [Your form table]
Column Name: Signature
Type: Multiple lines of text
Format: Text area
Max length: 15,000-20,000 characters
Description: Capture user signature with pen input control
```

### Metadata Configuration
```
Type: Attribute
Attribute Logical Name: [Signature column]
Label: Signature
Control Style: Code Component
Control Name: [Pen input or signature control]
Placeholder: Click here to sign
Required: Yes/No (based on business requirement)
Visible: Yes
```

### Form Settings
```
Enable pen input: Yes
Show pen input control: Yes
Allow handwriting: Yes
Drawing color: [Optional - typically black]
Canvas size: [Auto or specify dimensions]
```

---

## Key Concepts

**Multiple Lines of Text Column:**
Dataverse field type that stores text data. Used here to store signature data (encoded as Base64 or similar format).

**Pen Input Control:**
Power Apps component that enables drawing/writing on a canvas. Captures handwriting and converts to storable format.

**Code Component:**
Advanced form component that provides custom functionality beyond standard controls.

**Sync:**
Process that synchronizes Power Pages configuration changes to the portal, making new components available to users.

---

## Best Practices

1. **Column Size:** Use 15,000+ character limit to accommodate signature data encoding
2. **Required Field:** Mark signature as required for legal/compliance documents
3. **Clear Functionality:** Provide clear button to allow signature correction
4. **Testing:** Test signature capture on various devices (desktop, tablet, mobile)
5. **Validation:** Verify signature was captured before form submission
6. **Storage:** Ensure signature data is properly stored and retrievable
7. **Compliance:** Document signature capture for audit trail

---

## Browser and Device Compatibility

**Supported Browsers:**
- Chrome/Edge (latest versions)
- Firefox (latest version)
- Safari (latest version)

**Device Compatibility:**
- Desktop computers (with mouse)
- Tablets (with stylus or touch)
- Mobile devices (with touch input)

**Notes:**
- Signature capture works best on touch-enabled devices
- Mouse-based signatures on desktop are supported but may be less natural
- Test on target devices before deployment

---

## Troubleshooting

**Issue: Pen input control not appearing**
- Verify metadata is configured with "Code Component" control style
- Check "Enable pen input" is set to Yes in form settings
- Confirm sync was performed after configuration changes
- Verify table has pen input component installed

**Issue: Signature not saving**
- Check column has sufficient character limit (15,000+)
- Verify form submission is completing successfully
- Review user permissions on the signature field
- Check browser console for JavaScript errors

**Issue: Signature appears as text gibberish**
- This is normal - signature is encoded as Base64 text
- Signature data is properly stored, just not human-readable in raw form
- Rendering in reports/documents will display properly

**Issue: Drawing canvas too small or unresponsive**
- Clear browser cache and reload form
- Try different browser
- Test on device with touch support
- Check browser zoom level (should be 100%)

**Issue: Metadata not applying to form**
- Verify attribute logical name matches exactly
- Confirm type is set to "Attribute"
- Check control style is "Code Component"
- Publish all customizations from Power Apps
- Sync portal configuration

---

## Related Patterns

- P01: Enable File Upload for Subgrid Components
- P06: Configuring a 3D Matrix Setup with Parent-Child Relationship
- P11: Enable Field Validation Using Regular Expressions

---

## Implementation Checklist

- [ ] Navigate to Dataverse table
- [ ] Create new column (Multiple lines of text)
- [ ] Set max character count to 15,000+
- [ ] Save column to table
- [ ] Open form linked to table
- [ ] Add signature column to form
- [ ] Select pen input component
- [ ] Save form changes
- [ ] Publish form from Power Apps
- [ ] Go to Power Pages Management
- [ ] Open form record
- [ ] Create metadata entry
- [ ] Set Type: Attribute
- [ ] Select signature column
- [ ] Set Control Style: Code Component
- [ ] Save metadata
- [ ] Enable pen input in additional settings
- [ ] Go to Power Pages portal
- [ ] Sync configuration
- [ ] Test signature capture in form
- [ ] Test on desktop device
- [ ] Test on tablet/mobile device
- [ ] Verify signature saves with record
- [ ] Test form submission flow

---

## Compliance and Legal Considerations

**Signature Capture:**
- Digital signatures capture intent and consent
- Suitable for many regulatory contexts
- Document retention requirements apply
- Audit trail should be maintained

**Recommendations:**
- Include timestamp with signature
- Maintain record of IP address/device
- Log who signed and when
- Consider compliance requirements for your industry

---

## Questions or Need Help?

For additional information or assistance with implementation:

**Email:** your.email@example.com  
**LinkedIn:** [Your Profile]

---

**Version:** 1.0  
**Last Updated:** February 2025  
**Status:** Active
