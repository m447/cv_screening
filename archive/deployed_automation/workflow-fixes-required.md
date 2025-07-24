# Workflow Fixes Required

## Issues Found and Solutions

### 1. Get All New CVs Node ✅
**Issue**: Using incorrect `resource: "fileFolder"` format
**Fix**: Replace with properly configured node from `fixed-get-all-cvs-node.json`

### 2. Extract from File Node 🔧
**Issue**: Missing required `binaryPropertyName` field
**Fix**: Add this parameter to the node:
```json
"parameters": {
  "operation": "pdf",
  "binaryPropertyName": "data",
  "options": {}
}
```

### 3. Email Subject Expression Error 🔧
**Issue**: Nested expressions `{{{{` are not supported
**Fix**: Update both email nodes' subject lines:
- Email - Standard: `"CV Processed: {{ $json['Candidate Name'] }} (Score: {{ $json.Score }})"`
- Email - Error Alert: `"⚠️ CV Processing Error: {{ $json._fileName }}"`

### 4. Parse AI Response Code 🔧
**Issue**: Missing `_shouldResearch` field for agent routing
**Fix**: Add this line before the final closing brace:
```javascript
'_shouldResearch': processingSuccess && (aiAnalysis.overall_score >= 85)
```

### 5. AI Agent System Prompt 🔧
**Issue**: Generic prompt, needs specific position focus
**Fix**: Update AI Agent for Scoring system message:
```
You are an expert CV screening agent analyzing candidates for a Data Analyst position requiring Python, SQL, and data visualization skills.

Analyze the CV and return ONLY valid JSON:
{
  "name": "candidate full name",
  "email": "email if found", 
  "overall_score": 0-100,
  "technical_skills": "comma-separated list",
  "years_experience": number,
  "experience_summary": "brief summary",
  "key_strengths": "top 3 strengths",
  "concerns": "any red flags or gaps",
  "recommendation": "hire/maybe/no with reasoning",
  "ideal_next_steps": "suggested interview focus areas"
}
```

### 6. Google Drive Credentials 🔧
**Issue**: "Forbidden - perhaps check your credentials?" error
**Solution**: 
1. Click on each Google Drive node (Get All New CVs, Download CV, Move to Processed, Move to Failed)
2. Click on the credentials dropdown
3. Select "Google Drive account" again
4. Save the node
5. This refreshes the OAuth token

### 7. Research Agent Input 🔧
**Issue**: Agent expects structured input
**Fix**: Update Candidate Online Research prompt to use correct field references:
```
Candidate Information:
- Name: {{ $('Parse AI Response').item.json['Candidate Name'] }}
- Score: {{ $('Parse AI Response').item.json.Score }}
- Skills: {{ $('Parse AI Response').item.json['Matched Keywords'] }}
```

## Quick Fix Steps

1. **Import Fixed Node**:
   - Delete current "Get All New CVs" node
   - Import from `fixed-get-all-cvs-node.json`
   - Reconnect to Split In Batches

2. **Fix Extract from File**:
   - Click node
   - Add `binaryPropertyName: "data"`
   - Save

3. **Fix Email Subjects**:
   - Remove double brackets from both email nodes
   - Use single bracket expressions

4. **Update Parse AI Response**:
   - Add `_shouldResearch` field
   - Save code node

5. **Configure AI Agents**:
   - Update system prompts as shown above
   - Ensure temperature is set to 0.2

6. **Refresh All Credentials**:
   - Re-select credentials for all Google nodes
   - This forces OAuth token refresh

## Testing After Fixes

1. Place a test PDF in the CV upload folder
2. Execute workflow manually
3. Check for:
   - Successful file listing
   - PDF extraction working
   - AI agent returning valid JSON
   - Proper routing based on score
   - Email sending without errors
   - Files moving to correct folders

## Expected Flow

```
Get All New CVs (Fixed)
    ↓
Split In Batches (5)
    ↓
Download CV
    ↓
Extract from File (with data field)
    ↓
AI Agent for Scoring (Data Analyst focused)
    ↓
Parse AI Response (with _shouldResearch)
    ↓
Log to Sheets
    ↓
Score >= 85?
    ├─ Yes → Research Agent → Log Plan
    └─ No → Continue
    ↓
Check Success
    ├─ Success → Email → Move to Processed
    └─ Failed → Error Email → Move to Failed
```