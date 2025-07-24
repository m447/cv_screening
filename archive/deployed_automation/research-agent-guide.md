# CV Research Agent - Setup & Usage Guide

## What This Enhanced Agent Does

Beyond querying your CV database, this agent can now:
1. **Research candidates online** - Find LinkedIn, GitHub, social profiles
2. **Enrich profiles** - Add information from public sources
3. **Verify credentials** - Check online presence and projects
4. **Generate reports** - Compile comprehensive candidate research

## Example Commands

### Basic Research
- "Research John Doe who scored 85"
- "Find the LinkedIn profile of our top candidate"
- "Check if Sarah Johnson has a GitHub account"

### Bulk Research
- "Research all candidates with score > 80"
- "Find LinkedIn profiles for top 5 candidates"
- "Check online presence of today's applicants"

### Specific Searches
- "Find Python projects by candidate Mike Chen"
- "Look for publications by Dr. Emily Watson"
- "Check startup experience for candidate Alex Rivera"

### Verification
- "Verify the work history of candidate #3"
- "Cross-check skills listed by John Smith"
- "Find references to ABC Company employment"

## Setup Instructions

### 1. Import All Workflows
```bash
1. research-agent-workflow.json (main agent)
2. web-search-subworkflow.json (Google search capability)
3. web-scraper-subworkflow.json (webpage extraction)
```

### 2. Configure Google Search API
For the web search to work:
1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Enable Custom Search API
3. Create API credentials
4. Set up Custom Search Engine at [cse.google.com](https://cse.google.com)
5. Update in web-search-subworkflow:
   - YOUR_GOOGLE_API_KEY
   - YOUR_SEARCH_ENGINE_ID

### 3. Alternative: Use SerpAPI
If Google setup is complex, use SerpAPI:
```json
{
  "url": "https://serpapi.com/search",
  "queryParameters": {
    "api_key": "YOUR_SERPAPI_KEY",
    "q": "{{ $json.query }}",
    "num": "5"
  }
}
```

### 4. Connect Sub-workflows
In n8n:
1. Save all three workflows
2. Get the IDs of the sub-workflows
3. Update the main agent workflow with correct sub-workflow IDs

## Privacy & Ethics Guidelines

The agent is configured to:
- Only use publicly available information
- Respect privacy settings on social platforms
- Not store sensitive personal data
- Provide source links for transparency

## Research Workflow Example

When you ask: "Research top 3 candidates with scores > 85"

The agent will:
1. Query Google Sheets for matching candidates
2. For each candidate:
   - Search "{name} {email domain} LinkedIn"
   - Search "{name} GitHub developer"
   - Search "{name} {key skills}"
3. Extract relevant information from found pages
4. Compile a summary report

## Output Format

```
**Candidate Research Report**

Name: John Doe
Score: 87/100
Email: john.doe@email.com

**Online Presence:**
- LinkedIn: /in/johndoe (500+ connections, Senior Developer at XYZ)
- GitHub: github.com/johndoe (42 repositories, 150+ stars)
- Personal Site: johndoe.dev

**Key Findings:**
- Active open source contributor (React, Node.js)
- Speaker at JSConf 2023
- Published article on "Scaling Microservices"
- Verified employment at XYZ Corp (2020-present)

**Recommendations:**
- Strong technical background aligns with position
- Community involvement shows leadership
- Consider for senior roles
```

## Advanced Features

### 1. Automated Enrichment
Set up scheduled workflow to research new high-scoring candidates automatically

### 2. Social Media Integration
Add Twitter/X API for additional insights

### 3. Professional Networks
Integrate with professional databases for verification

### 4. Report Generation
Create PDF reports with full research findings

## Limitations & Considerations

1. **API Limits**: Google Search has daily limits
2. **Rate Limiting**: Don't research too many candidates at once
3. **Data Accuracy**: Always verify critical information
4. **Legal Compliance**: Follow local privacy laws (GDPR, etc.)

## Troubleshooting

**No search results?**
- Check API credentials
- Verify search engine configuration
- Test with simple queries first

**Scraping fails?**
- Some sites block automated access
- LinkedIn requires special handling
- Consider using dedicated APIs

**Agent confused?**
- Be specific in queries
- Start with single candidate research
- Build complexity gradually