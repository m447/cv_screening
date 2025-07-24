# AI Agent Integration for CV Workflow

## Option 1: Query Agent (Recommended First Step)

Add this parallel to your main workflow:

```
Chat Trigger → AI Agent (with tools) → Response
                     ↓
              - Google Sheets Tool (query CVs)
              - Memory (remember context)
              - Custom Tool (trigger main workflow)
```

### Agent Prompts Examples:
- "Show me all candidates scored above 80 this week"
- "Find Python developers with 5+ years experience"
- "How many CVs were processed today?"
- "Reprocess CVs in the failed folder"

## Option 2: Dynamic Criteria Agent

Replace fixed OpenAI prompt with agent-driven analysis:

```
Current: Fixed prompt → "Analyze for Data Analyst position"
New: Agent determines → "What position is this CV for?" → Dynamic criteria
```

## Option 3: Full Integration

```yaml
Main Workflow:
  - Schedule Trigger
  - Get All CVs
  - For each CV:
    - AI Agent analyzes:
      * Determine best matching position
      * Score against multiple roles
      * Suggest optimal placement
    - Route to appropriate team
    - Send personalized emails

Parallel Chat Workflow:
  - Chat interface for queries
  - Manual override options
  - Bulk operations
```

## Benefits:

1. **Flexibility**: Change criteria without editing workflow
2. **Intelligence**: Agent learns from decisions
3. **Interactivity**: Query and manage via chat
4. **Scalability**: Handle multiple positions dynamically

## Implementation Steps:

1. Start with Query Agent (Option 1)
   - Add chat trigger workflow
   - Connect Google Sheets tool
   - Test basic queries

2. Add Intelligence (Option 2)
   - Replace fixed criteria
   - Let agent categorize CVs

3. Full Integration (Option 3)
   - Merge both approaches
   - Complete AI-driven hiring

## Tools Needed:

- **Google Sheets Tool**: Query CV database
- **Memory**: Remember conversation context
- **OpenAI Model**: Power the agent
- **Execute Workflow Tool**: Trigger main CV workflow
- **Gmail Tool**: Send emails

Would you like me to create a simple Query Agent to start?