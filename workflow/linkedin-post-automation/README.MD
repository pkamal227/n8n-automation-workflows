# LinkedIn Post Generation & Approval Automation

Automate your LinkedIn content creation with AI-powered post generation and built-in approval workflow.

## 📋 Overview

This workflow streamlines LinkedIn content creation by combining AI generation, human approval, and automated publishing. It's perfect for marketing teams, social media managers, and content creators who want to maintain quality control while automating repetitive tasks.

## ✨ Features

- **AI Content Generation**: Uses OpenAI GPT-4o-mini to create professional LinkedIn posts
- **Approval Workflow**: Email-based approval with Yes/No/Cancel options and feedback
- **Google Sheets Integration**: Manage post ideas and track status in a spreadsheet
- **Automated Publishing**: Posts directly to LinkedIn after approval
- **Status Tracking**: Real-time updates on post status and published URLs
- **Customizable Prompts**: Tailor AI output with custom instructions per post

## 🎯 Use Cases

- Social media managers handling multiple LinkedIn accounts
- Marketing teams requiring content approval workflows
- Content creators maintaining consistent posting schedules
- Agencies managing client LinkedIn accounts
- Personal branding automation with quality control

## 🏗️ Workflow Architecture

Schedule Trigger → Get Pending Posts (Google Sheets) → Generate Content (OpenAI) → Format Data → Send Approval Email (Gmail) → Approval Logic (Switch) → [If Approved] Post to LinkedIn → Update Sheet (Completed)

## 📦 Prerequisites

### Required Services

1. **n8n** (v1.0.0+)
2. **Google Sheets** - Active Google account with spreadsheet created
3. **OpenAI API** - API key from https://platform.openai.com
4. **Gmail** - Gmail account for sending approval emails
5. **LinkedIn** - LinkedIn organization/company page with admin access

### Required n8n Credentials

- Google Sheets OAuth2
- OpenAI API Key
- Gmail OAuth2
- LinkedIn Community Management OAuth2

## 🚀 Installation

### Step 1: Import Workflow

1. Download `workflow.json` from this directory
2. Open your n8n instance
3. Click **Workflows** → **Import from File**
4. Select the downloaded JSON file
5. Click **Import**

### Step 2: Set Up Google Sheet

Create a Google Sheet with these columns (exact names):

| Column Name | Type | Description |
|-------------|------|-------------|
| Post Description | Text | Your post idea/topic |
| Instructions | Text | Specific instructions for AI |
| Status | Text | "Pending" or "Completed" |
| Output | Text | LinkedIn post URN (auto-filled) |
| Post Link | Text | Direct link to post (auto-filled) |
| row_number | Number | Row identifier (auto-filled) |

**Example Row:**
```
Post Description: "Share insights on zero-trust security architecture"
Instructions: "Write in conversational tone, include 3 key benefits, add relevant hashtags"
Status: "Pending"
```

### Step 3: Configure Credentials

#### Google Sheets OAuth2
1. In n8n: **Settings** → **Credentials** → **Add Credential**
2. Select **Google Sheets OAuth2 API**
3. Follow OAuth flow to authenticate
4. Save credential

#### OpenAI API
1. Get API key from https://platform.openai.com/api-keys
2. In n8n: Add **OpenAI Credentials**
3. Paste your API key
4. Save credential

#### Gmail OAuth2
1. In n8n: Add **Gmail OAuth2 Credentials**
2. Complete OAuth authentication
3. Grant necessary permissions
4. Save credential

#### LinkedIn Community Management
1. In n8n: Add **LinkedIn Community Management OAuth2**
2. Authenticate with organization admin account
3. Grant page management permissions
4. Save credential

### Step 4: Configure Workflow Parameters

Open the workflow and update these nodes:

#### 1. Schedule Trigger Node
Set your preferred schedule (e.g., daily at 9 AM)

#### 2. Get Data from Sheets Node
```
documentId: "YOUR_GOOGLE_SHEET_ID"
sheetName: "Sheet1"
```

To find your Sheet ID: Open your Google Sheet and look at the URL between `/d/` and `/edit`

#### 3. OpenAI Chat Model Node
```
model: "gpt-4o-mini"
```

#### 4. Generate Post Content Node
Customize the AI prompt to match your expertise and style

#### 5. Send Content Confirmation Node
```
sendTo: "approver@example.com"
```

#### 6. Post to LinkedIn Node
```
organization: "YOUR_LINKEDIN_ORG_ID"
```

### Step 5: Test the Workflow

1. Add a test row to your Google Sheet with Status = "Pending"
2. In n8n, click **Execute Workflow** (manual test)
3. Check each node for successful execution
4. Verify approval email is received
5. Test the approval process
6. Confirm LinkedIn post publishes (if approved)
7. Check Google Sheet updates

### Step 6: Activate

1. Toggle **Active** switch in the top-right corner
2. The workflow will now run on the configured schedule

## 📖 Usage Guide

### Adding New Post Ideas

1. Open your Google Sheet
2. Add a new row with Post Description, Instructions, and Status = "Pending"
3. Leave other columns empty (auto-filled by workflow)

### Approval Process

