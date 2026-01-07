# Employee Leave Request Processing and Notification System

## Overview
This n8n workflow automates the employee leave request process by monitoring form submissions, storing requests in a centralized sheet, and sending notifications to HR and employees.

## What This Workflow Does
- **Monitors** a Google Form for new leave requests
- **Stores** all leave requests in a centralized tracking sheet
- **Notifies** HR team of new leave requests
- **Sends** confirmation emails to employees
- **Handles** errors with admin notifications

## Workflow Steps

### 1. **Trigger: Google Sheets Trigger**
   - Monitors the Google Form response sheet every minute
   - Triggers when a new leave request is submitted

### 2. **Workflow Configuration**
   - Extracts leave request data:
     - Employee Name
     - Employee Email
     - Leave Type (Sick, Vacation, Personal, etc.)
     - From Date
     - To Date
     - Full Day/Half Day
     - Reason
   - Sets submission timestamp
   - Sets initial status as "Pending"

### 3. **Store in Leave Sheet**
   - Saves the leave request to a centralized tracking sheet
   - Records:
     - Employee details
     - Leave dates and type
     - Reason
     - Status (Approved/Pending/Rejected)

### 4. **Send Email to HR**
   - Notifies HR team of the new leave request
   - Includes all request details
   - Allows HR to review and approve

### 5. **Send Confirmation to Employee**
   - Sends acknowledgment email to the employee
   - Confirms request has been received
   - Provides request details
   - Sets expectations for approval timeline

### 6. **Error Handling**
   - **Email Admin on Error**:
     - Triggers if sheet update fails
     - Alerts admin with error details

## Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- Google account with access to Google Sheets and Forms
- Gmail account for sending notifications

### Step 1: Import the Workflow
1. Open your n8n instance
2. Click on "Workflows" → "Import from File"
3. Select `Employee Leave Request Processing and Notification System.json`
4. Click "Import"

### Step 2: Create Google Form
1. Create a new Google Form titled "Leave Request Form"
2. Add the following fields:
   - **Name** (Short answer)
   - **Email** (Email)
   - **Type of leave** (Multiple choice: Sick Leave, Vacation, Personal Leave, etc.)
   - **Leave From Date(s)** (Date)
   - **Leave To Date(s)** (Date)
   - **Full Day/Half Day** (Multiple choice: Full Day, Half Day)
   - **Reason** (Paragraph)

3. Link the form to a Google Sheet (Form → Responses → Create Spreadsheet)

### Step 3: Create Leave Tracking Sheet
1. Create a new Google Sheet named "Leave Requests"
2. Add the following columns:
   - `Employee Name`
   - `Employee Email`
   - `Leave Type`
   - `Reason`
   - `From Date`
   - `To Date`
   - `Status`

**Example Sheet Structure:**
```
| Employee Name | Employee Email      | Leave Type | Reason           | From Date  | To Date    | Status   |
|---------------|---------------------|------------|------------------|------------|------------|----------|
| John Doe      | john@company.com    | Vacation   | Family trip      | 2026-02-01 | 2026-02-05 | Approved |
| Jane Smith    | jane@company.com    | Sick Leave | Medical checkup  | 2026-01-15 | 2026-01-15 | Pending  |
```

### Step 4: Set Up Credentials
1. **Google Sheets Trigger OAuth2**:
   - Click on the "Google Sheets Trigger" node
   - Click "Create New Credential"
   - Follow Google OAuth flow to authorize

2. **Google Sheets OAuth2**:
   - Click on "Store in Leave Sheet" node
   - Create credential and authorize

3. **Gmail OAuth2**:
   - Click on any email node
   - Create credential and authorize Gmail

### Step 5: Configure Nodes
1. **Google Sheets Trigger** node:
   - Select your Form Response sheet
   - Choose "Form Responses 1" tab
   - Set event to "rowAdded"

2. **Store in Leave Sheet** node:
   - Select your Leave Requests tracking sheet
   - Verify column mappings

3. **Send Email to HR** node:
   - Update the `sendTo` email address with your HR team email

4. **Email Admin on Error** node:
   - Update the `sendTo` email address with your admin email

### Step 6: Test the Workflow
1. Submit a test leave request through your Google Form:
   ```
   Name: Test Employee
   Email: test@company.com
   Type of leave: Vacation
   Leave From Date: 2026-02-01
   Leave To Date: 2026-02-03
   Full Day/Half Day: Full Day
   Reason: Testing the workflow
   ```
2. Wait up to 1 minute for the trigger
3. Verify:
   - Request appears in Leave Requests sheet
   - HR receives notification email
   - Employee receives confirmation email

### Step 7: Activate the Workflow
1. Toggle the "Active" switch in the top-right corner
2. The workflow will now run automatically

