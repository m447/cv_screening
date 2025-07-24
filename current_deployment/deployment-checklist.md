# Deployment Checklist

## Pre-Deployment
- [ ] Import workflow JSON into n8n
- [ ] Configure all OAuth2 credentials
- [ ] Set OpenAI API key
- [ ] Verify Google Drive folder IDs
- [ ] Check Google Sheets permissions

## Configuration
- [ ] Update email recipient in both email nodes
- [ ] Adjust AI Agent prompt if needed
- [ ] Set batch size (currently 5)
- [ ] Configure trigger schedule if using scheduled mode

## Testing
- [ ] Place test CVs in New folder
- [ ] Run workflow manually
- [ ] Verify Google Sheets logging
- [ ] Check email notifications
- [ ] Confirm files moved to correct folders
- [ ] Test error handling with invalid PDF

## Go Live
- [ ] Clear test data from Google Sheets
- [ ] Empty Processed/Failed folders
- [ ] Activate workflow trigger
- [ ] Monitor first batch processing
- [ ] Check for any errors in execution history

## Post-Deployment
- [ ] Monitor daily for first week
- [ ] Review AI scoring accuracy
- [ ] Adjust prompts based on results
- [ ] Document any customizations