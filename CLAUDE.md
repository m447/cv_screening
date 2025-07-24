# Claude Assistant Guide for CV Screening Workflow

## Overview
This is a CV screening automation workflow that uses AI to analyze resumes. I'm here to help you implement and troubleshoot this n8n workflow.

## Quick Start for Beginners

### 1. Import the Workflow
1. Open your n8n instance
2. Click "Workflows" → "Add Workflow" → "Import from File"
3. Select `/current_deployment/cv-screening-workflow-latest.json`
4. Click "Import"

### 2. Set Up Credentials (REQUIRED)
You need 4 credentials. Here's how to add each one:

#### Google Drive OAuth2
1. In n8n, go to "Credentials" → "Add Credential" → "Google Drive OAuth2"
2. Follow the OAuth flow to connect your Google account
3. Make sure to grant access to Google Drive

#### Google Sheets OAuth2
1. Add "Google Sheets OAuth2" credential
2. Use the same Google account as Drive
3. Grant sheets access permissions

#### Gmail OAuth2
1. Add "Gmail OAuth2" credential
2. Use the same Google account
3. Grant email sending permissions

#### OpenAI API
1. Get your API key from https://platform.openai.com/api-keys
2. Add "OpenAI API" credential
3. Paste your API key

### 3. Update Workflow Settings

#### Email Recipients
1. Double-click "Email - Standard" node
2. Change `svama85@gmail.com` to your email
3. Do the same for "Email - Error Alert" node

#### Google Drive Folders
Check these folder IDs match yours:
- New CVs: `1dMuo2J3L52QqDmHj0UOEe9MSk0gsPSNa`
- Processed: `1CQWsocqKluQnSkUdMGQsL-Owsr7aQXQn`
- Failed: `1mu9rgElrKMU1vt22EC7KaKWquw1EkJDP`

## Common Issues & Fixes

### "Missing value for input variable" Error
**Problem**: AI Agent prompt has special characters
**Fix**: 
1. Click "AI Agent for Scoring" node
2. Make sure the prompt field says: `{{ $json.text }}`
3. Check system message has no curly braces

### "Resource not found" Error
**Problem**: File ID not available in error path
**Fix**:
1. Click "Move to Failed" node
2. Change File ID to: `{{ $('Filter PDFs Only').item.json.id }}`

### No JSON Output from AI
**Problem**: AI returns text with markdown formatting
**Fix**: The "Parse AI Response" node already handles this - no action needed

### Duplicate Entries in Google Sheets
**Problem**: Workflow processes same CV multiple times
**Fix**: Make sure CVs are moved out of the New folder after processing

## Testing Your Workflow

### Step 1: Prepare Test Files
1. Place 2-3 PDF CVs in your "New CVs" Google Drive folder
2. Make sure they're actual PDFs (not Google Docs)

### Step 2: Test Manually
1. Click "Execute Workflow" button
2. Watch the execution - green means success, red means error
3. Check your Google Sheet for results
4. Check your email for notifications

### Step 3: Verify Results
- ✅ CVs should appear in Google Sheets with scores
- ✅ You should receive email notifications
- ✅ PDFs should move to Processed folder
- ✅ Failed files should move to Failed folder

## Workflow Components Explained

1. **Monitor CV Folder** - Watches for new files (can be scheduled or real-time)
2. **Get All CVs via API** - Lists all PDFs in the folder
3. **Filter PDFs Only** - Ensures only PDF files are processed
4. **Split In Batches** - Processes 5 CVs at a time (prevents overload)
5. **Download CV** - Gets the actual file content
6. **Extract from File** - Converts PDF to text
7. **AI Agent for Scoring** - Analyzes CV with GPT-4
8. **Parse AI Response** - Extracts JSON from AI response
9. **Prepare Sheet Data** - Formats data for Google Sheets
10. **Log to Sheets** - Saves results
11. **Email Notifications** - Sends success/error emails
12. **Move Files** - Organizes processed files

## AI Scoring Criteria

The AI evaluates candidates on:
- **Technical Skills**: Python, SQL, data visualization
- **Experience**: Years in relevant roles
- **Score Range**:
  - 90-100: Highly Qualified
  - 70-89: Qualified
  - 50-69: Possibly Qualified
  - 0-49: Unqualified

## Need Help?

### If workflow fails:
1. Check the execution log (click on the failed node)
2. Look for red error messages
3. Common fixes:
   - Re-authenticate Google credentials
   - Check folder permissions
   - Verify OpenAI API key is valid
   - Ensure PDFs aren't corrupted

### To modify the workflow:
- Change scoring criteria: Edit "AI Agent for Scoring" system message
- Add more email recipients: Edit email nodes
- Process more files: Change "Split In Batches" batch size
- Different file types: Modify "Filter PDFs Only" code

## Important Commands

When debugging, I might ask you to:
- `npm run lint` - Check code quality
- `npm run typecheck` - Verify types
- Check n8n logs for detailed errors

## Tips for Success

1. Start with 1-2 test files before processing many
2. Monitor your OpenAI usage (costs money per CV)
3. Keep your Google Sheets organized
4. Regularly clean out Processed folder
5. Test email notifications with your own address first

Remember: This workflow automates CV screening but should support, not replace, human judgment in hiring decisions.