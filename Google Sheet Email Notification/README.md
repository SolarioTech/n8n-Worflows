# Google Sheet New Row Email Notification

## Overview
This n8n workflow provides a simple yet powerful automation that monitors a Google Sheet and sends email notifications whenever a new row is added. It includes error handling to alert administrators if the notification fails.

## What This Workflow Does
- **Monitors** a Google Sheet for new row additions
- **Sends** email notifications to HR with the new row data
- **Handles** errors gracefully with admin alerts
- **Provides** real-time notifications for form submissions or data entries

## Workflow Steps

### 1. **Trigger: New Row Added**
   - Monitors the specified Google Sheet every minute
   - Triggers when a new row is detected

### 2. **Workflow Configuration**
   - Sets up email addresses:
     - HR Email (notification recipient)
     - Admin Email (error alert recipient)
   - Passes through all row data

### 3. **Map Row Data**
   - Extracts data from the new row:
     - Name
     - Email
     - Request Type
     - Date
   - Formats the email subject and body

### 4. **Send Email to HR**
   - Sends formatted email to HR with all row details
   - Includes all relevant information from the new entry

### 5. **Error Handling**
   - **HR Email Error Handler** (Error Trigger):
     - Activates if the HR email fails to send
   - **Send Alert to Admin**:
     - Notifies admin of the failure
     - Includes error details and execution information

## Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- Google account with access to Google Sheets
- Gmail account for sending notifications

### Step 1: Import the Workflow
1. Open your n8n instance
2. Click on "Workflows" → "Import from File"
3. Select `Google Sheet New Row Email Notification.json`
4. Click "Import"

### Step 2: Create or Prepare Your Google Sheet
1. Create a new Google Sheet or use an existing one
2. Recommended columns (customize as needed):
   - `Name`
   - `Email`
   - `Request Type`
   - `Date`
   - Any additional fields relevant to your use case

**Example Sheet Structure:**
```
| Name        | Email                | Request Type | Date       |
|-------------|----------------------|--------------|------------|
| John Doe    | john@example.com     | Support      | 2026-01-07 |
| Jane Smith  | jane@example.com     | Sales        | 2026-01-07 |
```

### Step 3: Set Up Credentials
1. **Google Sheets Trigger OAuth2**:
   - Click on the "New Row Added" node
   - Click "Create New Credential"
   - Follow Google OAuth flow to authorize

2. **Gmail OAuth2**:
   - Click on "Send Email to HR" node
   - Create credential and authorize Gmail

### Step 4: Configure Nodes
1. **New Row Added** node:
   - Click on the node
   - Select your Google Sheet from the dropdown
   - Choose the appropriate sheet tab
   - Verify polling interval (default: every minute)

2. **Workflow Configuration** node:
   - Update `hrEmail` with the recipient email address
   - Update `adminEmail` with the admin/IT email address

3. **Map Row Data** node:
   - Customize the `emailSubject` if needed
   - Modify the `emailBody` to match your sheet columns:
     ```javascript
     'A new row has been added to the Google Sheet:\n\n' +
     'Name: ' + ($json.Name || 'N/A') + '\n' +
     'Email: ' + ($json.Email || 'N/A') + '\n' +
     'Request Type: ' + ($json['Request Type'] || 'N/A') + '\n' +
     'Date: ' + ($json.Date || 'N/A') + '\n\n' +
     'Please review this new entry.'
     ```
   - Replace column names with your actual column names

4. **Send Alert to Admin** node:
   - Verify the admin email address is correct

### Step 5: Test the Workflow
1. Click "Execute Workflow" to test manually
2. Add a test row to your Google Sheet:
   ```
   Name: Test User
   Email: test@example.com
   Request Type: Test
   Date: 2026-01-07
   ```
3. Wait up to 1 minute for the trigger
4. Check that the email was received

### Step 6: Activate the Workflow
1. Toggle the "Active" switch in the top-right corner
2. The workflow will now run automatically

## Email Templates

### HR Notification Email
```
Subject: New Request Added

A new row has been added to the Google Sheet:

Name: [Name]
Email: [Email]
Request Type: [Request Type]
Date: [Date]

Please review this new entry.
```

