# n8n Workflows Repository

Welcome to the n8n Workflows Repository! This collection contains 7 production-ready automation workflows designed to streamline common business processes including HR operations, IT asset management, project management, and financial operations.

## 📚 Table of Contents
- [About n8n](#about-n8n)
- [n8n Core Components](#n8n-core-components)
- [Repository Overview](#repository-overview)
- [Available Workflows](#available-workflows)
- [Getting Started](#getting-started)
- [Prerequisites](#prerequisites)
- [Installation Guide](#installation-guide)
- [Best Practices](#best-practices)
- [Support & Resources](#support--resources)

---

## 🤖 About n8n

**n8n** (pronounced "n-eight-n") is a powerful, open-source workflow automation tool that allows you to connect various apps and services to automate repetitive tasks without writing code. It's an excellent alternative to tools like Zapier, Make (formerly Integromat), or Microsoft Power Automate.

### Why n8n?

- **Open Source**: Free to use, self-hostable, and fully transparent
- **No Code/Low Code**: Visual workflow builder with drag-and-drop interface
- **Extensible**: 400+ integrations and the ability to write custom nodes
- **Self-Hosted**: Full control over your data and workflows
- **Fair Pricing**: Cloud option available with generous free tier
- **Active Community**: Large community with shared workflows and support

### Key Features

✅ **Visual Workflow Editor**: Build automations with an intuitive drag-and-drop interface  
✅ **Conditional Logic**: Add IF/THEN conditions, switches, and filters  
✅ **Data Transformation**: Manipulate and format data between services  
✅ **Error Handling**: Built-in error triggers and retry mechanisms  
✅ **Scheduling**: Run workflows on schedules or trigger them via webhooks  
✅ **Debugging**: Test workflows step-by-step with execution logs  
✅ **Version Control**: Export workflows as JSON for backup and sharing  

---

## 🧩 n8n Core Components

Understanding these core components will help you work with the workflows in this repository:

### 1. **Nodes**
Nodes are the building blocks of n8n workflows. Each node performs a specific action.

**Types of Nodes:**
- **Trigger Nodes**: Start a workflow (e.g., Schedule, Webhook, Google Sheets Trigger)
- **Regular Nodes**: Perform actions (e.g., Send Email, Update Database, HTTP Request)
- **Core Nodes**: Built-in utilities (e.g., Set, IF, Filter, Code)

**Common Nodes Used in This Repository:**
- `Google Sheets Trigger`: Monitors Google Sheets for new rows or changes
- `Google Sheets`: Read/write data to Google Sheets
- `Gmail`: Send emails via Gmail
- `Set`: Transform and map data between nodes
- `IF`: Conditional branching based on data
- `Filter`: Filter out items that don't meet criteria
- `Error Trigger`: Handle errors gracefully

### 2. **Workflows**
A workflow is a sequence of connected nodes that automate a process from start to finish.

**Workflow Structure:**
```
Trigger Node → Processing Nodes → Action Nodes → Output
```

**Example:**
```
Google Sheets Trigger → Set Variables → IF Condition → Send Email → Update Sheet
```

### 3. **Credentials**
Credentials store authentication information for connecting to external services.

**Common Credential Types:**
- **OAuth2**: Google Sheets, Gmail, Slack (most secure)
- **API Key**: Many REST APIs
- **Username/Password**: Basic authentication

**Security Note**: Credentials are encrypted and stored securely in n8n.

### 4. **Expressions**
Expressions allow you to dynamically reference data from previous nodes.

**Syntax:**
```javascript
{{ $json.fieldName }}              // Access current item data
{{ $node["Node Name"].json.field }} // Access specific node data
{{ $now.toISO() }}                  // Current timestamp
{{ $json.amount * 1.1 }}            // Calculations
```

### 5. **Executions**
Each time a workflow runs, it creates an execution with a complete log of:
- Input data for each node
- Output data from each node
- Errors (if any)
- Execution time

### 6. **Connections**
Connections link nodes together and define the flow of data.

**Connection Types:**
- **Main**: Standard data flow (green line)
- **Error**: Error handling path (red line)

---

## 📁 Repository Overview

This repository contains **7 automated workflows** organized by business function:

```
n8n Workflows/
├── Asset Allocation Tracker/
│   ├── IT Asset Assignment and Inventory Notification System.json
│   └── README.md
├── Employee Onboarding (Mini Version)/
│   ├── Automated Employee Onboarding Communication and IT Notification.json
│   └── README.md
├── Google Sheet Email Notification/
│   ├── Google Sheet New Row Email Notification.json
│   └── README.md
├── Invoice Due Reminder/
│   ├── Daily Overdue Invoice Payment Reminder System.json
│   └── README.md
├── Leave Request Workflow/
│   ├── Employee Leave Request Processing and Notification System.json
│   └── README.md
├── Project Kickoff Automation/
│   ├── Approved Project Kickoff and Stakeholder Notification.json
│   └── README.md
└── Weekly Project Status Report/
    ├── Automated Weekly Project Status Report to Leadership.json
    └── README.md
```

### Workflow Categories

**🏢 HR & People Operations**
- Employee Onboarding
- Leave Request Workflow

**💻 IT Operations**
- Asset Allocation Tracker

**📊 Project Management**
- Project Kickoff Automation
- Weekly Project Status Report

**💰 Finance & Accounting**
- Invoice Due Reminder

**🔔 General Automation**
- Google Sheet Email Notification

---

## 🚀 Available Workflows

### 1. **Asset Allocation Tracker**
**Purpose**: Automate IT asset assignment and inventory management

**What it does:**
- Monitors new asset assignments in Google Sheets
- Validates Asset IDs to prevent duplicates
- Updates central inventory automatically
- Sends notifications to employees, IT team, and admins

**Use cases:**
- IT asset management
- Equipment tracking
- Hardware assignment
- Inventory control

📖 [View detailed documentation](Asset%20Allocation%20Tracker/README.md)

---

### 2. **Employee Onboarding (Mini Version)**
**Purpose**: Streamline new employee onboarding process

**What it does:**
- Monitors onboarding sheet for new hires
- Sends personalized welcome emails to employees
- Notifies IT team to prepare equipment and accounts
- Automates initial communication

**Use cases:**
- HR onboarding automation
- New hire communication
- IT setup coordination
- Welcome email automation

📖 [View detailed documentation](Employee%20Onboarding%20%28Mini%20Version%29/README.md)

---

### 3. **Google Sheet Email Notification**
**Purpose**: Real-time email notifications for Google Sheet updates

**What it does:**
- Monitors any Google Sheet for new rows
- Sends formatted email notifications
- Includes error handling with admin alerts
- Works with forms, manual entries, or API updates

**Use cases:**
- Form response notifications
- Lead tracking alerts
- Support ticket creation
- Event registration notifications
- General data entry alerts

📖 [View detailed documentation](Google%20Sheet%20Email%20Notification/README.md)

---

### 4. **Invoice Due Reminder**
**Purpose**: Automate payment reminders for overdue invoices

**What it does:**
- Runs daily to check for overdue invoices
- Sends professional payment reminder emails
- Tracks reminder history
- Calculates days overdue automatically

**Use cases:**
- Accounts receivable management
- Payment collection
- Cash flow improvement
- Customer payment reminders

📖 [View detailed documentation](Invoice%20Due%20Reminder/README.md)

---

### 5. **Leave Request Workflow**
**Purpose**: Automate employee leave request processing

**What it does:**
- Monitors Google Form submissions
- Stores requests in centralized tracking sheet
- Notifies HR team of new requests
- Sends confirmation emails to employees

**Use cases:**
- Leave management
- Time-off tracking
- HR request processing
- Employee self-service

📖 [View detailed documentation](Leave%20Request%20Workflow/README.md)

---

### 6. **Project Kickoff Automation**
**Purpose**: Automate project kickoff communications

**What it does:**
- Monitors project tracker for approved projects
- Sends kickoff emails to project teams
- Notifies stakeholders with executive summary
- Can create project documentation automatically

**Use cases:**
- Project management
- Stakeholder communication
- Team coordination
- Project initiation

📖 [View detailed documentation](Project%20Kickoff%20Automation/README.md)

---

### 7. **Weekly Project Status Report**
**Purpose**: Automated weekly project status reporting

**What it does:**
- Runs on weekly schedule
- Collects data from project tracking sheet
- Calculates metrics and KPIs
- Sends formatted HTML report to leadership

**Use cases:**
- Executive reporting
- Project portfolio management
- Status tracking
- Leadership dashboards

📖 [View detailed documentation](Weekly%20Project%20Status%20Report/README.md)

---

## 🎯 Getting Started

### Prerequisites

Before using these workflows, you'll need:

1. **n8n Instance**
   - Option A: [n8n Cloud](https://n8n.io/cloud/) (easiest, free tier available)
   - Option B: Self-hosted (Docker, npm, or desktop app)

2. **Google Account**
   - For Google Sheets and Gmail integration
   - Admin access to create OAuth credentials

3. **Basic Understanding**
   - Familiarity with Google Sheets
   - Basic understanding of email automation
   - Willingness to learn n8n interface

### Quick Start Guide

#### Step 1: Set Up n8n

**Option A: n8n Cloud (Recommended for Beginners)**
1. Go to [n8n.io/cloud](https://n8n.io/cloud/)
2. Sign up for a free account
3. Access your n8n instance via web browser

**Option B: Self-Hosted with Docker**
```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

**Option C: Self-Hosted with npm**
```bash
npm install n8n -g
n8n start
```

Access n8n at: `http://localhost:5678`

#### Step 2: Import a Workflow

1. Download a workflow JSON file from this repository
2. In n8n, click **"Workflows"** → **"Import from File"**
3. Select the downloaded JSON file
4. Click **"Import"**

#### Step 3: Configure Credentials

1. Click on any node that requires credentials (red warning icon)
2. Click **"Create New Credential"**
3. Follow the OAuth flow or enter API credentials
4. Test the connection

#### Step 4: Configure the Workflow

1. Update any hardcoded email addresses
2. Select your Google Sheets
3. Customize email templates
4. Test with sample data

#### Step 5: Activate

1. Click **"Execute Workflow"** to test manually
2. Verify everything works correctly
3. Toggle **"Active"** switch to enable automation

---

## 📖 Installation Guide

### Detailed Setup for Google Integrations

Most workflows in this repository use Google Sheets and Gmail. Here's how to set them up:

#### Google Sheets Setup

1. **Create OAuth2 Credentials** (one-time setup):
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project or select existing
   - Enable Google Sheets API
   - Create OAuth 2.0 credentials
   - Add authorized redirect URI: `https://your-n8n-instance/rest/oauth2-credential/callback`

2. **In n8n**:
   - Click on Google Sheets node
   - Select "Create New Credential"
   - Choose "Google Sheets OAuth2 API"
   - Enter Client ID and Client Secret
   - Click "Connect my account"
   - Authorize access

#### Gmail Setup

1. **Enable Gmail API** in Google Cloud Console
2. **In n8n**:
   - Click on Gmail node
   - Create "Gmail OAuth2" credential
   - Follow same OAuth flow as Google Sheets

### Workflow-Specific Setup

Each workflow has unique requirements. Refer to the individual README files for:
- Required Google Sheet structure
- Column names and data formats
- Email template customization
- Specific node configurations

---

## ✅ Best Practices

### Workflow Management

1. **Test Before Activating**
   - Always test with sample data first
   - Verify all emails are sent correctly
   - Check data is written to sheets properly

2. **Use Descriptive Names**
   - Rename nodes to describe their purpose
   - Add notes to complex logic
   - Document any customizations

3. **Error Handling**
   - Implement error triggers where appropriate
   - Set up admin notifications for failures
   - Monitor execution logs regularly

4. **Version Control**
   - Export workflows as JSON files regularly
   - Keep backups of working configurations
   - Document changes in commit messages

### Security

1. **Protect Credentials**
   - Never share credential files
   - Use OAuth2 when possible
   - Rotate API keys periodically

2. **Data Privacy**
   - Be mindful of sensitive data in workflows
   - Use appropriate access controls
   - Comply with GDPR/privacy regulations

3. **Access Control**
   - Limit who can edit workflows
   - Use separate credentials for production
   - Audit workflow changes

### Performance

1. **Optimize Polling**
   - Don't poll too frequently (respect API limits)
   - Use webhooks when available
   - Batch operations when possible

2. **Monitor Resources**
   - Check execution times
   - Optimize large data operations
   - Archive old execution data

### Maintenance

1. **Regular Reviews**
   - Review execution logs weekly
   - Update email templates as needed
   - Verify credentials haven't expired

2. **Documentation**
   - Keep README files updated
   - Document customizations
   - Share knowledge with team

3. **Testing**
   - Test after n8n updates
   - Verify integrations still work
   - Update deprecated nodes

---

## 🔧 Common Customizations

### Changing Email Recipients

In any email node:
```javascript
// Single recipient
"sendTo": "user@example.com"

// Multiple recipients
"sendTo": "user1@example.com, user2@example.com"

// Dynamic from data
"sendTo": "={{ $json.email }}"
```

### Modifying Schedules

In Schedule Trigger nodes:
- Every minute: Quick testing, high resource usage
- Every 5 minutes: Balanced for most use cases
- Hourly: Low-priority notifications
- Daily: Reports and summaries
- Custom cron: Advanced scheduling

### Adding Conditional Logic

Use IF nodes to create branches:
```javascript
// Check if value exists
{{ $json.fieldName }}

// Compare values
{{ $json.amount > 1000 }}

// Multiple conditions
{{ $json.status === "Approved" && $json.amount < 5000 }}
```

---

## 🆘 Support & Resources

### Official n8n Resources

- **Documentation**: [docs.n8n.io](https://docs.n8n.io)
- **Community Forum**: [community.n8n.io](https://community.n8n.io)
- **YouTube Channel**: [n8n YouTube](https://www.youtube.com/c/n8n-io)
- **GitHub**: [github.com/n8n-io/n8n](https://github.com/n8n-io/n8n)

### Learning Resources

- **n8n Academy**: Free courses and tutorials
- **Workflow Templates**: Browse 1000+ pre-built workflows
- **Blog**: Tips, tricks, and use cases
- **Discord**: Real-time community support

### Troubleshooting

**Common Issues:**

1. **Workflow not triggering**
   - Check if workflow is activated
   - Verify credentials are valid
   - Review trigger node configuration

2. **Emails not sending**
   - Check Gmail OAuth2 credential
   - Verify email addresses are correct
   - Check spam folders
   - Review Gmail sending limits

3. **Data not updating in sheets**
   - Verify Google Sheets credential
   - Check column names match exactly (case-sensitive)
   - Ensure correct sheet tab is selected

4. **Execution errors**
   - Review execution log for details
   - Check node configuration
   - Verify data format matches expectations

### Getting Help

1. **Check the README**: Each workflow has detailed documentation
2. **Review Execution Logs**: Most issues are visible in logs
3. **Search Community Forum**: Someone may have solved your issue
4. **Ask the Community**: Post on forum or Discord
5. **GitHub Issues**: Report bugs or request features

---

## 🤝 Contributing

If you create improvements or new workflows:

1. Test thoroughly with sample data
2. Document setup instructions
3. Include email templates
4. Add troubleshooting section
5. Share with the community!

---

## 📄 License

These workflows are provided as-is for educational and commercial use. Feel free to modify and adapt them to your needs.

---

## 🙏 Acknowledgments

- Built with [n8n](https://n8n.io) - Fair-code workflow automation
- Powered by Google Workspace integrations
- Inspired by the n8n community

---

## 📞 Contact

For questions about these specific workflows, please refer to individual workflow README files or open an issue in the repository.

**Happy Automating! 🚀**
