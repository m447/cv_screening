# Advanced Agent Integration Examples

## Example 1: Trigger Main Workflow from Chat

Add an Execute Workflow tool to let the agent trigger CV processing:

```json
{
  "parameters": {
    "description": "Trigger the main CV processing workflow to process new CVs",
    "name": "Process New CVs",
    "workflowId": "YOUR_MAIN_WORKFLOW_ID"
  },
  "name": "Execute CV Workflow",
  "type": "@n8n/n8n-nodes-langchain.toolWorkflow",
  "position": [650, 600]
}
```

Then you can say: "Process all new CVs now"

## Example 2: Send Follow-up Emails

Add Gmail tool for the agent to send emails:

```json
{
  "parameters": {
    "description": "Send email to candidates or recruiters",
    "name": "Send Email"
  },
  "name": "Gmail Tool",
  "type": "@n8n/n8n-nodes-langchain.toolGmail",
  "position": [650, 700]
}
```

Commands like: "Email all candidates with score > 90"

## Example 3: Multi-Position Analysis

Replace fixed criteria with dynamic agent analysis:

```javascript
// In your main workflow's OpenAI prompt
const agentPrompt = `
Analyze this CV for the best matching position:
- Data Analyst
- Backend Developer  
- DevOps Engineer
- Product Manager

Return:
1. Best matching position
2. Score for each position
3. Recommendation
`;
```

## Example 4: Intelligent Routing

Create position-specific queues:

```
Agent analyzes CV → Determines best fit → Routes to:
- Data Team Queue (for Data Analyst matches)
- Engineering Queue (for Developer matches)
- Operations Queue (for DevOps matches)
- Product Queue (for PM matches)
```

## Example 5: Learning Agent

Store decisions for improvement:

```javascript
// After each CV processing
const feedback = {
  cv_id: cvId,
  agent_recommendation: position,
  human_decision: actualHire,
  score_accuracy: scoreMatch
};

// Agent learns from patterns
// Improves recommendations over time
```

## Gradual Implementation Path

1. **Start**: Basic Query Agent (current)
2. **Add**: Workflow trigger tool
3. **Enhance**: Email capabilities
4. **Upgrade**: Dynamic position matching
5. **Advanced**: Full AI-driven routing

Each step builds on the previous, maintaining your "keep it simple" approach while adding intelligence.