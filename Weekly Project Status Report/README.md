# Automated Weekly Project Status Report to Leadership

## Overview
This n8n workflow automates the creation and distribution of weekly project status reports to leadership, providing consistent updates on project progress, risks, and key metrics.

## What This Workflow Does
- **Runs** automatically every week on a scheduled day/time
- **Collects** project data from tracking sheets
- **Compiles** comprehensive status report
- **Sends** formatted report to leadership team
- **Tracks** project health and trends over time

## Workflow Steps

### 1. **Trigger: Weekly Schedule**
   - Runs automatically every week (e.g., Friday at 4 PM)
   - Configurable day and time

### 2. **Fetch Project Data**
   - Retrieves all active projects from Google Sheet
   - Collects:
     - Project names and IDs
     - Current status
     - Progress percentage
     - Budget utilization
     - Risks and issues
     - Upcoming milestones

### 3. **Calculate Metrics**
   - Computes summary statistics:
     - Total active projects
     - On-track vs. at-risk projects
     - Budget variance
     - Timeline adherence
     - Completion rate

### 4. **Format Status Report**
   - Creates professional HTML email report
   - Includes:
     - Executive summary
     - Project-by-project status
     - Key metrics and KPIs
     - Risks and issues
     - Upcoming milestones
     - Recommendations

### 5. **Send Report to Leadership**
   - Emails report to leadership team
   - CC's relevant stakeholders
   - Attaches detailed data (optional)

## Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- Google account with access to Google Sheets
- Gmail account for sending reports

### Step 1: Import the Workflow
1. Open your n8n instance
2. Click on "Workflows" → "Import from File"
3. Select `Automated Weekly Project Status Report to Leadership.json`
4. Click "Import"

### Step 2: Create Project Tracking Sheet
1. Create a new Google Sheet named "Project Status Tracker"
2. Add the following columns:
   - `Project ID`
   - `Project Name`
   - `Project Manager`
   - `Status` (On Track, At Risk, Delayed, Completed)
   - `Progress %` (0-100)
   - `Budget` (Total budget)
   - `Spent` (Amount spent)
   - `Start Date`
   - `End Date`
   - `Current Phase`
   - `Risks/Issues`
   - `Next Milestone`
   - `Milestone Date`

**Example Sheet Structure:**
```
| Project ID | Project Name     | PM         | Status    | Progress % | Budget   | Spent    | Risks/Issues          |
|------------|------------------|------------|-----------|------------|----------|----------|-----------------------|
| PRJ-001    | Website Redesign | Jane Doe   | On Track  | 75%        | $50,000  | $35,000  | None                  |
| PRJ-002    | Mobile App       | John Smith | At Risk   | 45%        | $100,000 | $60,000  | Resource shortage     |
| PRJ-003    | Data Migration   | Alice Lee  | Delayed   | 30%        | $75,000  | $45,000  | Technical challenges  |
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
   - Set day of week (e.g., Friday)
   - Set time (e.g., 4:00 PM)
   - Choose timezone
   - Verify schedule is correct

2. **Fetch Project Data** node:
   - Select your Project Status Tracker sheet
   - Choose the appropriate sheet tab
   - Verify all columns are mapped

3. **Send Report to Leadership** node:
   - Update `sendTo` with leadership email addresses (comma-separated)
   - Update `cc` if needed
   - Customize sender name

### Step 5: Test the Workflow
1. Populate your sheet with sample project data
2. Click "Execute Workflow" to test manually
3. Verify the report email is received
4. Check that all data is formatted correctly

### Step 6: Activate the Workflow
1. Toggle the "Active" switch
2. The workflow will now run automatically on schedule

## Report Template

### Weekly Project Status Report Email
```
Subject: Weekly Project Status Report - Week of [Date]

EXECUTIVE SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Total Active Projects: [Count]
✅ On Track: [Count] ([Percentage]%)
⚠️ At Risk: [Count] ([Percentage]%)
🔴 Delayed: [Count] ([Percentage]%)
✓ Completed This Week: [Count]

Overall Budget Utilization: [Percentage]%
Projects Over Budget: [Count]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PROJECT DETAILS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[For each project:]

📊 [Project Name] (PRJ-XXX)
   Project Manager: [Name]
   Status: [Status Icon] [Status]
   Progress: [Progress Bar] [Percentage]%
   Budget: $[Spent] / $[Total] ([Utilization]%)
   Current Phase: [Phase]
   Next Milestone: [Milestone] - [Date]
   Risks/Issues: [Description or "None"]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

