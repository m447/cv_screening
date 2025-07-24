# CV Test Cases for AI Screening System

## Test Scenarios Created

### 1. CV_John_Senior_DataScientist.txt
**Expected Results:**
- Score: 85-100
- Category: Highly Qualified
- Key Strengths: 8+ years experience, Python expert, ML/AI expertise, proven ROI

### 2. CV_Sarah_Mid_Analyst.txt
**Expected Results:**
- Score: 50-70
- Category: Qualified
- Key Strengths: 3 years relevant experience, SQL and Python skills, Power BI experience

### 3. CV_Mike_Junior_Graduate.txt
**Expected Results:**
- Score: 20-40
- Category: Possibly Qualified
- Key Points: Recent graduate, basic Excel/SQL knowledge, eager to learn

### 4. CV_Lisa_Wrong_Field.txt
**Expected Results:**
- Score: 0-20
- Category: Unqualified
- Red Flags: Wrong field (Graphic Designer), no data analysis experience

### 5. CV_Alex_Career_Change.txt
**Expected Results:**
- Score: 40-60
- Category: Possibly Qualified / Qualified
- Key Points: Career changer, completed bootcamp, has Python/SQL, marketing analytics background

### 6. CV_Brief_Note.txt
**Expected Results:**
- Score: 10-30
- Category: Unqualified
- Red Flags: Insufficient information, unprofessional format

## How to Test

1. Convert each .txt file to PDF:
   - Open in any text editor
   - Print to PDF
   - Save with same name

2. Upload to your Google Drive test folder one at a time

3. Watch the AI analysis results in Google Sheets

4. Compare actual scores with expected results

## What to Look For

- **Consistency**: Similar CVs should get similar scores
- **Nuance**: AI should recognize career changers and bootcamp graduates
- **Red Flags**: Should identify wrong field or insufficient info
- **Recommendations**: Should provide actionable insights

## Success Metrics

- John (Senior): Should score highest
- Sarah (Mid): Should be qualified
- Alex (Career Change): Should be recognized as promising
- Lisa (Wrong Field): Should be filtered out
- Mike (Junior): Should be "possibly qualified"
- Tom (Brief): Should flag as insufficient