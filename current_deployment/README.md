# CV Screening Automation - Current Deployment

## Overview
This is the current production version of the CV screening automation workflow using n8n with AI Agent integration.

## Workflow Components

### Main Workflow File
- `cv-screening-workflow-latest.json` - The complete n8n workflow ready for import

### Key Features
1. **Automated CV Processing** - Monitors Google Drive folder for new CVs
2. **AI-Powered Analysis** - Uses OpenAI GPT-4o-mini for CV evaluation
3. **Structured Output** - Returns JSON with scores, skills, recommendations
4. **Google Sheets Logging** - Tracks all processed CVs with results
5. **Email Notifications** - Sends alerts for processed and failed CVs
6. **File Management** - Moves processed files to appropriate folders

### Google Drive Folder Structure
- **New CVs**: `1dMuo2J3L52QqDmHj0UOEe9MSk0gsPSNa`
- **Processed**: `1CQWsocqKluQnSkUdMGQsL-Owsr7aQXQn`
- **Failed**: `1mu9rgElrKMU1vt22EC7KaKWquw1EkJDP`

### Google Sheets
- **Tracker**: `1xfU9rzjBpb3NEHYzvMuHZ2R2QW_IYe5i3dCW7Ls-bII`

## How to Deploy

1. Import `cv-screening-workflow-latest.json` into n8n
2. Update credentials for:
   - Google Drive OAuth2
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API
3. Update the AI Agent system message if needed
4. Test with sample CVs
5. Activate the workflow

## AI Agent Configuration

The AI Agent is configured to:
- Analyze CVs for Data Analyst positions
- Look for Python, SQL, and data visualization skills
- Score candidates 0-100
- Categorize as: Highly Qualified / Qualified / Possibly Qualified / Unqualified
- Return structured JSON output

## Troubleshooting

### Common Issues
1. **"Missing value for input variable"** - Check AI Agent system message formatting
2. **"Resource not found"** - Verify file IDs in Move nodes
3. **No JSON output** - Update Parse AI Response node to handle markdown

### Testing
Place test CV PDFs in the New CVs folder and manually execute the workflow to verify all components work correctly.

## Version History
- **2025-07-24**: Current version with AI Agent integration and fixed JSON parsing