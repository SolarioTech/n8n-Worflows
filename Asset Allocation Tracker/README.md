# IT Asset Assignment and Inventory Notification System

## Overview
This n8n workflow automates the IT asset assignment process by monitoring new asset allocations, updating inventory records, and sending notifications to relevant stakeholders.

## What This Workflow Does
- **Monitors** a Google Sheet for new asset assignment entries
- **Validates** asset IDs to prevent duplicates
- **Updates** the central Asset Inventory automatically
- **Notifies** employees, IT team, and admins via email
- **Handles** duplicate asset detection with admin alerts

## Workflow Steps

### 1. **Trigger: New Asset Assignment Row**
   - Monitors the "Asset Allocation" Google Sheet every minute
   - Triggers when a new row is added

### 2. **Workflow Configuration**
   - Extracts and normalizes data from the new row:
     - Employee Name
     - Employee Email
     - Asset Type
     - Asset ID
     - Assignment Date
   - Sets admin and IT team email addresses

### 3. **Filter**
   - Validates that the Asset ID is not empty
   - Prevents processing of incomplete entries

### 4. **Check for Duplicate Asset ID**
   - Searches the Asset Inventory sheet for existing Asset ID
   - Returns any matching records

### 5. **Is Duplicate Asset?**
   - **If Duplicate Found:**
     - Sends alert email to admin with duplicate details
     - Workflow stops to prevent inventory corruption
   
   - **If No Duplicate:**
     - Proceeds to update inventory and send notifications

### 6. **Update Asset Inventory Sheet**
   - Adds/updates the asset record with:
     - Asset ID
     - Asset Type
     - Assigned To (Employee Name)
     - Assignment Date
     - Status: "Active"

### 7. **Send Confirmation to Employee**
   - Emails the employee with:
     - Asset Type
     - Asset ID
     - Assignment Date
     - Care instructions

### 8. **Notify IT Team**
   - Sends notification to IT team with:
     - Employee details
     - Asset information
     - Confirmation that inventory is updated

## Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- Google account with access to Google Sheets
- Gmail account for sending notifications

### Step 1: Import the Workflow
1. Open your n8n instance
2. Click on "Workflows" → "Import from File"
3. Select `IT Asset Assignment and Inventory Notification System.json`
4. Click "Import"

### Step 2: Configure Google Sheets
1. **Create Asset Allocation Sheet** (Input):
   - Create a new Google Sheet
   - Add columns: `Employee Name`, `Employee Email`, `Asset Type`, `Asset ID`, `Assignment Date`
   
2. **Create Asset Inventory Sheet** (Database):
   - Create another Google Sheet
   - Add columns: `Asset ID`, `Asset Type`, `Assigned To`, `Assignment Date`, `Status`

### Step 3: Set Up Credentials
1. **Google Sheets Trigger OAuth2**:
   - Click on the "New Asset Assignment Row" node
   - Click "Create New Credential"
   - Follow Google OAuth flow to authorize

2. **Google Sheets OAuth2**:
   - Click on "Update Asset Inventory Sheet" node
   - Create credential and authorize

3. **Gmail OAuth2**:
   - Click on any email node
   - Create credential and authorize Gmail

### Step 4: Configure Nodes
1. **New Asset Assignment Row** node:
   - Select your Asset Allocation Sheet
   - Choose the appropriate sheet tab

2. **Check for Duplicate Asset ID** node:
   - Select your Asset Inventory Sheet
   - Verify the sheet tab

3. **Update Asset Inventory Sheet** node:
   - Select your Asset Inventory Sheet
   - Verify column mappings

4. **Workflow Configuration** node:
   - Update `adminEmail` field with your admin email
   - Update `itTeamEmail` field with your IT team email

### Step 5: Test the Workflow
1. Click "Execute Workflow" to test manually
2. Add a test row to your Asset Allocation Sheet:
   ```
   Employee Name: John Doe
   Employee Email: john.doe@company.com
   Asset Type: Laptop
   Asset ID: LAP-001
   Assignment Date: 2026-01-07
   ```
3. Wait up to 1 minute for the trigger to detect the new row
4. Verify emails are sent and inventory is updated

### Step 6: Activate the Workflow
1. Toggle the "Active" switch in the top-right corner
2. The workflow will now run automatically

## Email Templates

### Employee Confirmation Email
```
Subject: Asset Assignment Confirmation - [Asset Type]

Dear [Employee Name],

This email confirms that the following asset has been assigned to you:
- Asset Type: [Asset Type]
- Asset ID: [Asset ID]
- Assignment Date: [Assignment Date]

Please take good care of this asset and report any issues to the IT team.

Best regards,
IT Department
```

### IT Team Notification
```
Subject: New Asset Assignment - [Asset Type] to [Employee Name]

A new asset has been assigned:
- Employee Name: [Employee Name]
- Asset Type: [Asset Type]
- Asset ID: [Asset ID]
- Assignment Date: [Assignment Date]

The Asset Inventory has been updated and the employee has been notified.
```

### Admin Duplicate Alert
```
Subject: ALERT: Duplicate Asset ID Detected - [Asset ID]

A duplicate Asset ID has been detected in the system:
- Asset ID: [Asset ID]
- Asset Type: [Asset Type]
- Employee Name: [Employee Name]
- Assignment Date: [Assignment Date]

Please review the Asset Inventory immediately.
```

## Customization Options

### Change Polling Frequency
- Edit the "New Asset Assignment Row" node
- Modify the poll time (default: every minute)
- Options: every minute, every 5 minutes, hourly, etc.

### Add Additional Fields
1. Update your Google Sheets with new columns
2. Modify the "Workflow Configuration" node to include new fields
3. Update the "Update Asset Inventory Sheet" column mappings
4. Adjust email templates to include new information

### Add More Notification Recipients
- Duplicate email nodes
- Change the `sendTo` parameter
- Connect to the appropriate workflow path

## Troubleshooting

### Workflow Not Triggering
- Verify the Google Sheets Trigger credential is valid
- Check that the correct sheet and tab are selected
- Ensure the workflow is activated (toggle switch is ON)

### Emails Not Sending
- Verify Gmail OAuth2 credentials are connected
- Check spam/junk folders
- Ensure email addresses in configuration are correct

### Duplicate Detection Not Working
- Verify the "Check for Duplicate Asset ID" node has correct sheet selected
- Ensure "Asset ID" column name matches exactly in both sheets

### Data Not Updating in Inventory
- Check Google Sheets OAuth2 credential
- Verify column mappings in "Update Asset Inventory Sheet" node
- Ensure column names match exactly (case-sensitive)

## Best Practices
- Use consistent Asset ID naming conventions (e.g., LAP-001, MON-045)
- Regularly review the Asset Inventory for accuracy
- Keep admin and IT team emails up to date
- Archive old asset assignments periodically
- Test workflow after any modifications

## Support
For issues or questions:
1. Check n8n documentation: https://docs.n8n.io
2. Review workflow execution logs in n8n
3. Verify all credentials are valid and not expired