1. **Receive Email**: Get approval email with generated content
2. **Review Content**: Check the AI-generated post
3. **Make Decision**: Select Yes (publish), No (reject), or Cancel (no action)
4. **Add Feedback** (Optional): Provide comments in the textarea

### Monitoring Status

Check your Google Sheet for real-time updates:
- **Status**: "Pending" → "Completed"
- **Output**: LinkedIn post URN
- **Post Link**: Direct URL to the published post

## 🎨 Customization Options

### Modify AI Prompt

Edit the **Generate Post Content** node to customize output style, format, and requirements.

### Change AI Model

In the **OpenAI Chat Model** node:
- `gpt-4o-mini`: Fast and cost-effective
- `gpt-4o`: Higher quality, more expensive
- `gpt-4-turbo`: Balanced option

### Adjust Schedule

Common schedule patterns:
- Every 2 hours
- Every Monday at 9 AM
- Every weekday at 9 AM

### Add Multiple Approvers

1. Duplicate the **Send Content Confirmation** node
2. Add different email addresses
3. Use **Merge** node to handle multiple responses
4. Modify **Switch** node logic for multiple approvals

### Add Image Support

Extend the workflow to include images:
1. Add "Image URL" column to Google Sheet
2. Fetch image in workflow
3. Include in LinkedIn post parameters

## 🐛 Troubleshooting

### Workflow Not Triggering

**Issue**: Scheduled workflow doesn't run

**Solutions**:
- Verify workflow is **Active** (toggle in top-right)
- Check schedule trigger configuration
- Review n8n execution logs (Executions tab)
- Ensure n8n instance is running (for self-hosted)

### OpenAI Errors

**Issue**: "Rate limit exceeded" or "Invalid API key"

**Solutions**:
- Verify API key is valid and active
- Check OpenAI account has sufficient credits
- Reduce request frequency if hitting rate limits
- Verify model name is correct: `gpt-4o-mini`

### LinkedIn Posting Fails

**Issue**: "Authentication failed" or "Invalid organization"

**Solutions**:
- Refresh LinkedIn OAuth credentials
- Verify organization ID is correct
- Confirm you have admin access to the page
- Check LinkedIn API rate limits
- Ensure page is a Company Page (not personal profile)

### Google Sheets Not Updating

**Issue**: Status doesn't change or post link not added

**Solutions**:
- Verify `row_number` column exists
- Check Sheet ID and sheet name match exactly
- Confirm OAuth scopes include edit permissions
- Test Google Sheets credential separately
- Ensure column names match exactly (case-sensitive)

### Approval Email Not Received

**Issue**: No approval email arrives

**Solutions**:
- Check spam/junk folder
- Verify approver email address is correct
- Test Gmail credential separately
- Check Gmail API quotas
- Review execution logs for error messages

### Empty or Malformed Posts

**Issue**: AI generates poor quality content

**Solutions**:
- Provide more detailed instructions
- Add examples in the prompt
- Try a more capable model (gpt-4o vs gpt-4o-mini)
- Review and refine your prompt template
- Include more context in Post Description

## 📊 Performance & Costs

### Expected Costs (per post)

- **OpenAI API**: ~$0.01-0.03 per post (gpt-4o-mini)
- **Google Sheets API**: Free (within quotas)
- **Gmail API**: Free (within quotas)
- **LinkedIn API**: Free

### Rate Limits

- **OpenAI**: 3 requests/minute (tier 1), 60 requests/minute (tier 2+)
- **Google Sheets**: 100 requests/100 seconds/user
- **Gmail**: 250 emails/day (OAuth), 100/hour
- **LinkedIn**: Varies by endpoint, typically 100/day for posts

### Optimization Tips

1. **Batch Processing**: Process multiple posts in one execution
2. **Smart Scheduling**: Run during off-peak hours
3. **Caching**: Store frequently used data
4. **Error Handling**: Add retry logic for transient failures

## 🔒 Security Best Practices

- Never commit credentials to version control
- Use environment variables for sensitive data
- Regularly rotate API keys
- Review OAuth permissions periodically
- Use separate credentials for testing vs production
- Enable 2FA on all accounts
- Monitor API usage for anomalies

## 📈 Analytics & Monitoring

### Track Performance

Add these columns to your Google Sheet:
- `Generated At`: Timestamp of generation
- `Approved At`: Timestamp of approval
- `Published At`: Timestamp of publishing
- `Approver`: Who approved the post

### Monitor Metrics

- Posts generated per week
- Approval rate
- Time from generation to publish
- OpenAI token usage
- Error frequency

## 🔄 Updates & Maintenance

### Regular Tasks

- **Weekly**: Review error logs
- **Monthly**: Check API usage and costs
- **Quarterly**: Update credentials if needed
- **Yearly**: Review and optimize workflow logic

### Version History

- **v1.0**: Initial release - LinkedIn post generation with approval

## 🤝 Contributing

Found a bug or have an improvement? Please:
1. Open an issue in the main repository
2. Submit a pull request with your changes
3. Include updated documentation

## 📝 License

MIT License - See repository root for details

## 🙏 Credits

- Built with [n8n](https://n8n.io)
- Powered by [OpenAI](https://openai.com)
- Integrates with Google Sheets, Gmail, and LinkedIn

---

[← Back to Main Repository](../../README.md)
