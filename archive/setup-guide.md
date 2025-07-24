# n8n CV Screening Automation Setup Guide

## Overview
This guide will help you set up an automated CV screening system using n8n and Google Workspace services. The system monitors Google Drive for new CVs, scores them based on job requirements, tracks candidates in Google Sheets, and sends notifications via Gmail.

## Prerequisites
- n8n instance (self-hosted or cloud)
- Google Workspace account with admin access
- Google Drive folder for CV uploads
- Google Sheets for candidate tracking
- Gmail for notifications

## Setup Steps

### 1. Google Workspace Preparation

#### Create Google Drive Folder
1. Create a dedicated folder in Google Drive named "CV Uploads" or similar
2. Note the folder ID from the URL (after `/folders/`)
3. Set appropriate sharing permissions for HR team

#### Create Google Sheets Tracker
1. Create a new Google Sheet named "CV Screening Tracker"
2. Create a sheet named "Candidates" with these columns:
   - A: Date Applied
   - B: Candidate Name
   - C: CV Link
   - D: Score
   - E: Category
   - F: Matched Keywords
   - G: Red Flags
   - H: Status
3. Note the spreadsheet ID from the URL

### 2. n8n Configuration

#### Import Workflow
1. In n8n, click "Add workflow" → "Import from File"
2. Select `n8n-cv-screening-workflow.json`
3. The workflow will appear in your editor

#### Configure Credentials
You need to set up three OAuth2 credentials:

1. **Google Drive**
   - Go to Credentials → Create New → Google Drive OAuth2
   - Follow the OAuth flow to authenticate
   - Name it "Google Drive account"

2. **Google Sheets**
   - Go to Credentials → Create New → Google Sheets OAuth2
   - Follow the OAuth flow to authenticate
   - Name it "Google Sheets account"

3. **Gmail**
   - Go to Credentials → Create New → Gmail OAuth2
   - Follow the OAuth flow to authenticate
   - Name it "Gmail account"

#### Update Workflow Variables
Replace these placeholders in the workflow:
- `{{GOOGLE_DRIVE_CREDENTIALS_ID}}` → Your Google Drive credential ID
- `{{GOOGLE_SHEETS_CREDENTIALS_ID}}` → Your Google Sheets credential ID
- `{{GMAIL_CREDENTIALS_ID}}` → Your Gmail credential ID
- `{{GOOGLE_SHEET_ID}}` → Your Google Sheets ID
- `{{HR_EMAIL}}` → Your HR team email address
- `{{COMPANY_NAME}}` → Your company name

### 3. Customize Scoring Criteria

Edit `scoring-criteria.json` to match your job requirements:
```json
{
  "requiredSkills": {
    "keywords": ["python", "javascript", "etc..."]
  }
}
```

### 4. Activate the Workflow

1. Click "Execute Workflow" to test
2. Upload a test CV to your Google Drive folder
3. Verify the CV appears in Google Sheets
4. Check for email notifications
5. Once tested, activate the workflow

## Testing the System

1. **Test File Upload**
   - Upload a PDF resume to the monitored folder
   - Wait 5 minutes (or trigger manually)
   - Check Google Sheets for new entry

2. **Test Scoring**
   - Upload CVs with different skill levels
   - Verify scoring accuracy
   - Check categorization (qualified/unqualified)

3. **Test Notifications**
   - Ensure qualified candidates trigger HR emails
   - Verify candidate acknowledgment emails
   - Check email formatting

## Monitoring and Maintenance

### Daily Checks
- Review Google Sheets for new candidates
- Check n8n execution history for errors
- Monitor email delivery status

### Weekly Tasks
- Review and update scoring criteria based on results
- Export candidate data for reporting
- Clear processed CVs from upload folder

### Monthly Reviews
- Analyze screening effectiveness
- Update keywords based on successful hires
- Review automation performance metrics

## Troubleshooting

### Common Issues

1. **Workflow not triggering**
   - Check Google Drive folder permissions
   - Verify credential authentication
   - Ensure workflow is active

2. **Scoring errors**
   - Check CV text extraction
   - Verify scoring logic in Code node
   - Review keyword matching

3. **Email not sending**
   - Verify Gmail credentials
   - Check recipient email addresses
   - Review Gmail sending limits

### Debug Mode
- Use n8n's execution log to debug
- Add console.log statements in Code nodes
- Test with manual workflow execution

## Advanced Features

### Multiple Positions
1. Create separate scoring criteria files
2. Use different Google Drive folders
3. Duplicate and modify workflows

### Integration with ATS
- Export data to your ATS via API
- Sync candidate status updates
- Automate interview scheduling

### Analytics Dashboard
- Connect Google Sheets to Data Studio
- Create visualization dashboards
- Track screening metrics

## Security Best Practices

1. **Access Control**
   - Limit Google Drive folder access
   - Use service accounts where possible
   - Regular credential rotation

2. **Data Protection**
   - Enable Google Sheets access logs
   - Regular data backups
   - GDPR compliance for EU candidates

3. **Monitoring**
   - Set up error alerts
   - Monitor for unusual activity
   - Regular security audits

## Support

For issues or questions:
1. Check n8n documentation
2. Review Google Workspace API docs
3. Contact your IT administrator