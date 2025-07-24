# Dual Agent Architecture for CV Processing

## Overview

This architecture separates concerns into two specialized AI agents:

### 1. **Scoring Agent** (In-Workflow)
- Analyzes CV content
- Scores candidates (0-100)
- Determines best position match
- Identifies key strengths
- Makes hiring recommendations

### 2. **Research Agent** (Async)
- Researches high-scoring candidates online
- Enriches profiles with public data
- Verifies credentials
- Provides deeper insights

## Benefits of Dual Agent Approach

1. **Specialization**: Each agent focuses on its expertise
2. **Performance**: Research doesn't slow down CV processing
3. **Flexibility**: Easy to update scoring criteria or research methods
4. **Scalability**: Can run multiple instances of each agent
5. **Cost Control**: Only research high-value candidates

## How It Works

```
CV Upload → Scoring Agent → Score ≥ 85? 
                ↓                    ↓
           Log Results          Research Agent
                ↓                    ↓
           Move to Folder      Enrich Profile
```

## Scoring Agent Features

### Dynamic Position Matching
Instead of fixed "Data Analyst" criteria, the agent:
- Evaluates CV against multiple positions
- Suggests best fit
- Provides score for each role
- Adapts to new positions easily

### Consistent Evaluation
- Uses structured prompts
- Returns JSON format
- Maintains scoring standards
- Explains reasoning

### Example Output
```json
{
  "name": "John Doe",
  "email": "john@example.com",
  "overall_score": 87,
  "best_position": "Backend Developer",
  "position_scores": {
    "Backend Developer": 87,
    "DevOps Engineer": 72,
    "Data Analyst": 45
  },
  "technical_skills": "Python, Django, PostgreSQL, Docker",
  "experience_summary": "5 years backend development",
  "key_strengths": "Strong API design, microservices architecture",
  "recommendation": "Strong candidate for senior backend role"
}
```

## Research Agent Features

### Triggered Automatically
- Only for candidates scoring ≥ 85
- Runs asynchronously
- Doesn't block main workflow

### Research Capabilities
- LinkedIn profile search
- GitHub activity check
- Portfolio/website review
- Publication search
- Credential verification

### Research Output
Stored separately, includes:
- Online profiles found
- Project highlights
- Community involvement
- Red flags (if any)
- Additional insights

## Implementation Steps

### Phase 1: Basic Dual Agents
1. Deploy scoring agent workflow
2. Update main workflow to use scoring agent
3. Deploy research agent (manual trigger)
4. Test with sample CVs

### Phase 2: Automated Research
1. Connect research agent to main workflow
2. Set threshold (e.g., score ≥ 85)
3. Configure research storage
4. Add notification for findings

### Phase 3: Advanced Features
1. Multi-position scoring
2. Industry-specific research
3. Competitive analysis
4. Team fit assessment

## Configuration

### Scoring Agent Settings
```javascript
// In scoring agent system prompt
const positions = [
  "Data Analyst",
  "Backend Developer", 
  "DevOps Engineer",
  "Product Manager"
];

const scoringCriteria = {
  technicalSkills: 40,
  experience: 30,
  education: 20,
  softSkills: 10
};
```

### Research Agent Triggers
```javascript
// Conditions for research
if (score >= 85) {
  triggerResearch();
}

// Or for specific positions
if (score >= 80 && position === "Senior Developer") {
  triggerDeepResearch();
}
```

## Advantages Over Single Agent

1. **Faster Processing**: CVs scored quickly without waiting for research
2. **Better Resource Use**: Expensive research only for qualified candidates  
3. **Modular Updates**: Change scoring without affecting research
4. **Error Isolation**: Research failures don't impact scoring
5. **Clear Audit Trail**: Separate logs for scoring and research

## Future Enhancements

### Scoring Agent Evolution
- Learn from hiring decisions
- Adjust weights based on success
- Industry-specific models
- Multi-language support

### Research Agent Evolution  
- Real-time social media monitoring
- Patent and publication tracking
- Competitive intelligence
- Reference checking automation

## Simple Start Approach

1. **Week 1**: Implement scoring agent only
2. **Week 2**: Add basic research for top candidates
3. **Week 3**: Automate research triggers
4. **Week 4**: Enhance with advanced features

This gradual rollout ensures stability while adding powerful capabilities.