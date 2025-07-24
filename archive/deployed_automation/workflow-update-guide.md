# Workflow Update Guide - Add Dual Agent Architecture

Since API updates have credential formatting issues, here's how to manually update your workflow to use the dual-agent architecture:

## Step 1: Create the Agent Workflows First

1. **Import Scoring Agent Workflow**
   - Go to Workflows → Import
   - Import `scoring-agent-workflow.json`
   - Save and note the workflow ID

2. **Import Research Agent Workflow**
   - Import `research-agent-workflow.json`
   - Save and note the workflow ID

## Step 2: Update Your Main Workflow

### Add New Nodes

1. **Add "Execute Workflow" Node (for Scoring Agent)**
   - Position: After "Extract from File"
   - Name: "Call Scoring Agent"
   - Settings:
     - Workflow: Select your Scoring Agent workflow
     - Input Data:
       ```json
       {
         "cv_text": "={{ $json.text }}",
         "position_context": "Data Analyst, Backend Developer, DevOps Engineer",
         "file_info": {
           "id": "={{ $('Download CV').item.json.id }}",
           "name": "={{ $('Download CV').item.json.name }}"
         }
       }
       ```

2. **Replace "Prepare OpenAI Request" Node**
   - Delete the current "Prepare OpenAI Request" node
   - The Scoring Agent now handles this

3. **Update "Parse AI Response" Node**
   - Rename to: "Prepare Data for Logging"
   - Update the code to:
   ```javascript
   const analysis = $json.analysis;
   const processed = analysis && analysis.overall_score !== undefined;
   const fileData = $('Download CV').item.json;
   
   return {
     'Date Applied': new Date().toISOString().split('T')[0],
     'Candidate Name': analysis.name || 'Unknown',
     'CV Link': fileData.webViewLink || fileData.alternateLink,
     'Score': analysis.overall_score || 0,
     'Category': analysis.best_position || 'Unknown',
     'Matched Keywords': analysis.technical_skills || '',
     'Red Flags': analysis.concerns || 'None',
     'Status': processed ? 'AI Reviewed' : 'Error - Review Needed',
     'AI Recommendation': analysis.recommendation || 'No recommendation',
     'Years Experience': analysis.experience_summary || 'Unknown',
     'Strengths': analysis.key_strengths || '',
     '_processingSuccess': processed,
     '_fileId': fileData.id,
     '_fileName': fileData.name,
     '_shouldResearch': processed && analysis.overall_score >= 85
   };
   ```

4. **Add Score Check for Research**
   - Add IF node after "Log to Sheets"
   - Name: "Score >= 85?"
   - Condition: `{{ $json._shouldResearch }} equals true`

5. **Add Research Agent Trigger**
   - Add "Execute Workflow" node on TRUE path of score check
   - Name: "Trigger Research Agent"
   - Workflow: Select your Research Agent workflow
   - Continue On Fail: Yes
   - Input:
   ```json
   {
     "task": "research",
     "candidate": {
       "name": "={{ $json['Candidate Name'] }}",
       "email": "={{ $json.email }}",
       "score": "={{ $json.Score }}",
       "position": "={{ $json.Category }}",
       "skills": "={{ $json['Matched Keywords'] }}"
     }
   }
   ```

## Step 3: Update Connections

### Remove Old Connections
1. Disconnect "Prepare OpenAI Request" → "Call OpenAI API"
2. Disconnect "Call OpenAI API" → "Parse AI Response"

### Add New Connections
1. "Extract from File" → "Call Scoring Agent"
2. "Call Scoring Agent" → "Prepare Data for Logging"
3. "Log to Sheets" → "Score >= 85?"
4. "Score >= 85?" (true) → "Trigger Research Agent"
5. "Score >= 85?" (false) → "Check Processing Success"
6. "Trigger Research Agent" → "Check Processing Success"

## Step 4: Update Google Sheets

Add new columns to track agent results:
- Best Position (which role fits best)
- Position Scores (scores for each role)
- Research Status (pending/completed)
- Research Findings (link to research results)

## Step 5: Test the Updated Workflow

1. Deactivate current workflow
2. Save all changes
3. Test with a single CV first
4. Check that:
   - Scoring Agent analyzes correctly
   - High scores trigger research
   - Data logs properly to sheets
   - Files move to correct folders

## Benefits of This Update

1. **Dynamic Scoring**: No longer limited to "Data Analyst" - evaluates for multiple positions
2. **Smart Research**: Only researches top candidates (score ≥ 85)
3. **Modular Design**: Easy to update scoring criteria or research methods
4. **Better Performance**: Research runs async, doesn't slow main process
5. **Richer Data**: More insights per candidate

## Troubleshooting

**Agent not found?**
- Ensure agent workflows are saved
- Check workflow permissions

**No research triggered?**
- Verify score threshold in IF node
- Check Research Agent is active

**Data not logging?**
- Update Google Sheets column mappings
- Check field names match