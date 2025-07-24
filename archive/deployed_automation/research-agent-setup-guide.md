# Research Agent Setup Guide

## Two Options Available

### Option 1: Simple Research Agent (Recommended to Start)
- Creates research plans for candidates
- Logs to Google Sheets queue
- Manual research execution
- Perfect for testing the concept

### Option 2: Advanced Research Agent
- Fully automated research
- Web search integration
- Compiles findings automatically
- Sends alerts for 90+ scores

## Simple Research Agent Setup

### What It Does
1. Receives candidate info from main workflow
2. AI agent creates research plan:
   - Search queries to run
   - Platforms to check
   - Verification checklist
3. Logs plan to "Research Queue" sheet
4. Returns status to main workflow

### How to Set Up

1. **Import Workflow**
   ```
   Import: simple-research-agent.json
   Save and note the workflow ID
   ```

2. **Create Research Queue Sheet**
   - Add new sheet tab: "Research Queue"
   - Columns:
     - Candidate
     - Score  
     - Position
     - Research Plan
     - Status
     - Date Added

3. **Update Main Workflow**
   - Click "Trigger Research (Optional)" node
   - Set workflow ID to your research agent ID

### Research Plan Example

When triggered, the agent creates:
```json
{
  "searchQueries": [
    "John Doe Data Analyst LinkedIn",
    "site:linkedin.com/in John Doe",
    "John Doe Python portfolio"
  ],
  "platformsToCheck": [
    "LinkedIn",
    "GitHub",
    "Personal Website"
  ],
  "researchChecklist": [
    "Verify current employment",
    "Check Python project examples",
    "Review data visualization work",
    "Confirm years of experience"
  ],
  "priority": "high"
}
```

## Manual Research Process

After plans are logged:

1. **Review Queue Daily**
   - Check Research Queue sheet
   - Sort by score/priority

2. **Execute Research**
   - Copy search queries
   - Run in browser
   - Check suggested platforms

3. **Update Results**
   - Mark status as "Completed"
   - Add findings to candidate notes

## Benefits of Agent Approach

1. **Intelligent Planning**
   - AI generates targeted searches
   - Customized per candidate
   - Focuses on relevant skills

2. **Structured Process**
   - Consistent research approach
   - Nothing gets missed
   - Clear audit trail

3. **Flexible Execution**
   - Manual for now
   - Can automate later
   - Control over depth

## Future Enhancement Path

### Phase 1: Current
- Agent creates research plans
- Manual execution
- Results logged manually

### Phase 2: Semi-Automated
- Agent performs basic searches
- Returns top 3 results
- Human verifies findings

### Phase 3: Fully Automated
- Complete web search integration
- Profile extraction
- Automated reports

## Testing the Research Agent

1. **Find a High-Scoring CV**
   - Should score 85+
   - Has identifiable name

2. **Trigger Main Workflow**
   - Process the CV
   - Watch for research trigger

3. **Check Research Queue**
   - New entry should appear
   - Review the research plan

4. **Execute Manually**
   - Follow the plan
   - Note effectiveness

## Common Issues

**No research triggered?**
- Check score threshold (>= 85)
- Verify workflow connection
- Check _shouldResearch field

**Research plan too generic?**
- Enhance agent prompt
- Provide more context
- Include specific skills

**Queue not updating?**
- Check sheet permissions
- Verify sheet name
- Test Google Sheets connection