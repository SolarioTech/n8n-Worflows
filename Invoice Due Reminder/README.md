# Daily Overdue Invoice Payment Reminder System

## Overview
This n8n workflow automates the process of sending payment reminders for overdue invoices. It monitors an invoice tracking sheet daily and sends reminder emails to customers with outstanding payments.

## What This Workflow Does
- **Monitors** an invoice tracking Google Sheet daily
- **Identifies** overdue invoices automatically
- **Sends** professional payment reminder emails to customers
- **Tracks** reminder history and invoice status

## Workflow Steps

### 1. **Trigger: Daily Schedule**
   - Runs automatically every day at a specified time
   - Checks for overdue invoices

### 2. **Fetch Overdue Invoices**
   - Retrieves all invoices from the Google Sheet
   - Filters for invoices with "Overdue" or "Pending" status
   - Calculates days overdue

### 3. **Filter Overdue Invoices**
   - Checks if the due date has passed
   - Validates invoice data completeness
   - Ensures customer email is present

### 4. **Send Payment Reminder**
   - Sends personalized reminder email to customer
   - Includes:
     - Invoice number
     - Amount due
     - Original due date
     - Days overdue
     - Payment instructions

### 5. **Update Reminder Status**
   - Updates the invoice sheet with:
     - Last reminder sent date
     - Number of reminders sent
     - Current status

## Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- Google account with access to Google Sheets
- Gmail account for sending reminders

### Step 1: Import the Workflow
1. Open your n8n instance
2. Click on "Workflows" → "Import from File"
3. Select `Daily Overdue Invoice Payment Reminder System.json`
4. Click "Import"

### Step 2: Create Invoice Tracking Sheet
1. Create a new Google Sheet named "Invoice Tracker"
2. Add the following columns:
   - `Invoice Number`
   - `Customer Name`
   - `Customer Email`
   - `Amount`
   - `Issue Date`
   - `Due Date`
   - `Status` (Paid, Pending, Overdue)
   - `Last Reminder Sent`
   - `Reminders Count`

**Example Sheet Structure:**
```
| Invoice # | Customer Name | Customer Email      | Amount  | Issue Date | Due Date   | Status  | Last Reminder | Reminders |
|-----------|---------------|---------------------|---------|------------|------------|---------|---------------|-----------|
| INV-001   | Acme Corp     | billing@acme.com    | $5,000  | 2025-12-01 | 2025-12-31 | Overdue | 2026-01-05    | 2         |
| INV-002   | Tech LLC      | accounts@tech.com   | $3,500  | 2026-01-01 | 2026-01-31 | Pending |               | 0         |
```

### Step 3: Set Up Credentials
1. **Google Sheets OAuth2**:
   - Click on the Google Sheets node
   - Click "Create New Credential"
   - Follow Google OAuth flow to authorize

2. **Gmail OAuth2**:
   - Click on the Gmail node
   - Create credential and authorize Gmail

### Step 4: Configure Nodes
1. **Schedule Trigger** node:
   - Set the time to run daily (e.g., 9:00 AM)
   - Choose timezone
   - Set days to run (weekdays only or all days)

2. **Fetch Overdue Invoices** node:
   - Select your Invoice Tracker Google Sheet
   - Choose the appropriate sheet tab
   - Verify column mappings

3. **Send Payment Reminder** node:
   - Customize email template
   - Update sender name/signature
   - Add company logo or branding (optional)

4. **Update Reminder Status** node:
   - Select your Invoice Tracker sheet
   - Verify column mappings for updates

### Step 5: Test the Workflow
1. Add a test overdue invoice to your sheet:
   ```
   Invoice #: TEST-001
   Customer Name: Test Customer
   Customer Email: your-test-email@example.com
   Amount: $100
   Issue Date: 2025-12-01
   Due Date: 2025-12-31
   Status: Overdue
   ```
2. Click "Execute Workflow" to test manually
3. Verify the reminder email is sent
4. Check that the sheet is updated with reminder info

### Step 6: Activate the Workflow
1. Toggle the "Active" switch in the top-right corner
2. The workflow will now run automatically on schedule

## Email Template

