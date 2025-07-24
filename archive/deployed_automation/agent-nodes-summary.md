# Agent Nodes Added to Your Workflow

## What's Been Added

### 1. **AI Agent for Scoring** (Node: agent-node-1)
- Position: [100, 400]
- Type: LangChain Agent
- Configured with GPT-4o
- System prompt for CV analysis

### 2. **OpenAI Chat Model** (Node: chat-model-1)
- Position: [300, 500]
- Connected to your OpenAI credentials
- Powers the agent's reasoning

### 3. **Score >= 85?** (Node: score-check)
- Position: [800, 300]
- Checks if candidate scored 85 or higher
- Routes to research agent if true

### 4. **Trigger Research (Optional)** (Node: research-trigger)
- Position: [1000, 350]
- Executes research agent workflow for high scorers
- Runs asynchronously (doesn't wait for completion)

## Current Workflow Structure

```
Schedule Trigger
    ↓
Get All CVs
    ↓
Split in Batches (5)
    ↓
Download CV
    ↓
Extract PDF Text
    ↓
Prepare OpenAI Request
    ↓
Call OpenAI API
    ↓
Parse AI Response
    ↓
Log to Sheets
    ├─→ Score >= 85?
    │     ├─ Yes → Trigger Research → Check Success
    │     └─ No → Check Success
    └─→ Check Processing Success
          ├─ Success → Email → Move to Processed
          └─ Failed → Error Email → Move to Failed
```

## What You Need to Do Next

### 1. Update the Parse AI Response Node
Edit the code to add the `_shouldResearch` field:
```javascript
// Add this line at the end of the return statement
'_shouldResearch': processingSuccess && (aiAnalysis.score >= 85)
```

### 2. Configure the Research Trigger
- Click on "Trigger Research (Optional)" node
- Update the workflow selection to your actual research agent workflow
- Set the input data mapping

### 3. Connect AI Agent Nodes
The agent nodes are created but not connected to the main flow yet. You can:
- Use them to replace the current OpenAI API call
- Or keep both for comparison

### 4. Test the Flow
1. Place a high-scoring CV (that should score > 85)
2. Run the workflow manually
3. Check if research agent is triggered

## Benefits of Current Setup

1. **Agent nodes ready** - Infrastructure for smarter CV analysis
2. **Research trigger added** - High scorers get deep research
3. **Flexible routing** - Easy to adjust score threshold
4. **Backward compatible** - Current flow still works

## Optional Next Steps

1. **Replace OpenAI call with Agent**
   - Connect Extract PDF → AI Agent
   - Agent can handle multiple position matching

2. **Add more intelligence**
   - Agent can determine best position
   - Dynamic scoring based on role

3. **Enhanced research criteria**
   - Research based on position + score
   - Different research depth by role