### Admin Error Alert
```
Subject: ALERT: Email Notification Failed

The email notification to HR FAILED.

Workflow: [Workflow Name]
Failed Node: [Node Name]
Error Message: [Error Details]
Execution ID: [Execution ID]
Time: [Timestamp]
```

## Customization Options

### Modify Email Content
Edit the "Map Row Data" node to customize:
- Email subject line
- Email body format
- Which fields to include
- Formatting (HTML, plain text, etc.)

### Add More Recipients
1. Duplicate the "Send Email to HR" node
2. Update the `sendTo` parameter
3. Connect it to the "Map Row Data" node

### Change Column Mappings
Update the `emailBody` expression in "Map Row Data" to match your sheet:
```javascript
'New Entry Details:\n\n' +
'Field 1: ' + ($json['Your Column Name'] || 'N/A') + '\n' +
'Field 2: ' + ($json['Another Column'] || 'N/A') + '\n'
```

### Add Conditional Logic
1. Insert an IF node after "Map Row Data"
2. Set conditions (e.g., only notify for specific request types)
3. Route different types to different email templates

### Format as HTML Email
Update the email message to use HTML:
```javascript
'<h2>New Request Submitted</h2>' +
'<p><strong>Name:</strong> ' + $json.Name + '</p>' +
'<p><strong>Email:</strong> ' + $json.Email + '</p>' +
'<p><strong>Request Type:</strong> ' + $json['Request Type'] + '</p>'
```

### Change Polling Frequency
- Edit the "New Row Added" node
- Modify poll time:
  - Every minute (real-time, higher resource usage)
  - Every 5 minutes (balanced)
  - Every hour (low resource usage)

## Use Cases

### 1. Form Response Notifications
- Connect to a Google Form
- Get notified when someone submits a response
- Perfect for contact forms, surveys, registrations

### 2. Lead Tracking
- Monitor new leads added to a sheet
- Notify sales team immediately
- Ensure quick follow-up

### 3. Support Ticket Creation
- Track new support requests
- Alert support team
- Integrate with ticketing systems

### 4. Inventory Updates
- Monitor stock level changes
- Alert when new items are added
- Track inventory movements

### 5. Event Registrations
- Get notified of new event sign-ups
- Send confirmation emails
- Update attendee lists

## Troubleshooting

### Workflow Not Triggering
- Verify the Google Sheets Trigger credential is valid
- Check that the correct sheet and tab are selected
- Ensure the workflow is activated (toggle is ON)
- Try re-authorizing the Google Sheets credential

### Emails Not Sending
- Verify Gmail OAuth2 credentials are connected
- Check spam/junk folders
- Ensure email addresses are valid
- Check Gmail sending limits (not exceeded)

### Wrong Data in Emails
- Verify column names match exactly (case-sensitive)
- Check the "Map Row Data" node expressions
- Review the execution log to see actual data

### Error Alerts Not Working
- Verify the "HR Email Error Handler" node is connected
- Check that the admin email in "Send Alert to Admin" is correct
- Test by intentionally causing an error (invalid email address)

### Duplicate Notifications
- Check if multiple workflows are monitoring the same sheet
- Verify the trigger is set to "rowAdded" event only
- Consider adding a "processed" column to track handled rows

## Best Practices
- Use descriptive column names in your Google Sheet
- Test with sample data before going live
- Keep email templates concise and clear
- Regularly check the execution log for errors
- Update email addresses when team members change
- Consider rate limits for high-volume sheets
- Archive old data periodically

## Advanced Enhancements

### Add Data Validation
1. Insert a Filter node after "Workflow Configuration"
2. Check for required fields
3. Only proceed if data is valid

### Store Processed Rows
1. Add a "Status" column to your sheet
2. Update the status after processing
3. Filter out already-processed rows

### Multi-Channel Notifications
- Add Slack notification node
- Send SMS via Twilio
- Post to Microsoft Teams

### Attach Sheet Data
- Use Google Sheets node to fetch additional data
- Create a PDF report
- Attach to the email

## Support
For issues or questions:
1. Check n8n documentation: https://docs.n8n.io
2. Review workflow execution logs in n8n
3. Verify all credentials are valid and not expired
4. Test with a simple example first