## Email Templates

### HR Notification Email
```
Subject: New Leave Request: [Employee Name]

New Leave Request Submitted

Employee: [Employee Name]
Leave Type: [Leave Type]
From: [From Date]
To: [To Date]
Reason: [Reason]
Status: Pending

Please review and approve/reject this request in the Leave Requests sheet.
```

### Employee Confirmation Email
```
Subject: Leave Request Confirmation

Dear [Employee Name],

We are pleased to inform you that your leave request has been approved.

Leave Type: [Leave Type]
From: [From Date]
To: [To Date]
Reason: [Reason]

Please ensure all handovers are completed before your leave period begins.

Enjoy your time off!

Best regards,
HR Team
```

### Admin Error Alert
```
Subject: ERROR: Leave Request Sheet Update Failed

Leave Request Processing Error

Failed to update the Leave Requests sheet.

Employee: [Employee Name]
Leave Type: [Leave Type]
From: [From Date]
To: [To Date]

Error Details: Please check the workflow execution logs.
```

## Customization Options

### Add Approval Workflow
1. Add a column "Approval Status" to the Leave Requests sheet
2. Create a second workflow that monitors status changes
3. Send different emails based on approval/rejection

### Calculate Leave Balance
1. Add a "Leave Balance" sheet
2. Fetch employee's remaining leave days
3. Include balance in confirmation email
4. Deduct days when approved

### Manager Approval
1. Add "Manager Email" field to the form
2. Send approval request to manager instead of HR
3. Update status based on manager's response

### Calendar Integration
1. Add Google Calendar node
2. Create calendar event when leave is approved
3. Share with team for visibility

### Conditional Notifications
- Different email templates for different leave types
- Escalate long leave requests (>5 days) to senior management
- Auto-approve half-day requests

### Multi-Language Support
1. Add "Preferred Language" field to form
2. Store multiple email templates
3. Send emails in employee's preferred language

## Advanced Features

### Leave Balance Tracking
```
1. Create "Leave Balance" sheet with employee data
2. Fetch current balance when request is submitted
3. Check if employee has sufficient leave days
4. Reject automatically if insufficient balance
5. Update balance when approved
```

### Automatic Approval Rules
- Auto-approve requests < 2 days
- Auto-approve if submitted > 30 days in advance
- Require manager approval for > 5 days

### Team Calendar View
1. Create a shared Google Calendar
2. Add approved leave as events
3. Color-code by leave type
4. Share with entire team

### Reporting Dashboard
Track metrics like:
- Total leave days taken per employee
- Most common leave types
- Peak leave periods
- Approval rates

### Slack Integration
1. Add Slack node
2. Post leave requests to HR channel
3. Allow approval via Slack buttons
4. Update status automatically

## Troubleshooting

### Workflow Not Triggering
- Verify Google Sheets Trigger credential is valid
- Check that the correct form response sheet is selected
- Ensure the workflow is activated
- Verify polling interval

### Emails Not Sending
- Verify Gmail OAuth2 credentials
- Check spam/junk folders
- Ensure email addresses are valid
- Check Gmail sending limits

### Data Not Saving to Leave Sheet
- Verify Google Sheets OAuth2 credential
- Check column names match exactly (case-sensitive)
- Ensure the correct sheet tab is selected
- Review execution log for errors

### Wrong Data in Emails
- Verify form field names match the workflow configuration
- Check the "Workflow Configuration" node mappings
- Ensure form response sheet column names are correct

### Duplicate Entries
- Check if multiple workflows are monitoring the same form
- Verify the trigger is set to "rowAdded" only
- Consider adding a unique ID column

## Best Practices
- **Test thoroughly** with sample data before going live
- **Communicate** the new process to all employees
- **Train HR staff** on using the tracking sheet
- **Set clear expectations** for approval timelines
- **Regularly review** leave balances and policies
- **Archive old requests** periodically
- **Backup** the leave tracking sheet regularly
- **Monitor** workflow execution for errors

## Integration Ideas
- **Payroll System**: Sync approved leave with payroll
- **Project Management**: Update project timelines when team members are on leave
- **Slack/Teams**: Notify team channels of upcoming absences
- **Calendar**: Block out leave days automatically
- **HR System**: Sync with HRIS for comprehensive tracking

## Compliance Considerations
- Ensure data privacy (GDPR, etc.)
- Maintain leave request records as required by law
- Provide audit trail for all approvals/rejections
- Secure access to leave data
- Follow company leave policies

## Support
For issues or questions:
1. Check n8n documentation: https://docs.n8n.io
2. Review workflow execution logs in n8n
3. Verify all credentials are valid and not expired
4. Contact your n8n administrator