KEY HIGHLIGHTS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✨ Achievements This Week:
   • [Achievement 1]
   • [Achievement 2]

⚠️ Attention Required:
   • [Issue 1]
   • [Issue 2]

📅 Upcoming Milestones (Next 2 Weeks):
   • [Milestone 1] - [Project] - [Date]
   • [Milestone 2] - [Project] - [Date]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

RECOMMENDATIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[Automated or manual recommendations based on data]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

For detailed project information, please refer to the Project Status Tracker.

Best regards,
Project Management Office
```

## Customization Options

### Add Visual Charts
1. Use a charting service API (e.g., QuickChart)
2. Generate:
   - Project status pie chart
   - Budget utilization bar chart
   - Timeline Gantt chart
   - Trend graphs
3. Embed images in email

### Conditional Formatting
- Highlight at-risk projects in red
- Show on-track projects in green
- Bold overdue milestones
- Add warning icons for budget overruns

### Custom Metrics
Add calculations for:
- Velocity (progress per week)
- Burn rate (spending per week)
- Estimated completion date
- ROI projections
- Resource utilization

### Multi-Sheet Aggregation
1. Collect data from multiple sheets:
   - Different departments
   - Different project types
   - Different regions
2. Aggregate into single report
3. Include department-specific sections

### Recipient Customization
- Different reports for different stakeholders
- Executive summary for C-level
- Detailed reports for project managers
- Department-specific reports

## Advanced Features

### Trend Analysis
```
1. Store historical data in a separate sheet
2. Compare week-over-week changes
3. Identify trends:
   - Improving projects
   - Declining projects
   - Budget trends
4. Include trend indicators in report (↑↓→)
```

### Automated Alerts
- Send immediate alerts for critical issues
- Notify if project status changes to "At Risk"
- Alert when budget exceeds threshold
- Warn about upcoming deadline misses

### Interactive Dashboard
1. Create Google Data Studio dashboard
2. Link to live project data
3. Include dashboard link in email
4. Auto-refresh data

### Attachment Options
- Export detailed data as CSV
- Attach project timeline PDF
- Include risk register
- Add budget breakdown spreadsheet

### AI-Powered Insights
1. Integrate with AI service (OpenAI, etc.)
2. Generate insights from project data
3. Provide recommendations
4. Predict project outcomes

## Use Cases

### 1. Executive Leadership
- High-level portfolio overview
- Strategic decision support
- Resource allocation insights

### 2. PMO (Project Management Office)
- Detailed project tracking
- Risk management
- Capacity planning

### 3. Department Heads
- Department-specific projects
- Resource utilization
- Budget management

### 4. Stakeholders
- Project transparency
- Progress visibility
- Milestone tracking

## Troubleshooting

### Workflow Not Running on Schedule
- Verify workflow is activated
- Check Schedule Trigger configuration
- Ensure timezone is correct
- Review execution history

### Missing Project Data
- Verify Google Sheets credential
- Check column names match exactly
- Ensure sheet tab is correct
- Review filter conditions

### Report Formatting Issues
- Check HTML email template
- Verify data types are correct
- Test with sample data
- Review email client compatibility

### Email Not Delivered
- Verify Gmail OAuth2 credential
- Check recipient email addresses
- Review Gmail sending limits
- Check spam folders

## Best Practices
- **Consistent data entry**: Standardize how project data is entered
- **Regular updates**: Ensure project managers update status weekly
- **Clear status definitions**: Define what "On Track", "At Risk", etc. mean
- **Timely delivery**: Send reports at consistent times
- **Actionable insights**: Include recommendations, not just data
- **Follow up**: Address issues raised in reports
- **Archive reports**: Keep historical records
- **Solicit feedback**: Improve report format based on leadership input

## Metrics to Track
- **Report open rate**: Are leaders reading the reports?
- **Response time**: How quickly are issues addressed?
- **Project success rate**: Correlation with reporting
- **Data accuracy**: Validate against actual outcomes
- **Stakeholder satisfaction**: Survey recipients

## Integration Ideas
- **Slack**: Post summary to leadership channel
- **Microsoft Teams**: Share report in Teams
- **PowerBI**: Link to interactive dashboard
- **Jira**: Pull data directly from Jira
- **Asana**: Sync with Asana project data

## Support
For issues or questions:
1. Check n8n documentation: https://docs.n8n.io
2. Review workflow execution logs in n8n
3. Verify all credentials are valid and not expired
4. Contact your n8n administrator
