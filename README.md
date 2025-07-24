# CV Screening Automation with n8n + AI Agents

An intelligent CV screening workflow that combines n8n's visual automation with AI-powered analysis to streamline recruitment processes.

## 🚀 Overview

This project demonstrates the power of combining:
- **n8n**: Open-source workflow automation platform
- **AI Agents**: GPT-4 powered intelligent decision making
- **Agentic CLI + MCP**: Command-line AI assistance for workflow management

The system automatically processes CVs from Google Drive, analyzes them with AI, logs results to Google Sheets, and sends email notifications - all without writing a single line of code.

## 📋 Features

- **Automated CV Processing**: Monitors Google Drive for new resumes
- **AI-Powered Analysis**: Uses GPT-4 to evaluate candidates
- **Structured Scoring**: 0-100 scoring with skill extraction
- **Data Tracking**: Logs all results to Google Sheets
- **Smart Routing**: Organizes CVs based on qualification scores
- **Email Notifications**: Alerts for high-scoring candidates and errors
- **Error Handling**: Gracefully handles corrupted files and API failures

## 🏗️ Architecture

```
Google Drive → PDF Extract → AI Agent → Parse Response → Google Sheets
     ↓                                                          ↓
File Monitor                                           Email Notifications
     ↓                                                          ↓
Filter PDFs → Process Batch → Analyze → Route Files → Archive/Failed
```

## 📁 Project Structure

```
41_cv_screening/
├── current_deployment/
│   ├── cv-screening-workflow-latest.json    # Main n8n workflow
│   ├── README.md                            # Deployment guide
│   └── deployment-checklist.md              # Setup checklist
├── CLAUDE.md                                # AI assistant instructions
└── n8n-agentic-cli-presentation.md         # Benefits & features presentation
```

## 🛠️ Setup Instructions

### Prerequisites

1. **n8n Instance**: Self-hosted or cloud (https://n8n.io)
2. **Google Account**: With Drive, Sheets, and Gmail access
3. **OpenAI API Key**: For GPT-4 access
4. **Agentic CLI (Optional)**: For enhanced workflow management

### Installation

1. **Import Workflow**
   ```bash
   # In n8n UI: Workflows → Import → Select cv-screening-workflow-latest.json
   ```

2. **Configure Credentials**
   - Google Drive OAuth2
   - Google Sheets OAuth2
   - Gmail OAuth2
   - OpenAI API

3. **Update Configuration**
   - Email recipients in notification nodes
   - Google Drive folder IDs:
     - New CVs: `1dMuo2J3L52QqDmHj0UOEe9MSk0gsPSNa`
     - Processed: `1CQWsocqKluQnSkUdMGQsL-Owsr7aQXQn`
     - Failed: `1mu9rgElrKMU1vt22EC7KaKWquw1EkJDP`
   - Google Sheets ID: `1xfU9rzjBpb3NEHYzvMuHZ2R2QW_IYe5i3dCW7Ls-bII`

4. **Test & Activate**
   - Place test PDFs in New CVs folder
   - Execute workflow manually
   - Verify results in Google Sheets
   - Activate for automatic processing

## 🤖 Using with Agentic CLI + MCP

The workflow can be managed through natural language commands:

```bash
# Create workflow
> "Create a CV screening workflow that scores candidates and sends alerts"

# Debug issues
> "My CV workflow is failing on some PDFs"

# Modify scoring
> "Make the scoring more strict for senior positions"

# Check status
> "Show me the last 10 processed CVs"
```

## 📊 Workflow Components

1. **Google Drive Trigger**: Monitors folder for new files
2. **Filter PDFs Only**: Ensures only PDFs are processed
3. **Download CV**: Gets file content
4. **Extract Text**: Converts PDF to text
5. **AI Agent**: Analyzes CV with GPT-4
6. **Parse Response**: Extracts structured data
7. **Prepare Data**: Formats for spreadsheet
8. **Log to Sheets**: Records results
9. **Smart Router**: Sorts CVs by score
10. **Email Alerts**: Sends notifications

## 🎯 AI Scoring Criteria

The AI evaluates candidates on:
- **Technical Skills**: Python, SQL, data visualization tools
- **Experience**: Years in relevant roles
- **Education**: Relevant degrees and certifications
- **Soft Skills**: Communication, teamwork, problem-solving

**Score Ranges**:
- 90-100: Highly Qualified
- 70-89: Qualified
- 50-69: Possibly Qualified
- 0-49: Unqualified

## 🐛 Troubleshooting

### Common Issues

1. **"Missing value for input variable"**
   - Check AI Agent prompt formatting
   - Ensure system message uses: `{{ $json.text }}`

2. **"Resource not found"**
   - Verify Google Drive folder permissions
   - Update file reference: `{{ $('Filter PDFs Only').item.json.id }}`

3. **No JSON output from AI**
   - The Parse AI Response node handles markdown formatting
   - Check AI Agent system prompt for output format instructions

4. **Duplicate entries**
   - Ensure files are moved after processing
   - Check trigger settings for polling interval

## 📈 Performance & Costs

- **Processing Speed**: ~30 seconds per CV
- **Batch Size**: 5 CVs (configurable)
- **OpenAI Costs**: ~$0.10 per CV (GPT-4)
- **Success Rate**: 95%+ with proper setup

## 🔒 Security Considerations

- All credentials stored securely in n8n
- OAuth2 for Google services
- API keys encrypted at rest
- No CV data persisted outside Google services
- Compliant with GDPR requirements

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

This project is provided as-is for educational and commercial use.

## 🙏 Acknowledgments

- n8n team for the amazing automation platform
- OpenAI for GPT-4 API
- Google for Workspace APIs
- Anthropic for Claude and MCP protocol

---

**Built with ❤️ using n8n + AI Agents + Agentic CLI**