### Payment Reminder Email
```
Subject: Payment Reminder - Invoice [Invoice Number]

Dear [Customer Name],

This is a friendly reminder that payment for the following invoice is now overdue:

Invoice Details:
- Invoice Number: [Invoice Number]
- Amount Due: [Amount]
- Original Due Date: [Due Date]
- Days Overdue: [Days Overdue]

Please arrange payment at your earliest convenience. If you have already sent payment, please disregard this reminder.

Payment Methods:
- Bank Transfer: [Account Details]
- Credit Card: [Payment Link]
- Check: [Mailing Address]

If you have any questions or concerns regarding this invoice, please don't hesitate to contact us.

Thank you for your prompt attention to this matter.

Best regards,
[Your Company Name]
Accounts Receivable Department
[Contact Information]
```

## Customization Options

### Change Schedule Frequency
- Edit the Schedule Trigger node
- Options:
  - Daily at specific time
  - Multiple times per day
  - Weekdays only
  - Custom cron expression

### Escalation Logic
Add conditional logic based on days overdue:

1. **1-7 days overdue**: Friendly reminder
2. **8-14 days overdue**: Urgent reminder
3. **15+ days overdue**: Final notice + CC manager

Implementation:
1. Add an IF node after filtering
2. Check `daysOverdue` value
3. Route to different email templates
4. Escalate to different recipients

### Multi-Currency Support
1. Add a "Currency" column to your sheet
2. Update the email template to include currency symbol
3. Format amounts based on currency

### Payment Link Integration
1. Add a "Payment Link" column
2. Include the link in the reminder email
3. Track clicks (if using a link shortener)

### Reminder Frequency Control
Prevent sending reminders too frequently:
1. Add a Filter node to check `Last Reminder Sent` date
2. Only send if X days have passed since last reminder
3. Customize frequency based on amount or customer tier

## Advanced Features

### Tiered Reminder System
```
Day 1 overdue: Gentle reminder
Day 7 overdue: Standard reminder
Day 14 overdue: Urgent reminder
Day 30 overdue: Final notice + late fee warning
Day 45 overdue: Collections notice
```

### Customer Segmentation
- VIP customers: More lenient reminders
- Standard customers: Regular reminders
- High-risk customers: Immediate reminders

### Reporting Dashboard
1. Create a summary sheet
2. Track:
   - Total overdue amount
   - Number of overdue invoices
   - Average days overdue
   - Reminder effectiveness rate

### Integration with Accounting Software
- Connect to QuickBooks, Xero, or FreshBooks
- Sync invoice data automatically
- Update payment status when received

### SMS Reminders
1. Add a Twilio node
2. Send SMS for high-priority invoices
3. Use for final reminders

## Troubleshooting

### Workflow Not Running on Schedule
- Verify the workflow is activated
- Check the Schedule Trigger configuration
- Ensure timezone is set correctly
- Review execution history for errors

### Emails Not Sending
- Verify Gmail OAuth2 credentials
- Check Gmail sending limits (500/day for free accounts)
- Ensure customer email addresses are valid
- Check spam folders

### Wrong Invoices Being Selected
- Verify the filter logic in the workflow
- Check date format in Google Sheet (YYYY-MM-DD)
- Ensure "Status" column values match exactly

### Sheet Not Updating
- Verify Google Sheets OAuth2 credential
- Check column names match exactly (case-sensitive)
- Ensure the correct sheet tab is selected

### Duplicate Reminders
- Check if multiple workflows are running
- Verify the "Last Reminder Sent" filter is working
- Review execution log for multiple triggers

## Best Practices
- **Test thoroughly** before activating with real customer data
- **Set appropriate reminder frequency** (not too aggressive)
- **Personalize emails** to maintain good customer relationships
- **Monitor response rates** and adjust messaging
- **Keep accurate records** of all reminders sent
- **Provide multiple payment options** for customer convenience
- **Include contact information** for questions
- **Respect business hours** when scheduling reminders
- **Comply with regulations** (e.g., GDPR, CAN-SPAM)

## Compliance Considerations
- Include unsubscribe option if required by law
- Respect customer communication preferences
- Maintain records of all communications
- Follow local debt collection regulations
- Provide clear dispute resolution process

## Metrics to Track
- **Reminder effectiveness rate**: % of invoices paid after reminder
- **Average days to payment**: After first, second, third reminder
- **Overdue amount trend**: Is it increasing or decreasing?
- **Customer response rate**: How many customers respond to reminders?

## Support
For issues or questions:
1. Check n8n documentation: https://docs.n8n.io
2. Review workflow execution logs in n8n
3. Verify all credentials are valid and not expired
4. Test with sample data first
