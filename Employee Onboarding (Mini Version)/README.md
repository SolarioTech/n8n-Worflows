# Automated Employee Onboarding Communication and IT Notification

## Overview
This n8n workflow streamlines the employee onboarding process by automatically sending welcome emails to new employees and notifying the IT team to prepare necessary equipment and accounts.

## What This Workflow Does
- **Monitors** a Google Sheet for new employee entries
- **Sends** personalized welcome emails to new employees
- **Notifies** IT team to set up accounts and equipment
- **Automates** the initial onboarding communication process

## Workflow Steps

### 1. **Trigger: New Employee Entry**
   - Monitors the onboarding Google Sheet every minute
   - Triggers when a new employee row is added

### 2. **Workflow Configuration**
   - Extracts employee data from the new row:
     - Employee Name
     - Employee Email
     - Start Date
     - Department
     - Position
   - Sets IT team and HR email addresses

### 3. **Send Welcome Email to Employee**
   - Sends a personalized welcome email to the new employee
   - Includes:
     - Welcome message
     - Start date confirmation
     - Department and position details
     - Next steps information

### 4. **Notify IT Team**
   - Sends notification to IT team with:
     - New employee details
     - Equipment preparation requirements
     - Account setup needs
     - Start date for preparation timeline

## Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- Google account with access to Google Sheets
- Gmail account for sending notifications

### Step 1: Import the Workflow
1. Open your n8n instance
2. Click on "Workflows" → "Import from File"
3. Select `Automated Employee Onboarding Communication and IT Notification.json`
4. Click "Import"

### Step 2: Create Google Sheet
1. Create a new Google Sheet named "Employee Onboarding"
2. Add the following columns:
   - `Employee Name`
   - `Employee Email`
   - `Start Date`
   - `Department`
   - `Position`
   - `Manager Name` (optional)

### Step 3: Set Up Credentials
1. **Google Sheets Trigger OAuth2**:
   - Click on the trigger node
   - Click "Create New Credential"
   - Follow Google OAuth flow to authorize

2. **Gmail OAuth2**:
   - Click on any email node
   - Create credential and authorize Gmail

### Step 4: Configure Nodes
1. **Trigger Node**:
   - Select your Employee Onboarding Google Sheet
   - Choose the appropriate sheet tab
   - Set polling interval (default: every minute)

2. **Workflow Configuration** node:
   - Update `itTeamEmail` with your IT team email
   - Update `hrEmail` with your HR email (if needed)

3. **Email Nodes**:
   - Customize email templates as needed
   - Adjust subject lines and message content

### Step 5: Test the Workflow
1. Click "Execute Workflow" to test manually
2. Add a test employee to your Google Sheet:
   ```
   Employee Name: Jane Smith
   Employee Email: jane.smith@company.com
   Start Date: 2026-01-15
   Department: Marketing
   Position: Marketing Manager
   ```
3. Wait up to 1 minute for the trigger
4. Verify both emails are sent

### Step 6: Activate the Workflow
1. Toggle the "Active" switch in the top-right corner
2. The workflow will now run automatically

## Email Templates

### Welcome Email to Employee
```
Subject: Welcome to [Company Name]!

Dear [Employee Name],

Welcome to [Company Name]! We're excited to have you join our team.

Your Details:
- Start Date: [Start Date]
- Department: [Department]
- Position: [Position]

Before your first day:
- Check your email for IT setup instructions
- Complete any pre-boarding forms sent by HR
- Prepare any questions for your manager

We look forward to seeing you on [Start Date]!

Best regards,
HR Team
```

### IT Team Notification
```
Subject: New Employee Setup Required - [Employee Name]

A new employee is joining and requires IT setup:

Employee Details:
- Name: [Employee Name]
- Email: [Employee Email]
- Start Date: [Start Date]
- Department: [Department]
- Position: [Position]

Please prepare:
- Email account setup
- System access and permissions
- Hardware (laptop, monitor, accessories)
- Software licenses
- Department-specific tools

Start Date: [Start Date]
```

## Customization Options

### Add More Notification Recipients
1. Duplicate email nodes
2. Update the `sendTo` parameter (e.g., manager, department head)
3. Customize the email content for each recipient

### Include Additional Employee Information
1. Add columns to your Google Sheet
2. Update the "Workflow Configuration" node to capture new fields
3. Include new fields in email templates

### Change Polling Frequency
- Edit the trigger node
- Modify poll time (every minute, every 5 minutes, hourly, etc.)

### Add Conditional Logic
- Insert an IF node to handle different departments differently
- Route to different email templates based on position level
- Add special handling for remote vs. on-site employees

### Integrate with Other Systems
- Add nodes to create accounts in other systems (Slack, project management tools)
- Connect to HR management systems
- Trigger additional workflows for specific departments

## Advanced Features

### Add Onboarding Checklist
1. Create a second Google Sheet for onboarding tasks
2. Add a node to populate the checklist when a new employee is added
3. Include checklist link in the welcome email

### Schedule Follow-up Emails
1. Add a "Wait" node after the initial emails
2. Send a follow-up email 1 day before start date
3. Send a "first day" email on the start date

### Track Onboarding Status
1. Add a "Status" column to your Google Sheet
2. Update the status after each step completes
3. Create a dashboard to monitor onboarding progress

## Troubleshooting

### Workflow Not Triggering
- Verify Google Sheets Trigger credential is valid
- Check that the correct sheet and tab are selected
- Ensure the workflow is activated

### Emails Not Sending
- Verify Gmail OAuth2 credentials are connected
- Check spam/junk folders
- Ensure email addresses are valid

### Missing Data in Emails
- Verify column names in Google Sheet match exactly (case-sensitive)
- Check the "Workflow Configuration" node mappings
- Review the execution log for errors

### Duplicate Emails Being Sent
- Check if the workflow was triggered multiple times
- Verify the Google Sheets trigger is set to "rowAdded" event only
- Consider adding a filter to check for already-processed entries

## Best Practices
- Test the workflow thoroughly before activating
- Keep email templates professional and welcoming
- Update IT team email if team members change
- Regularly review and update onboarding information
- Archive old employee entries periodically
- Coordinate with HR to ensure data accuracy

## Integration Ideas
- **Slack**: Send welcome message to company Slack channel
- **Jira/Asana**: Create onboarding tasks automatically
- **Calendar**: Schedule first-day meetings
- **Badge System**: Trigger badge creation for office access
- **Learning Platform**: Enroll in onboarding courses

## Support
For issues or questions:
1. Check n8n documentation: https://docs.n8n.io
2. Review workflow execution logs in n8n
3. Verify all credentials are valid and not expired
4. Contact your n8n administrator
