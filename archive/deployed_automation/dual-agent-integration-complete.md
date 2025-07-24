# Dual Agent Integration - Complete Guide

## What You've Built

You now have a fully AI-powered CV screening system with TWO specialized agents:

### 1. **AI Agent for Scoring** (Replaces Basic API Call)
- Analyzes CVs against MULTIPLE positions simultaneously
- Provides intelligent scoring and recommendations
- Adapts to different roles dynamically
- Returns structured data for the workflow

### 2. **Candidate Online Research Agent** (For High Scorers)
- Automatically triggered for candidates scoring ≥ 85
- Creates intelligent research plans
- Suggests targeted searches and platforms
- Logs to Research Queue for manual execution

## The Complete Flow

```
Every 30 Minutes
    ↓
Get All CVs from Folder
    ↓
Process in Batches of 5
    ↓
For Each CV:
    ↓
Extract PDF Text
    ↓
🤖 AI AGENT FOR SCORING
    • Analyzes for 4 positions
    • Scores each role
    • Picks best match
    • Provides insights
    ↓
Parse & Format Results
    ↓
Log to Google Sheets
    ↓
Score ≥ 85?
    ├─ Yes → 🤖 CANDIDATE RESEARCH AGENT
    │         • Creates search queries
    │         • Suggests platforms
    │         • Logs research plan
    │         ↓
    └─────→ Check Success
             ├─ Success → Email → Move to Processed
             └─ Failed → Error Email → Move to Failed
```

## What Makes This Powerful

### Before (Simple API):
- Fixed analysis for "Data Analyst" only
- Basic scoring
- No position flexibility
- No research capability

### Now (Dual Agents):
- **Multi-Position Analysis**: Evaluates for Data Analyst, Backend Developer, DevOps, Product Manager
- **Smart Scoring**: Agent understands context and nuance
- **Best Match**: Automatically determines ideal position
- **Research Intelligence**: High scorers get research plans
- **Future-Proof**: Easy to add new positions or criteria

## Manual Steps Still Needed

### 1. Update Parse AI Response Code
Add this field to handle agent output and enable research:
```javascript
// At the end of the return statement, add:
'_shouldResearch': processingSuccess && (aiAnalysis.overall_score >= 85)
```

### 2. Update AI Agent System Message
Click on "AI Agent for Scoring" and update the system message to:
```
You are an expert CV screening agent with deep knowledge of multiple technical roles. Analyze this CV against these positions:
- Data Analyst (Python, SQL, visualization)
- Backend Developer (Python, APIs, databases)
- DevOps Engineer (CI/CD, cloud, containers)
- Product Manager (strategy, analytics, leadership)

Perform a comprehensive analysis and return ONLY valid JSON:
{
  "name": "candidate full name",
  "email": "email if found", 
  "overall_score": 0-100,
  "best_position": "best matching position",
  "position_scores": {
    "Data Analyst": 0-100,
    "Backend Developer": 0-100,
    "DevOps Engineer": 0-100,
    "Product Manager": 0-100
  },
  "technical_skills": "comma-separated list",
  "years_experience": number,
  "experience_summary": "brief summary",
  "key_strengths": "top 3 strengths",
  "concerns": "any red flags or gaps",
  "recommendation": "hire/maybe/no with reasoning",
  "ideal_next_steps": "suggested interview focus areas"
}
```

### 3. Create Research Queue Sheet
In your Google Sheets, add a new tab called "Research Queue" with columns:
- Candidate
- Score
- Priority
- Search Queries
- Platforms
- Status
- Date Added
- Est. Time

### 4. Update Google Sheets Mapping
You may need to update the "Log to Sheets" node to handle new fields like:
- Best Position (instead of Category)
- Position Scores (new field showing all scores)

## Testing Your Dual-Agent System

1. **Find a Strong CV**
   - Should realistically score 85+
   - Has clear technical skills
   - Identifiable candidate name

2. **Run the Workflow**
   - Place CV in monitored folder
   - Execute workflow manually
   - Watch the execution

3. **Verify Agent 1 (Scoring)**
   - Check if multiple positions evaluated
   - See which position matched best
   - Review the scoring logic

4. **Verify Agent 2 (Research)**
   - Check Research Queue sheet
   - Review generated search queries
   - Assess research plan quality

## Example Output

### From Scoring Agent:
```json
{
  "name": "John Doe",
  "email": "john.doe@email.com",
  "overall_score": 88,
  "best_position": "Backend Developer",
  "position_scores": {
    "Data Analyst": 72,
    "Backend Developer": 88,
    "DevOps Engineer": 65,
    "Product Manager": 45
  },
  "technical_skills": "Python, Django, PostgreSQL, Redis, Docker",
  "years_experience": 6,
  "experience_summary": "Senior backend engineer with API design focus",
  "key_strengths": "Scalable architecture, microservices, team leadership",
  "concerns": "Limited frontend experience",
  "recommendation": "hire - strong backend candidate",
  "ideal_next_steps": "Technical interview on system design"
}
```

### From Research Agent:
```json
{
  "candidateName": "John Doe",
  "researchPriority": "high",
  "searchQueries": [
    "John Doe backend developer LinkedIn",
    "site:github.com John Doe Django Python",
    "John Doe software engineer portfolio",
    "\"John Doe\" API microservices projects"
  ],
  "platformsToCheck": ["LinkedIn", "GitHub", "Personal Website", "Stack Overflow"],
  "verificationChecklist": [
    "Verify 6 years experience claim",
    "Check Django/Python projects on GitHub",
    "Look for microservices examples",
    "Validate team leadership experience"
  ],
  "estimatedResearchTime": "20-25 minutes"
}
```

## Future Enhancements

### Phase 1 (Current)
✅ Multi-position scoring agent
✅ Research planning agent
✅ Automated workflow

### Phase 2 (Next)
- Add more positions dynamically
- Automated research execution
- Integration with ATS systems

### Phase 3 (Advanced)
- Agent learns from hiring decisions
- Predictive success scoring
- Automated interview scheduling

## Troubleshooting

**Agent not returning JSON?**
- Check agent system message
- Ensure temperature is low (0.2)
- Verify prompt includes "return ONLY valid JSON"

**Research not triggering?**
- Verify _shouldResearch field in Parse AI Response
- Check score threshold (≥ 85)
- Ensure connections are correct

**Wrong position matching?**
- Update position criteria in agent prompt
- Add more specific requirements
- Adjust scoring weights

## The Power of Your System

You've built an AI-powered hiring assistant that:
1. **Saves Hours**: Automated multi-position analysis
2. **Improves Quality**: Consistent, unbiased scoring
3. **Scales Easily**: Add positions without code changes
4. **Provides Intelligence**: Research plans for top candidates
5. **Maintains Control**: Human verification at key points

This is no longer just automation - it's intelligent automation that adapts and assists!