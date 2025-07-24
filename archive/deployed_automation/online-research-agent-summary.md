# Candidate Online Research Agent - Integrated

## What's Been Added

Your workflow now has an integrated "Candidate Online Research" agent that:

1. **Activates for high-scoring candidates** (score >= 85)
2. **Creates intelligent research plans** with:
   - 5-7 targeted search queries
   - Specific platforms to check
   - Verification checklist
   - Potential red flags to watch for
   - Time estimates
3. **Logs to Research Queue** in Google Sheets
4. **Continues workflow** to email notifications

## How It Works

```
CV Scored >= 85?
    ↓
Candidate Online Research Agent
    ↓
Creates Research Plan:
    - "John Doe Data Analyst LinkedIn"
    - "site:github.com John Doe Python"
    - Check: Current employment
    - Flag: Employment gaps
    ↓
Log to Research Queue
    ↓
Continue to Email/Move Files
```

## Research Plan Example

The agent generates structured plans like:
```json
{
  "candidateName": "John Doe",
  "researchPriority": "high",
  "searchQueries": [
    "John Doe Data Analyst LinkedIn",
    "site:linkedin.com/in John Doe",
    "John Doe Python portfolio GitHub",
    "\"John Doe\" data visualization projects",
    "John Doe company XYZ data analyst"
  ],
  "platformsToCheck": [
    "LinkedIn",
    "GitHub",
    "Personal Portfolio",
    "Medium/Blog posts"
  ],
  "verificationChecklist": [
    "Verify current employment at stated company",
    "Check Python project examples on GitHub",
    "Validate years of experience claimed",
    "Look for data visualization portfolio"
  ],
  "potentialRedFlags": [
    "Large employment gaps",
    "No online presence despite tech role",
    "Inconsistent job titles/dates"
  ],
  "estimatedResearchTime": "15-20 minutes",
  "recommendedActions": [
    "Deep dive if score > 90",
    "Quick verification if 85-89"
  ]
}
```

## Google Sheets Setup

Add a "Research Queue" sheet with columns:
- Candidate
- Score
- Priority (high/medium/low)
- Search Queries
- Platforms
- Status (Pending/Completed)
- Date Added
- Est. Time

## What You Need to Do

1. **Update Parse AI Response**
   Add this line to return statement:
   ```javascript
   '_shouldResearch': processingSuccess && (aiAnalysis.score >= 85)
   ```

2. **Create Research Queue Sheet**
   - Open your Google Sheets
   - Add new tab: "Research Queue"
   - Add the columns listed above

3. **Test with High-Scoring CV**
   - Find/create a CV that should score 85+
   - Run the workflow
   - Check Research Queue for the plan

## Benefits

1. **Intelligent Planning**: AI creates targeted research strategies
2. **No External Dependencies**: Works inline, no separate workflow needed
3. **Audit Trail**: All research plans logged for tracking
4. **Time Efficient**: Know how long research will take
5. **Priority-Based**: Higher scores get higher priority

## Manual Research Process

1. Check Research Queue daily
2. Start with high priority candidates
3. Copy search queries to browser
4. Check suggested platforms
5. Update status when complete

This keeps the automation smart while maintaining human control over the actual research execution.