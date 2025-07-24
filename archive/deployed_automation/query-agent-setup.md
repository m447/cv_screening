# CV Query Agent Setup Guide

## What This Does

Creates a chat interface to query your CV database with natural language:
- "Show me all candidates scored above 80"
- "Find Python developers with 5+ years experience"  
- "How many CVs were processed today?"
- "List failed CVs from this week"

## Setup Instructions

### 1. Import the Workflow
- Open n8n
- Go to Workflows → Import from File
- Select `query-agent-workflow.json`

### 2. Configure Credentials
The workflow needs:
- **OpenAI API** (same as main workflow)
- **Google Sheets** (same as main workflow)

### 3. Activate the Workflow
- Click "Activate" in top right
- The chat interface will be available

### 4. Access the Chat
- Look for the webhook URL in the Chat Trigger node
- Open in browser for web interface
- Or integrate with Slack/Teams

## Example Queries

**Basic Queries:**
- "Show all CVs"
- "How many candidates do we have?"
- "Show CVs processed today"

**Filtered Searches:**
- "Find candidates with score > 85"
- "Show me data analysts"
- "List senior developers"

**Analytics:**
- "Average score of all candidates"
- "How many failed to process?"
- "Top 5 candidates this week"

## How It Works

1. **Chat Trigger**: Receives your questions
2. **AI Agent**: Understands intent and decides what to query
3. **Google Sheets Tool**: Fetches data from your CV log
4. **Memory**: Remembers conversation context
5. **Response**: Returns formatted results

## Next Steps

1. Test with simple queries first
2. Add more tools:
   - Execute Workflow tool (trigger main CV workflow)
   - Gmail tool (send follow-up emails)
   - Custom analysis tools

3. Enhance with:
   - Filtering by date ranges
   - Exporting results to CSV
   - Bulk operations

## Integration Options

- **Slack**: Add Slack trigger instead of Chat
- **API**: Use webhook for custom apps
- **Dashboard**: Connect to monitoring tools